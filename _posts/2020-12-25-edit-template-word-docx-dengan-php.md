---
layout: post
title: Mengedit Template Word (*.docx) menggunakan PHP
categories: programming
---

Dua hari yang lalu aku dihubungi oleh teman kuliah yang saat ini sudah menjabat kepala subbidang di pemkot Madiun, dulunya dia seorang developer PHP. Dia menanyakan bagaimana caranya mengedit sebuah template word menggunakan PHP. Kutanya balik, ini mau bikin semacam formulir atau mencetak tabel yang barisnya tak terhingga. Dia menjawab kalo ini template word ini mau dipake untuk membuat cetakan SK CPNS tepat sih petikan SK CPNS, secara dia sekarang berada di institusi yang mengurusi PNS/ASN di pemkot Madiun. Seingatku, dulu pernah bikin model template word seperti ini pas membuat aplikasi desktop untuk mengolah data dan menyajikan dalam bentuk dokumen word (*.docx) biar hasilnya tetep bisa diedit secara mandiri, jika ada kesalahan kecil misalnya kolom tanda tangan yang terpotong karena page break dan sebagainya.

Lalu aku coba browsing dan mendapatkan sebuah halaman di Github yaitu [PHPWord](https://github.com/PHPOffice/PHPWord). Mulai kubaca beberapa prasyarat, cara instalasi dan cara menggunakan pustaka tersebut.

##### Prasyarat / requirement

- [PHP 5.3.3+](http://www.php.net)
- [XML Parser extension](http://www.php.net/manual/en/xml.installation.php)
- [Zend/Escaper Component](http://framework.zend.com/manual/current/en/modules/zend.escaper.introduction.html)
- [Zend/Stdlib Componet](http://framework.zend.com/manual/current/en/modules/zend.stdlib.hydrator.html)
- [Zip extension](http://php.net/manual/en/book.zip.php) (opsional bila menulis untuk format OOXML atau ODF)
- [GD extension](http://php.net/manual/en/book.image.php) (opsional bila menambahkan gambar)
- [XMLWriter extension](http://php.net/manual/en/book.xmlwriter.php) (opsional bila menulis untuk format OOXML atau ODF)
- [XSL extension](http://php.net/manual/en/book.xsl.php) (opsional bila menggunakan XSL)
- [dompdflibrary](https://github.com/dompdf/dompdf) (opsional bila akan menulis PDF)

##### Instalasi

Langkah termudah dari proses instalasi adalah menggunakan [Composer](https://getcomposer.org). Hampir semua framework PHP populer sudah menggunakan Composer seperti Slim dan Laravel. Jalankan perintah berikut :
```sh
composer require phpoffice/phpword
```
Setelah berhasil menginstal **PHPWord** dan prasyaratnya selanjutnya adalah pembuatan template dokumen word (*.docx), contoh nama file ```Template.docx```. Dalam template dokumen word tersebut, ```variabel``` yang akan ubah harus diberikan nama seperti contoh ini ```${variabel}```. ![template](https://i.ibb.co/W29hqwN/2020-12-25-085829.png)

##### Eksekusi

Setelah membuat template dokumen word seperti di atas, saatnya membuat sebuah kode PHP. Nama filenya bebas, ambil contoh ```tes.php```. Kode PHP seperti di bawah ini :

```php
<?php

require 'vendor/autoload.php';

$templateProcessor = new \PhpOffice\PhpWord\TemplateProcessor('Template.docx');

$templateProcessor->setValues([
    'nama' => 'Eko Yudhi Prastowo',
    'nip' => '12345678 123456 1 123',
    'alamat' => 'Kudus'
]);

header("Content-Disposition: attachment; filename=Template.docx");

$templateProcessor->saveAs('ekoyudhi.docx');
```

Selanjutnya jalankan kode PHP tersebut dalam command line/shell.
```sh
php tes.php
```
Maka akan terbentuk sebuah file dokumen word dengan nama ```ekoyudhi.docx```.Bisa juga menjalankannya dengan memanggil file tersebut melalui browser (apabila sudah terinstall webbrowser).

Penjelasan kode di atas adalah pertama melakukan load semua library yang akan dibutuhkan dalam hal ini phpword, dll. Selanjutnya PHP akan melakukan impor template yang telah dibuat. Setelah berhasil impor template, PHP akan mencari beberap variabel yang cocok pada template seperti ```nama```, ```nip```, dan ```alamat``` serta akan mengisinya dengan value yang telah ditulis. Selanjutnya akan menyimpan dalam sebuah file dokumen word yang lain. Jika berhasil akan ada sebuah file dokumen word yang isinya seperti di bawah ini ![hasil](https://i.ibb.co/k66pcSn/2020-12-25-085909.png).

Demikianlah penjelasan singkatnya, semoga temanku sudah dapat implement phpword ini di dalam aplikasinya.
