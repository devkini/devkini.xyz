---
layout: post
title: Langkah Pertama Menjadi Penyumbang di Github
author: kamal
level: 2
featured: true
tags:
    - git
    - github
    - open source
    - malay
summary: >
            Bagaimana untuk mula menjadi penyumbang di Github dengan menguji
            patch atau pull-request daripada developer lain.
            
---

Ramai developer sekarang sudah mula menggunakan Github untuk menyimpan code yang mereka hasilkan, ataupun mendapatkan code daripada developer lain. Namun fungsi sebenar Github adalah untuk memudahkan kolaborasi. Salah satu bentuk kolaborasi adalah dengan menguji *patch/changes* yang dihantar oleh *developer* lain.

<!--more-->

Mulakan dengan *fork* repo yang anda berminat untuk menjadi penyumbang. Untuk tujuan
artikel ini, saya memilih projek Laravel di URL berikut:-

    https://github.com/laravel/framework

Setelah proses *fork* selesai, saya akan mendapat salinan repo yang sama di URL:-

    https://github.com/k4ml/framework

Langkah kedua adalah dengan *checkout* repo yang baru kita fork tadi:-

    git clone https://github.com/k4ml/framework.git

Langkah ketiga - add repo asal Laravel sebagai *upstream*:-

    cd framework
    git remote add upstream https://github.com/laravel/framework.git

Sekarang kita masuk ke bahagian yang ditunggu-tunggu. 'Menarik' *pull-request* oleh developer lain dan mengujinya di komputer kita. Format arahan untuk tujuan ini adalah:-

    git fetch origin pull/<pull-request-id/head:<local-branch-name-to-pull>

Sebelum itu, pastikan dulu nama branch yang menjadi target *pull-request* tersebut. Jika anda lihat pada gambar di bawah, anda boleh baca ayat seperti berikut - "  themsaid  wants to merge 1 commit into laravel:5.2 from themsaid:validate-unique-for-arrays".

<img src="http://i.imgur.com/LzueXT5.png">

Ini bermaksud target *pull-request* ini adalah branch bernama `5.2`. Jadi pastikan kita berada dalam branch tersebut:-

    git checkout 5.2

Anda boleh dapatkan senarai *pull request* di url https://github.com/laravel/framework/pulls.
Saya pilih [*pull request* bernombor 12612](https://github.com/laravel/laravel/pull/12612).

    git fetch upstream pull/12612/head:test-pr

Anda akan mendapat output lebih kurang berikut:-

    remote: Counting objects: 4, done.
    remote: Compressing objects: 100% (4/4), done.
    remote: Total 4 (delta 0), reused 3 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From https://github.com/laravel/framework
     * [new branch]      refs/pull/12612/head -> test-pr

Untuk memastikan anda mendapat *changes* yang betul, jalankan command `git diff`:-

    git diff 5.2...test-pr

Bandingkan dengan *changes* yang anda lihat di https://github.com/laravel/framework/pull/12612/files:-

<img src="http://i.imgur.com/ePPjZV8.png"><br />

<div class="admonition-warning">
    Perhatikan penggunaan triple dot dalam command `git diff` di atas. Terdapat perbezaan antara penggunaan
    double dot dan triple dot.
</div>
<div>&nbsp;</div>

Sekarang anda boleh mula menguji *pull request* tersebut dan seterusnya berkongsi hasil yang anda dapat dalam ruangan [diskusi](https://github.com/laravel/framework/pull/12612) *pull request* berkenaan.
