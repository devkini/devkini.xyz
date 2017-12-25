---
layout: post
title: 'Menguruskan Libraries/Dependencies'
date: 2017-05-01
level: 1
featured: true
summary: >
            Bagaimana menguruskan libraries/dependencies secara sistematik dalam projek
            programming.
tags:
    - php
    - npm
    - composer
    - pip
    - gem
---

Setiap projek *programming* sama ada besar atau kecil biasanya tidak hanya mengandungi kod-kod aturcara yang ditulis oleh *programmer* itu sahaja. Ia juga akan turut mengandungi kod aturcara yang ditulis oleh pihak ketiga.

Ini kerana kebanyakkan masalah umum dalam *programming* biasanya sudah ada penyelesaianya. Dalam erti kata lain, sudah wujud kod yang ditulis oleh *programmer* lain yang dapat menyelesaikan masalah tersebut. Apa yang perlu dilakukan adalah mencari kod-kod tersebut dan seterusnya memasukkan kod tersebut bersama-sama aturcara yang kita sedang tulis ini.

<!--more-->

Setiap bahasa pengaturcaraan mempunyai mekanisma tersendiri untuk memasukkan kod pihak ketiga ini ke dalam aturcara kita. Ia biasanya dapat dikenali melalui fungsi seperti `import`, `require`, `include`, `load`, `use` dan sebagainya.

Pada mulanya, proses mencari kod-kod pihak ketiga atau biasanya dipanggil *libraries* ini dilakukan secara manual. Maknanya, jika *programmer* A memerlukan kod berkaitan pemprosesan fail CSV, maka dia mungkin akan memulakan proses pencarian di laman enjin carian (*search engine*) atau bertanyakan di forum-forum berkaitan. Proses manual ini pada mulanya memadai namun masalah bermula apabila kod di dalam *libraries* ini turut bergantung kepada kod-kod lain.

Contohnya A mungkin terjumpa kod `easycsv.php`. Dia pun memasukkan kod `easycsv.php` ini ke dalam kod beliau tetapi mendapat *error* seperti:-

    Warning: include(basecsv.php): failed to open stream: No such file or directory in ..

Rupa-rupanya kod `easycsv.php` turut bergantung kepada kod `basecsv.php` yang ditulis oleh programmer lain. Ini bermakna A perlu kembali mencari di mana hendak mendapatkan kod `basecsv.php` ini. Jika hanya melibatkan satu atau dua *libraries* sahaja, ia mungkin tidak begitu menyusahkan tapi apabila melibatkan jumlah *external libraries* yang banyak, satu solusi yang lebih baik amat diperlukan.

Ketiadaan cara yang khusus untuk menyelesaikan masalah di atas akan mendorong *programmer* untuk mengambil tindakan seperti:-

* Tidak menggunakan *external libraries* - mendorong kepada *re-inventing the wheel*.
* Tidak mengemaskini *external libraries* yang digunakan kerana ia memerlukan sekali lagi proses mencari kemaskini
yang baru.
* *Copy-and-paste* kod antara projek.

Jika diperhatikan pada proses mencari *libraries* di atas, kita akan dapati ianya adalah satu bentuk aktiviti rutin yang berulang dan hampir sama. Yang berbeza hanyalah pada nama *libraries* dan di mana ia dapat diperolehi. Jika *libraries* ini dapat dikumpulkan di satu tempat dan disusun dalam susunan tertentu, satu program khas dapat ditulis untuk mencari dan memasukkan *libraries* tersebut ke dalam aplikasi yang kita sedang bina. Daripada sinilah lahirnya konsep *package management*. 

Masalah *A depend on B and B depend on C* ini merupakan satu masalah klasik dalam Sains Komputer dan pelbagai *algorithm* telah pun dibina untuk menyelesaikan masalah ini.

{:.image-caption}
![](http://i.imgur.com/8tz9TBH.png#center "Algoritma penyelesaian masalah di atas")*Rajah [Dependency Resolving Algorithm](http://www.electricmonk.nl/log/2008/08/07/dependency-resolving-algorithm/)*


Berasaskan *algorithm* ini, setiap bahasa pengaturcaraan mempunyai *tools* tersendiri untuk menguruskan *libraries
dependencies* ini. Berikut disenaraikan beberapa *tools* berkenaan:-

* Python - PIP
* PHP - Composer
* .NET - Nuget
* JavaScript - npm, bower
* Haskell - Cabal
* Lua - Luarocks
* Ruby - Rubygems, bundler
* Perl - CPAN


## Rujukan
* http://en.wikipedia.org/wiki/Dependency_graph
* https://en.opensuse.org/openSUSE:Libzypp_satsolver
* http://sahandsaba.com/understanding-sat-by-implementing-a-simple-sat-solver-in-python.html
