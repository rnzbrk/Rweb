---
authors:
- admin
categories:
- Psikometri
date: "2020-01-20T00:00:00Z"
draft: false
featured: false
image:
  caption: 
  focal_point: ""
  placement: 2
  preview_only: false
lastmod: "2019-04-17T00:00:00Z"
projects: []
subtitle: 'Mann Whitney U, Wilcoxon, Kruskal Wallis ve Ki-Kare Analizleri'
summary: Mann Whitney U, Wilcoxon, Kruskal Wallis ve Ki-Kare Analizleri
tags:
- nonparametrik
- r studio
- veri analizi
- rmarkdown
- psikometri
title: 'R ile Nonparametrik İstatistikler'
math: true
---


**Bu belgede kullanılan örnekler Şener Büyüköztürk'ün Sosyal Bilimler için Veri Analizi El Kitabı isimli kaynağından yazarın izni ile alınmıştır.**


# 1. Nonparametrik İstatistikler


Hipotez testleri oluşturulurken, herhangi bir evrenden çekilen örneklem dağılımı yardımıyla evren dağılımı hakkında birtakım bilgilere sahip olunabilir. Evren dağılımı bilindiği durumlarda parametrik istatistikler (t-testi, ANOVA, ANCOVA...) kullanılır. Ancak evren dağılımı bilinmediği ya da herhangi bir varsayımda bulunulmadığı durumlarda nonparametrik testler kullanılır.  

## 1.1. Varsayımlar
Nonparametrik testler, parametrik testlerin varsayımlarının karşılanmaması durumunda kullanılan istatistiksel yöntemlerdir. Parametrik ve nonparametrik testlerin varsayımları aşağıdaki gibidir:

- Parametrik testlerde örneklem veya örneklemlerin çekildikleri evrenlerin **normal dağılım göstermesi** ve **varyanslarının homojen olması** istenir. Nonparametrik testlerde ise evren dağılımı konusunda bir varsayım bulunmamaktadır.
- Parametrik testlerde örneklemdeki **gözlem sayılarının 30 veya 30 üzeri olması** istenirken, nonparametrik testler **gözlem sayısının 30'dan az** olduğu durumlarda kullanılabilir. Gözlem sayısının 30 olarak belirlenmesi örneklem istatistiklerinin genellikle 30'a yakınken normal dağılım gösterdiği varsayımıdır. Ancak 30 ve üzerinde olup normal dağılım göstermeyen durumlarla da karşılaşılabilir. Bu bakımdan sadece örneklem sayısına bakarak nonparametrik test yapmaya karar vermek hatalı olabilir. 
- Parametrik testlerde verilerin en az **eşit aralık** düzeyinde ve **sürekli** olması varsayımı vardır. Nonparametrik testlerde ise veri tipi en az **sıralama** düzeyindedir ve süreklilik berlirtmeyebilir.
- Hem parametrik hem de nonparametrik testlerde örneklem evrenden **rastgele** seçilmelidir. 

Nonparametrik testlerde verilerin sıralam düzeyinde kullanılmasını söyle açıklayalım. 
Örneğin A sınıfındaki öğrencilerin boy uzunluklarını içeren bir veri üretelim.

```{r error=TRUE}
set.seed(15)
boy = sample(160:180, 14, replace = TRUE)
names(boy) = paste("ID", 1:14, sep = "")
boy

sort(boy)
rank(boy)
sort(rank(boy))

```

Dikkat edilirse 11 numaralı öğrencinin en kısa boya sahip olduğu (162 cm), 6 numaraalı öğrencinin ise en uzun boya sahip olduğu (180 cm) görülmüştür. 

Nonparametrik testler gruplar arasındaki sistematik farklılıkları karşılaştırmak için kullanılır. Parametrik testlerde genelde ortalamalar karşılaştırılırken,  nonparametrik testlerde Karşılaştırmalar genellikle ortanca değere, verinin şekline ve dağılımına göre yapılır. Bu bakımdan nonparametrik testler ile ilgili araştırmalarda dikkat edilmesi gereken bazı noktalar vardır. 
- Nonparametrik testler oluşturulurken hipotezlerin ve yorumlamaların ortalamalar üzerine kurulmamasına dikkat etmek gerekir. Elde edilen fark dağılımlara ait farktır ve ortalamaları yorumlamak mümkün değildir.
- Veriler sıralama ölçeği düzeyinde olduğu için yorumlanması eşit aralıklı ölçekteki gibi yapılmamalıdır. 
- Nonparametrik testler, parametrik testlere göre genellikle güçlü değillerdir. Bu bakımdan etki büyüklüklerinin ve güç analizlerinin verilmesi önemlidir. 




# 2. Mann Whitney-U Testi

İki ilişkisiz örneklemden elde edilen puanların karşılaştırılmasında kullanılır. Parametrik testlerdeki **bağımsız örneklem t-testi** ile benzerdir. Mann Whitney-U testinde iki grup birbirinden bağımsızdır ve bağımlı değişkenler sıralama düzeyindedir. İki Örneklem Wilcoxon İşaretli Sıralar Testi olarak da adlandırılır. 

