---
layout: post
title: 'Apa yang bitcoin miner buat sebenarnya ?'
date: 2017-12-24
tags:
    - bitcoin
summary: >
            Apa sebenarnya permasalah matematik yang dikatakan perlu diselesaikan oleh
            miner bitcoin ? Atau ianya lebih kepada percubaan bergantung kepada nasib ?
---

Tujuan utama bitcoin mining adalah untuk mengesahkan transaksi-transaksi yang hendak dimasukkan ke dalam blockchain. Oleh kerana rangkaian bitcoin adalah terpencar (decentralized), tiada entiti tunggal yang diberikan tanggungjawab untuk mengesahkan transaksi. Setiap node di dalam rangkaian boleh memasukkan transaksi ke dalam blockchain jika memenuhi syarat-syarat tertentu.

Syarat-syarat inilah yang perlu dilakukan oleh bitcoin miner. Ideanya adalah, jika sesiapa saja boleh memasukkan transaksi ke dalam blockchain, maka kita perlu pastikan operasi yang dilakukan oleh node tersebut mempunyai darjah kesusahan yang amat tinggi. Ini bertujuan tidak menggalakkan entiti tertentu daripada memasukkan transaksi palsu tapi sebaliknya menggalakkan mereka untuk berkolaborasi dalam terus 'menghidupkan' rangkaian. Jika mereka mematuhi syarat-syarat ini, disamping dapat memasukkan transaksi ke dalam blockchain, mereka juga dihadiahkan sejumlah bitcoin baru yang secara tidak langsung berfungsi sebagai sumber penghasilan duit baru dalam rangkaian bitcoin.

Konsep lakukan sesuatu untuk diterima ini dinamakan 'proof of work' (POW). POW memerlukan penggunaan kuasa perkomputeran yang tinggi yang secara langsung memerlukan sumber kuasa elektrik dan seperti yang kita sedia maklum, pastinya memerlukan modal yang banyak. Tapi apa sebenarnya POW yang perlu dilakukan oleh bitcoin miner ? Selalu kita dengar mereka perlu menyelesaikan permasalah matematik yang sukar. Tapi apakah permasalah matematik tersebut ?

Sebenarnya tiada permasalahan matematik yang jitu perlu diselesaikan. Formulanya telah siap ada [sejak berkurun lamanya][hashcash]. Apa yang perlu dilakukan hanyalah menukar beberapa pembolehubah dengan harapan mendapat jawapan yang memenuhi syarat-syarat yang ditetapkan oleh rangkaian. Setiap transaksi yang hendak dimasukkan ke dalam blockchain, perlu mempunyai pengenalan atau ID. ID ini boleh diperolehi dengan menjalankan operasi 'hashing' ke atas transaksi tersebut. Ini adalah operasi biasa dan setiap pengaturcara pasti pernah menggunakannya untuk tujuan tertentu. Contoh hashing jika menggunakan Python adalah:-

```
import hashlib
print(hashlib.sha256('hello'.encode('utf8')).hexdigest())
```
Output yang dihasilkan adalah `2cf24dba5fb0a30e26e83b2ac5b9e29e1b161e5c1fa7425e73043362938b9824`. Contoh di atas menggunakan fungsi hashing sha256 di mana output yang dihasilkan adalah nombor rawak 256 bit. Dalam contoh di atas juga, kita paparkan hash tersebut dalam bentuk hexadecimal di mana setiap karakter mewakili 4 bit, jadi panjang keseluruhan karakter adalah 64. Ciri utama fungsi hash adalah untuk setiap input yang sama ia akan menghasilkan hash yang sama. Tapi pada masa yang sama, tidak mungkin sama sekali untuk mengetahui apakah input hanya berdasarkan kepada output.

Apa-apa hash pun boleh menjadi ID kepada blockchain, sebab ia hanyalah nombor rawak yang dihasilkan oleh oleh fungsi hashing seperti ditunjukkan kat atas. Tapi itu akan menjadi sangat mudah kepada miner. Maka protokol bitcoin menetapkan nombor yang terhasil daripada fungsi hash, mestilah kurang daripada satu nombor _target_. Inilah yang menjadikan proses mencari hash ini sukar.

