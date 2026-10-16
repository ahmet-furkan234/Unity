
Signal Receiver, Unity'nin Timeline sistemi içerisinde belirli bir zamana gelindiğinde bir olay (event) tetiklemek için kullanılan bileşendir.

Örneğin geliştirdiğin Galaxy Strike oyununda uzay gemisi Timeline üzerinde belirlediğin rotayı takip ederken belirli bir noktaya geldiğinde düşmanların ortaya çıkmasını, patlama efektinin çalışmasını veya ses çalınmasını sağlayabilirsin.

## 1. Signal Receiver nasıl çalışır?

Signal Emitter

Signal Receiver

UnityEvent

Bu sistemde üç temel kavram bulunur:

- Signal Emitter: Timeline üzerinde sinyalin hangi saniyede gönderileceğini belirler.
    
- Signal Asset: Gönderilen sinyalin kimliğini temsil eder.
    
- Signal Receiver: Sinyali alır ve bağlı UnityEvent fonksiyonlarını çalıştırır.
    

Örneğin Timeline'ın 3. saniyesine bir Signal Emitter koyarsan, oynatma o noktaya ulaştığında ilgili Signal Receiver tetiklenir.

## 2. Signal Receiver nasıl kullanılır?

![Use markers and signals for footsteps | Timeline | 1.8.10](https://images.openai.com/static-rsc-4/wZDVKo5k_wTAQbeUwK6HeZf1rZ_bQFL5JuQEJCKa-EnTSkob6MbZR39Ky3szXriLU9FinRXqXm765IbFm_NtWwX5OyvWZORPlk0q0vVPX8mhsmIgqndqXuHy5l_I7N5nK-8Zq-6bI9GLfFxqyi0ZZ9k8pWUFfTOx0viYJe_pbDI?purpose=inline)

Adım 1 — Signal Track oluştur

Timeline penceresinde `+` veya Track menüsünden Signal Track ekle. Ardından istediğin saniyeye bir Signal Emitter yerleştir.

![Use markers and signals for footsteps | Timeline | 1.8.10](https://images.openai.com/static-rsc-4/4U06jeDPTvy7x0sjpzLCCfvKemHIgJj2-JGIYonENJx8CJ-V1dV8TrCowju2F4n4UBoJXLLwW5B9EVAqK3tHSXIvVcxaCY4tDz9-JraITCYKatQLESMlgSjgG_mmvpPFHuRJBdS4hJmnqZdM9VRZUvgH7O9fDf8PHZlBUB-wXmk?purpose=inline)

Adım 2 — Receiver ayarla

Bir GameObject'e `Signal Receiver` component'i ekle. Signal Track'in binding alanına bu nesneyi bağla. Emitter için Signal Asset oluştur ve Receiver'da aynı sinyale bir Reaction ekle.

Adım 3 — Fonksiyon bağla

Örneğin düşman oluşturacak bir script yazalım:

```
using UnityEngine;

public class EnemySpawner : MonoBehaviour
{
    [SerializeField] private GameObject enemyPrefab;
    [SerializeField] private Transform spawnPoint;

    public void SpawnEnemy()
    {
        Instantiate(
            enemyPrefab,
            spawnPoint.position,
            spawnPoint.rotation
        );
    }
}
```

Ardından:

1. `EnemySpawner` script'ini bir GameObject'e ekle.
    
2. Signal Receiver'ın ilgili Reaction bölümündeki `+` butonuna bas.
    
3. `EnemySpawner` component'ini event alanına bağla.
    
4. `EnemySpawner → SpawnEnemy()` fonksiyonunu seç.
    

Artık Timeline belirlediğin saniyeye geldiğinde `SpawnEnemy()` çalışacak.

## 3. Signal Receiver ile Animation Event arasındaki fark

| Özellik            | Signal Receiver  | Animation Event          |
| ------------------ | ---------------- | ------------------------ |
| Kullanıldığı yer   | Timeline         | Animation Clip           |
| Tetikleme          | Timeline zamanı  | Animasyon zamanı         |
| UnityEvent desteği | Var              | Doğrudan yok             |
| Kullanım amacı     | Sahne olayları   | Animasyona bağlı olaylar |
| Örnek              | Düşman oluşturma | Ayak sesi çalma          |

## 4. Signal Receiver ne zaman kullanılmalı?

Galaxy Strike projen üzerinden düşünelim:

- Gemi rotanın belirli noktasına geldiğinde düşman oluşturmak.
    
- Bir boss savaşı başlamadan önce uyarı sesi çalmak.
    
- Timeline'ın belirli saniyesinde patlama efekti başlatmak.
    
- Sinematik sırasında kamera değiştirmek.
    
- Oyuncunun kontrolünü geçici olarak kapatmak.
    

Önemli: Signal Receiver zaman tabanlı çalışır. Yani doğrudan geminin dünya üzerindeki konumunu kontrol etmez. Geminin belirli bir koordinata ulaşmasıyla olay tetiklemek istiyorsan Trigger Collider veya konum kontrolü daha uygun olabilir.

Bir sonraki önemli konu ise Signal Emitter'ın `Retroactive` ve `Emit Once` ayarlarıdır. Bu ayarlar özellikle Timeline'ı ileri-geri sardığında veya tekrar oynattığında sinyalin nasıl davranacağını belirler.