Mann Whitney-U testinde eşit aralıklı düzeyde veri olmadığından **ortalamalar** yerine **dağılımlar** karşılaştırılır. Mann Whitney-U testinde hipotezler ilişkisiz grupların ortancaları üzerine değil bir gruptaki gözlemlerin diğer gruptaki gözlemlerden büyük/küçük/farklı olduğu üzerine kurulur. 

Mann Whitney-U analizleri *wilcox.test()* fonksiyonu ile yürütülebilir. Fonksiyonunun kullanımı aşağıdaki gibidir: 

```{r error= TRUE, eval=FALSE}
wilcox.test(x, y = NULL,
            alternative = c("two.sided", "less", "greater"),
            conf.int = FALSE, conf.level = 0.95, …)

```


*wilcox.test()* fonksiyonunun argümanları Tablo 2.1’de açıklanmıştır.

_**Tablo 2.1**_ _wilcox.test() fonksiyonu argümanları ve açıklamaları_

| **Argümanlar**  | **Açıklamalar**                                                                            |
|-------------|------------------------------------------------------------------------------------------------|
| x           | Birinci örnekleme ait sayısal vektör                                                           |
| y           | İkinci örnekleme ait sayısal vektör                                                            |
| alternative | Hipotezin özelliğini belirtir. İki yönlü için "two.sided", büyüktür "greater", küçüktür "less" |
| conf.int    | Güven aralığı kestirimleri istenirse "TRUE"                                                    |
| conf.level  | Güven aralığının düzeyini belirler. Halihazırda 0.95 olarak atanmıştır.                        |
| paired      | Ölçümlerin ilişkisini belirler. Halihazırda "FALSE" olarak kayıtlıdır. "TRUE" seçilir.         |

  
## 2.1. Testin Aşamaları

_**Örnek**_

Araştırmacı, anti-sosyal davranış gösteren ilkokul öğrencilerine iletişim becerileri kazandırmaya yönelik bir programın etkiliğini incelemek istiyor. Bu tür davranışları gözlenen 14 öğrenci yansız olarak iki gruba (deney-kontrol) ayrılıyor. Deney grubu olarak atanan öğrenciler, iletişim becerilerini arttırmaya yönelik dört haftalık X programını alırken, diğer öğrenciler üzerinde hiçbir işlem yapılmıyor. Dört haftalık süre sonunda öğrencilerin iletişim becerileri, geliştirilen bir araçla ölçülüyor. Yüksek puan, iletişim becerilerinin yüksek olduğunu göstermektedir. Çalışmaya ilişkin veriler **"iletisim.csv"** dosyasına kaydedilmiştir. 

## 2.2. Araştırma Problemi ve Hipotezlerin Oluşturulması 


_**Araştırma Problemi**_: X programına katılan öğrencilerin iletişim becerileri, böyle bir programa katılmayan öğrencilere göre anlamlı bir şekilde daha yüksek midir? 

_**Sıfır Hipotezi**_ $H_{0}$ : X programına katılan öğrencilerin iletişim becerileri, böyle bir programa katılmayan öğrencilere göre anlamlı bir şekilde daha yüksektir.

_**Alternatif Hipotezi**_ $H_{1}$ : X programına katılan öğrencilerin iletişim becerileri, böyle bir programa katılmayan öğrencilere göre anlamlı bir şekilde daha yüksek değildir.



Mann Whitney-U analizleri için veriler *read.csv2()* fonksiyonu yardımıyla okutulup, *iletisim* nesnesi olarak kaydedilir. *str()* fonksiyonu yardımıyla nesnenin yapısı incelenir.

```{r error= TRUE}

iletisim <- read.csv2("https://raw.githubusercontent.com/rnzbrk/NonparametrikTestler_Workshop/master/iletisim.csv", header=TRUE)
iletisim

str(iletisim)



```
 
Dikkat edilirse **iletisim** nesnesine ait *grup* ve *iletisim* değişkenlerinin her ikisi de tamsayı (integer) değerlerdir. **grup** değişkeni faktör değişkene dönüştürülmelidir. Bu sayede deney ve kontrol olmak üzere iki farklı grup oluşturulabilir. 
 
```{r error= TRUE}


iletisim$grup <- factor(iletisim$grup, c(1,2), labels=c("Deney","Kontrol"))
iletisim$IletisimSira <- rank(iletisim$iletisim)

str(iletisim)



```


## 2.3. Betimsel İstatistiklerin Hesaplanması 


Bir sonraki adımda verinin yapısını incelemek adına çeşitli gösterimlerde bulunulacaktır. **iletisim** veri setindeki değişkenlerin gruplara göre betimsel istatistikleri *tapply()* ve *summary()* fonksiyonları ile gösterilmiştir. 


