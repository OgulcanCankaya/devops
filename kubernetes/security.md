# Güvenlik

Burada cloud güvenliğine dair kullanılabilen araçlardan veya yöntemlerden bahsetmeyeceğiz. Onun yerine Cloud Native güvenliğinin temel ilkelerinden konuşacağız.

### Güvenliğin 4C'si

Cloud, Cluster, Container ve Code.

Cloud yapılanmamız yani datacenter veya Cloud Provider partisinin kendi güvenliği ilk sıradaki bariyerdir.
Bu bariyerin aşılması halinde Cluster üzerindeki denemeler başlayacaktır. 
AWS, GCP veya Azure kendi güvenlik standartlarını geliştirmiş ve kullanmaktadır.

Cluster üzerinde best-practice olarak düşünülebilecek kavramları ve daha fazlasını [buradan](https://kubernetes.io/docs/concepts/security/overview/) inceleyebilirsiniz.

Konteynerlerinizi deploy sürecine dahil etmeden önce mümkünse Harbor gibi araçlar üzerinde güvenlik açıkları için teste tabi tutun.
Konteyner imajlarınız için kullandığınız base imajlardan son ürüne kadar imzalı (signed) imajlar kullanmaya gayret edin.

Privileged user (root) kullanımını azaltın ve gerekmedikçe root kullanıcısı üzerinden çalışan servisleri kullanmamaya çalışın.

Kod güvenliği de burada büyük bir yer kaplayacaktır. Günümüz sistemlerinde kod güvenliği büyük bir rol almaktadır. Statik kod analizleri ile veya temel ağ güvenliği prensipleri ile bu açıkların farkına varabilirsiniz.

Not: Son derlediğim PHP uygulamasının docker konteyneri üzerinde 600'den fazla güvenlik açığı bulunuyordu.
Node.js üzerinde ise 200'e yakın. Bunların karşılaştırmasını Harbor gibi bir registry sistemi üzerinden veya trivy kullanarak yapabilirsiniz.