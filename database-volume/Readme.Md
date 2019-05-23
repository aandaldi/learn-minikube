### Deploy MySQL on minikube
 We will to deploy a single mysql on one container
 
 apply file 
- deployment-mysql.yaml
- persistent-volume-claim-mysql.yaml
- persistent-volume-mysql.yaml
- service-mysql.yaml
 
 dalam service mysql menggunakan `nodePort:30036` yang akan membuat port static 30036, jadi setiap kali podsnya down dan up lagi, port akan tetap berada di 30036.
 
 
###### Volume di minikube
- untuk menggunakan volume ini, kita harus membuat persistant volume terlebih dahulu, kemudian menambahkan `hostPath`
dimana ini akan mengarahkan file file yang di mount ke path ini didalam host.
- selanjutnya buat persistant volume claim untuk membuat blok dari persistant volume sebelumnya.
- setelah itu didalalam file deployment yaml, kita tambahkan volumeMount dan volumes. contohnya sebagai berikut:
        
        spec:
          containers:
            volumeMounts:
              - name: persistent-storage-db
                mountPath: /var/lib/mysql
          volumes:
            - name: persistent-storage-db
              persistentVolumeClaim:
                claimName: mysql-pv-claim
            
    
    claimName harus mengarah ke PVC name
    
 untuk mengakses data di dalam host yang sudah di mounting ke pods/container bisa dengan cara ssh ke host tersebut,
    
    minikube ssh