```{r error= TRUE}

tapply(iletisim$iletisim, iletisim$grup, summary)

tapply(iletisim$IletisimSira, iletisim$grup, summary)
tapply(iletisim$IletisimSira, iletisim$grup, sum)

```
Yukarıda elde edilen çıktılar incelendiğinde üç özet değer elde edildiği görülebilir. İlki verilerin eşit aralık olarak kabul edildiği durumdaki değerlerdir. İkincisi ise verilerin sıralama düzeyinde olduğu duruma ait değerlerdir. İkinci çıktıdan Deney grubu için elde edilen 9,86 ile Kontrol grubu için elde edilen 5,14 değerleri Mann Whitney-U testinde rapor edilen Sıra Ortalamaları değerleridir. Son olarak ise Deney ve Kontrol grubuna ait Sıra Toplamları değerleri elde edilmiştir. Bu değer de Mann Whitney-U testinde rapor edilmesi istenen bir değerdir. 



## 2.4. Varsayımların Test Edilmesi

Mann Whitney-U testinin varsayımları olan 1) iki grubun birbirinden bağımsız olması ve 2) bağımlı değişkenin en az sıralama ölçeğinde olması varsayımı test edilir. İkinci varsayım halihazırda sağlandığı için bu adımda birinci varsayıma ilişkin değerler gösterilecektir. Grupların ortancaları arasındaki farkı oraya koymak için bağımsız değişkenin her bir düzeyine göre bağımlı değişkenin dağılımının benzer olması beklenir. Dağılımların yerinin önemi yoktur yanlızca dağılımın şekillerinin birbirine benzemesi gerekmektedir. 

Deney ve kontrol grubu grafiklerini aynı grafikte göstermek için grafik penceresi *par()* fonksiyonu ile belirlenir. Bu sayede 1 satır ve 2 sütunlu bir boş çerçeve oluşturulmuş olur. Daha sonra bağımsız değişkenin her düzeyleri olan Deney ve Kontrol grupları için histogramlar aşağıdaki gibi çizilir. 


```{r error= TRUE}
par(mfrow=c(1,2))
hist(iletisim$iletisim[iletisim$grup == "Deney"], 
     main = "Deney Grubu Histogram", xlab= "İletişim Puan")
hist(iletisim$iletisim[iletisim$grup == "Kontrol"], 
     main = "Kontrol Grubu Histogram", xlab= "İletişim Puan")

par(mfrow=c(1,1))
```


Deney ve kontrol grubuna ait histogramların şekilleri birbirinden farklı çıkmıştır. Bu bakımdan ortanca değerler yerine ortalama sıra (mean rank) değerleri rapor edilmelidir. Eğer histogramlar birbirine benzer çıkmış olsaydı iletişim becerilerini farklarını açıklamak için ortanca değerler kullanılabilecekti. 


 
## 2.5. Mann Whitney-U testinin oluşturulması

Mann Whitney-U testi sıfır hipotezini test etmektedir. *p* değeri belirlenen kritik değerden (0,05) küçük olduğu durumda sıfır hipotezi reddedilir. *wilcox.test(sayısal_değişken ~ faktör_değişken*)* formu kullanılarak hesaplamalar yapılır. 



```{r error= FALSE}


wilcox.test(iletisim$Iletisim ~ iletisim$grup, alternative = "greater")
 
```


Analiz sonucunda *W=41* değeri ve *p=0,02035* olmak üzere iki değer elde edilir. Wilcoxon W değeri sıra değerlerinin toplamının en düşüğünü gösterir ve *p* değerini hesaplamak için kullanılır. 

## 2.6. Sonuçların Raporlanması

Mann Whitney-U testi sonuçlarına göre anti-sosyal davranışları gözlenen çocuklardan X programına katılanlarla, böyle bir programa katılmayanların uygulama sonrasındaki iletişim becerileri Tablo 2.2'de verilmiştir. Buna göre, dört haftalık bir deneysel çalışma sonunda, X programına katılan çocuklar ile böyle bir programa katılmayan çocukların iletişim becerileri arasında anlamlı bir fark olduğu bulunmuştur (W=41, p=<0,05). Sıra ortalamaları dikkate alındığında, X programına katılan çocukların, programa katılmayan çocuklara göre iletişim becerilerinin daha yüksek olduğu anlaşılmaktadır. Bu bulgu, X programının, anti-sosyal dvranış gösteren çocukların iletişim becerilerini arttırmada etkili olduğunu gösterir.   

_**Tablo 2.2**_ _İletişim Becerilerinin Gruba Göre Mann Whitney-U Testi Sonuçları_

|   Grup  |   n |  Sıra Ortalaması |  Sıra Toplamı |    W |    p |
|:-------:|:---:|:----------------:|:-------------:|:----:|:----:|
| Deney   | 7   | 9,86             | 69            | 41,0 | 0,02 |
| Kontrol | 7   | 5,14             | 36            |      |      |


 
 
 
 
 
 
# 3. Wilcoxon İşaretli Sıralar Testi

İlişkili iki örnekleme ait puanlar arasındaki farkın anlamlılığını test etmek için kullanılır. Parametrik testlerdeki **bağımlı örneklem t-testi** ile benzerdir. Wilcoxon İşaretli Sıralar testinde gözlem çiftleri birbirinden bağımsızdır ve bağımlı değişkenler sıralama düzeyindedir. 

