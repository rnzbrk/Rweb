---
title: R Fonksiyonlarında Hata Mesajlarının Özelleştirilmesi
author: admin
date: '2020-05-23'
slug: r-fonksiyonlar-nda-hata-mesajlar-n-n-özelle-tirilmesi
categories:
  - Fonksiyonlar ve Döngüler
tags:
  - fonksiyon
  - hata mesajı
  - rstudio
subtitle: 'R kullanıcı fonksiyonlarında hata mesajları'
summary: 'R kullanıcı fonksiyonlarında hata mesajları'
authors: []
lastmod: '2020-05-23T19:22:33+03:00'
featured: no
image:
  caption: ''
  focal_point: ''
  preview_only: no
projects: []
math : true
---



Merhaba, 

Bu yazıda R yazılımda hata mesajlarının nasıl özelleştirileceğine değineceğim. R yazılımında hem temel (base) pakette hem de farklı paketlerdeki foksiyonlar kullanmak istemediğinizde kendi fonksiyonlarınızı yazmak isteyebilirsiniz. Bu tür fonksiyonlara **kullanıcı tanımlı (User defined)** fonksiyonlar denir. 

Kullanıcı tanımlı fonksiyonların nasıl oluşturulduğu [R ile Veri Analizi ve Psikometri Uygulamaları](https://www.erenozberk.com/talk/rkitap/) kitabımızda ayrıntılı olarak anlatıldı. İlgilenenler 6. Bölümdeki "Fonksiyonlar" kısmını inceleyebilirler. 

Şimdi anlatacağım konu ise kullanıcı tanımlı fonksiyonların hata mesajları düzeyinde özelleştirilmesi. R yazılımını öğretmeye başlayınca farkettiğim bir durum var: Kişiler hata kodlarını ne kadar iyi anlarsa kod yazma becerileri daha hızlı oluyordu. Çünkü hatanın nerede ve nasıl oluştuğunu anlayıp o kısma odaklanabiliyorlardı. Bu yüzden sistem tarafından üretilen hata mesajlarını okumanın önemi büyüktür. Bana göre, hata mesajlarını çözmek, bir bakıma kullandığınız paketi hatta yazılımı çözme yolunda en büyük adımdır. 


Hata kodlarını özelleştirmeye başladığınızda yazdığınız fonksiyonlar daha işlevsel olacaktır. Örneğin matematikteki çarpma işaretinin ($\Pi$) fonksiyonunu yazalım. Bu fonksiyon ile nesnemizdeki sayıların çarpımını bize verecektir. 




```r
carpim <- function(x) {
carpim <- 1
for(i in x) {
carpim <- carpim*i
}
carpim
}
```


Fonksiyonu tanımladık. Şimdi iki tane sayısal vektör tanımlayalım. Birisi **x** vektörü olsun ve hiç kayıp veri olmasın. İkincisi ise **y** vektörü olsun ve kayıp veri olsun. 



```r
x <- c(1,4,5,6,7,8,9,21)

y <- c(1,4,NA,6,7,8,9,21)

x
```

```
## [1]  1  4  5  6  7  8  9 21
```

```r
y
```

```
## [1]  1  4 NA  6  7  8  9 21
```

Her iki sayısal vektörü **carpim** fonksiyonu kullanarak çalıştıralım



```r
carpim(x)
```

```
## [1] 1270080
```

```r
carpim(y)
```

```
## [1] NA
```


Dikkat ederseniz **x** vektöründe istediğimiz çıktıyı aldık ama **y** vektöründe istediğimiz çıktıyı alamadık ve sadece **NA** değerini verdi. Ancak bu **NA** değeri sorunun nerede olduğunu bize anlatmıyor. 

Yazdığımız fonksiyona tekrar bakalım. Biz fonksiyonu kayıp verisi olmayan vektörler için yazmışız. Yani varsayımımız şu: Burada sadece tam veriler çalışır. Ama iyi bir fonksiyon yazmak istiyorsak olası tüm durumları da düşünmek zorundayız ve bunlara alternatifler oluşturmalıyız. 

İşte tam da bu noktada; verilerimizde kayıp bir veri olsa idi hata mesajı nasıl yazılmalı idi ona dikkat etmeliyiz. 

Hata mesajları ile ilgili birçok paket bulunmakta. Ben bildiklerimi aşağıda sıraladım.  

- Tony Fishettti [assertr](https://github.com/tonyfischetti/assertr)
- Hadley Wickham [assertthat](https://github.com/hadley/assertthat)
- Michel Lang [checkmate](https://github.com/mllg/checkmate)
- Stefan Bache [ensurer](https://github.com/smbache/ensurer)
- Gaston Sanchez [tester](https://github.com/gastonstat/tester)
- Richard Cotton [assertive](https://bitbucket.org/richierocks/assertive/)

Dilerseniz bu paketleri kullanabilirsiniz. Ama sorun hala devam ediyor. Nedeni de şu: Hata mesajları **İngilizce** ve ben mesajlarımı **Türkçe** istiyorum. Bu yüzden iş başa düşüyor. 
 
Fonksiyonumu güncelliyorum. Ve artık bir koşul koymam gerekiyor. Kendime şunu soruyorum "Ya verilerimde kayıp veri var ise?" Aslında bu "ya" kısmı R dilindeki **if** koşuluna denk geliyor. Bu nedenle bir koşul ekliyorum fonksiyonuma. Koşulda *anyNA* fonksiyonunu kullanıyorum. Bu fonksiyon kayıp verinin varlığını Boolean (TRUE-FALSE)mantığı ile test ediyor. *warning* yani *uyarı* fonksiyonu ile de istediğim mesajı yazabiliyorum. *return* fonksiyonu ile de hesaplanamadığına dair bir bilgilendirme yapıyorum. Eğer *return* kullanmasa idim çıktı "NA" olarak yer alacaktı. 





```r
carpim <- function(x, na.rm = FALSE) {
if(anyNA(x)){
warning("Verinizde kayıp veri (NA) bulunmaktadır. Lütfen kontrol ediniz")
    return("Hesaplanamadı")
}
carpim <- 1
for(i in x) {
carpim <- carpim*i
}
carpim
}
```



```r
carpim(y)
```

```
## Warning in carpim(y): Verinizde kayıp veri (NA) bulunmaktadır. Lütfen kontrol
## ediniz
```

```
## [1] "Hesaplanamadı"
```



Dikkat ederseniz artık sorunun nereden kaynaklı olduğunu görebiliyoruz. Ben hata kodlarının Türkçe olmasını önemsiyorum. Özellikle de kullanıcı tanımlı fonksiyonlarda olmasını önemsiyorum. Bu sayede kullandığınız kod diğer kişiler tarafından da kolaylıkla anlaşılabilecektir. 








