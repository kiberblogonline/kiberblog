---
layout: post
title: "HTTP Protokoli va cURL Buyruqlar Satri Asoslari"
author: muallif
categories: [ kiberxavfsizlik, tarmoq ]
tags: [ http, curl, bash ]
image: assets/images/post1.jpg
---

### HTTP -> Hypertext Transfer Protocol
**Hypertext** — boshqa resurslarga havolalarini (link) o'z ichiga olgan matn va o'quvchilar osongina tushunib oladigan matnni anglatadi.

HTTP mijoz (client) va serverdan iborat. Mijoz serverdan resurs so'raydi, server so'rovlarni qayta ishlaydi va so'ralgan resursni qaytaradi. **HTTP port uchun standart port: 80.**

* **FQDN** -> Fully Qualified Domain Name (Domenning to'liq nomi).
* **URL** -> Uniform Resource Locator (Yagona resurs qidiruvchisi).

HTTP orqali resursga kirish URL orqali amalga oshiriladi.

#### URL Tuzilishi:
`http://admin:password@inlanefreight.com:801/dashboard.php?login=true#status`

* **Scheme:** `http`
* **User info:** `admin:password`
* **Host:** `inlanefreight.com`
* **Port:** `801`
* **Path:** `/dashboard.php`
* **Query string:** `?login=true`
* **Fragment:** `#status`

> **Eslatma:** Har doim ham bularning barchasi to'liq bo'lishi shart emas. Asosiysi — **scheme** va **host**.

---

### HTTP So'rovi Qanday Ishlaydi? (Ketma-ketlik)

1.  **DNS orqali IP manzil topish:** Brauzerga `kiberblog.online` deb yozilganda, sahifa ochilishidan oldin orqa fonda:
    * Birinchi `/etc/hosts` fayli ichidan qidiradi, topilmasa:
    * DNS serverga so'rov jo'natadi va server IP manzilni qaytaradi.
2.  **HTTP so'rovini yuborish:** IP manzildagi serverning 80-portiga (standart) so'rov yuboriladi. Bu so'rov *"Menga asosiy sahifani ber"* deganidir.
3.  **Server javobi:** Veb-server so'rovni qabul qiladi. Default holatda `index.html` faylini o'qiydi va uni **HTTP Response** (Javob) sifatida qaytaradi. Javob bilan birga **Status Code** ham keladi (masalan, hammasi joyida bo'lsa `200 OK`).
4.  **Saytning ko'rinishi (Rendering):** Brauzer serverdan kelgan `index.html` kodi va ma'lumotlarini qabul qilib oladi va ularni biz ko'radigan chiroyli veb-sahifa ko'rinishiga keltiradi.

---

### cURL (Client URL) nima?

**cURL** — bu buyruqlar satri (command line) orqali ma'lumot almashish uchun mo'ljallangan dastur. U asosan HTTP protokoli bilan ishlaydi, lekin boshqa ko'plab protokollarni ham qo'llab-quvvatlaydi. 

`curl` veb-sahifani chiroyli formatda emas, balki sahifaning xom kodini (**raw format**) chiqarib beradi. Skriptlar yozish va jarayonlarni avtomatlashtirish uchun juda qulay bo'lib, pentesterlar uchun so'rov-javob tarkibini tezda tahlil qilish imkonini beradi.

#### Asosiy Buyruqlar va Bayroqlar (Flags):

**1. Oddiy so'rov yuborish:**
curl [http://kiberblog.online](http://kiberblog.online)
(HTML kodini terminalga chiqaradi)

2. Faylni yuklab olish va saqlash:

-o : Faylni o'zingiz xohlagan nom bilan saqlash:

Bash
curl -o mine.html URL
-O : Faylni serverdagi original nomi bilan saqlash:

Bash
curl -O URL
3. "Silent" (Jim) rejim:

-s : cURL ishlayotgan vaqtda yuklanish jarayoni haqidagi ortiqcha ma'lumotlarni (statistika, tezlik) ko'rsatmaydi:

Bash
curl -s -O URL
4. Yordam olish:

-h , --help all , man curl

Qo'shimcha Muhim Bayroqlar:
-d : HTTP Post so'rov bilan birga ma'lumot (data) yuborish.

-i : Serverdan kelgan javob sarlavhalarini (headers) ko'rsatish.

-u : Serverga foydalanuvchi nomi va parolini yuborish (Basic Authentication).

-A : "User-Agent" nomini o'zgartirib yuborish (masalan, boshqa brauzer yoki bot qilib ko'rsatish).

-v : Verbose — jarayonni juda batafsil (nima bo'layotganini bosqichma-bosqich) ko'rsatadi.

HTTPS (Hypertext Transfer Protocol Secure)
HTTPS — bu HTTP'ning xavfsiz ko'rinishi bo'lib, u ma'lumotlarni shifrlash uchun SSL/TLS protokollaridan foydalanadi. U standart 443-port orqali ishlaydi.

HTTP'dan farqli o'laroq, bu yerda ma'lumotlar uchinchi shaxslar (buzuq niyatli kimsalar) tomonidan o'qib bo'lmaydigan holatda uzatiladi.

Brauzer va server o'rtasida aloqa o'rnatilishidan oldin "Handshake" (qo'l siqishish) jarayoni amalga oshiriladi. Agar SSL sertifikati xato bo'lgan saytga ulanmoqchi bo'lsak, xavfsizlikni chetlab o'tish uchun curl tarkibida -k bayrog'ini ishlatishimiz kifoya. Biroq, real hayotda sertifikatsiz saytlarga ishonmaslik kerak.

HTTP Request va Responses Tarkibi:
So'rov (Request) - Mijoz serverdan biror narsa so'raydi:

Method: GET (olmoq) / POST (yubormoq)

Path: Resursning manzili (masalan: /users/login.html)

Version: HTTP versiyasi (masalan: HTTP/1.1)

Headers: Host (qaysi domen), User-Agent (qaysi brauzer), cookie...

HTTP Javobi (Response) - Server javob qaytaradi:

Status kodlar bilan birga keladi.

Headers: Server haqida ma'lumot, sana, ma'lumot turi (Content-Type).

Body: Asosiy ma'lumot — HTML kod, rasm, json ma'lumot yoki PDF hujjat.