Wilcoxon İşaretli Sıralar testinde analizler fark puanları üzerinden yürütülür. Fark puanları küçükten büyüğe doğru, 1'den başlayarak işaretine dikkat edilmeksizin sıra sayıları verilmesi temeline dayanır. Daha sonra + ve - işaretli olan fark puanlarının sıra sayıları toplanır. Test edilen durum gerçekte, bu iki sıra sayıların toplamı arasındaki farktır. Aynı puanlar analiz dışı tutulur ve analiz küçük olan sıra toplamları üzerine kurulur. Z istatistiği kullanılır. 

Wilcoxon İşaretli Sıralar testi az denekli yürütülen grupiçi karşılaştırmalarda kullanılır. Deneklerin fark puanlarının normal dağılım göstermediği durumlarda bağıımlı örneklem t-testi yerine tercih edilir.  


Wilcoxon İşaretli Sıralar analizleri *wilcox.test()* fonksiyonu ile yürütülebilir. Fonksiyonunun kullanımı aşağıdaki gibidir. 

```{r error= TRUE, eval=FALSE}
wilcox.test(x, y = NULL,
            alternative = c("two.sided", "less", "greater"),
            paired= TRUE, conf.int = FALSE, conf.level = 0.95, …)

```


Daha önce Tablo 2.1’de açıklanan *wilcox.test()* fonksiyonunun argümanlarından "paired=TRUE" argümanı eklenir.  


## 3.1. Testin Aşamaları

_**Örnek**_

Konuşma becerilerini geliştirmede kullanılmak amacıyla hazırlanan X programının etkililiği araştırılıyor. Okulöncesi eğitime devam eden ve konuşma güçlüğü çeken sekiz çocuğun uygulama öncesi ve sonrası bir konuşma testinden aldıkları puanlar **"konusma.csv"** dosyasına kaydedilmiştir. 


## 3.2. Araştırma Problemi ve Hipotezlerin Oluşturulması


_**Araştırma Problemi**_ Çocukların konuşma becerileri, uygulanan X programı sonrasında anlamlı bir farklı mıdır?

_**Sıfır Hipotezi**_ $H_{0}$ : Çocukların X programına ait öncesi ve sonrası konuşma becerileri arasında anlamlı bir farkdır.

_**Alternatif Hipotezi**_ $H_{1}$ : Çocukların X programına ait öncesi ve sonrası konuşma becerileri arasında anlamlı bir fark yoktur. 



Wilcoxon İşaretli Sıralar analizleri için veriler *read.csv2()* fonksiyonu yardımıyla okutulup, *iletisim* nesnesi olarak kaydedilir. *str()* fonksiyonu yardımıyla nesnenin yapısı incelenir.

```{r error= TRUE}

konusma <- read.csv2("https://raw.githubusercontent.com/rnzbrk/NonparametrikTestler_Workshop/master/konusma.csv", header=TRUE)
konusma

str(konusma)



```
 
Dikkat edilirse **konusma** nesnesine faktör değişken bulunmamaktadır. Burada aynı bireye ait öntest ve sontest puanları ilişkili ölçümleri belirtmektedir. İlişkili ölçümlerde fark değerleri üzerinden işlemler yapıldığından sontest puanlarından öntest puanlarını çıkararak fark değişkeni tanımlanmış ve daha sonra sıra numarası verilmiştir. Ayrıca *cut()* fonksiyonu kullanılarak sıfırdan düşük değerlere "Negatif Sıra", sıfırdan büyük değerlere ise "Pozitif Sıra", sıfıra eşit değerlere de "Eşit" ismi verilerek üç faktörlü grup değişkeni oluşturulmuştur. 

 
```{r error= TRUE}


konusma$fark <- konusma$sontest - konusma$ontest

konusma$grup <- cut(konusma$fark, c(-10,-0.99, 0.099,+10),
                    labels = c('Negatif Sıra','Eşit','Pozitif Sıra'), right = FALSE)

konusma$farkSira <- rank(abs(konusma$fark))
str(konusma)



```



## 3.3. Betimsel İstatistiklerin Hesaplanması 

Bir sonraki adımda verinin yapısını incelemek adına çeşitli gösterimlerde bulunulacaktır. **konusma** veri setindeki değişkenlerin gruplara göre betimsel istatistikleri *tapply()* ve *summary()* fonksiyonları ile gösterilmiştir. 


```{r error= TRUE}

summary(konusma)


tapply(konusma$farkSira, konusma$grup, summary)
tapply(konusma$farkSira, konusma$grup, sum)

```

Yukarıda elde edilen çıktılar incelendiğinde üç özet değer elde edildiği görülebilir. İlki verilerin eşit aralık olarak kabul edildiği durumdaki değerlerdir. İkincisi ise verilerin sıralama düzeyinde olduğu duruma ait değerlerdir. İkinci çıktıdan Sıra Ortalamaları değerleri elde edilir. Son olarak ise Sıra Toplamları değerleri elde edilmiştir. Elde edilen değerler Wilcoxon İşaretli Sıralar testinde rapor edilir. 


## 3.4. Varsayımların Test Edilmesi

Wilcoxon İşaretli Sıralar testinin fark dağılımlarının normal olup olmadığına bakılır. Bunun için fark değerlerinin histogramı çizilmiştir.




