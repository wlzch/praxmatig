---
layout: post
title: "Amazon Web Service For Beginner Part 1"
date: 2013-09-25 20:28
comments: true
categories: [aws, technology, cloud]
---

## Apa itu Amazon Web Service (AWS) ?
AWS adalah layanan cloud computing yang berupa kumpulan web service yang ditawarkan oleh [Amazon](http://aws.amazon.com). Sebelum kita membahas aws lebih lanjut, kita akan membahas apa sebenarnya cloud computing itu?

## Cloud Computing
{% blockquote AWS http://aws.amazon.com/what-is-cloud-computing/ %}
Whether you are running applications that share photos or support the critical operations of your business, you need rapid access to flexible and low cost IT resources. The term "cloud computing" refers to the on-demand delivery of IT resources via the Internet with pay-as-you-go pricing.
{% endblockquote %}

Amazon mendefinisikan cloud computing sebagai layanan penyedia sumber daya TI yang dapat ditingkatkan atau diturunkan sesuai dengan kebutuhan dan juga harga yang tergantung pemakaian. Hal ini berarti jika web Anda sedang memiliki traffic yang tinggi Anda dapat menaikkan sumber daya yang Anda perlukan sendiri tanpa menunggu konfirmasi dari pihak provider (AWS). Selain itu, Anda hanya perlu membayar jika Anda benar-benar memakai sumber daya tersebut.

Jika Anda pernah memakai web hosting dengan pembayaran bulanan/tahunan Anda pasti tahu bahwa kita harus membayar secara penuh meskipun tidak ada traffic pada web kita. Selain itu proses upgrade/downgrade memerlukan konfirmasi dari provider. Hal ini berbeda dengan aws dimana kita hanya membayar untuk sumber yang dipakai (tidak sepenuhnya tetapi akan saya jelaskan nanti).

## Layanan AWS
AWS menyediakan bermacam-macam layanan yaitu:

1. Elastic Compute Cloud (EC2)
2. Simple Storage Service (S3)
3. CloudFront
4. Elastic Block Store (EBS)
5. Simple Email Service (SES)
6. Route 53
7. Relational Database Service (RDS)
8. Elastic Beanstalk
9. Auto Scaling
10. ElasticCache
11. [Dan banyak lagi](http://aws.amazon.com/products/)

Karena postingan ini untuk beginner dan tidak semua layanan bisa saya gunakan, saya hanya akan membahas beberapa layanan saja.

## Kelebihan AWS

* Amazon sendiri menggunakan AWS untuk menjalankan amazon.com. Hal ini berarti bahwa kita dapat menggunakan infrastruktur yang sama dengan infrastruktur yang digunakan oleh amazon.com.
* Kita dapat memilih region dimana layanan AWS yang kita pakai akan ditaruh. Semakin dekat user dengan region maka akses akan menjadi lebih cepat. Misalnya jika anda memiliki target pasar hanya di Indonesia maka region Singapore merupakan pilihan yang paling bagus karena letaknyalah paling dekat.
* Mudah menaikkan/menurunkan resource yang diperlukan sesuai kebutuhan.
* Bayar hanya resource yang dipakai.

## Free Usage Tier
Jika anda memiliki kartu kredit dan ingin mencoba, AWS menyediakan [free usage tier](http://aws.amazon.com/free/) sehingga Anda dapat mencoba layanannya selama 1 tahun (dan beberapa kondisi lainnya). Anda akan dikenai biaya jika pemakaian melebihi batasan maksimal yang diperbolehkan oleh AWS. Ikuti tutorial [ini](http://www.techrepublic.com/blog/datacenter/initial-sign-up-on-amazon-web-services/5020) jika Anda ingin mendaftar.

## Elastic Compute Cloud (EC2)
[EC2](http://aws.amazon.com/ec2) adalah layanan yang paling populer diantara layanan-layanan lainnya. EC2 menyediakan kapasitas komputasi yang dapat ditambah atau diturunkan sesuka hati (atau bisa juga diatur secara otomatis untuk menaikkan/menurunkan kapasitas komputasi sesuai dengan traffic).

EC2 menggunakan istilah *instance* yang secara sederhana dapat kita anggap sebagai server. Instance dapat juga disamakan dengan VPS dimana kita memiliki kontrol penuh atas instance tersebut. Web server, firewall, mail server, database server atau apapun dapat kita install dan konfigurasikan dalam instance tersebut.

Untuk menggunakan EC2, anda hanya perlu:

* Pilih AMI (Amazon Machine Image) yang telah disediakan oleh AWS atau bisa Anda pilih dari AWS marketplace. Selain itu, Anda dapat juga membuat AMI Anda sendiri.

  AMI dapat berisi hanya Sistem Operasi ataupun Sistem Operasi yang telah disertai dengan aplikasi, data dan konfirgurasi. AWS marketplace menyediakan beragam AMI untuk Anda pilih.

* Konfigurasi security dan network access instance Anda.

* Pilih tipe instance yang Anda mau kemudian jalankan instance tersebut. Sebuah instance hanya memerlukan beberapa menit untuk siap dipakai.

* Pilih region dimana Anda ingin instance Anda ditaruh kemudian berikan IP pada instance tersebut.

Perlu diperhatikan bahwa EC2 hanya menyediakan instance storage yang sifatnya tidak permanen. Data yang tersimpan dalam instance storage akan hilang jika Anda melakukan terminate terhadap instance tersebut. Untuk storage yang bersifat permanen Anda dapat menggunakan EBS yang akan dijelaskan nanti. Lalu apa gunanya instance storage? Instance storage memiliki kecepatan akses yang lebih cepat daripada EBS sehingga instance storage cocok untuk menyimpan data sementara dan memerlukan akses yang cepat. Contoh yang paling cocok menurut saya adalah caching.

## Tipe Instance EC2

AWS menyediakan banyak tipe instance EC2 yang dapat dipilih sesuai kebutuhan dengan spesifikasi dan harganya masing-masing.

Beberapa tipe instance diantaranya:

* M1 Small Instance 1.7 GB memory, 1EC2 Compute Unit, 160GB instance storage, 32bit/64bit
* M1 Medium Instance 3.75 GB memory, 2EC2 Compute Unit, 410GB instance storage, 32bit/64bit
* Micro Instance 613MB memory, 1EC2 (2EC2 untuk traffic yg tiba2 meningkat), tidak ada instance storage, harus memakai EBS, 32bit/64bit. Micro instance dapat kita gunakan secara gratis sesuai dengan free usage tier.
* Dan masih banyak tipe instance [lainnya](http://aws.amazon.com/ec2/instance-types/).

## Harga

Harga layanan EC2 aga sedikit rumit. Harga yang harus dibayar untuk layanan EC2 tergantung pada beberapa hal seperti:

### Harga/Jam/Instance

Untuk setiap instance yang sedang aktif, aws akan mengcharge masing-masing instance berdasarkan berapa jam instance tersebut aktif. AWS menyediakan 3 pilihan pembayaran instance:

1. [On-demand Instance](http://aws.amazon.com/ec2/pricing/#on-demand)

   Pembayaran on-demand instance didasarkan langsung pada jam aktif instance. Harga dari instance juga tergantung kepada jenis instance yang Anda pilih apakah micro instance, small instance atau jenis instance lainnya dimana harganya bervariasi mulai dari $0.027/jam untuk micro instance, $0.08/jam untuk small instance sampai pada $1.36/jam untuk second generation standard on-demand instance versi untuk extra-large instance.

   Jika Anda menggunakan free usage tier dari AWS, Anda dapat menggunakan micro instance selama 720jam/bulan yang artinya jika Anda hanya menggunakan 1 micro instance, Anda dapat memakainya secara gratis selama 1 tahun. Tetapi hati-hati karena masih ada biaya lain yang harus diperhatikan

2. [Reserved Instance](http://aws.amazon.com/ec2/pricing/#reserved)

   Anda juga dapat memilih pembayaran untuk reserved instance dimana Anda akan mendapatkan harga/jam yang jauh lebih murah daripada on-demand instance. Tetapi selain harga/jam, Anda juga harus melakukan pembayaran 1x diawal. Reserved instance dapat dibeli dalam paket 1 tahun ataupun 3 tahun. Jika Anda me-reserve small instance Anda harus melakukan pembayaran $76 di awal ditambah dengan $0.049 untuk setiap jam instance dihidupkan.

    Jika Anda mereserve small instance paket 1 tahun Anda akan menghabiskan $499.36/tahun dimana jika Anda menggunakan on-demand instance Anda perlu menghabiskan $691.2/tahun. Perbedaan yang cukup besar bukan?

3. [Spot Instance](http://aws.amazon.com/ec2/pricing/#spot)

   Harga dari spot instance dapat berubah-rubah sesuai dengan banyaknya persediaan/permintaan atas spot instance tersebut. Spot instance saya rasa lebih jarang digunakan karena harga instance harus dimonitor terus-menerus.

### [Data Transfer](http://aws.amazon.com/ec2/pricing/#DataTransfer)

Anda juga harus membayar data yang masuk/keluar(bandwidth) dari instance Anda.

### [EBS Optimized Instance](http://aws.amazon.com/ec2/pricing/#EBS-Optimized)

Anda dapat mengaktifkan fitur EBS-optimized pada instance Anda untuk mendapatkan I/O yang cepat. Namun Anda harus membayar untuk fitur ini.

### [Elastic IP Adresses](http://aws.amazon.com/ec2/pricing/#elastic-ip)

Anda akan mendapatkan 1 IP address secara gratis untuk digunakan pada instance Anda. Tetapi Anda akan dikenakan biasa untuk IP tambahan. Selain itu jika IP address yang Anda buat tidak digunakan pada instance aktif manapun, Anda juga akan dikenakan biaya.

Sekian penjelasan singkat untuk EC2. Untuk penjelasan lengkapnya silahkan kunjungi [EC2](http://aws.amazon.com/ec2/).

Selanjutnya mungkin saya akan menulis mengenai Simple Storage Service (S3) dan Cloudfront. Jika ada pertanyaan silahkan hubungi saya di [@WilHn](https://twitter.com/wilhn) :D
