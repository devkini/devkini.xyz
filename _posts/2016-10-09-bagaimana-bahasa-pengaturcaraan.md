---
layout: post
title: Bagaimana Bahasa Pengaturcaraan Dibina ?
author: kamal
tags:
    - programming
    - language
---

Setelah terlibat dalam bidang pembangunan perisian komputer mungkin ini antara persoalan yang sering bermain di fikiran kita. Bagaimana kod-kod yang kita tulis itu dapat diubah ke satu bentuk yang boleh dijalankan oleh komputer untuk menghasilkan pelbagai aplikasi-aplikasi hebat yang kita guna sama ada pada komputer, telefon bimbit ataupun apa saja peranti elektronik, termasuk kereta yang kita pandu setiap hari.

Bagi yang pernah mengikuti kursus Sains Komputer di universiti mungkin sudah biasa dengan istilah seperti parser, compiler, interpreter dan sebagainya. Ia adalah antara ‘tools’ yang digunakan untuk membina sebuah bahasa pengaturcaraan.

Hampir semua bahasa pengaturcaraan, terutamanya bahasa aras tinggi (high level language) dibina dalam step yang sama. Bahasa aras tinggi ini seperti C, Java, Python, C#, PHP, Ruby, JavaScript dan banyak lagi mempunyai bentuk yang seakan-akan bahasa manusia iaitu English. Namun ini tidak bermakna bahasa pengaturcaraan hanya boleh ditulis seperti English, cuma mungkin majoriti bahasa pengaturcaraan ini bermula dari dunia orang-orang Inggeris.

**Jadi bagaimana bahasa pengaturcaraan ini dibina?**

<!--more-->

Bagaimanakah komputer boleh memahami bahawa ayat yang kita taip sebagai “source code” itu boleh difahami sebagai arahan? Seperti bahasa manusia juga, bahasa pengaturcaraan mempunyai tatabahasanya. Jika dalam bahasa manusia, adanya konsep seperti noun, verb, subject, predicate, dan sebagainya. Dalam bahasa pengaturcaraan, ada juga konsep tatabahasanya seperti statement, expression, variable literals, operator, dan sebagainya.

Note: From the concept above, relate to parsers and tokenizer.

Oleh kerana bahasa pengaturcaraan ini ditulis menggunakan karakter-karakter tertentu, peringkat pertama pembinaan bahasa pengaturcaraan adalah memecahkan karakter-karakter ini kepada kumpulan-kumpulan tertentu. Proses ini dipanggil tokenizing dan tools yang digunakan dipanggil tokenizer. Sebagai contoh, katakan sebuah bahasa pengaturcaraan seperti di bawah:-

```
var name = ‘ali’;
```

Proses tokenizing mungkin akan memecahkan kod di atas sebagai:-

```
[var] [space] [name] [space] [=] [space] [‘] [ali] [space] [‘] [;]
```

Saya gunakan `[ ]` sebagai kotak untuk menunjukkan kod-kod tersebut dipecahkan. Setiap kotak-kotak di atas dipanggil token.

Setelah dipecahkan kepada token-token, step seterusnya adalah untuk menganalis setiap token bagi memastikan ia adalah karakter yang dibenarkan. Contohnya bahasa pengaturcara mungkin tidak membenarkan nombor digunakan sebagai nama variable jadi kod seperti dibawah:-

```
var 1 = ‘ali’;
```

perlu dikenalpasti sebagai tidak valid dan mesej tertentu perlu diberikan kepada pengaturcara untuk menunjukkan ia salah. Pada tahap ini pengaturcara akan mendapat error message seperti “Syntax error”, “parse error” dan sebagainya.
Keseluruhan proses tokenizing dan analisis sintaks ini dipanggil parsing. Tools yang digunakan dinamakan parser. Jadi untuk membina sebuah bahasa pengaturcaraan anda sendiri, perkara pertama yang anda perlu lakukan adalah membina parser kepada bahasa anda.

Mungkin masih sukar untuk anda membayangkan keseluruhan proses ini, dan ia sememangnya amat kompleks tapi untuk memudahkan pemahaman, bagi sebaris kod ringkas sebelum ini, sebuah parser yang naif boleh ditulis seperti berikut:-

```php
$src = “var name = ‘ali’;”;
$tokens = explode(' ', $src);
foreach ($tokens as $token) {
    if ($token == ‘var’) {
        // do something
    }
}
```

sekarang anda nampak bagaimana prosesnya bukan … ?

### Parser generator
Hari ini bagaimanapun kebanyakkan parser tidak lagi ditulis secara manual “from scratch”. Ada pelbagai jenis program untuk menjana parser bagi bahasa kita. Parser generator ini memerlukan kita define bahasa kita dalam bentuk skema tertentu (dipanggil grammar). Grammar yang biasa digunakan adalah BNF (Backus-Naur Form).

Daripada spesifikasi BNF, kita boleh gunakan parser generator untuk generate parser bagi bahasa yang kita cuba bina. Untuk bahasa Python umpamanya, anda boleh lihat grammar’nya di https://docs.python.org/2/reference/grammar.html. Untuk PHP, anda boleh lihat skema grammar ia - http://lxr.php.net/xref/PHP_TRUNK/Zend/zend_language_parser.y. Ia bukannya BNF tapi dalam format yang digunakan oleh YACC, parser generator yang digunakan oleh pembangun PHP.

Jika anda sekadar ingin cuba-cuba tukar beberapa keyword dalam bahasa sedia ada, contohnya tukar ‘if’ dalam PHP kepada ‘jika’, anda boleh bermula dengan grammar file ini. Di samping itu, ada banyak senarai grammar yang anda boleh adaptasi sebagai asas kepada bahasa anda sendiri - http://www.antlr3.org/grammar/list.html

### AST

Hasil daripada *parsing* adalah apa yang dipanggil Abstract Syntax Tree (AST). Token-token arahan sebelum ini kini wujud dalam bentuk data structure yang sedia untuk diproses. Daripada sini, kita boleh membuat keputusan sama ada hendak generate *compiler* atau pun *interpreter* kepada language tersebut. *compiler* akan menjana code contohnya seperti kod binari yang dapat dijalankan terus oleh komputer ataupun kepada *bytecode* yang akan dijalankan oleh vm. Manakala untuk *interpreter*, kita akan menjalankan terus arahan daripada code yang ditulis.

Selain menjana kod binari, kita juga boleh menjana kod bahasa lain yang lain sedia ada, contohnya menjada kod JavaScript, yang mana kod tersebut dapat dijalankan dalam persekitaran yang menyokong JavaScript. Teknik ini dipanggil *transpiling*. Kelebihan teknik ini adalah kita tidak perlu risau bagaimana hendak menjalankan kod bahasa pengaturcaraan kita dan cuma bergantung kepada program yang sedia ada.

### Compiler/Interpreter

### Rujukan

http://compilers.iecc.com/crenshaw/tutor1.txt
http://www.codeproject.com/Articles/50377/Create-Your-Own-Programming-Language
http://en.wikipedia.org/wiki/Comparison_of_parser_generators
http://nitschinger.at/Writing-a-simple-lexer-in-PHP
http://stackoverflow.com/questions/133886/simple-regex-based-lexer-in-python
http://kore-nordmann.de/blog/do_NOT_parse_using_regexp.html
http://www.playwithlua.com/?p=66
http://ruslanspivak.com/lsbasi-part1/

### Nota
When writing a custom parser you can - and should - use regular expressions in the tokenizer. They do a great job here. To rephrase it:
Use regular expressions to recognize words, not structures.