```{r error= TRUE}

median(konusma$ontest)
median(konusma$sontest)
hist(konusma$fark, main = "Fark Ortalamaları Histogramı", xlab= "Konuşma Becerisi Puan")


```


Grafik incelendiğinde eşleşmiş ölçümlerin farklarının normal olmadığı ve böylece parametrik testlerin yerine nonparametrik testlerin uygulanması gerektiği yorumu yapılabilir. Parametrik testlerde daima ortalama yerine ortanca değerlerinin yorumlanmasına dikkat ediniz. 


 
## 3.5. Wilcoxon İşaretli Sıralar testinin oluşturulması

Wilcoxon İşaretli Sıralar testi sıfır hipotezini test etmektedir. *p* değeri belirlenen kritik değerden (0,05) küçük olduğu durumda sıfır hipotezi reddedilir. *wilcox.test(sayısal_değişken, sayısal_değişken, paired=TRUE)* formu kullanılarak hesaplamalar yapılır. 



```{r error= FALSE}


wilcox.test(konusma$ontest, konusma$sontest, paired = TRUE)

```


Analiz sonucunda *V=2* değeri ve *p=0,0485* olmak üzere iki değer elde edilir. Wilcoxon W değeri sıra değerlerinin toplamının en düşüğünü gösterir ve *p* değerini hesaplamak için kullanılır. V değeri hesaplanan Z değerini belirtmektedir. 

## 3.6. Sonuçların Raporlanması

sonuçlarına göre çocukların öncesi ve sonrası konuşma becerilerinin anlamlı bir farlılık gösterip göstermediğine ilişkin Wilcoxon İşaretli Sıralar testi sonuçları Tablo 3.1'de verilmiştir. Analiz sonuçları, araştırmaya katılan çocukların kouşma becerileri testinden aldıkları deney öncesi ve sonrası puanları arassında anlamlı bir fark olduğunu göstermektedir (Z=2; p=<0,05). Fark puanlarının sıra ortalaması ve toplamları dikkate alındığında, gözlenen bu farkın pozitif sıralar, yani son test puanı lehine olduğu görülmektedir. Bu sonuçlara göre, düzenlenen X programının çocukların konuşma becerilerini geliştirmede önemli bir etkisinin olduğu söylenebilir. 



_**Tablo 3.1**_ _İletişim Becerilerinin Gruba Göre Mann Whitney-U Testi Sonuçları_

| Sontest-Öntest |   n |  Sıra Ortalaması |  Sıra Toplamı |   Z |   p    |
|----------------|-----|------------------|---------------|-----|--------|
| Negatif Sıra   | 1   | 3                | 3             | 2   | 0,0485 |
| Pozitif Sıra   | 6   | 5,33             | 32            |     |        |
| Eşit           | 1   |                  |               |     |        |


 
 
 
 
 
 
 
 
# 4. Kruskal Wallis Testi

Kruskal Wallis testi Mann Whitney-U testinin ikiden fazla örnekleme uygulanan halidir. Analizde k tane örneklemin bağımlı değişken üzerinde anlamlı farklılık gösterip göstermediği incelenir. ANOVA testinin nonparametrik halidir ve varsayımları arasında a) bağımlı değişkenin en az sıralama ölçeği düzeyinde olması b) gözlemlerin birbirinden bağımsız olmasını gerektirir. ANOVA'dan farklı olarak her bir alt gruba ait normal dağılım ve varyansların eşitliği varsayımı bulunmaz. 

Kruskal Wallis testinde hipotezler ortanca değerler üzerine kurulmaz.Aksine hipotezler bir grupta gözlemlerin diğer gruptan büyük/eşit/küçük olduğu üstüne kurulmaktadır.  


Kruskal Wallis analizleri *kruskal.test()* fonksiyonu ile yürütülebilir. Fonksiyonunun kullanımı aşağıdaki gibidir. 

```{r error= TRUE, eval=FALSE}

kruskal.test(x, g, …)


```



## 4.1. Testin Aşamaları

_**Örnek**_

Okuma güçlüğü çeken bir grup öğrencinin, okuma becerilerini geliştirmede üç farklı eğitim programının (A-B-C) etkililikleri araştırılıyor. Bu amaçla, ilkokul birinci sınıf öğrencileri iiçnden belirlenen 12 öğrenci, yansız olarak 3 gruba atanıyor ve öğrenciler, normal eğitimlerinin yanı sıra üç haftalık bir okuma becerilerini geliştirme eğitimine katılıyorlar. Çalışmaya ilişkin denemelik veriler **okuma.csv** isimli dosyadada bulunmaktadır. 

## 4.2. Araştırma Problemi ve Hipotezlerin Oluşturulması


_**Araştırma Problemi**_ Öğrencilerin okuma becerileri, alınan eğitim programına göre anlamlı farklılık göstermekte midir?

_**Sıfır Hipotezi**_ $H_{0}$ : Öğrencilerin okuma becerileri, alınan eğitim programına göre anlamlı farklılık göstermektedir.

