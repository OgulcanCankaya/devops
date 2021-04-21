#Storage

Konteynerler üzerinde yaılan sanallaştırma sonrasında konteynerler özelinde farklı dosyalar ve dosya sistemleri
ile çalışabilirsiniz. Bu verilerin sürekliliğini ise docker üzerinde konteyner'e volume tanımlaması yaparak sağlıyorduk. 

Kubernetes üzerinde tutmak istediğiniz veriler işlem hızı ve sürekliliği için öncelikle etcd servisinin 
geçici depolama alanında tutulur.

Bu yüzdendir ki bir veritabanı pod'u hayatının sonuna geldiğinde veya çöktüğünde eğer persistent volume (pv) tanımlamamışsak
kubelet servisi konteyneri yeniden başlatacak ve büyük ihtimalle verilerimiz de önceki pod ile birlikte kaybolmuş olacaktır.

Bu tarz durumlar yaşamamak adına, pod'ların verilerini tutabilmeleri ve kararlı veri bloklarını saklayabilmeleri için 
persistent volume yapısına ihtiyaç bulunmaktadır.

###Persistent Volume nedir ? Nasıl istenir ? Kıtlanır mı ?

Persistent volume nedir sorusuna verilebilecek en sade yanıt ise: kubernetes cluster'ı içerisindeki pod'larınızın verilerinin
saklanması için kullanabileceğiniz fiziki alanlardır.

Persistent volume yapısı sistem yöneticisi veya dinamik alan yöneticisi tarafından oluşturulmuş olabilir ve amacı bir node 
üzerinde belirli bir pod için bir alan oluşturmak ve bu pod'un verilerinin kararlılığını sağlamaktır.

PV çalışma sistemi düşünüldüğünde basit birer kaynak olarak düşünülebilirler ve bu basit kaynakların kullanımı söz konusu olduğunda 
PV claim (PVC) devreye girer. Belirli bir persistent volume, belirli bir boyuttaki alanı kullanıcının erişimine açacaktır. 
Kullanıma açılan disk alanının kullanılabilmesi ve bir pod'a bağlanabilmesi için bir PVC'nin bu PV üzerinde hak istemesi gerekmekte ve
bu istek ve kaynağın bağlanması halinde bu isteğin kullanılacak olan pod üzerinde işletilmesi gerekmektedir.

Basit bir PV tanımı

```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:
    storage: 5Gi
  volumeMode: Filesystem
  accessModes:
    - ReadWriteOnce
  persistentVolumeReclaimPolicy: Recycle
  storageClassName: slow
```

Burada gösterilen PersistentVolume tanımı bizler için bir node üzerinde 5GB boyutunda bir alan oluşturacak ve adını pv0003 olarak belirleyecektir.
Burada dikkat edilmesi gerekn bir nokta ise sec.storageClassName alanı olmalıdır. Burada storage class oluşturulacak alanın yönetimini sağlayacak ve
gelen PVC isteklerine göre alanın verilmesini veya verilmemesini sağlayacaktır.

###PersistentVolumeClaim 

PersistentVolumeClaim'in temel işlevine biraz önce değinmiştim. PVC'lerin temel fonksiyonunun işleyebilmesi için gerekli birkaç bilgi vardır.
Statik alan yönetimi söz konusu olduğunda PVC'nin bağlanabilmesi için bir PV yaratılmış olmalıdır. Bu alan sistemin yöneticisi tarafından yaratılır.

Basit bir PVC örneği
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: foo-pvc
spec:
  storageClassName: "" # Empty string must be explicitly set otherwise default StorageClass will be set
  volumeName: pv0003
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 200Mi
```
Buradaki istek pv0003 adındaki PV alanına bağlanacaktır. Belirtilen isimde bir PV'nin hazırda bulunmaması halinde ise istek yine de yaratılacak ancak 
belirtilen isimdeki alan oluşturulana kadar bekletilecektir. Oluşturulan alanın büyükliğinin, isteğin büyüklüğünden fazla veya eşit olması halinde
istek, alana bağlanacaktır.
Peki, belirli bir pv adı vermeden bu istekleri nasıl bağlayabiliriz? Bu konuda storageClass'ları incelemek yararlı olacaktır.

Nitekim isteğin alana bağlanması kubernetes tarafından gerçekleştirilse de istek için hazırda bir alan olmaması halinde, dinamik olarak yönetilebilen 
bir storageClass çok işimize yarayacaktır.

###StorageClass

StorageClass'lar belirli provisioner'lar etkisi ile dinamik olarak alan yönetiminde veya alanların gruplanmasında kullanılabilirler.


*Bu dökümantasyonu yerel alanlar kullanarak hazırladığım ve cloud üzerinde çalışan servisler üzerinde bu yapılandırmalara aşina olmadığım için *
*local dinamic provisioner ve storagePool kullanarak devam edeceğim.*

OpenEBS lokal dinamik alan yöneticisinin yönetimindeki storageClass tanımı

'''
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: openebs-cstor
  annotations:
    openebs.io/cas-type: cstor
    cas.openebs.io/config: |
      - name: StoragePoolClaim
        value: "cstor-disk-pool"
      - name: ReplicaCount
        value: "1"
    storageclass.kubernetes.io/is-default-class: "true"
provisioner: openebs.io/provisioner-iscsi
```
