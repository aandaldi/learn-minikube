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
      name: flask-simple
      labels:
        run: flask-simple
        app: test-deployment
        tier: flask-simple
    spec:
      replicas: 2
      template:
        metadata:
          labels:
            run: flask-simple
        spec:
          containers:
          - name: flask-simple
            image: aand/learn-flask:latest
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
      name: flask-simple
      labels: 
    spec:
      selector:
        run: flask-simple
      sessionAffinity: ClientIP
      ports:
        - name: port1
          protocol: TCP
          port: 5000
          #targetPort: 5000
          nodePort: 30001
      type: NodePort

    ~~~
    
    Note:
    ~~~
    using `nodePort` eg `nodePort: 30001` to create static port. 
    
4. apply Yaml File using `apply -f`

    `kubeclt apply -f Service-learn-docker.yaml`
    
    `kubeclt apply -f Service-learn-docker.yaml`
    
Or You can run the app using command line 

`kubectl run flask-kubernetes-example --image=aandaldi/learn-docker:latest --port=5000 --image-pull-policy=Never`

`kubectl expose deployment flask-kubernetes-example --type=NodePort`

5. You access the app using url. check the url using `minikube service <service-name> --url`