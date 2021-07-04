#Healtcheck

Pod içerisindeki konteynerlerin ve Pod'un genel urumunun kontrol edilmesi amacıyla kullanılan ve 
    pod veya konteyner hatalarına hızlı çözüm üretmemizi sağlayan yapılardır. 
Pod yaşamı esnasında ve öncesinde bu kontrolleri gerçekleştirerek pod'un sahip olması gereken 
    fonksiyonalitenin kaybolmamasından emin olabiliriz.
İlk olarak Pod yaşam süresi esnasında kullanılan livenessProbe'a bakalım.

###LivenessProbe

Liveness Probe adından da anlaşılabileceği gibi podun canlılığını test eder. Bir pod'un 
fonksiyonel olup olmadığının bilinmesi gerekli rollback veya upgrade işlemleri için önemli olacaktır.

Liveness Probe pod'un canlılığını test etmek için komutlar çalıştırabilir ve sonucuna bağlı olarak 
    bir zamanlayıcı ile ya hata bildirir ya da bekleme süresine girer.

Basit bir liveness probe detayı aşağıdaki pod tanımında yer almaktadır.

```
apiVersion: v1
kind: Pod
metadata:
  labels:
    test: liveness
  name: liveness-exec
spec:
  containers:
  - name: liveness
    image: k8s.gcr.io/busybox
    args:
    - /bin/sh
    - -c
    - touch /tmp/healthy; sleep 30; rm -rf /tmp/healthy; sleep 600
    livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
```
Buradaki detayda görebileceğimiz üzere healthy dosyası var olduğu sürece liveness probe hata vermeyecektir.
Busybox konfigürasyonunda ise 30 saniye sonra silinecek olan dosyayı görebiliyorsunuz.
Yaklaşık 35 saniye sonra hata vererek durdurulacak olan bu pod'un konteyner bazlı liveness probe sahibi olduğunu görebilirsiniz.

###ReadinessProbe

Liveness probe ile aralarındaki tek farklılık readinessProbe işleminin konteynerin doğru başlayıp başlamadığını kontrol etmesidir.
Bazen farklı servislerin kullanımı söz konusu olduğunda veya konteynerin başlama süresinde  çok fazla işlem yapıldığında,
    bu süre zarfında uygulamayı sağlıklı bir başlama işlemi için tabiri caiz ise karantinada tutmak isteyebilirsiniz.
Uygulamanıza istek göndermezsiniz ancak başka servislere de bağlamazsınız.

Bu tarz durumlarda konteynerin/pod'un tam operasyonel kapasiteye çıktığını test edebilmek için yardımınıza readynessProbe koşabilir.

Yukarıdaki konfigürasyon ile farkı `livenessProbe` yazan yerde artık `readinessProbe` yazıyor. 

```
readinessProbe:
  exec:
    command:
    - cat
    - /tmp/healthy
  initialDelaySeconds: 5
  periodSeconds: 5
```

Pod'un devreye girmesinden 5 saniye sonra healthy dosyası okunmaya çalışılacaktır. Dosyanın var olması halinde ready state'e geçiş yapılabilir.


###Dipnotlar

Yukarıdaki komut kullanımı dışında Http istekleri atabilir veya eski tip uygulamalar ile çalışıyorsanız liveness veya readiness probelarını bekletmek isteyebilirsiniz.
startupProbe kullanımı bunu sizin için halledebilir.

```
livenessProbe:
  httpGet:
    path: /healthz
    port: liveness-port
  failureThreshold: 1
  periodSeconds: 10
```
Daha önceden belirtildiği şekilde http istekleri atmak ve hata sınırlaması koyarak belirli sayıdan hatanın ardından pod'u devre dışı bırakmak veya alarm verdirmek mümkündür.
