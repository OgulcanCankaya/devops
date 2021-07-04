# Güvenlik

Burada cloud güvenliğine dair kullanılabilen araçlardan veya yöntemlerden bahsetmeyeceğiz. Onun yerine Cloud Native güvenliğinin temel ilkelerinden konuşacağız.

### Güvenliğin 4C'si

Cloud, Cluster, Container ve Code.

Cloud yapılanmamız yani datacenter veya Cloud Provider partisinin kendi güvenliği ilk sıradaki bariyerdir.
Bu bariyerin aşılması halinde Cluster üzerindeki denemeler başlayacaktır. 
AWS, GCP veya Azure kendi güvenlik standartlarını geliştirmiş ve kullanmaktadır.

Cluster üzerinde best-practice olarak düşünülebilecek kavramları [buradan](https://kubernetes.io/docs/concepts/security/overview/) görebilirsiniz.
