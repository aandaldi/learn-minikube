# Learn Minikube

1. Install minikube, KVM
2. Install kubectl

# Perintah Dasar
1. `minikube start` `minikube stop` `minikube delete` 
2. `minikube ssh`
3. `minikube dashboard`
4. `eval $(minikube docker-env)` menggunakan Docker Daemon dari minikube yang sedang berjalan

![alt text](https://www.redhat.com/cms/managed-files/kubernetes-diagram-2-824x437.png)

1. Pod
Adalah satu grup container instance. Kita bisa menjalankan beberapa container 
(misalnya aplikasi web + redis cache + logging service) dalam satu pod. 
Antar container dalam satu pod bisa saling mengakses dengan menggunakan alamat localhost. 
Anggap saja pod seperti laptop yang kita pakai coding. Untuk mengakses database dari aplikasi kita,
biasanya kita pakai alamat localhost.

2. Node
Adalah representasi dari satu mesin. Mesin ini bisa saja mesin virtual (seperti VPS atau dropletnya DigitalOcean) atau fisik.

3. Service
Merupakan mekanisme untuk mengekspos pod kita ke dunia luar. 
Aplikasi kita yang berjalan dalam pod tidak memiliki alamat IP yang tetap. 
Agar bisa diakses oleh aplikasi lain atau oleh user, kita perlu alamat IP yang tetap. 
Service menyediakan alamat IP yang tetap, yang nantinya akan kita arahkan ke pod kita dengan menggunakan selector.


#### Pod dan Kontroler

Kontroler dapat membuat dan mengelola banyak Pod untuk kamu, menangani replikasi dan menyediakan kemampuan penyembuhan diri sendiri pada lingkup kluster. 
Sebagai contoh, jika sebuah Node gagal, kontroler akan otomatis mengganti Pod tersebut dengan menempatkan Pod yang identik di Node yang lain.

Beberapa contoh kontroler yang berisi satu atau lebih Pod meliputi:

- Deployment
- StatefulSet
- DaemonSet

Secara umum, kontroler menggunakan template Pod yang kamu sediakan untuk membuat Pod.

###### Perintah secara umum

~~~
# delete all pods
 kubectl delete --all pods --namespace=default

# deete all deployments
 kubectl delete --all deployments --namespace=default
 
# delete all services
 kubectl delete --all services --namespace=default

~~~