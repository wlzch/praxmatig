---
layout: post
title: "Vertical Centering Image"
date: 2013-03-01 20:04
comments: true
categories: [html, css, image, vertical center]
---

Hari ini saya akan bercerita tentang cara untuk melakukan vertical centering pada image, masalah yang lumayan umum ditemui untuk website yang akan menampilkan gambar dengan berbagai macam dimensi dan orientasi.
<!-- more -->

Karena judul KP saya berhubungan dengan foto, jadi saya sendiri juga menemui masalah ini pada saat pembuatan dan akan membagikan caranya kepada anda sekarang. Sebelumnya, inilah beberapa contoh dari apa yang terjadi ketika image dengan dimensi yang berbeda dimasukkan.

[![Vertical Center Image](http://img521.imageshack.us/img521/2050/rawpose3.jpg)](http://img521.imageshack.us/img521/2050/rawpose3.jpg)
[![Vertical Center Image](http://img4.imageshack.us/img4/4779/rawpose2.jpg)](http://img4.imageshack.us/img4/4779/rawpose2.jpg)
[![Vertical Center Image](http://img803.imageshack.us/img803/6627/rawpose1.jpg)](http://img803.imageshack.us/img803/6627/rawpose1.jpg)

Dapat dilihat bahwa pada gambar pertama yang berorientasi landspace dan berdimensi besar, gambar tersebut memenuhi containernya secara pas.

Pada gambar kedua dengan orientasi portrait dan height yang besar, gambar memenuhi bagian vertical secara pas tetapi diposisikan ke tengah secara horizontal.

Di gambar terakhir dengan orientasi landspace dan dimensi yang lebih kecil dari container dapat kita lihat bahwa gambar diposisikan ketengah secara vertical dan horizontal.

Berikut adalah html+css yang dibutuhkan melakukan image vertical centering. Anda dapat menggunakan jsfiddle dibawah untuk melakukan percobaan.

{% jsfiddle KZQT8 html,css,result,js %}

Inti dari teknik ini sebenarnya terletak pada [inline-block](http://www.impressivewebs.com/inline-block) display dan [vertical-align](https://developer.mozilla.org/en-US/docs/CSS/vertical-align).

Menurut [Robert Nyman](http://robertnyman.com/2010/02/24/css-display-inline-block-why-it-rocks-and-why-it-sucks/):
> INLINE-BLOCK ADALAH SUATU CARA UNTUK MEMBUAT ELEMEN INLINE TETAPI MEMILIKI SIFAT-SIFAT BLOCK SEPERTI WIDTH, HEIGHT, MARGIN DAN PADDING.

Dan menurut [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/CSS/vertical-align)
> VERTICAL-ALIGN MENSPESIFIKASIKAN BAGAIMANA SUATU ELEMEN AKAN DIPOSISIKAN SECARA VERTIKAL. VERTICAL-ALIGN HANYA BERLAKU PADA ELEMEN YANG BERDISPLAY INLINE ATAU TABLE-CELL.

Berdasarkan definisi diatas dapat disimpulkan bahwa 2 atau lebih elemen dapat disejajarkan secara vertical jika elemen tersebut bersifat inline atau table-cell.

Jika 2 elemen yang berdisplay inline-block diberikan vertical-align: middle maka elemen-elemen tersebut akan diposisikan ke tengah secara vertikal. Hasilnya dapat dilihat pada gambar berikut.

{% img center http://img266.imageshack.us/img266/9262/inlineblock.jpg %}

Dengan teknik-teknik tersebut, maka kita telah dapat membuat Vertical Center Image!!

``` css
.image-viewer {
  background-color: #000;
  line-height: 480px;
  text-align: center;
}
```

.image-viewer adalah container dari image. Disini kita harus memberikan line-height dengan nilai yang diinginkan untuk height minimal dari .image-viewer sehingga meskipun gambar yang ada didalamnya kecil container tidak akan ikut mengecil tetapi jika gambar didalamnya besar container akan ikut membesar. Selain itu kita memberikan text-align: center sehingga image didalamnya akan terposisi ke tengah secara horizontal. Ingatlah bahwa kita telah mendefinisikan image sebagai elemen inline sehingga bisa dialignkan sebagaimana kita mengalignkan text.

``` css
.image-viewer img:before {
  max-height: 720px;
  content: "";
  display: inline-block;
  height: 100%;
  vertical-align: middle;
}
```

:before merupakan salah satu pseudoclasses yang dapat digunakan. Ingat bagaimana 2 elemen inline-block dapat divertical-alignkan? Disini kita akan membuat elemen invisible yang digunakan untuk membantu mengalignkan image.

{% img center http://img203.imageshack.us/img203/742/verticalalign.jpg %}

kotak yang berwarna hijau tersebutlah yang akan membantu image tervertical-align. heightnya diset menjadi 100% sehingga image bisa benar-benar diposisikan ditengah.

``` css
.image-viewer img {
  max-height: 720px;
  max-width: 720px;
  vertical-align: middle;
  display: inline-block;
}
```

Saya rasa anda sudah dapat mengerti baris css diatas. max-height dan max-width digunakan sehingga image tidak akan ditampilkan terlalu besar sehingga menggangu tampilan lainnya.

Pada postingan berikutnya, saya akan memberikan tutorial untuk menjustify div seperti pada [deviantart.com](http://deviantart.com)
