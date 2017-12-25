---
layout: post
title: 'Bagaimana web development dalam Python ?'
date: 2015-05-19
author: kamalmustafa
level: 1
tags:
    - python
    - web
    - malay
summary: >
            Sepintas lalu bagaimana proses pembangunan aplikasi web dalam Python dijalankan.
categories: python
---

Bila bercakap pasal topik web development, mungkin ramai di kalangan developer yang sudah biasa dengan bahasa pengaturcaraan PHP yang memang banyak digunakan untuk membangunkan aplikasi web. Python juga boleh digunakan untuk membangunkan ... Saya akan cuba tunjukkan sepintas lalu bagaimana proses pembangunan aplikasi web dalam Python dilakukan.

<!--more-->

Terdapat agak banyak *framework* yang boleh digunakan untuk membangunkan aplikasi web dalam Python. Anda mungkin akan terjumpa dengan nama-nama seperti Django, Flask, Pyramid, Web2py, Bottle dan sebagainya. Untuk tulisan ini bagaimana pun saya hanya akan menggunakan Django sebagai contoh memandangkan ia antara yang paling popular.

Django pertama kali diterbitkan pada tahun 2005, hampir 10 tahun yang lalu. Ianya hampir seusia dengan satu lagi *framework* yang amat popular di kalangan komuniti bahasa pengaturcaraan Ruby iaitu Ruby On Rails (RoR). Django berasal daripada aplikasi yang digunakan oleh sebuah syarikat penerbitan online iaitu Lawrence Journal-World untuk membangunkan laman-laman web mereka. Aplikasi tersebut kemudian di'refactor' kepada sebuah *framework* yang diterbitkan di bawah lesen Sumber Terbuka (Open Source).

## Permulaan
Development dalam Django dimulakan dengan membina struktur asas aplikasi yang hendak dibangunkan. Ia boleh dicapai dengan menggunakan *command line tools* yang disediakan oleh Django. Ia kelihatan seperti berikut:-

```
django-admin startproject blog
```

Arahan di atas akan menghasilkan *folder* dalam struktur seperti berikut:-

<img src="http://i.imgur.com/saUI4NO.png"></img>
<img src="http://i.imgur.com/oLqzu4p.png"></img>

## Development server
Web development adalah satu bentuk pembangunan aplikasi dalam model *client-server*. Aplikasi yang dibangunkan biasanya adalah server dan diakses melalui *client* seperti *web browser*. Oleh itu perlu kepada satu aplikasi *server* yang akan menjalankan aplikasi yang dibangunkan. Dalam platform PHP, *server* yang biasa digunakan adalah Apache HTTP Server (Apache). Ia bagaimana pun memerlukan proses konfigurasi yang melecehkan dan mengambil masa *developer*. Hanya dalam versi kebelakangan ini PHP turut menyediakan *development server*.

Aplikasi Django boleh dijalankan (untuk proses development sahaja) melalui arahan berikut:-

```
python manage.py runserver
```

## Routing
*Routing* adalah satu proses *mapping* antara *url path* dengan *code* yang hendak dijalankan dalam sebuah aplikasi web. Konsep *routing* tidak begitu diketahui dalam platform PHP kecuali akhir-akhir ini apabila penggunaan *framework* seperti Laravel, Yii, CakePHP, Codeigniter dan lain-lain. *Routing* dalam PHP dilakukan secara *implicit* di mana URL itu sendiri mengandungi nama *script* yang hendak dijalankan. Contohnya anda akan biasa jumpa URL seperti berikut:-

* http://www.website.com/index.php
* http://www.website.com/article.php

Dalam Django, proses *routing* dilakukan secara *explicit* dan developer harus *define* bagaimana sesuatu URL itu hendak dipadankan dengan *code* yang hendak dijalankan. Ia dinyatakan dalam fail `urls.py` seperti berikut:-

```
from django.conf.urls import include, url
from django.contrib import admin

urlpatterns = [
    # Examples:
    url(r'^$', 'blog.views.home', name='home'),
    url(r'^blog/', include('blog.urls')),

    url(r'^admin/', include(admin.site.urls)),
]
```

