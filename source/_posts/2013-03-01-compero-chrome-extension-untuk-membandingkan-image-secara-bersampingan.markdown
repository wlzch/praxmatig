---
layout: post
title: "Compero: Chrome Extension Untuk Membandingkan Image Secara Bersampingan"
date: 2013-03-01 19:31
comments: true
categories: [Chrome Extensions, Compero]
---

Pada postingan kali ini saya akan membahas bagaimana saya membuat compero, chrome extension pertama saya yang gunanya untuk membandingkan image-image secara berdampingan.
[Compero](http://img839.imageshack.us/img839/4298/screenshotfdg.png)

Untuk mencobanya download [compero](https://github.com/wlzch/compero/archive/master.zip), setelah itu mengikuti [tutorial](http://developer.chrome.com/extensions/getstarted.html) pada bagian "Load The Extension"

Cara penggunaan compero saya usahakan semudah mungkin. Tapi kalau anda memiliki ide yang lebih baik, silahkan komentar atau submit issue di [repository](https://github.com/wlzch/compero).

Setelah compero terinstall jika anda mengklik kanan pada image anda dapat melihat tulisan "Add to Compero". Jika anda mengklik tulisan tersebut, maka secara otomatis image tersebut dimasukkan ke compero. (terima kasih kepada [suwandi halim](https://github.com/kiddo) untuk idenya). Jika anda mengklik icon Compero di toolbar, anda telah dapat melihat image yang anda kirimkan tadi! Mudah kan?

Pada saat anda memasukkan image, maka image tersebut akan diresize menjadi 200px. Tapi jangan khawatir karena ada slider untuk mengatur ukuran image dari range 200px - 400px. Range ini yang dipilih karena batasan chrome yang hanya memungkinkan popup memiliki lebar 800px. Jika anda ingin menghapus image yang telah anda masukkan, anda tinggal mengklik tombol clear dan image pun akan hilang.

Dengan kebutuhan seperti diatas, saya akan mengajari anda bagaimana membuat chrome extension.
<!-- more -->

### Chrome Extension
Chrome Extension digunakan untuk menambah fungsionalitas dari chrome browser. Extension dapat dibuat hanya dengan html + css + javascript. Jadi siapapun yang memahami ketiga bahasa tersebut dapat membuatnya. Semua extension dapat diinstall dari chrome [webstore](https://chrome.google.com/webstore/category/extensions). Setelah terinstall, akan muncul icon disebelah omnibox(tempat anda mengetikkan url).

### Manifest
Setiap extension membutuhkan 1 file manifest.json. File ini memuat konfigurasi-konfigurasi yang akan dibaca oleh chrome pada saat penginstalan extension.

``` json manifest.json
{
    "manifest_version": 2,
    "name": "Image Comparer",
    "description": "Compares 2 or more images",
    "homepage_url": "https://github.com/wlzch/compero.git",
    "version": "0.1",
    "icons": {
      "16": "icon16.png",
      "48": "icon48.png",
      "128": "icon128.png"
    },
    "offline_enabled": true,
    "permissions": [
      "storage",
      "tabs",
      "http://*/*",
      "https://*/*",
      "contextMenus",
      "chrome://favicon/"
    ],
    "background": {
        "scripts": ["background.js"]
    },
    "browser_action": {
      "default_icon": "icon.png",
      "defaul_title": "Compero",
      "default_popup": "popup.html"
    }
}
```

#### manifest_version
Mendefinisikan versi manifest yang digunakan. Versi yang dianjurkan adalah versi 2 karena versi manifest versi 1 sudah deprecated.

#### name
Nama dari extension.

#### description
Deskripsi singkat extension yang akan muncul di chrome webstore

#### homepage_url
Halaman utama dari extension.

#### version
Harus diingat bahwa versi ini beda dengan manifest_version. Versi disini mengacu kepada versi dari extension. Setiap kali extension diupload ke chrome webstore, extension harus dinaikkan.

#### icons
Icon-icon yang akan digunakan oleh chrome webstore. Dianjurkan untuk membuat icon dengan ukuran 16x16, 48x48 dan 128x128.

#### offline_enabled
Mendefinisikan apakah extension aktif jika chrome tidak terhubung dengan internet.

#### permissions
Mendefinisikan permision-permission apa saja yang diperlukan oleh extension. Anda harus mendefinisikan permission untuk fitur-fitur tertentu. Misalnya Compero membutuhkan chrome storage untuk menyimpan data, contextMenus untuk menambah menu pada saat klik kanan. Compero juga mendefinisikan permission "http://*/*"; dan "https://*/*"; yang artinya Compero dapat mengakses seluruh website yang dikunjungi user. Jika anda hanya perlu berinteraksi dengan salah satu website misalnya google.com anda dapat menggantinya dengan "https://*.google.com"; dan "http://*.google.com";

#### background
Mendefinisikan script yang akan dijalankan dibackground. Sciprt ini akan berjalan terus sepanjang browser tidak ditutup.

#### browser_action
Mendefinisikan icon yang akan muncul di toolbar google chrome termasuk icon, title (tulisan yang muncul jika dihover), badge (tulisan diatas icon) dan popup (halaman html yang muncul jika icon diklik)

Untuk konfigurasi lengkap dapat dilihat di website chrome

### Background.js
``` javascript
var images = [];

var getImages = function() {
    return images;
}

var clearImages = function() {
    images = [];
}

var addImage = function(info, tab) {
    images.push(info.srcUrl);
}

chrome.contextMenus.create({"title": "Add to Compero", "contexts": ["image"], "onclick": addImage});
```
Fungsi dari background.js sangat mudah, kita akan menambahkan action pada saat image diklik kanan dengan menggunakan contextMenus API

``` javascript
chrome.contextMenus.create({"title": "Add to Compero", "contexts": ["image"], "onclick": addImage});
```
Snippet diatas akan menambahkan action “Add to Compero” jika dilakukan klik kanan pada image (didefinisikan oleh key “contexts”). Jika action “Add to Compero” diklik, maka fungsi addImage akan dijalankan dengan mengirimkan 2 parameter kedalamnya. Parameter pertama bertipe onClickData yang berisi info mengenai elemen yang diklik. Parameter kedua bertipe Tab yang berisi informasi mengenai tab dimana action tersebut dilakukan.

Fungsi addImage hanyalah fungsi sederhana yang akan menambahkan url dari image yang di klik kedalam variabel images. Karena action “Add to Compero” hanya didefinisikan pada image, maka kita dapat langsung menggunaka srcUrl dari tipe onClickData untuk mengambil url dari image.

Selain itu, terdapat fungsi getImages dan clearImages. Fungsi-fungsi ini akan digunakan oleh halaman popup untuk mengambil dan menghapus data image.

### Popup.html
``` html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title>Compero - Image Comparer</title>
    <link href="jquery-ui.min.css" type="text/css" rel="stylesheet" />
    <link href="style.css" type="text/css" rel="stylesheet" />
    <script src="jquery.min.js"></script>
    <script src="jquery-ui.min.js"></script>
    <script src="popup.js"></script>
  </head>
  <body>
    <div id="controls">
        <div><button id="clear-images">Clear</button></div>
        <div id="slider"></div>
    </div>
    <div id="images">
    </div>
  </body>
</html>
```
Popup.html inilah yang akan ditampilkan jika icon compero di klik. Terdapat 1 tombol untuk menghapus images, slider untuk mengatur ukuran image dan 1 div sebagai container dari image yang akan kita bandingkan.

### Popup.js
``` javascript
var storage, bg, imageContainer;
var getImages = function(callback) {
    var images;
    storage.get('images', function(items) {
        images = items.images || [];

        callback(images);
    });
}

var insertImage = function(src) {
    var image = $('<img>');
    image.addClass('image');
    image.attr('src', src);
    image.appendTo(imageContainer);
}

var addImageToStorage = function(src) {
    getImages(function(images) {
        images.push(src);
        storage.set({images: images});
    });
}

var loadBackgroundImages = function() {
    var bgImages = bg.getImages();
    for (var i = 0, len = bgImages.length; i < len; i++) {
        insertImage(bgImages[i]);
        addImageToStorage(bgImages[i]);
    }
    bg.clearImages();
}

var loadStorage = function() {
    getImages(function(images) {
        for (var i = 0, len = images.length; i < len; i++) {
            insertImage(images[i]);
        }
    });
}

var loadImages = function() {
    loadBackgroundImages();
    loadStorage();
}

var onMessageListener = function(request, sender, sendResponse) {
    insertImage(request.src);
    addImageToStorage(request.src);
}

var clearClicked = function() {
    storage.clear();
    $('.image').remove();
}

document.addEventListener('DOMContentLoaded', function () {
    bg = chrome.extension.getBackgroundPage();
    storage = chrome.storage.local;
    imageContainer = $('#images');
    var slider = $('#slider');
    $('#clear-images').click(clearClicked);
    slider.slider({
        min: 200,
        max: 800,
        step: 10,
        value: 200,
        slide: function(event, ui) {
            var images = $('.image');
            images.width(slider.slider('value')+"px");
            images.height(slider.slider('value')+"px");
        }
    });
    loadImages();
});
```
Popup.js merupakan script yang paling penting untuk compero. Akan kita bahas fungsinya satu-persatu.

``` javascript
document.addEventListener('DOMContentLoaded', function () {
    bg = chrome.extension.getBackgroundPage();
    storage = chrome.storage.local;
    imageContainer = $('#images');
    var slider = $('#slider');
    $('#clear-images').click(clearClicked);
    slider.slider({
        min: 200,
        max: 800,
        step: 10,
        value: 200,
        slide: function(event, ui) {
            var images = $('.image');
            images.width(slider.slider('value')+"px");
            images.height(slider.slider('value')+"px");
        }
    });
    loadImages();
});
```
Pada snippet diatas, kita akan melisten pada event DomContentLoaded. Event ini akan terjadi ketika terjadi action klik pada icon compero dan DOM dari halaman tersebut telah terbentuk.

Pada line 1, variabel bg di set sebagai background page yang telah kita definisikan sebelumnya dengan method getBackgroundPage. Dengan adanya variabel bg, kita dapat mengakses setiap property dan method yang ada pada background page termasuk method getImages dan clearImages yang nantinya akan kita gunakan.

Pada line 2, variabel storage di set sebagai chrome storage. Kita akan menggunakannya untuk menyimpan dan mengambil url-url dari gambar yang telah diadd sebelumnya.

Setiap kali icon compero di klik, kita akan memanggil fungsi loadImages dimana fungsi – fungsi bantuan lainnya.

``` javascript
var insertImage = function(src) {
    var image = $('<img>');
    image.addClass('image');
    image.attr('src', src);
    image.appendTo(imageContainer);
}
```

insertImage berfungsi untuk membuat tag image baru dan memasukkannya ke DOM.

``` javascript
var getImages = function(callback) {
    var images;
    storage.get('images', function(items) {
        images = items.images || [];

        callback(images);
    });
}
```

getImages berfungsi untuk mengambil url image-image yang telah disimpan. getImages menerima 1 parameter yaitu callback yaitu berupa fungsi yang akan dijalankan setelah image-image terload. fungsi callback akan diberikan 1 parameter berupa image-image yang terload tadi. Hal ini dikarenakan storage bersifat asynchronous (non-blocking). Singkatnya, jika biasanya program itu jalan baris-perbaris secara berurutan, storage.get akan menjalankan fungsinya dan langsung melanjutkan program bahkan sebelum fungsinya mengembalikan hasil. Setelah fungsi tersebut siap dijalankan barulah parameter yang berupa fungsi lain akan dijalankan.

``` javascript
var addImageToStorage = function(src) {
    getImages(function(images) {
        images.push(src);
        storage.set({images: images});
    });
}
```

addImageToStorage berfungsi untuk menambahkan url image ke storage.

``` javascript
var loadBackgroundImages = function() {
    var bgImages = bg.getImages();
    for (var i = 0, len = bgImages.length; i < len; i++) {
        insertImage(bgImages[i]);
        addImageToStorage(bgImages[i]);
    }
    bg.clearImages();
}
```

loadBackgroundImages berfungsi mengambil image-image yang tadinya telah diadd di background page. Dapat dilihat dari method bg.getImages(). Setiap image yang tadinya telah diadd akan dimasukkan ke storage dan kemudian di masukkan ke DOM. Setelah semuanya dimasukkan, url-url yang ada dibackground page akan dihapus sehingga tidak terjadi duplikasi jika fungsi ini dipanggil kembali.

``` javascript
var loadStorage = function() {
    getImages(function(images) {
        for (var i = 0, len = images.length; i < len; i++) {
            insertImage(images[i]);
        }
    });
}
```

loadStorage berfungsi untuk mengambil image-image yang terdapat di storage.

``` javascript
var clearClicked = function() {
    storage.clear();
    $('.image').remove();
}
```

kemudian jika tombol clear di klik, kita akan membersihkan storage dan menghapus semua image dari DOM.

Begitulah code dari compero. Mudah kan? Jadi, cobalah dibuat. Hitung-hitung untuk membunuh waktu dan menambah pengetahuan.

Silahkan dikomentari jika ada ide atau masalah.
