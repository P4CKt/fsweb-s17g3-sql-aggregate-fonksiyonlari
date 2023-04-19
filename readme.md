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
#ilk 3 soruyu join kullanmadan yazın.

    1) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

===

	SELECT
	ogrenci.ograd, ogrenci.ogrsoyad, islem.atarih
	FROM ogrenci, islem
	WHERE
	ogrenci.ogrno = islem.ogrno;

===

    2) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

===

	SELECT
	k.kitapadi,t.turadi FROM kitap AS k, tur AS t
	WHERE
	k.turno=t.turno AND t.turadi IN ("Hikaye","Fıkra")

===
	OR
===

	SELECT
	k.kitapadi,t.turadi FROM kitap AS k, tur AS t
	WHERE
	k.turno=t.turno AND t.tuRno IN (6,4)

===



    1) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları listeleyin.

===

	SELECT
	o.ogrno,o.ograd,o.ogrsoyad,k.kitapadi
	FROM ogrenci AS o , kitap AS k
	WHERE o.sinif in ('10A','10C')

===

    4) Öğrencinin adını, soyadını ve kitap aldığı tarihleri listeleyin.

===

	SELECT
	o.ograd,o.ogrsoyad,i.atarih
	FROM ogrenci AS o
	INNER JOIN islem i on i.ogrno=o.ogrno

===

    5) Fıkra ve hikaye türündeki kitapların adını ve türünü listeleyin.

===

	SELECT
	k.kitapadi,t.turadi
	FROM kitap AS k
	INNER JOIN tur t on t.turno =k.turno

===

    6) 10B veya 10C sınıfındaki öğrencilerin numarasını, adını, soyadını ve okuduğu kitapları, öğrenci adına göre listeleyin.

===

	SELECT
	o.ogrno ,o.ograd,o.ogrsoyad,k.kitapadi  
	FROM ogrenci as o
	INNER JOIN islem i on i.ogrno =o.ogrno
	INNER JOIN kitap k on k.kitapno=i.kitapno
	WHERE o.sinif in ('10B','10C')
	ORDER BY o.ograd

===

    7) Kitap alan öğrencinin adı, soyadı, kitap aldığı tarih listelensin. Kitap almayan öğrencilerinde listede görünsün.

===

    	SELECT
    	o.ograd, o.ogrsoyad,i.atarih
    	FROM ogrenci o
    	LEFT JOIN islem i on i.ogrno=o.ogrno

===

    8) Kitap almayan öğrencileri listeleyin.

===


	SELECT
	o.ograd, o.ogrsoyad,i.atarih
	FROM ogrenci o
	LEFT JOIN islem i on i.ogrno=o.ogrno
	WHERE ISNULL(i.atarih)

===

    9) Alınan kitapların kitap numarasını, adını ve kaç defa alındığını kitap numaralarına göre artan sırada listeleyiniz.

===

	SELECT
	k.kitapno,k.kitapadi,COUNT(i.kitapno) AS KitapKere
	FROM kitap k
	INNER JOIN islem i on k.kitapno=i.kitapno
	GROUP BY k.kitapno

===

    10) Alınan kitapların kitap numarasını, adını kaç defa alındığını (alınmayan kitapların yanında 0 olsun) listeleyin.

===

	SELECT
	k.kitapno,k.kitapadi,COUNT(i.kitapno) AS KitapKere
	FROM kitap k
	INNER JOIN islem i on k.kitapno=i.kitapno
	GROUP BY k.kitapno

===

    11) Öğrencilerin adı soyadı ve aldıkları kitabın adı listelensin.

===

    	SELECT
    	o.ograd,o.ogrsoyad,k.kitapadi
    	FROM ogrenci as o
    	INNER JOIN islem i on i.ogrno =o.ogrno
    	INNER JOIN kitap k on k.kitapno=i.kitapno
    	ORDER BY o.ograd

===

    12) Her öğrencinin adı, soyadı, kitabın adı, yazarın adı soyad ve kitabın türünü ve kitabın alındığı tarihi listeleyiniz. Kitap almayan öğrenciler de listede görünsün.

===

	SELECT
	o.ograd,o.ogrsoyad,k.kitapadi,y.yazarad,y.yazarsoyad ,t.turadi ,i.atarih  
	FROM ogrenci as o
	LEFT JOIN islem i on i.ogrno =o.ogrno
	LEFT JOIN kitap k on k.kitapno=i.kitapno
	LEFT JOIN tur t on t.turno =k.turno
	LEFT JOIN yazar y on y.yazarno=k.yazarno
	ORDER BY o.ograd

