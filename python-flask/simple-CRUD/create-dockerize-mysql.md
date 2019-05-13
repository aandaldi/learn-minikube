###Setting Database external "Dockerized" dengan MySQL

File file yang harus di persiapkan adalah (bisa di lihat di folder yaml):

1. persistent-volume-mysql.yaml
2. persistent-volume-claim-mysql.yaml
3. deployment-mysql.yaml
4. service-mysql.yaml

apply semua file tersebut.

######Remark:
This deployment uses the mysql:5.6 image.
The replicas are set to 1, so the ReplicaSet ensures that 1 pod replica is running at any given time.
The ReplicaSet manages all the pods with labels that match the selector. In my case these labels are:


    |  Label Key  | Label Value | 
    | :---------: | :---------: |
    | app         | mysql       |
    | version     | 1.0         |
    | environment | testing     |
    

#####Mysql-client
`kubectl run -it --rm --image=mysql:5.6 --restart=Never mysql-client -- mysql -h mysql -ppassword`


 