---
layout: post
title: "Introduction to Android Programming"
date: 2013-05-05 11:56
comments: true
categories: [android, programming, tutorial]
---

Pasar smartphone berkembang [sangat pesat](http://www.businesswire.com/news/home/20121017005479/en/Strategy-Analytics-Worldwide-Smartphone-Population-Tops-1) beberapa tahun belakangan. Pada tahun 2011 terdapat 700an juta smartphone dan pada tahun 2012 telah berkembang menjadi 1 miliar smartphone dan angka ini diprediksi akan terus naik. Dari angka tersebut, android menguasai [52%](http://www.comscore.com/Insights/Press_Releases/2013/5/comScore_Reports_March_2013_U.S._Smartphone_Subscriber_Market_Share) market share untuk smartphone mengalahkan iOS.

Terus apa hubungannya dengan judul "Introduction to Android Programming"? Tentu ada. Dengan semakin banyaknya pengguna smartphone (khususnya android dalam kasus ini), semakin terbuka kesempatan kita sebagai developer untuk menargetkan pasar aplikasi smartphone. Oleh karena itulah saya merasa perlu mempelajari pembuatan aplikasi smartphone.

Saya sendiri mulai belajar programming android sekitar 1 bulan yang lalu karena saya beserta rekan saya [Suwandi Halim](https://twitter.com/wandi_lin) akan membuat startup yang menggunakan android sebagai platformnya. Kesan pertama saya ketika belajar android programming adalah susah. Hal ini mungkin dikarenakan saya lebih terbiasa dengan teknologi web dan menggunakan bahasa dinamis seperti php atau javascript sedangkan android menggunakan Java sebagai bahasa utamanya.

Dalam 1 bulan belajar saya mendapatkan banyak masalah. Untung saja resource untuk develop aplikasi android sangat banyak sekali tersedia di internet. Hampir setiap masalah yang saya pernah saya temui sudah pernah dialami oleh orang lain dan sudah ada solusinya.

Untuk yang terbiasa dengan develop aplikasi web, terdapat beberapa perbedaan sewaktu membuat aplikasi android yang harus diperhatikan:

*   Efisiensi Baterai

    Sewaktu membuat aplikasi web, kita jarang memperhatikan efisiensi dari kode yang kita buat. Namun hal tersebut harus diubah sewaktu develop aplikasi mobile mengingat terbatasnya baterai yang ada pada smartphone.

*   Long Running Process

    Long running process adalah proses yang menghabiskan waktu yang lama untuk selesai. Misalnya membuka halaman yang berisi produk-produk. Hal ini biasanya tidak terlalu diperhatikan dalam aplikasi web karena browser yang menggunakan tab sehingga sambil menunggu halaman di load, user bisa mengerjakan hal lain terlebih dahulu. Namun tidak begitu dengan aplikasi android. Android merender tampilan aplikasinya di 1 thread yang biasa disebut ui thread. Jika kita melakukan long running process pada ui thread tersebut, user tidak dapat melakukan hal lain hingga process tersebut selesai. Hal ini dapat diatasi dengan menggunakan Asynchronouse Task yang akan kita pelajari juga nantinya.

*   Resource yang terbatas

    Device android memiliki spesifikasi yang berbeda-beda seperti spesifikasi processor dan ram yang berbeda-beda. Untuk membuat aplikasi yang responsif pada setiap device, kita harus meminimalkan penggunaan processor dan ram.

*   Beragam dimensi dan resolusi

    Banyaknya dimensi dan resolusi yang ada pada handset android kemudian juga tampilan portrait dan landscape juga membuat program android menjadi lebih susah.
    
*   Backward Compatible

    Versi android yang beragam juga merupakan salah satu masalah yang harus dihadapi. API yang ada pada OS 4.2 berbeda dengan API yang ada pada 1.6. Anda dapat memutuskan untuk mengsupport sampai OS versi berapa dengan memperhatikan [penggunaan versi OS yang beredar di pasaran](http://developer.android.com/about/dashboards/index.html).

Kita akan membuat aplikasi sederhana "Guess the Number" dimana user akan memasukkan angka kemudian sistem akan memutuskan apakah angka yang harus ditebak lebih kecil/sama/lebih besar dari angka yang dimasukkan user.

Sebelumnya, anda harus mendownload [sdk android](http://developer.android.com/sdk/index.html) yang disediakan oleh google terlebih dahulu. Meskipun anda dapat menggunakan editor apapun yang anda inginkan, namun saya anjurkan untuk menggunakan ADT Bundle yang disediakan pada halaman download tersebut dimana didalamnya terdapat Eclipse dan Android Development Tool(ADT).

Setelah anda menginstall ADT bukalah Eclipse editor. Pilih File > New > Android Application Project. Kemudian akan muncul halaman berikut

{% img center http://imageshack.us/a/img853/9506/screenshotat20130505204.png %}

*   Application Name

    Nama aplikasi yang akan muncul di Play Store

*   Project Name

    Nama project yang akan dibuat di Eclipse

*   Package Name

    Package (namespace) yang digunakan. Biasanya penamaan package mengikuti konvensi topleveldomain.namadomain.namaaplikasi misalnya com.praxmatig.androidtutorial. Penamaan package harus diperhatikan karena begitu anda mempublish aplikasi anda ke Play Store, anda tidak dapat mengubahnya lagi.

*   Minimum Required SDK
    
    Versi android minimal yang bisa diinstall oleh handset. Harus diperhatikan bahwa jika anda memilih versi yang lebih kecil anda harus memperhatikan apakah API yang anda gunakan tersedia pada versi tersebut.

*   Target SDK
    
    Versi android terakhir dimana aplikasi ini dapat bekerja. 

Kemudian tinggal next next sampai selesai. Eclipse kemudian akan membuatkan struktur folder android secara otomatis:

*   src

    Berisi sumber kode. Di folder inilah kita akan melakukan pengkodean

*   gen

    Berisi file-file yang digenerate secara otomatis oleh system android. Folder ini tidak boleh diganti secara manual. Jika anda menggunakan version control maka folder ini harus diignore.

*   res

    Berisi resource-resource seperti layout, image, string dan menu. Resource-resource yang ada disini akan bisa diakses melalui kode dengan variable R yang digenerate secara otomatis didalam folder gen tadi. Untuk layout dapat diakses melalui `R.layout.[nama_file_layout]`. Untuk image dapat diakses melalui `R.drawable.[nama_drawable]` dan sebagainya.

*   assets

    Folder ini berisi folder atau file yang hampir sama dengan res, tetapi file yang ada pada folder assets tidak akan digenerate otomatis kedalam R.

*   libs

    Library-library yang kita gunakan akan dimasukkan ke folder ini.

*   bin

    Berisi file-file .class yang digenerate otomatis oleh sistem dan file .apk aplikasi kita yang akan dijalankan oleh android emulator. Folder ini juga harus diignore pada version control.

## Komponen Dasar Android

### [Activity](http://developer.android.com/guide/components/activities.html)

Aplikasi pada umumnya dijalankan melalui fungsi main. Namun tidak begitu halnya android. Aplikasi android tersusun atas beberapa activity. Activity merupakan komponen aplikasi yang menyediakan tampilan dimana user dapat berinteraksi dengannya. Misalkan ada 1 activity yang menampilkan list dari produk yang jika di tap akan membuka 1 activity baru untuk menampilkan detail dari item tersebut.

### [Service](http://developer.android.com/guide/components/services.html)

Service adalah komponen aplikasi yang akan berjalan di background. Biasanya digunakan untuk menjalankan long running process.

### [Content Provider](http://developer.android.com/guide/topics/providers/content-providers.html)

Content Provider berfungsi untuk mengatur data misalnya mengambil data dari web dan memasukkannya ke database lokal. Android menggunakan SQLite sebagai database lokal. Dengan menggunakan content provider anda bisa mengambil atau menyediakan data aplikasi anda untuk aplikasi lain misalnya aplikasi anda bisa menggunakan data kontak yang disediakan sistem android.

### [Broadcast Receiver](http://developer.android.com/reference/android/content/BroadcastReceiver.html)

Broadcast Receiver adalah komponen yang merespon terhadap pemberitahuan broadcast misalnya anda dapat mengetahui apabila layar mati, low baterai atau adanya pengambilan foto baru.

## [Manifest](http://developer.android.com/guide/topics/manifest/manifest-intro.html)

Jika anda melihat project anda di eclipse, anda akan melihat file AndroidManifest.xml. Setiap aplikasi akan memiliki 1 manifest. Manifest digunakan untuk menunjukkan informasi-informasi dari aplikasi kepada sistem Android.

``` xml
<?xml version="1.0" encoding="utf-8"?>
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.praxmatig.androidtutorial"
    android:versionCode="1"
    android:versionName="1.0" >

    <uses-sdk
        android:minSdkVersion="8"
        android:targetSdkVersion="17" />

    <application
        android:allowBackup="true"
        android:icon="@drawable/ic_launcher"
        android:label="@string/app_name"
        android:theme="@style/AppTheme" >
<!--
    android:name merupakan class yang digunakan untuk activity ini
    android:label merupakan label yang akan ditampilkan untuk Activity tersebut
    intent-filter memberikan informasi kepada sistem bagaimana activity tersebut akan dijalankan
    action MAIN menunjukkan bahwa activity ini akan dijadikan activity awal yang dijalankan tanpa input dan tidak menghasilkan output
    category LAUNCHER menunjukkan bawah activity ini yang akan dijalankan ketika launcher di tap
-->
        <activity
            android:name="com.praxmatig.androidtutorial.MainActivity"
            android:label="@string/app_name" >
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />

                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
        </activity>
    </application>

</manifest>
```

## MainActivity

``` java
package com.praxmatig.androidtutorial;

import android.os.Bundle;
import android.app.Activity;

public class MainActivity extends Activity {

	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
	}

}

```

Setiap activity memiliki siklus hidup yang akan dilaluinya. 2 siklus yang paling penting untuk diimplementasikan adalah onCreate dan onPause.

### onCreate

onCreate akan dipanngil oleh sistem ketika membuat activity. onCreate adalah tempat yang cocok untuk melakukan inisialisasi. Di onCreate lah anda biasanya memanggil setContentView untuk mengdefine layout mana yang digunakan oleh activity.

### onPause

onPause dipanggil oleh sistem ketika user meninggalkan activity. Misalnya ketika anda menekan tombol menu untuk keluar dari activity. onPause adalah tempat yang cocok untuk menyimpan data-data yang diperlukan ketika user kembali ke activity tersebut.

setContentView(R.layout.activity_main) menunjukkan bahwa MainActivity menggunakan layout activity_main. File activity_main dapat ditemukan pada folder res/layout/activity_main.xml. Seperti yang telah ditulis sebelumnya, eclipse akan secara otomatis mengenerate R berdasarkan file-file yang ada di folder res.

Bukalah activity_main.xml dan masukkan xml berikut:

``` xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".MainActivity" >

    <EditText
        android:id="@+id/number"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignParentLeft="true"
        android:layout_alignParentTop="true"
        android:layout_marginLeft="16dp"
        android:layout_marginTop="21dp"
        android:inputType="numberSigned"
        android:hint="@string/number"
        android:ems="10" >

        <requestFocus />
    </EditText>

<!--
    android:onClick menunjukkan fungsi apa yang akan dijalankan ketika button di click
-->
    <Button
        android:id="@+id/guess"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/number"
        android:layout_alignRight="@+id/number"
        android:layout_below="@+id/number"
        android:layout_marginTop="20dp"
        android:text="@string/guess"
        android:onClick="guess" />
</RelativeLayout>
```

Jika anda menggunakan ADT terbaru anda dapat menggunakan graphical editor untuk membuat layout. xml diatas merupakan xml yang digenerate dengan menggunakan graphical editor. Kemudian buka res/values/string.xml dan pastekan xml berikut

``` xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">AndroidTutorial</string>
    <string name="action_settings">Settings</string>
    <string name="hello_world">Hello world!</string>
    <string name="number">Number</string>
    <string name="guess">Guess</string>
</resources>
```

Anda dapat mencoba tampilannya dengan menekan tombol ctrl+f11 yang akan menghasilkan tampilan seperti berikut (Anda akan diminta untuk membuat Android Emulator sebelum dapat menjalankan aplikasi).

{% img center http://imageshack.us/a/img694/9118/androidtutorial.png %}

Masukkan kode berikut kedalam MainActivity.java

``` java
package com.praxmatig.androidtutorial;

import java.util.Random;

import android.os.Bundle;
import android.util.Log;
import android.view.View;
import android.widget.EditText;
import android.widget.Toast;
import android.app.Activity;

public class MainActivity extends Activity {

	// Biasanya digunakan untuk debugging
	private final static String TAG = MainActivity.class.getSimpleName();
	
	// Digunakan untuk menghasilkan nilai random setiap kali aplikasi dijalankan
	private Random random;
	
	// Angka yang harusnya dimasukkan oleh user
	private int expectedNumber;
	
	/**
	 * Fungsi yang dijalankan pertama kali ketika activity dijalankan.
	 * Di fungsi inilah seharusnya dilakukan inisialisasi varibel.
	 */
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		
		random = new Random();
		expectedNumber = random.nextInt(100);
		Log.d(TAG, Integer.toString(expectedNumber)); //Memunculkan nilai expectedNumber pada tab LogCat
	}
	
	/**
	 * Fungsi guess akan dijalankan ketika user mengclick button.
	 * 
	 * @param view
	 */
	public void guess(View view) {
		EditText numberView = (EditText)findViewById(R.id.number);
		int guessedNumber = Integer.parseInt(numberView.getText().toString());
		Log.d(TAG, Integer.toString(guessedNumber));
		// Toast adalah class yang digunakan untuk memunculkan semacam informasi singkat kepada user
		if (guessedNumber > expectedNumber) {
			Toast.makeText(getApplicationContext(), "Too big", Toast.LENGTH_SHORT).show();
		} else if (guessedNumber < expectedNumber) {
			Toast.makeText(getApplicationContext(), "Too small", Toast.LENGTH_SHORT).show();			
		} else {
			Toast.makeText(getApplicationContext(), "Right", Toast.LENGTH_SHORT).show();
		}
	}

}
```

Jalankan aplikasi dan voila! Aplikasi android pertamamu telah selesai. Tidak begitu susah bukan? Jika ada pertanyaan silahkan komentar atau email saya di wilzichi92 [at] gmail.com atau tweet ke saya pada [@wilhn](https://twitter.com/wilhn)
