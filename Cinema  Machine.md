TkMatE, Unity’de **Cinemachine**, oyun kameralarını daha kolay ve profesyonel şekilde yönetmek için kullanılan bir kamera sistemidir.

Normalde Unity’de kamerayı karakteri takip ettirmek, yumuşak hareket ettirmek, hedefe baktırmak veya kamera geçişleri yapmak için bunların çoğunu kendin kodlarsın. **Cinemachine bu işleri hazır sistemlerle çözer.**

### Temel mantığı

Şöyle düşünebilirsin:

```
Oyuncu
   ↓
Cinemachine
   ↓
Main Camera
   ↓
Ekranda gördüğün görüntü
```

Cinemachine genellikle gerçek kamerayı sürekli elle hareket ettirmek yerine, **kameranın nasıl davranması gerektiğini tanımlar**.

Örneğin karakterin var:

```
Player
Position: (10, 0, 5)
```

Sen kameranın:

- Player'ı takip etmesini,
- biraz yukarıdan ve arkadan durmasını,
- karakter hareket ederken yumuşakça yetişmesini,
- mouse ile karakterin çevresinde dönmesini

isteyebilirsin.

Bunları sıfırdan yapmak için kabaca şöyle sistemler yazman gerekebilir:

```
cameraPosition = playerPosition + offset;
```

ardından yumuşatma:

```
Vector3.Lerp(...)
```

dönüş:

```
Quaternion.LookRotation(...)
```

mouse kontrolü, çarpışma kontrolü vs. eklenir.

Cinemachine bunların önemli bir kısmını hazır sunar.

### Cinemachine ile neler yapılabilir?

En yaygın kullanım **oyuncu takip kamerasıdır**:

```
        Camera
          \
           \
          Player →
```

Karakter ilerledikçe kamera otomatik olarak onu takip eder.

Ayrıca hedefe odaklanan kameralar oluşturabilirsin:

```
Camera ─────────→ Enemy
```

Ya da farklı kamera açıları arasında geçiş yapabilirsin:

```
Normal Camera
      ↓
Boss alanına gir
      ↓
Boss Camera
      ↓
Savaş başlasın
      ↓
Normal Camera
```

Cinemachine geçişi doğrudan sert şekilde yapmak yerine **blend** ile yumuşatabilir.

Örneğin:

```
Camera A ─────────────→ Camera B
          2 saniye
```

Bu özellikle cutscene ve sinematik sahnelerde çok kullanışlıdır.

### Cinemachine 3'te önemli kavramlar

Yeni Unity/Cinemachine sürümlerinde özellikle şu kavramlarla karşılaşacaksın:

**Cinemachine Camera:** Kameranın nasıl davranacağını tanımlar. Takip edeceği nesne, bakacağı hedef, pozisyon davranışı gibi ayarlar burada bulunur.

**Tracking Target:** Genellikle Player gibi kameranın takip edeceği nesnedir.

```
Cinemachine Camera
       │
       │ Tracking Target
       ↓
     Player
```

**Cinemachine Brain:** Cinemachine ile gerçek Unity Camera arasındaki yönetici gibidir. Hangi Cinemachine Camera'nın aktif olacağına ve kameralar arasındaki geçişlere yardım eder.

```
Cinemachine Camera 1 ─┐
Cinemachine Camera 2 ─┼──→ Cinemachine Brain → Main Camera
Cinemachine Camera 3 ─┘
```

Yani **Cinemachine Camera = kamera davranışını tarif eder**, **Main Camera = görüntüyü gerçekten render eder** diye düşünebilirsin.

### Neden önemli?

Örneğin üçüncü şahıs bir oyun yapıyorsan:

```
          CAMERA
            ↓

           👤 Player
            ↑
      kamera takip eder
```

Cinemachine sayesinde kamera takibi, dönüş, smoothing/damping, hedefleme, kamera geçişleri ve kamera çarpışması gibi konularda çok daha az kod yazarsın.

**Kısaca notuna şöyle yazabilirsin:**

> **Cinemachine:** Unity'de kamera hareketlerini ve davranışlarını yönetmeyi kolaylaştıran sistemdir. Oyuncu takibi, hedefe bakma, yumuşak kamera hareketleri, üçüncü şahıs kamera sistemleri ve kameralar arası sinematik geçişler gibi işlemleri hazır bileşenlerle gerçekleştirmeyi sağlar.

Bir sonraki önemli konu **Cinemachine Camera + Tracking Target + Position Control + Rotation Control + Damping** ilişkisi. Bunları öğrendiğinde Cinemachine'in çalışma mantığı büyük ölçüde oturuyor.

Cinemachine’i uygulamalı öğren

- Unity’de Cinemachine kamera kurulumunu göster
- Cinemachine kavramlarını karşılaştır