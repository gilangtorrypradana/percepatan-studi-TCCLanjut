Docker compose berfungsi untuk menjalankan container docker secara
bersamaan.

Docker compose ini sangat berguna ketika aplikasi kita terpisah -- pisah
pada komputer yang berbeda, contohnya adalah aplikasi yang dibuat berada
pada 1 container sedangkan database yang akan digunakan oleh aplikasi
tersebut berada pada container yang lain. Ketika menggunakan docker
compose maka kita dapat menjalankan kedua container tersebut secara
bersamaan dan bahkan kita dapat melakukan link ke container yang kita
inginkan.

Langkah instalasi docker-compose:

1\. sudo su -- root

2\. curl -L
https://github.com/docker/compose/releases/download/1.19.0/docker-compose-\`uname
-s\`-\`uname -m\` -o /usr/local/bin/docker-compose

3\. chmod +x /usr/local/bin/docker-compose

4\. chmod -R 777 /usr/local/bin/docker-compose

5\. docker-compose -version, akan muncul :

docker-compose version 1.19.0, build 9e633ef

sampai sini docker-compose sudah siap digunakan.

Berikut contoh penggunaan docker-compose untuk installasi NGINX :

1\. buat folder /docker/nginx dan cd /docker/nginx

2\. nano docker-compose.yml :

--- awal file ---

 web:

  ports:

   - "8080:80"

  volumes:

  - ./www:/usr/share/nginx/html:rw

--- akhir file ---

3\. jalankan file compose : docker-compose up -d

4\. cek service yang dijalankan : docker-compose ps

5\. akses ke laman web nginx menggunakan browser
http:/ip\_server\_docker:8080/

6\. letakan isi laman web pada folder /docker/nginx/www/

mudah sekali bukan?

perintah detail lain nya untuk docker maupun docker-compose dapat
dilihat dengan menambahkan --help dibelakang command yang ingin
diketahui, misal : docker --help atau docker-compose --help atau
docker-compose up --help

sumber :

https://rizkimufrizal.github.io/belajar-docker/

http://www.drupalindonesia.com/article/tutorial/konfigurasi-dasar-docker-sebagai-development-environment-untuk-drupal

http://www.dimasrio.com/2017/03/cara-menggunakan-docker-compose.html
