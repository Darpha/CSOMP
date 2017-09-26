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

	1. En yüksek korelasyonlu vektörleri bulan optimizasyon birimi, 
	2. En Küçük Kareler çözümünü yapan EKK birimi, 
	3. Kontrol ve veri girişi. 

Optimizasyon adımı; Φ’nin sütunları ile artık vektör arasındaki iç çarpım hesaplamasını içerir. Amaç artık vektör ile en yüksek korelasyonlu olan sütun vektörünün indeksini bulmaktır. Algoritmanın başında artık vektörü ölçüm vektörünün değerini alır. İlk indeks bulunduktan sonra, Φ'nin ilgili sütunu, EKK aşamasında kullanılmak üzere seçilen sütunların kaydedildiği bir matrise eklenir. Her yinelemede, daha önce seçilen sütunun etkisini gidermek için artık vektörünün güncellenmesi gerekmektedir. Bu işlem, toplam K (seyreklik değeri) defa tekrarlanır. Algoritmanın bu bölümü, hızlı ve verimli bir iç çarpım hesaplaması için paralel çarpma devreleri kullanılarak gerçekleştirilmiştir.

Φ'nin seçilen sütunları, her yinelemede, aşağıda verilen üst-belirlenmiş (ing.overdetermined) sistemi çözmek için kullanılır.

y = Φ ̂x

Her yinelemede Φ matrisinin boyutları değişmektedir ve daha önce seçilen sütunlar sonraki yinelemelerde aşamalı olarak kullanılmaktadır. Böyle bir sistem genellikle EKK yaklaşımı ile çözülmektedir. Yeniden oluşturulan sinyalin artık vektörüne dik olmasını sağlamak için her adımda EKKişlemi gerçekleştirilmelidir. Bu, daha sonraki yinelemeler için aynı sütunun seçilmesini önlemekteve algoritma, K yineleme sonra işlemini tamamlamaktadır. EKKbirimi temel olarak ölçeklenebilir sayıda işleme elemanları (İE), karşılaştırma birimi, bellek (RAM) blokları ve denetleyiciden oluşur. İE'ler matris çarpması işlemini paralel yürüterek hızlandırmak için kullanılmaktadır.

Kontrol; bir Sonlu Durum Makinesi (SDM) ile gerçekleştirilmiştir. Ana cihaz ile FPGA arasındaki veri aktarımı bir UART modülü ile sağlanmıştır. Sıfırlamadan sonra devre Boş durumuna geçmektedir.UART modülüneveri ulaştığı zaman, SDM, ölçüm matrisi Φ'yi ve ölçüm vektörü y'yi almak üzere UART Okuma durumuna geçer. Alınan veriler özel bellek bloklarına yazıldıktan sonra, devre başlama sinyalini beklemeye başlar. Başlama sinyali ile geldiği zaman, devre iç çarpım hesaplamasının yapıldığı ve maksimum indeksin bulunduğuoptimizasyon durumuna geçmektedir. Cmatrisi Cholesky Matris durumuna geçildikten sonra, Φ'nin seçilen sütunlarından hesaplanır. Bundan sonra, kontrol C matrisinin tersini hesaplamak için Cholesky Ters durumuna geçer. Yapılan yineleme sayısına bağlı olarak, SDM ya başka bir indeks bulmak için tekrar optimizasyon durumuna geçer veya yeniden oluşturulmuş sinyali ana cihaza göndermek için UART Aktarım durumuna geçer. Mimarinin çeşitli parçaları için gereken çoklayıcılar, SDM'nin uygun durumlarında tanımlanmıştır.

![HL](/images/fig_HL.png)

### EKG Görüntüleyici Sistem

SA tekniğiyle algılanan sinyaller, tasarım içinde tanımlanan RAM bloklarında saklanmaktadır. Ölçüm matrisi ve dalgacık dönüşümü için gereken temel küme matrisi de EAD-DEA birimi içerisindeki RAM bloklarında başlangıç değeri olarak saklanmak amacıyla sentez aşamasında belirlenmektedir. Ölçüm vektörü kullanılan segmentuzunluğuna bağlı olarak, ilgili RAM bloğundan alınarak yeniden oluşturma birimine aktarılmaktadır. Bu aşamada ana modül yeniden oluşturma işleminin bitmesini beklemektedir. Bu işlem bittikten sonra elde edilen yeniden oluşturulmuş sinyal; görüntüleme arayüzü aracılığıyla video çıkışı olarak dışarıya verilmektedir. Sistem ilgili segmentin uzunluğuna göre bir süre boş konumda bekledikten sonra yeni bir ölçüm vektörünü yeniden oluşturma birimine yollayarak işleme devam etmektedir.

Tasarlanan sistem Zedboard geliştirme kartı üzerinde Xilinx Zynq-7000 SoC XC7Z020-CLG484 FPGA üzerinde uygulanmıştır. Yeniden oluşturma birimi her yeni segment için oluşturulan sinyali ardışık olarak bir RAM bloğuna yazmaktadır. Görüntüleyici sistem, yeniden oluşturulan sinyali ana modülde bulunan bu RAM bloğundan ardışık olarak okumakta ve bağlı olan monitörde görüntülemektedir. Görüntüleme arayüzü hem VGA hem de HDMI çıkışından çıktı verebilmektedir. 

Sistem Xilinx Vivado yazılımı ile sentezlenmiş ve uygulanmıştır. VGA arayüzü için 108 MHz (1280x1024 çözünürlük) frekansında bir saat hızı gerektiğinden sistemin hızı bu frekansa sabitlenmiştir. Geliştirme kartı üzerinde bulunan 100 MHz hızındaki osilatör, FPGA içinde bulunan PLL (PhaseLockedLoop) modülü yardımıyla gereken frekans üretilecek şekilde ayarlanmıştır. Tasarlanan EKG görüntüleyici sistem aşağıda görülebilmektedir.

![EKG](/images/EKG_fig.png)