Dalam contoh di atas, URL seperti http://localhost:8000/ akan dikendalikan oleh *module* bernama `blog.views.home`, manakala URL seperti http://localhost:8000/blog/ pula akan dikendalikan oleh *urls.py* yang lain - satu bentuk *nested routing*.

## Output / Response / Template
Dalam PHP, untuk memaparkan output anda hanya perlu menggunakan arahan `print` atau `echo` seperti berikut:-

```
echo 'Hello world';
```

Dalam Django, contoh *code* yang akan memaparkan output seperti di atas adalah:-

```
from django.http import HttpResponse

def home(request):
    return HttpResponse('Hello world')
```

Oleh kerana Python bukan bersifat *template language* seperti PHP, mengeluarkan output dalam bentuk HTML seperti di atas adalah satu proses melecehkan, satu bentuk *template language* khas disediakan oleh Django. Jadi output yang sama menggunakan *template language* adalah seperti berikut:-

```
from django.shortcuts import render

def home(request):
    context = {
        'data': 'Hello world',
    }
    return render(request, 'home.html', context)
```

Dan `home.html` mungkin kelihatan seperti berikut:-

```
<html>
<body>
{{ data }}
/body>
</html>
```

## Database / ORM
Dalam Python, interaksi kepada database adalah melalui *driver* spesifik bagi setiap database seperti SQLite, MySQL atau PostgreSQL. API bagi setiap *driver* tersebut adalah seragam. Ia kelihatan seperti berikut:-

```
cursor = db.cursor()
cursor.execute('SELECT * from articles')
for row in cursor.fetchall():
    print row['title']
```

Cara di atas bagaimana pun dianggap sangat *low-level*. Pada kebanyakkan masa, Python developer akan menggunakan ORM seperti
SQLAlchemy untuk melakukan interaksi antara database. Django turut menyediakan ORMnya sendiri. Langkah pertama dalam interaksi
database adalah dengan membina `models` berkaitan objek yang kita ingin simpan ke dalam database. Ia kelihatan seperti berikut:-

```
from django.db import models

class Article(models.Model):
    title = models.CharField(max_lenght=255)
    body = models.TextField()

class Comment(models.Model):
    body = models.TextField()
    article = models.ForeignKey(Article)
```
Seterusnya kita jalankan arahan berikut untuk membina *table* berkaitan (jika ia masih belum dibina):-

```
python manage.py migrate
```
Satu fungsi yang amat berguna dalam Django adalah *shell* yang boleh digunakan untuk bereksperimentasi dengan models-models di atas.

```
python manage.py shell
```
Dan anda akan mendapat *console* seperti berikut:-

```
>>> from blog.models import Article, Comment
>>> Article.objects.all() # show all articles
[]
>>> a = Article(title='Hello', body='Some body ...')
>>> a.save()
>>> a.comment_set.all() # show all comments for this article
[]
```

## Debugger
*Debugging* adalah proses yang perlu dilalui oleh semua programmer. Oleh itu mempunyai mekanisma *debugging* yang canggih akan sangat membantu dalam proses *development*. Python sendiri didatangkan dengan *built-in* *debugger* yang cukup *simple*. Dalam setiap program Python, anda boleh meletakkan *code berikut*:-

```
import pdb;pdb.set_trace()
```
Dan program tersebut akan dihentikan sementara (*pause*) bagi membolehkan anda untuk *inspect* *variables* pada baris tersebut. Manakala untuk *web development* konsep di atas telah dikembangkan lagi untuk membolehkan anda melakukan *debugging* melalui *browser*. Apabila berlaku *error* pada aplikasi anda, *web debugger* tersebut akan dipaparkan bagi membolehkan anda melakukan *inspection* terus daripada browser. Page *debugger* tersebut adalah seperti berikut:-

<img src="http://i.imgur.com/PclDVtU.png"></img>

## Penutup
Di atas adalah pandangan sepintas lalu bagaimana proses *development* dalam Python dijalankan. Jika anda berminat untuk mengetahui lebih lanjut boleh ajukan melalui ruangan komen di bawah atau melalui [Channel Telegram][tg] kami.

[fb]:https://www.facebook.com/groups/belajarprogramming/
[tg]:https://telegram.me/devkini
