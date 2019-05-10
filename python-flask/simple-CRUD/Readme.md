###### Simple Flask App on Minikube
start minikube and use docker daemon on minikube with `eval $(minikube docker-env)
`
1. Clone code from https://github.com/aandaldi/learn-docker.git
2. Open python-flask/simple-CRUD
    
    

3. Create Yaml file (service and deployment)
    example:
    Deployment-learn-docker.yaml
    ~~~
    
    ~~~
    
    Service-learn-docker.yaml
    
    ~~~
    

    ~~~
    
4. apply Yaml File using `apply -f`

    `kubeclt apply -f Service-learn-docker.yaml`
    
    `kubeclt apply -f Service-learn-docker.yaml`
    
Or You can run the app using command line 

`kubectl run flask-kubernetes-example --image=aandaldi/learn-docker:latest --port=5000 --image-pull-policy=Never`

`kubectl expose deployment flask-kubernetes-example --type=NodePort`

5. You access the app using url. check the url using `minikube service <service-name> --url`