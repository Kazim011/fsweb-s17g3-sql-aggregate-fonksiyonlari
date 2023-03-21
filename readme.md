# SQL Sorgu Alıştırmaları

Bu hafta SQL sorguları üzerine çalışıyorsunuz. Bugünkü alıştırmada sizin için hazırladığımız veritabanında aşağıda istediğimiz sonuçları elde etmenize yarayan SQL sorgularını oluşturacaksınız.

## Proje Kurulumu

Projeyi forklayın ve clonlayın. Tamamladığınızda da pushlayın.

### Kütüphane Bilgi Sistemi

Bu veritabanı, bir okulun kütüphanesinden öğrencilerin aldıkları kitapların bilgisini barındırmaktadır.

#### Tablolar

`ogrenci` tablosu öğrencilerin listesini tutar.
`islem` tablosu öğrencilerin kütüphaneden aldıkları kitapların bilgilerini tutar
`kitap` tablosu kütüphanedeki kitapların bilgisini tutar
`yazar` tablosu kitapların yazarları bilgisini tutar
`tur` tablosu kitapların türlerini tutar.

Tablo ilişiklerini görmek için [ktphn.png] dosyasına göz atın.

Yazdığınız sorguları buradan test edebilirsiniz: [https://ergineer.com/assets/materials/fkg36so5-kutuphanebilgisistemi-sql/]

##### Görevler

Aşağıda istenilen sonuçlara ulaşabilmek için gerekli SQL sorgularını yazın.

MIN-MAX, COUNT-AVG-SUM, GROUP BY, JOINS (INNER, OUTER, LEFT, RIGHT
#ilk 3 soruyu join kullanmadan yazın. 1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.
select o.ograd,o.ogrsoyad,i.atarih from ogrenci as o,islem as i
where i.ogrno = o.ogrno 2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.
select k.kitapadi,t.turadi from kitap as k ,tur as t
where t.turno = k.turno and t.turadi in ("Fıkra","Hikaye") 3) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.
select o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o
inner join islem as i on o.ogrno = i.ogrno
inner join kitap as k on k.kitapno = i.kitapno
where sinif = "10B" or sinif = "10C"
order by o.ogrno

    #join ile yazın
    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

    select o.ograd,o.ogrsoyad,i.atarih from ogrenci as o
    inner join islem as i on i.ogrno = o.ogrno

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

    select k.kitapadi,t.turadi from kitap as k
    inner join tur as t on t.turno = k.turno
    where turadi = "Fıkra" or turadi = "Hikaye"

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

    select o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi from ogrenci as o
    inner join islem as i on o.ogrno = i.ogrno
    inner join kitap as k on k.kitapno = i.kitapno
    where sinif = "10B" or sinif = "10C"
    order by o.ograd

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

    SELECT o.ograd, o.ogrsoyad, COALESCE(i.atarih, 'Kitap almamış') AS atarih
    FROM ogrenci AS o
    LEFT JOIN islem AS i ON o.ogrno = i.ogrno
    ORDER BY i.atarih;

    8) Kitap almayan öğrencileri listeleyin.

    SELECT o.ograd, o.ogrsoyad
    FROM ogrenci AS o
    WHERE NOT EXISTS (

SELECT 1
FROM islem AS i
WHERE o.ogrno = i.ogrno
); 9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.
SELECT i.kitapno, k.kitapadi, COUNT(\*) as alim_sayisi
FROM islem as i
INNER JOIN kitap as k ON i.kitapno = k.kitapno
GROUP BY i.kitapno, k.kitapadi
ORDER BY i.kitapno ASC; 10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

    SELECT k.kitapno, k.kitapadi, COUNT(i.kitapno) AS alim_sayisi
    FROM kitap as k
    LEFT JOIN islem as i ON k.kitapno = i.kitapno
    GROUP BY k.kitapno, k.kitapadi
    ORDER BY alim_sayisi DESC;

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

    select  concat(o.ograd,o.ogrsoyad) as Adı_Soyadı,k.kitapadi from ogrenci as o
    inner join islem as i on i.ogrno = o.ogrno
    inner join kitap as k on k.kitapno = i.kitapno

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

    select  concat(o.ograd,o.ogrsoyad) as Adı_Soyadı, k.kitapadi, concat(y.yazarad,y.yazarsoyad) as Yazar_Ad_Soyad, t.turadi ,i.atarih from ogrenci as o
    left join islem as i on i.ogrno = o.ogrno
    left join kitap as k on k.kitapno = i.kitapno
    left join yazar as y on y.yazarno = k.yazarno
    left join tur as t on t.turno = k.turno



    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

     select o.ograd,o.ogrsoyad , count(k.kitapadi) as kitap_sayisi from ogrenci as o
    inner join islem as i on i.ogrno = o.ogrno
    inner join kitap as k on k.kitapno = i.kitapno
    where o.sinif = "10A" or o.sinif = "10B"
    group by o.ograd,o.ogrsoyad

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

    select avg(sayfasayisi) as ortalama_sayfa_sayisi from kitap

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

    select * from kitap
    where sayfasayisi > (select avg(sayfasayisi) from kitap)

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

    select count(ogrno) from ogrenci

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    select count(ogrno) as "Toplam Öğrenci" from ogrenci;

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

    select count(distinct ograd) as isim_sayisi from ogrenci

    19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    select max(sayfasayisi) as en_fazla_olan from kitap

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

     select kitapadi,sayfasayisi from kitap
     where sayfasayisi = (select  max(sayfasayisi) from kitap)

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

    select min(sayfasayisi) as min_sayfaSayisi from kitap

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

    select kitapadi,sayfasayisi from kitap
    where sayfasayisi = (select min(sayfasayisi) from kitap)

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

    select sayfasayisi from kitap as k
    inner join tur as t on t.turno = k.turno
    where t.turadi = "Dram"
    order by k.sayfasayisi desc
    limit 1

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

    select o.ogrno, o.ograd,o.ogrsoyad , sum(sayfasayisi) from ogrenci as o
    inner join islem as i on i.ogrno = o.ogrno
    inner join kitap as k on k.kitapno = i.kitapno
    where o.ogrno = 15

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

    select ograd ,count(ograd) as isim_adedi from ogrenci
    group by ograd


    26) Her sınıftaki öğrenci sayısını bulunuz.

    select sinif ,count(sinif) as ogrenci_sayisi from ogrenci
    group by sinif

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.

    select sinif,cinsiyet ,count(cinsiyet) as ogrenci_sayısı from ogrenci
    group by sinif,cinsiyet

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

    select concat(o.ograd,o.ogrsoyad) as Adı_Soyadı , sum(sayfasayisi) as toplam_sayfa_sayisi  from ogrenci as o
    inner join islem as i on i.ogrno = o.ogrno
    inner join kitap as k on k.kitapno = i.kitapno
    group by Adı_Soyadı
    order by toplam_sayfa_sayisi  desc

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

    select Concat(o.ograd,o.ogrsoyad) as Ad_Soyad ,count(i.ogrno) as okudugukitap from ogrenci as o
    inner join islem as i on i.ogrno = o.ogrno
    group by i.ogrno