Analogi mudah ialah apabila anda membaling dadu. Jika syarat yang ditetapkan adalah anda mesti mendapat nombor kurang daripada 6, maka ia amat senang sekali. Dengan sekali baling, anda mungkin boleh mendapat nombor tersebut. Darjah kesukaran di sini adalah 1, atau 6/6. Tapi bagaimana kalau syaratnya nombor yang perlu anda dapat adalah 3 ? Ia akan menjadi 2 kali lebih sukar. Darjah kesukaran (difficulty) menjadi 6/3. Konsep yang sama diaplikasikan dalam bitcoin mining. Cuma nombor yang digunakan adalah sangat besar. Berapa besar ? Ok tengok pada nombor yang kecil dahulu. Untuk nombor bersaiz 8 bit, nombor paling besar kita boleh dapat adalah 256 atau 2 ^ 8. Untuk 16 bit, 65536 dan untuk 32 bit 4294967296. Seterusnya anda boleh kira sendiri.

Jadi 256 bit adalah satu nombor yang amat besar. Bagaimana jika nombor _target_ yang perlu diperolehi adalah amat kecil ? Ia akan memerlukan percubaan berjuta kali bagi kemungkinan mendapat nombor tersebut. Rangkaian bitcoin bagaimanapun akan 'adjust' nombor target ini berdasarkan tahap kemampuan hashing keseluruhan rangkaian. Jika ramai yang join mining dengan komputer yang sangat berkuasa, nombor target ini akan direndahkan supaya ia menjadi lebih sukar. Jika kemampuan hashing menurun maka nombor target akan dinaikkan supaya ia menjadi lebih senang. Objektif utama ialah supaya penghasilan block baru akan sentiasa berlaku dalam masa 10 minit.

Untuk melihat betapa sukar bagi mendapatkan nombor yang kurang daripada nombor target, kita boleh cuba dengan program mudah dalam Python seperti dibawah:-

```
import hashlib

target = '0000000000000000008e3dff56e95d4b9f954d64674e935e06505e86e02134e8'
target = '0111111111121875ca8e3dff56e95d4b9f954d64674e935e06505e86e02134e8'
target = '5fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff'
target = '9fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffff'
nonce = 1
while True:
    trans = 'trans 1' + 'trans 2' + 'trans 3' + str(nonce)
    hash = hashlib.sha256(trans.encode('utf8')).hexdigest()
    if int(hash, 16) < int(target, 16):
        print(hash)
        print(nonce)
        print(bin(nonce))
        break
    nonce += 1
```
Dalam contoh di atas, target yang pertama diambil daripada hash block [#500735](https://blockchain.info/block/00000000000000000051f3a7fabf5c19485fc30491f856d053a43cfafba7667d). Ia hampir mustahil untuk mendapatkan hash yang kurang daripada nombor tersebut menggunakan program python di atas dan menjalankannya hanya di atas laptop. Walaupun target dinaikkan sedikit, dengan menukar hanya nombor pertama kepada 0, ia masih mengambil masa. Malah dengan nombor yang amat besar seperti target ketiga ia masih sukar.

Walaupun sukar, apa yang kita dapat lihat di sini miner bukanlah menyelesaikan satu permasalahan matematik. Mereka hanya mencari satu nombor dengan kaedah cuba-jaya yang berulang kali, dengan setiap kali percubaan, mereka akan menukar beberapa pembolehubah yang dibenarkan seperti nonce atau menyusun kembali transaksi-transaksi yang hendak disahkan supaya boleh mendapat nombor hash berlainan. Miner yang pertama dapat meneka nombor yang menepati syarat akan diisytiharkan pemenang dan block yang merekah sahkan akan dimasukkan ke dalam blockchain.

[hashcash]:https://en.wikipedia.org/wiki/Hashcash
