**Bu proje, TÜBİTAK tarafından ARDEB - 3001 programı kapsamında desteklenmiştir.** 

Projede, Dik Eşleştirme Arayış (DEA) algoritmasını temel alan iki özgün donanım mimarisi önerilmiştir. Önerilen mimariler; farklı problem boyutlarına, amaçlanan uygulamaya ve hedef alınan FPGA'ya bağlı olarak uyarlanabilecek şekilde ölçeklenebilir olarak tasarlanmıştır. Gerçekleştirilen donanımlar FPGA üzerinde sentezlenmiş ve performansı değerlendirilmiştir. Tasarlanan donanımlar, gerçek zamanlı EKG yeniden çatma işlemi yapan bir sisteme, geri çatma birimi olarak entegre edilmiş ve elde edilen sinyaller FPGA üzerinden doğrudan video çıktısı alınarak görüntülenmiştir.

### Sıkıştırıcı Algılama

Sıkıştırıcı Algılama; Nyquist-Shannon örnekleme teorisinin belirttiğinden daha az sayıda örnek kullanarak sinyal elde edilmesine ve bu örneklerden geri çatılmasına olanak tanıyan, sinyal işleme alanında öne çıkan yöntemlerden biridir. Doğru bir şekilde sinyal geri çatımı için iki temel şart belirtilmiştir; alt-örneklenen sinyalin seyrekliği ve algılama yapısının rastgele olması. Çoğu sinyalin belirli bir etki alanında seyreklik özelliği göstermesi nedeniyle, gerçek zamanlı sinyalleri içeren uygulamalar SA yönteminden büyük ölçüde faydalanmaktadır. Orijinal sinyali yeniden elde edebilmek için karmaşık geri çatma algoritmalarına ihtiyaç duyması bu yöntemin dezavantajlarından biridir. Yazılım uygulamalarının gerçek zamanlı sinyal işleme sistemleri için yetersiz kalması, donanım hızlandırıcılarının bu iş için kullanılmasını gerekli kılmıştır.

![Sıkıştırıcı Algılama](/images/CS_basic.png)


### En Az Destek Dikgen Eşleştirme Arayış Algoritması

EAD-DEA algoritması her bir yinelemede ölçüm matrisi ve artık vektörü arasındaki maksimum korelâsyonu veren En az Destek Parametresi (L) tane sütunu seçerek bunların indekslerini kısmi destek kümesine ekler. Bu destek kümesi tahmini işareti elde etmek ve artık vektörünü güncellemek için kullanılır. Bu işlem L defa veya durdurma koşulu sağlanana kadar yinelenir. Görüldüğü üzere EAD-DEA algoritması kısmi destek kümesini, her bir destek kümesi adayını tek tek kontrol etmeksizin hızlı bir şekilde seçmektedir. Başlangıçta L değeri min⁡(L/2,M/2) olarak seçilir ve teorik olarak bulunan durdurma değerine ulaşıldığında veya L yineleme sonucunda işlem sonlanır.

![Algoritma](/images/alg.png)

### Tasarlanan Donanım Mimarisi

DEA algoritması için önerilen yapı şekilde gösterilmiştir. Temel olarak üç aşamadan oluşmaktadır:
	1) En yüksek korelasyonlu vektörleri bulan optimizasyon birimi, 
	2) En Küçük Kareler çözümünü yapan EKK birimi, 
	3) Kontrol ve veri girişi. 

![HL](/images/fig_HL.png)

