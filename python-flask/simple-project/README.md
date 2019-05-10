###### Simple Flask App on Minikube
start minikube and use docker daemon on minikube with `eval $(minikube docker-env)
`
1. Clone code from https://github.com/aandaldi/learn-docker.git
2. Open python-flask/simple-project
    
    you can build your code from docker file here or you can pull the images from docker hub 
    (https://cloud.docker.com/u/aandaldi/repository/docker/aandaldi/simple-crud-flask-mysql)

3. Create Yaml file (service and deployment)
    example:
    Deployment-learn-docker.yaml
    ~~~
    apiVersion: extensions/v1beta1
    kind: Deployment
    metadata:
      name: learn-docker
      labels:
        run: learn-docker
    spec:
      replicas: 2
      template:
        metadata:
          labels:
            run: learn-docker
        spec:
          containers:
          - name: learn-docker
            image: aandaldi/learn-docker
            imagePullPolicy: Never
            ports:
            - containerPort: 5000
              protocol: TCP
          imagePullSecrets:
          - name: regsecret
         
    ~~~
    
    Service-learn-docker.yaml
    
    ~~~
    kind: Service
    apiVersion: v1
    metadata:
      name: learn-docker
    spec:
      selector:
        run: learn-docker
      sessionAffinity: ClientIP
      ports:
        - name: port1
          protocol: TCP
          port: 5000
          targetPort: 5000
      type: NodePort

    ~~~
    
4. apply Yaml File using `apply -f`

    `kubeclt apply -f Service-learn-docker.yaml`
    
    `kubeclt apply -f Service-learn-docker.yaml`
    
Or You can run the app using command line 

`kubectl run flask-kubernetes-example --image=aandaldi/learn-docker:latest --port=5000 --image-pull-policy=Never`

`kubectl expose deployment flask-kubernetes-example --type=NodePort`

5. You access the app using url. check the url using `minikube service <service-name> --url`