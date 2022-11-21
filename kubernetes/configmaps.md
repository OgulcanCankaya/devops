Öncelikle ConfigMap yapısının bir konfigürasyon yönetim yapısını olduğunu anlamakta fayda var düşüncesindeyim. 
Secret yapısı gibi gizlenmesi gereken bilgiler için değil, konfigürasyonlar için kullanılmaktadır.
Bir ConfigMap aşağıdaki gibi tanımlana bilir.
    
```
apiVersion: v1
kind: ConfigMap
metadata:
  name: game-demo
data:
  # property-like keys; each key maps to a simple value
  player_initial_lives: "3"
  ui_properties_file_name: "user-interface.properties"

  # file-like keys
  game.properties: |
    enemy.types=aliens,monsters
    player.maximum-lives=5    
  user-interface.properties: |
    color.good=purple
    color.bad=yellow
    allow.textmode=true    
immutable: true/false
```
Görüldüğü gibi bu dosyada var olan bilgiler gizlenmesi gerekmeyen, servis kullanıcısının bilmesinde sakınca olmayan bilgiler. 
Aynı şekilde, burada verilen konfigürasyonların belirteç niteliğinde olduğuna da dikkat çekmek istiyorum. 
Burada bu kadar tekrar yapmamın sebebi ConfigMap'leri secret olarak kullanmanız halinde RBAC kullanılmaması halinde,
gizli tutulması gereken bilgileri diğer CKD/CKA'lar ile paylaşıyor olabilirsiniz.

Peki bir Pod bu konfigürasyonlara nasıl erişiyor? Aşağıdaki Pod tanımına bir bakalım.

```
apiVersion: v1
kind: Pod
metadata:
  name: configmap-demo-pod
spec:
  containers:
    - name: configmap-demo
      image: alpine
      command: ["sleep", "3600"]
      env:
        # Define the environment variable
        - name: PLAYER_INITIAL_LIVES # Notice that the case is different here
                                     # from the key name in the ConfigMap.
          valueFrom:
            configMapKeyRef:
              name: game-demo           # The ConfigMap this value comes from.
              key: player_initial_lives # The key to fetch.
        - name: UI_PROPERTIES_FILE_NAME
          valueFrom:
            configMapKeyRef:
              name: game-demo
              key: ui_properties_file_name
      volumeMounts:
      - name: config
        mountPath: "/config"
        readOnly: true
  volumes:
  # You set volumes at the Pod level, then mount them into containers inside that Pod
  - name: config
    configMap:
      # Provide the name of the ConfigMap you want to mount.
      name: game-demo
      # An array of keys from the ConfigMap to create as files
      items:
      - key: "game.properties"
        path: "game.properties"
      - key: "user-interface.properties"
        path: "user-interface.properties"

```
Burada bir alpine linux konteynerine 1 saat için süre tanınarak test yapma işlemi kolaylaşıyor.Eğer bu dosyaları apply ettiyseniz, 
> kubectl exec --stdin --tty configmap-demo -- /bin/bash 
komutu ile erişebilir ve > env
komutu yardımı ile environment variables listesini görebiliyo olmalısınız.


Peki bu konfigürasyonlar illa da çevre değişkeni olarak mı tanımlanmalı ? Tabi ki de hayır.
Bir konfigürasyonu bir dosya olarak da kullanmak isteyebiliriz. 
Burada dikkat edilmesi gereken nokta ise mountPath'in doğru verilmesi ve gerekiyorsa var olan konfigürasyou ezmesi.
Mounted olan ConfigMap'ler değiştirildikten sonra otomatik güncellenir ve Pod restart işlemine gerek duymazlar. 
Tabi buradaki güncelleme işlemi kubelet servisinin "Up-to-date check" işleminin periyoduna ve işlem süresine bağlıdır. 

```
apiVersion: v1
kind: Pod
metadata:
  name: mypod
spec:
  containers:
  - name: mypod
    image: redis
    volumeMounts:
    - name: foo
      mountPath: "/etc/foo"
      readOnly: true
  volumes:
  - name: foo
    configMap:
      name: myconfigmap
```
Peki ConfigMap objeleri her zaman Pod'lar için mi kullanılır? 
Hayır, bazı kubernetes addon'ları veya operatörleri de konfigürasyon yönetimi için ConfigMap kullanabilir.

