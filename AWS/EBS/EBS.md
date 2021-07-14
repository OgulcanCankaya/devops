# EBS
EBS (Elastic Block Store) bizlere EC2 kullanımı sırasında yüksek performanslı blok depolama hizmeti sunmak için tasarlanmış Amazon servisidir.
Her bir EBS, tekrar kullanılabilir bir şekilde EC2 instance'ının bulunduğu AvailabilityZone üzerinde yedeklenir ve bu şekilde bizlere HighAvailability ve veri dayanıklılığı sağlar.

EBS servisini kullanarak verilerinizin Snapshot'ını alabilir ve verinin güvvenliği, bütünlüğü ve yedeklenmesi aşamalarında bu snapshot'ı kullanabilirsiniz. Disaster Recovery veya backup gibi sebeplerle de kullanılabileceği gibi AWS üzerinde veri taşınması sırasında da kullanılabilir ve farklı AvailabilityZone'lar ve hesaplar üzerinde geçişler yaparak data migration işlemlerinde de kolaylık sağlayabilir.

EBS iş yükünüze veya çalıştırdığınız servislere bağlı olarak kullanabileceğiniz farklı seçenekler sunabilir. SSD tabanlı veritabanı ve önyükleme işlemlerini destekleyen depolama seçenekleri ve günlük işlemler için rahatlıkla kullanılabilir HDD destekli depolama seçenekleri sunulabilir. Bu EBS birim türlerine bir bakalım...

EBS Provisioned IOPS SSD (io2 Block Express)
Kritik ve anlık veri girdisi gerektiren işlemler için geliştirilmiş olan io2 EBS birim tipinin geliştirilmiş halidir ve milisaniyenin altında gecikme sunar. Burada yaklaşık olarak saniyede 4GB'a varan veri I/O işlemlerinden bahsedebiliriz. Kullanım amacına bağlı olarak kritik işlemlerde veya ilişkisel veritabanlarının kullanımında rahatlıkla kullanılabilirler. En az 4GB en fazla ise 64TB boyutlarında kullanabileceğiniz io2 Block Express zaman açısından kritik işlemlerin varlığında rahatlıkla fayda sağlayacaktır.

Block Express'ten bahsettikten sonra, EBS Provisioned IOPS SSD (io2) ve EBS Provisioned IOPS SSD (io1)'den bahsedecek olursak, maksimum kullanım boyutları farklarından ve milisaniye cinsinden tepki sürelerinde yaşanan minimal farklar bulunmaktadır.
