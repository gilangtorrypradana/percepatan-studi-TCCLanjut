Docker Swarm merupakan Container Orchestration System yang disediakan
oleh docker untuk memdistribusikan docker contrainer pada system
cluster. Dengan kita memanfaatkan [docker
swarm](http://www.dimasrio.com/2017/08/container-orchestration-docker-swarm.html),
kita dapat membuat sebuah management yang centralized baik itu create,
destroy, scaling up dan scaling down container pada docker swarm
manager.

![](media/image1.jpeg){width="6.5in" height="4.875in"}

**Konfigurasi Docker Swarm**

Untuk membuat sebuah docker swarm, diperlukan host yang sudah terinstall
docker engine. Pada contoh kali ini saya akan membuat 3 buah docker
node, dimana 1 node akan digunakan sebagai manager dan 2 buah node
lainnya digunakan untuk swarm worker. Agar lebih mudah membuat docker
host kita dapat menggunakan [docker
machine](http://www.dimasrio.com/2017/07/install-docker-machine-di-linux.html).

**Setup docker host menggunakan docker machine**

*docker-machine create -d virtualbox manager*\
*docker-machine create -d virtualbox node1*\
*docker-machine create -d virtualbox node2*

Output :

![](media/image2.png){width="4.166666666666667in" height="0.625in"}

**Setup docker swarm cluster pada node manager**

Login ke dalam docker swarm manager menggunakan docker-machine.

*docker-machine ssh manager*

Create initial sebagai master dengan ip dari host docker swarm manager
192.168.99.100

*docker swarm init \--advertise-addr 192.168.99.100*

Setelah di setup, akan muncul token yang digunakan untuk join node ke
docker swarm manager.

*docker swarm join \--token
SWMTKN-1-2rqhmivd1tx2c3pdw50x383tzf0v486kej1or3rk6ecjy31476-3cz5305q7mc2sfnvervpd8bmy
192.168.99.100:2377 *

Output :

![](media/image3.png){width="4.166666666666667in"
height="2.3645833333333335in"}

**Setup docker worker untuk join ke cluster**

Login ke dalam docker node1 menggunakan docker-machine.

*docker-machine ssh node1*

Jalankan command dari swarm manager pada node 1 dan node 2 untuk join
node pada cluster.\
\
Output :

![](media/image4.png){width="4.166666666666667in"
height="1.6770833333333333in"}

![](media/image5.png){width="4.166666666666667in"
height="1.5729166666666667in"}

Sempai disini semua node sudah join pada cluster.

![](media/image6.png){width="4.166666666666667in"
height="0.5729166666666666in"}

**Create service pada docker swarm**

Pada contoh kali ini saya akan membuat sebuah service http menggunakan
nginx dengan jumlah container 5.

*docker service create \--name web \--replicas=5 -p 8080:80
nginx:alpine*

Pastikan container sudah terbuat.

*docker service ls*

Output :

![](media/image7.png){width="4.166666666666667in"
height="0.4791666666666667in"}

Kita juga dapat melihat proses orchestration untuk service web yang
running pada node lain.

*docker service ps web*

Output :

![](media/image8.png){width="4.166666666666667in"
height="1.1770833333333333in"}

Sampai disini web service sudah running, pastikan sudah dapat di akses
via browser.

*http://192.168.99.100:8080*

Output:

![](media/image9.png){width="4.166666666666667in"
height="0.9166666666666666in"}

**Scaling up dan Scaling down**

Seperti yang sudah saya sebutkan di atas, salah satu fitur dari docker
swarm adalah scaling. Pada contoh kali ini, saya akan scale jumlah
container untuk service web.

*docker service scale web=10*

Output :

![](media/image10.png){width="4.166666666666667in"
height="2.1458333333333335in"}

**Inspect node via swarm manager**

Pada docker swarm manager kita dapat melihat informasi mengenai node
worker.

*docker node inspect \--pretty node1*

Output :

![](media/image11.png){width="3.3645833333333335in"
height="2.279505686789151in"}

**Setup mode drain untuk maintenance**

Jika kita ingin melakukan update pada host tanpa mengganggu container
yang sedang running dapat menggunakan mode drain. Mode drain akan
memindahkan seluruh container ke host yang sedang aktif. Pada contoh
kali ini saya kan mencoba setup mode drain pada docker swarm manager.

*docker node update \--availability drain manager*

Output:

![](media/image12.png){width="4.166666666666667in"
height="2.5833333333333335in"}

Dari gambar diatas, terlihat container yang running pada swarm manager
sudah pindah ke node1 dan node2.

Selanjutnya kita dapat update image melalui swarm manager. Sebagai
contoh saya akan update image dari nginx ke httpd.

*docker service update \--image httpd:alpine web*

Output:

![](media/image13.png){width="4.166666666666667in" height="2.53125in"}

Dari gambar diatas container sudah menggunakan images httpd.

**Remove service**

Jika kita ingin menghapus service dapat menggunakan perintah berikut.

*docker service rm web*

Demikian tutorial docker kali ini mengenai [container orchestration
dengan docker
swarm](http://www.dimasrio.com/2017/08/container-orchestration-docker-swarm.html).
Docker swarm sangat powerfull, akan tetapi jika sobat ingin mencoba
orchestration yang lebih menarik bisa
mencoba [Kubernetes](https://kubernetes.io/).