_**Alternatif Hipotezi**_ $H_{1}$ : Öğrencilerin okuma becerileri, alınan eğitim programına göre anlamlı farklılık göstermemektedir. 



Kruskal Wallis analizleri için veriler *read.csv2()* fonksiyonu yardımıyla okutulup, *okuma* nesnesi olarak kaydedilir. *str()* fonksiyonu yardımıyla nesnenin yapısı incelenir.

```{r error= TRUE}

okuma <- read.csv2("https://raw.githubusercontent.com/rnzbrk/NonparametrikTestler_Workshop/master/okuma.csv", header=TRUE)
okuma

str(okuma)



```
 
Dikkat edilirse **okuma** nesnesinde *yontem* faktör değişken iken *okuma* ise sayısal değişkendir. Veri setinde okuma puanlarına göre sıralamalar *rank()* fonksiyonu ile oluşturulup *okumaFark* değişkeni olarak kaydedilmiştir. 

 
```{r error= TRUE}


okuma$okumaFark <- rank(okuma$okuma)

okuma

```


## 4.3. Betimsel İstatistiklerin Hesaplanması 

Bir sonraki adımda verinin yapısını incelemek adına çeşitli gösterimlerde bulunulacaktır. Okuma puanlarının yöntemlere göre betimsel istatistikleri *tapply()* ve *summary()* fonksiyonları ile gösterilmiştir. 


```{r error= TRUE}

summary(okuma)



tapply(okuma$okumaFark, okuma$yontem, summary)
tapply(okuma$okumaFark, okuma$yontem, sum)

```

Yukarıda elde edilen çıktılar incelendiğinde üç özet değer elde edildiği görülebilir. İlki verilerin eşit aralık olarak kabul edildiği durumdaki değerlerdir. İkincisi ise verilerin sıralama düzeyinde olduğu duruma ait değerlerdir. Burada her bir yöntem için elde edilen ortalama değerleri A yöntemi için 2,50; B yöntemi için 6,75 ve C yöntemi için 10,25 olarak bulunmuştur. Bu değerler rapor edilmesi gereken Sıra ortalaması değerleridir. Son olarak ise A yöntemi için 10; B yöntemi için 27 ve C yöntemi için 41 olarak Sıra Toplamları değerleri elde edilmiştir. Elde edilen değerler Kruskal Wallis testinde rapor edilir. Elde edien sonuçlar incelendiğinde C yönteminin öğrencilerin okuma becerilerine diğer iki yöntemden daha fazla etki ettiği görülmüştür. 


## 4.4. Varsayımların Test Edilmesi

Kruskal Wallis testininde ortanca değerlerin nasıl farklılaştığını belirlemek adına her üç yöntem için kutu grafikleri çizilir. Kutu grafiğinin çiziminde *boxplot(bağımlıdeğişken~bağımsızdeğişken)* formatı kullanılır. 



```{r error= TRUE}
par(mfrow=c(1,2))
boxplot(okuma$okuma~okuma$yontem, main= "Yöntemlere Göre Okuma Becerisi Puanları")
boxplot(okuma$okumaFark~okuma$yontem, main= "Yöntemlere Göre Okuma Becerisi Sırası")
par(mfrow=c(1,1))
```


Kutu grafiğinde kutuların içindeki kalın yatay çizgiler ortanca değerleri ifade eder. Kutu grafiğinin alt ve üst değerleri de dağılımın en düşük ve en yüksek noktalarını belirtir. Burada da görüldüğü gibi C yönteminin diğer iki yöntemden daha fazla okuma becerisine katkı sağladığı görülmektedir. Sağdaki grafikte ise yöntemlere göre okuma becerilerinden elde edilen sıra değerleri grafiği çizilmiştir. Her iki grafik birbiriyle tutarlılık göstermektedir. 

Grafiklerin daha renkli gösterimleri için aşağıdaki kodları kullanabilirsiniz. 


```{r error= TRUE}
library("ggpubr")
library("ggplot2")

g1 <- ggboxplot(okuma, x = "yontem", y = "okuma", 
          color = "yontem", palette = c("#00AFBB", "#E7B800", "#FC4E07"),
          order = c("A", "B", "C"), main="Puan Karşılaştırma",
          ylab = "Okuma Becerisi Puanları", xlab = "Yöntem")

g2 <- ggboxplot(okuma, x = "yontem", y = "okumaFark", 
          color = "yontem", palette = c("#00AFBB", "#E7B800", "#FC4E07"),
          order = c("A", "B", "C"), main="Sıra Karşılaştırma",
          ylab = "Okuma Becerisi Sıralaması", xlab = "Yöntem")

ggarrange(g1, g2, ncol = 2, nrow = 1)

```

 
## 4.5. Kruskal Wallis testinin oluşturulması

Kruskal Wallis testinin amacı sıfır hipotezini test etmektedir. *p* değeri belirlenen kritik değerden (0,05) küçük olduğu durumda sıfır hipotezi reddedilir. *kruskal.test(bağımlı_değişken ~ bağımsız_değişken)* formu kullanılarak hesaplamalar yapılır. 



```{r error= FALSE}

kruskal.test(okuma$okuma ~ okuma$yontem)

```


