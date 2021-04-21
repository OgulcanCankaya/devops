#Servisler

Servisler uygulamaların "abstract" bir biçimde dış dünyaya suulması için geliştirilmiş bir yapıdır.
Kubernetes çalıştırdığınız uygulama podlarına kendi IP adresslerini vermekte ve bu IP adresleri üzerinden
uygulamanızın sunumunu yapabilmektedir. Bu yapı tipine gereksinimin nasıl doğduğu konusu ise pod yaşam döngüsüne
bakıldığında rahatlıkla anlaşılabilir.

Bir Deployment sonrasında ortaya çıkan podlar güncellendiğinde veya yeniden başlatıldığında farklı isimlerde veya 
farklı makinelerde bulunabilirler. Böyle bir durum söz konusu olduğunda değişen pod isimlerinin her biri için ayrı
network tanımları veya yapılandırmaları yapmanız gerekir. Böylesi bir iş yükünün ortadan kaldırılması için ise 
servisler ile çalışabiliriz.

##Basit servis tanımı

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376
```
Veya birden fazla portu bağlamak istediğinizde, docker üzerinde de yaptığınız gibi

```
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 9376
    - name: https
      protocol: TCP
      port: 443
      targetPort: 9377
```

Burada görebildiğiniz gibi belirli bir `app=MyApp` tanımına sahip olan bir pod,
bu servisin etki alanına girecek ve bu servis, MyApp'in 9376. Portunu TCP ile hedef alacaktır.
Burada dikkat edilmesi gereken bir detay nokta ise Endpoint tanımlarıdır.

selector.app tanımına sahip olan bir servisin Endpoint adresi bu tanıma uyan pod olacaktır şeklinde düşünülebilir.
Ancak bu tanımı kullanmamamız halinde Endpoint bağlama işlemini manuel olarak yapmamız gerekecektir.

```
apiVersion: v1
kind: Endpoints
metadata:
  name: my-service
subsets:
  - addresses:
      - ip: 192.0.2.42
    ports:
      - port: 9376
```
Şeklinde bir tanımlama ile servisimizi istediğimiz pod'a bağlamamız gerekecektir.

_ Bazen servislere olan ihtiyacımız uygulamamızı dünyaya açmak değil, uygulamamızın başka servislerle konuşmasını kolaylaştımak olabilir. (Bknz: ClusterIP) _

##Servisleri sunmak

###ClusterIP servis tipi

Bu servis tipi cluster içi erişimi kolaylaştıracak ve servis için, cluster içinde kullanılmak üzere bir IP adresi atayacaktır.
Servis tipleri içerisinde öntanımlı olarak eklenen servis tipi ClusterIP olarak belirlenmiştir. Nitekim, aksi belirtilmedikçe 
bir uygulamanın dışarı açılmasına gereksinim yoktur.

Bu servislere IP adresi ataması kubernetes engine tarafından otomatik olarak atanabilir.

```
apiVersion: v1
kind: Service
metadata:
  name: "myapp-service"
  namespace: "namespace"
spec:
  ports:
  - name: appPort
    port: 8065
    protocol: TCP
    targetPort: http
  selector:
    app: "myapp"
  type: ClusterIP
```

###Nodeport servis tipi

yml dosyası
```
apiVersion: v1
kind: Service
metadata:
  name: svc-nodeport-httpd
spec:
  type: NodePort
  ports:
  - port: 3050
    targetPort: 80
    nodePort: 31000
  selector:
    app: apache_webserver
```

NodePort servisleri docker üzerinde çalışan konteynerlerde olduğu gibi pod'un portu ile çalışan makinenin
fiziksel portu üzerinde oluşturulan bir port köprüsü şeklinde çalışırlar.
Burada dikkat edilebilecek naçizane nokta NodePort olarak belirlenmiş bir servisin çalışan makinenin
X portuna bağlanması makinenin X portunu kilitleyecektir. Bu yüzden 30000-32767 (default) arası portlarla sınırlı tutulmaya çalışılmalıdır.

NodePort servislerinin ise güzel yanı isteklerimizin nodeIP:nodePort şeklinde kullanılabilir olması ve basit araçlarla (wget, curl, vb.) test edilebilmesidir.

