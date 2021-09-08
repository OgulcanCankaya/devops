# EBS
EBS (Elastic Block Store) bizlere EC2 kullanımı sırasında yüksek performanslı blok depolama hizmeti sunmak için tasarlanmış Amazon servisidir.
Her bir EBS, tekrar kullanılabilir bir şekilde EC2 instance'ının bulunduğu AvailabilityZone üzerinde yedeklenir ve bu şekilde bizlere HighAvailability ve veri dayanıklılığı sağlar.

EBS servisini kullanarak verilerinizin Snapshot'ını alabilir ve verinin güvvenliği, bütünlüğü ve yedeklenmesi aşamalarında bu snapshot'ı kullanabilirsiniz. Disaster Recovery veya backup gibi sebeplerle de kullanılabileceği gibi AWS üzerinde veri taşınması sırasında da kullanılabilir ve farklı AvailabilityZone'lar ve hesaplar üzerinde geçişler yaparak data migration işlemlerinde de kolaylık sağlayabilir.

EBS iş yükünüze veya çalıştırdığınız servislere bağlı olarak kullanabileceğiniz farklı seçenekler sunabilir. SSD tabanlı veritabanı ve önyükleme işlemlerini destekleyen depolama seçenekleri ve günlük işlemler için rahatlıkla kullanılabilir HDD destekli depolama seçenekleri sunulabilir. Bu EBS birim türlerine bir bakalım...

EBS Provisioned IOPS SSD (io2 Block Express)
Kritik ve anlık veri girdisi gerektiren işlemler için geliştirilmiş olan io2 EBS birim tipinin geliştirilmiş halidir ve milisaniyenin altında gecikme sunar. Burada yaklaşık olarak saniyede 4GB'a varan veri I/O işlemlerinden bahsedebiliriz. Kullanım amacına bağlı olarak kritik işlemlerde veya ilişkisel veritabanlarının kullanımında rahatlıkla kullanılabilirler. En az 4GB en fazla ise 64TB boyutlarında kullanabileceğiniz io2 Block Express zaman açısından kritik işlemlerin varlığında rahatlıkla fayda sağlayacaktır.

Block Express'ten bahsettikten sonra, EBS Provisioned IOPS SSD (io2) ve EBS Provisioned IOPS SSD (io1)'den bahsedecek olursak, maksimum kullanım boyutları farklarından ve milisaniye cinsinden tepki sürelerinde yaşanan minimal farklar bulunmaktadır.

General Purpose SSD (gp2-gp3)
SSD teknolojisinin hayatımıza getirdiği hızı göz önünde bulundurursak gp2 ve gp3 tipi disklerin bizler için önemini anlayabiliriz. General purpose SSD tipi depolama aygıtlarında fiyat-performans konusunda gönlünüz rahat olabilir. $0.08/GB/Ay (gp2) ile $0.10/GB/Ay (gp3) (aylık, GB başına) arasında değişen değerleri ile küçük ve orta ölçekli sunucu sistemleri için uygun olan General Purpose tipi depolama servisini saniyede 250MB-1000MB arasında değişen bir performans ile kullanabilirsiniz. 1GB ile 16TB aralığında kullanabileceğiniz gp2 ve gp3 ile temel sunucu işlemlerinizi yüksek hızlarda gerçekleştirirsiniz. Birçok iş yükü için uygundur.
Throughput Optimized HDD (st1)
Adından da anlayabileceğimiz gibi bu disk tipinde I/O işlemlerinden ziyade disk okuma hızında bir iyileştirme yapılmıştır. Saniyede 500MB'a kadar okuma performansı sunar, tabi maksimum boyutta depolama kiraladığınızda. Minimum 125 GiB maksimum 16TiB depolama hacmi sunan st1, sıklıkla erişilen ancak fazla değişim olmayan sistemler için idealdir. Veritabanı çalıştırmayan web siteleri veya veri depolamanın (storage) ön planda olduğu sistemlerde de kullanılabilir. Hızdan taviz verebileceğiniz durumlar için $0.045/GB/Ay gibi bir ödeme miktarı ile gp2-gp3 tipi depolama seçeneklerine kıyasla yüzde 40 daha uygun fiyatlı olacaktır. 
Cold HDD (sc1)
Arkaplanda yine HDD ile çalışan sc1, st1'e kıyasla daha yavaş ve daha ucuzdur. Sequential yükler için optimize edilmiş sc1 tipi blok depolama birimlerini $0.015/GB/Ay gibi bir fiyata temin edebilirsiniz.
Konu ile ilgili detaylı bilgiye ve fiyatlandırmaya Amazon dökümanlarından ve EC2 instance oluştururken erişebilirsiniz.