Analiz sonucunda sd=(k-1) üzerinden kestirilen ki-kare istatistiği değeri hesaplanır.  *$x^{2}$=9,26* değeri ve *p=0,001* olmak üzere iki değer elde edilir. *p* değerinin kritik değer olan 0,05'ten küçük olması nedeniyle yöntemler arasında anlamlı bir farkın olduğu yorumu yapılabilir.  ,

Farkların hangi gruplar arasında olduğunu belirlemek için ANOVA analizlerindeki gibi Kruskal Wallis testinde de post-hoc testlerine başvurulur. Kruskal Wallis için post-hoc testi *pairwise.wilcox.test()* fonksiyonu yardımı ile yapılmaktadır. Fonksiyonunun kullanımı aşağıdaki gibidir: 

```{r error= TRUE, eval=FALSE}
pairwise.wilcox.test(x, g, p.adjust.method = p.adjust.methods, ...)

```


*pairwise.wilcox.test()* fonksiyonunun argümanları Tablo 4.1’de açıklanmıştır.

_**Tablo 4.1**_ _pairwise.wilcox.test() fonksiyonu argümanları ve açıklamaları_

| **Argümanlar**  | **Açıklamalar**                                                                                |
|-----------------|------------------------------------------------------------------------------------------------|
| x               | Bağımlı Değişken                                                                               |
| g               | Kategorik bağımsız değişken                                                                    |
| p.adjust.method | Nonparametrik ikili karşılaştırma yöntemleri:"holm","bonferroni","fdr","BY" ve "none"          |





```{r error= FALSE}

pairwise.wilcox.test(okuma$okuma, okuma$yontem, p.adj='fdr')

```


Post-hoc analizi sonucu p değerlerin karşılaştırıdığı bir matris elde edilir. Bu değerlere bakılarak *p < 0,05* koşulunu sağlayan ikililer belirlenir. Burada A-B ve A-C grupları arasında anlamlı bir fark olduğu; B-C grupları arasında anlamlı fark bulunmadığı yorumu yapılabilir. 

## 4.6. Sonuçların Raporlanması

Kruskal Wallis testi sonuçlarına göre farklı öğretim yöntemine göre eğitim alan, okuma güçlüğü çeken çocukların program sonrasındaki okuma becerileri testinden aldıkları Kruskal Wallis testi sonuçları Tablo 4.2'de verilmiştir. 

Analiz sonuçları, deneysel çalışmaya katılan çocukların okuma becerileri testinden aldıkları puanların, aldıkları öğretim yöntemine göre anlamlı bir şekilde farklılaitığını göstermektedir (*$x^{2}$(sd=2, n=12) = 9,26; p<0,001*).Bu bulgu, kullanılan üç yöntemin çocukların okuma becerilerini artırmada farklı etkilere sahip olduğunu gösterir. Grupların sıra ortalamaları dikkate alındığında, uygulama sonrasında en yüksek okuma becerisi sırasının C programına katılanların sahip olduğu, bunu B ve A'nın izlediği görülmektedir. 

Gruplar arasından gözlenen anlamlı farkın hangi gruplar arasasında çıktığını belirlemek adına Wilcoxon ikili karşılaştırma testi (post-hoc) kullanılmıştır. Elde edilen sonuçlara bakıldığında A-B ve A-C grupları arasında (B ve C lehine) anlamlı bir fark olduğu; B-C grupları arasında anlamlı fark bulunmadığı yorumu yapılabilir.


_**Tablo 4.2**_ _Okuma Becerilerinin Uygulanan Yönteme Göre Kruskal Wallis Testi Sonuçları_

| Yöntem | n |  Sıra Ortalaması | sd | ki-kare | p    |  Anlamlı Fark |
|--------|---|------------------|----|---------|------|---------------|
| A      | 4 | 2,50             | 2  | 9,26    | 0,01 |  A-B          |
| B      | 4 | 6,75             |    |         |      |  A-C          |
| C      | 4 | 10,25            |    |         |      |               |
 
 
 
 
 
 
# 5. Ki-kare Testi

Ki-kare testi kategorik bir değişkenin düzeylerine giren birey ya da nesnelerin anlamlı bir farklılık gösterip göstermediğini test eder. İyi uyum testi olarak da bilinir. Kategorik değişkenler arasındaki farkın anlamlığını inceler. Bu bakımdan gözlenen değerler ile beklenen değerlerin birbirinden anlamlı olarak farklılaşıp farklılaşmadığına bakar. 

Tek örneklem ve iki örneklem üzerinden ki-kare testleri yapılabilmektedir. Bu bölümde her iki tür de anlatılacaktır. 

Ki-kare testinde her bir kategoriye ait gözlenen ve beklenen değerler arasındaki farkın karesinin beklenen değere bölünmesiyle elde edilen bir ki-kare istatistiği değeri elde edilir ve her koşul için elde edilen bu değerler toplanır. Beklenen ve gözlenen değerler arasındaki ark azaldıkça yani birimlerin kategorilere dağılımının uyum düzeyi arrtıkça ki-kare istatistiğinin anlamlı çıkma olasılığı düşecektir (yani farklılık olmayacaktır). Bu testin kullanılabilmesi için beklenen değerin beşten küçük kategori sayısının, toplam kategori sayısının %20'sini aşmaması ve tüm kategorilerde bu değerin 1'den büyük olması  gerekir. Bu koşul sağlanmıyor ise kategorilerde birlleştirme yapılması önerilir. 

