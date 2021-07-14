### EC2
Termination Protection ön tanımlı olarak kapalı gelmekte.
Depolama tarafında, EBS üzerinde çalışan Root EBS Volume instance kapanırken silinmesi öntanımlı gelmekte.
Default AMI için EBS Root device encrypt edilebilir. 
Ek depolama üniteleri de encrypt edilebilir.

### SecurityGroup
Bütün inbound trafik blokedir ancak outbound trafiğe izin verilir.
Değişimler anlık olarak güncellenir.
Bir SecurityGroup üzerinde birden fazla EC2 instance'ı bulunabilir.
SecurityGroup'lar stateful'dur.
Spesifik IP adreslerini bloklama yapamazsınız, whitelist modunda çalışır. Bunun için NCAL kullanmanız gerekir (Network Access Control List).