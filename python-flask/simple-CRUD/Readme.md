###### Simple Flask App on Minikube
start minikube and use docker daemon on minikube with `eval $(minikube docker-env)`
in this session, we will create connection apps and database in different pod.

1. Clone code from https://github.com/aandaldi/learn-minikube.git
2. Open python-flask/simple-CRUD theb you can apply the deployment and service of database (https://github.com/aandaldi/learn-minikube/tree/master/database-volume).
    create manual test_crud schema database, because in this example not include ORM to create database.
    create schema like this:
    
        CREATE TABLE `tbl_user` (
         `user_id` bigint(20) NOT NULL AUTO_INCREMENT,
         `user_name` varchar(45) COLLATE utf8_unicode_ci DEFAULT NULL,
         `user_email` varchar(45) COLLATE utf8_unicode_ci DEFAULT NULL,
         `user_password` varchar(255) COLLATE utf8_unicode_ci DEFAULT NULL,
         PRIMARY KEY (`user_id`)
           ) ENGINE=InnoDB AUTO_INCREMENT=2 DEFAULT CHARSET=utf8 COLLATE=utf8_unicode_ci;
        
           /*Data for the table `tbl_user` */
        
           insert  into `tbl_user` values 
           (1,'Soumitra Roy Sarkar','contact@roytuts.com','pbkdf2:sha256:50000$obX7AAZv$61ba4f743eff511');

3. Create Yaml file (service and deployment) for app
    example:
     
     Service-flask-app.yaml
    
    ~~~
    kind: Service
    apiVersion: v1
    metadata:
      name: flask-app
      labels: 
        app: flask-app
    spec:
      selector:
        app: test-deployment
      sessionAffinity: ClientIP
      ports:
        - name: port1
          protocol: TCP
          port: 5000
          targetPort: 5000
      type: NodePort

    ~~~
    
    Deployment-flask-app.yaml
    ~~~
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: mysql
      labels: 
        # run: flask-app
        app: test-deployment
    spec:
      replicas: 1
      selector:
        matchLabels:
          app: test-deployment
      strategy:
        type: Recreate
      template:
        metadata:
          labels:
            # run: flask-app
            app: test-deployment
        spec:
          containers:
          - name: mysql
            image: mysql:5.6
            args:
              - "--ignore-db-dir=lost+found"
            volumeMounts:
              - name: persistent-storage-db
                mountPath: /var/lib/mysql
            env:
              - name : MYSQL_RANDOM_ROOT_PASSWORD
                value: 'yes'
                name: MYSQL_ROOT_PASSWORD
                value: 'root'
              - name: MYSQL_USER
                value: 'root'
              - name: MYSQL_PASSWORD
                value: 'root'
          volumes:
            - name: persistent-storage-db
              persistentVolumeClaim:
                claimName: mysql-pv-claim
            
    ~~~
   
   ######note:
   
   - untuk menghubungkan antar pods gunakan `selector` yang sama pada file service dan deployment di semua service 
   yang ingin di koneksikan. 
   - pada file konfigurasi koneksi ke database di aplikasi, gunakan nama service databasenya sebagai nama host, 
   sehingga walaupun endpointnya berubah, koneksi tetap diarahkan ke nama service tersebut;
   - setting node port di masing masing service untuk mengekspose service tersebut agar dapat diakses dari luar.

4. apply Yaml File using `apply -f`

        kubeclt apply -f service-flask-app.yaml
        kubeclt apply -f Deployment-flask-app.yaml
    
Or You can run the app using command line 

`kubectl run flask-kubernetes-example --image=aandaldi/learn-docker:latest --port=5000 --image-pull-policy=Never`

`kubectl expose deployment flask-kubernetes-example --type=NodePort`

5. You access the app using url. check the url using `minikube service <service-name> --url`