Tek örneklem ki-kare testi tek grup araştırmalarında tek bir değişkene ilişkin değişkenliğin imcelendiği deneysel ya da tarama çalışmalarında kullanılabilir.

Ki-kare analizleri *chisq.test()* fonksiyonu ile yürütülebilir. Fonksiyonunun ve argümanların kullanımı Tablo 5.1'de gösterilmiştir.

```{r error= TRUE, eval=FALSE}

chisq.test(x, y = NULL, correct = TRUE, p = rep(1/length(x))


```

_**Tablo 5.1**_ _chisq.test() fonksiyonu argümanları ve açıklamaları_



| Argümanlar | Açıklamalar                                      |
|------------|--------------------------------------------------|
| x          | Sayısal vektör ya da matris                      |
| y          | İkinci değişkene ait sayısal vektör ya da matris |
| correct    | Yates süreklilik düzeltilmesi kullanımı          |
| p          | Beklenen olasılık dağılımı                       |


## 5.1. Testin Aşamaları

_**Örnek**_

Anaokulundaki çocukların tercih ettikleri oyun türünü gözleyen araştırmacı gözlem sonuçlarına göre çocukların oynadıkları oyunlar arasında bir uyum olup olmadığını incelemek istemektedir.  

## 5.2. Araştırma Problemi ve Hipotezlerin Oluşturulması


_**Araştırma Problemi**_ Çocukların tercih ettikleri oyun türleri arasında anlamlı bir farklılık var mıdır?

_**Sıfır Hipotezi**_ $H_{0}$ : Çocukların tercih ettikleri oyun türleri arasında anlamlı bir farklılık vardır.

_**Alternatif Hipotezi**_ $H_{1}$ : Çocukların tercih ettikleri oyun türleri arasında anlamlı bir farklılık yoktur. 


Yapılan araştırmada çocukların oynadıkları oyunların üç farklı kategoride toplandığı belirlenmiştir. Kategoriler ilişikin gözlem sayıları aşağıdaki gibi belirtilmiştir. 
Fiziksel: 11
Küp oyunları: 7
Dramatik oyunlar:2 

```{r error= TRUE}

oyun <- c(11,7,2)
oyun

str(oyun)



```
 

## 5.3. Betimsel İstatistiklerin Hesaplanması 

Ki-kare analizlerinde veriye ait betimsel istatistiklerden sadece yüzdelik oranlara ilişkin ifadeler elde edilebilir.  *prop.table()* fonksiyonu ile her bir oyun türüne ait oranlar belirlenir. 


```{r error= TRUE}

prop.table(oyun)


```

Sonuçlar incelendiğinde tüm oyunlar içinde fiziksel oyunların %55, küp oyunlarının %35 ve dramatik oyunların ise %10'luk bir paya sahip olduğunu görmekteyiz. Bu elde edilenler gözlenen değerlerdir. Beklenen değerler ise her bir oyuna 1/3 olacak şekilde dağılmasıdır. Bu varsayımlar ile ki-kare analizi aşağıdaki gibi formülize edilir. 


 
## 5.4. Ki-kare testinin oluşturulması

Ki-kare testinin kategoriler arasındaki anlamlı farkları test etmektir. *p* değeri belirlenen kritik değerden (0,05) küçük olduğu durumda sıfır hipotezi reddedilir. *chisq.test(sayısal_değişken, p=beklenen_değer)* formu kullanılarak hesaplamalar yapılır. 



```{r error= FALSE}
oyun_sonuc <- chisq.test(oyun, p = c(1/3, 1/3, 1/3))
oyun_sonuc


oyun_sonuc$expected
oyun_sonuc$observed
oyun_sonuc$statistic
oyun_sonuc$p.value


```




## 5.5. Sonuçların Raporlanması

Analiz sonucunda tek örneklem ki-kare testi sonucuna göre gözlem yapılan 11'i fiziksel, 7'si küp ve 2'si dramatik türden oyun oynanmıştır. En az tercich edilen oyun türü dramatik en çok ise fizikseldir. Çocukların tercih ettikleri oyun türleri arasında gözlenen fark anlamlı bulunmuştur ($X^{2}$=6,1; p < 0,05)



_**Tablo 5.2**_ _Tercih edilen oyun türüne göre oynama sıklıklarının karşılaştırılmasına ilişkin $X^{2}$ testi sonuçları _

| Gruplar  | Gözlenen | Beklenen | $X^{2}$ | sd | p     |
|----------|----------|----------|---------|----|-------|
| Fiziksel | 11       | 6,7      | 6,1     | 2  | 0,047 |
| Küp      | 7        | 6,7      |         |    |       |
| Dramatik | 2        | 6,7      |         |    |       |
| Toplam   | 20       |          |         |    |       |
 
 
 
