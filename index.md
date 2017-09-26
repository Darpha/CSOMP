## Sıkıştırıcı Algılama ile Sinyal Yeniden Oluşturma

Bu projede, Dik Eşleştirme Arayış (DEA) algoritmasını temel alan iki özgün donanım mimarisi önerilmiştir. Önerilen mimariler; farklı problem boyutlarına, amaçlanan uygulamaya ve hedef alınan FPGA'ya bağlı olarak uyarlanabilecek şekilde ölçeklenebilir olarak tasarlanmıştır. Gerçekleştirilen donanımlar FPGA üzerinde sentezlenmiş ve performansı değerlendirilmiştir. Tasarlanan donanımlar, gerçek zamanlı EKG yeniden çatma işlemi yapan bir sisteme, geri çatma birimi olarak entegre edilmiş ve elde edilen sinyaller FPGA üzerinden doğrudan video çıktısı alınarak görüntülenmiştir.

### Sıkıştırıcı Algılama

Sıkıştırıcı Algılama; Nyquist-Shannon örnekleme teorisinin belirttiğinden daha az sayıda örnek kullanarak sinyal elde edilmesine ve bu örneklerden geri çatılmasına olanak tanıyan, sinyal işleme alanında öne çıkan yöntemlerden biridir. Doğru bir şekilde sinyal geri çatımı için iki temel şart belirtilmiştir; alt-örneklenen sinyalin seyrekliği ve algılama yapısının rastgele olması. Çoğu sinyalin belirli bir etki alanında seyreklik özelliği göstermesi nedeniyle, gerçek zamanlı sinyalleri içeren uygulamalar SA yönteminden büyük ölçüde faydalanmaktadır. Orijinal sinyali yeniden elde edebilmek için karmaşık geri çatma algoritmalarına ihtiyaç duyması bu yöntemin dezavantajlarından biridir. Yazılım uygulamalarının gerçek zamanlı sinyal işleme sistemleri için yetersiz kalması, donanım hızlandırıcılarının bu iş için kullanılmasını gerekli kılmıştır.

![Sıkıştırıcı Algılama](https://github.com/Darpha/CSOMP/blob/master/images/CS_basic.png)

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/Darpha/CSOMP/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
