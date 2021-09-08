# AMI

Amazon Machine Images (AMI), bizlere EC2 instance'ları için, launch işlemi sırasında kullanılacak bilgileri sağlar. Bir instance oluştururken bir AMI belirttiğimizi ve bu AMI'a bağlı olarak Centos/Debian/Windows vb. temelli EC2 instance'ları oluşturabildiğimizi hatırlarsınız. Örnek olarak bunu kullanabilir sanırım. Farklı gereksinimlerimiz için bu şekilde farklı AMI'lar kullanabiliriz.

Peki AMI'lar bünyelerinde ne bulundurur? 
Bir AMI, 
- Bir EBS snapshot'ı veya root volume template'i (instance-store-backed AMI),
- Spesifik AMI instance'ını kullanabilecek kullanıcıların listesi ve izin listeleri,
- Launch işlemi sonrasında oluşan instance'a bağlanacak Volume'lerin listesini barındırır.

Peki bir AMI satın alabilir, oluşturabilir veya satabilir miyiz ?

Bütün bu soruların cevabı evet. 