===

    13) 10A veya 10B sınıfındaki öğrencilerin adı soyadı ve okuduğu kitap sayısını getirin.

===

    	SELECT
    	o.ograd, o.ogrsoyad,COUNT(k.kitapno)
    	FROM ogrenci o
    	LEFT JOIN islem i on i.ogrno=o.ogrno
    	LEFT JOIN kitap k on k.kitapno=i.kitapno
    	WHERE o.sinif in('10A','10B')
    	GROUP BY o.ogrno

===

    14) Tüm kitapların ortalama sayfa sayısını bulunuz.
    #AVG

===

    SELECT  AVG(k.sayfasayisi) from kitap k

===

    15) Sayfa sayısı ortalama sayfanın üzerindeki kitapları listeleyin.

===

    SELECT * from kitap k
    WHERE k.sayfasayisi >
    (SELECT AVG(k.sayfasayisi) FROM kitap k)

===

    16) Öğrenci tablosundaki öğrenci sayısını gösterin

===

	SELECT COUNT(ogrno) from ogrenci

===

    17) Öğrenci tablosundaki toplam öğrenci sayısını toplam sayı takma(alias kullanımı) adı ile listeleyin.

    SELECT COUNT(ogrno) AS Toplam Sayı from ogrenci

===

    18) Öğrenci tablosunda kaç farklı isimde öğrenci olduğunu listeleyiniz.

===

    SELECT COUNT( DISTINCT ograd ) from ogrenci

=== 


19) En fazla sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

===

    select Max(sayfasayisi) from kitap

===

    20) En fazla sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

===

    select k.kitapadi ,k.sayfasayisi  from kitap k
    WHERE
    k.sayfasayisi  = (select Max(k.sayfasayisi) from kitap k)

===

    21) En az sayfa sayısı olan kitabın sayfa sayısını listeleyiniz.

===

    Select k.sayfasayisi  from kitap k
    WHERE k.sayfasayisi  = (SELECT Min(k.sayfasayisi) from kitap k)

===

    22) En az sayfası olan kitabın adını ve sayfa sayısını listeleyiniz.

===

    Select k.kitapadi ,k.sayfasayisi  from kitap k
    WHERE k.sayfasayisi  = (SELECT Min(k.sayfasayisi) from kitap k)

===

    23) Dram türündeki en fazla sayfası olan kitabın sayfa sayısını bulunuz.

===

	Select Max(k.sayfasayisi) from kitap k
	INNER JOIN tur t on t.turno =k.turno
	WHERE t.turadi='Dram'

===

    24) numarası 15 olan öğrencinin okuduğu toplam sayfa sayısını bulunuz.

===

    SELECT
    SUM(k.sayfasayisi)
    FROM ogrenci o
    LEFT JOIN islem i on i.ogrno=o.ogrno
    LEFT JOIN kitap k on k.kitapno=i.kitapno
    WHERE o.ogrno='15'

===

    25) İsme göre öğrenci sayılarının adedini bulunuz.(Örn: ali 5 tane, ahmet 8 tane )

===

    SELECT CONCAT(ograd ,COUNT(ograd),' tane')  from ogrenci o
    group by ograd

===

    26) Her sınıftaki öğrenci sayısını bulunuz.

===

	SELECT CONCAT(sinif ,COUNT(ograd),' ogrenci') from ogrenci o  
	group by sinif

===

    27) Her sınıftaki erkek ve kız öğrenci sayısını bulunuz.


===

    SELECT sinif ,COUNT(ogrno),cinsiyet  from ogrenci o
    group by sinif,cinsiyet

===

    28) Her öğrencinin adını, soyadını ve okuduğu toplam sayfa sayısını büyükten küçüğe doğru listeleyiniz.

===

    SELECT
    o.ograd, o.ogrsoyad,SUM(k.sayfasayisi)
    FROM ogrenci o
    LEFT JOIN islem i on i.ogrno=o.ogrno
    LEFT JOIN kitap k on k.kitapno=i.kitapno
    WHERE o.ogrno=i.ogrno
    GROUP BY o.ogrno
    ORDER BY SUM(k.sayfasayisi) DESC

===

    29) Her öğrencinin okuduğu kitap sayısını getiriniz.

===

    SELECT
    o.ograd, o.ogrsoyad,count(i.kitapno)
    FROM ogrenci o
    LEFT JOIN islem i on i.ogrno=o.ogrno
    WHERE o.ogrno=i.ogrno
    GROUP BY o.ogrno

===
