
Unity'de Particle System, oyunlarda ateş, duman, patlama, yağmur, kar, kıvılcım, büyü efektleri ve toz gibi görsel efektler oluşturmak için kullanılan bir sistemdir.

Particle System sayesinde yüzlerce veya binlerce küçük parçacığı tek tek GameObject oluşturmadan yönetebiliriz.

![テクニカルアーティスト	馬原伸夫の基礎から企業ポートフォリオまで学ぶ、ロジカルな四大属性ゲームエフェクト | コロソ.](https://images.openai.com/static-rsc-4/NbmZHNSszZJkGdfFJyL2SJQS5XlltmI_ACyFQTDCxSAY34iNswID7JTMnpq-NTKSzKMLDJBMU0fzY9Bh8qf_g0H749mdUNaRvb7ERJVmMs2BTYDWe9qEFiVnQj-w77y8vXbA8aSG1zPD6GPJZbhjo5u7KH2XCuTabt5a3s7lvzY?purpose=inline)

![UNI VFX: Realistic Explosions, Fire & Smoke for Visual Effect Graph](https://images.openai.com/static-rsc-4/r9c3hRAC09lolsEPnPboBfFup5HqGMFvJ1I1cF3cBleQHH-3-3cH2RPi4nXOV71PdV9X0iWHgw27wIQf3VpFnzBDIEKUOWdzKC0IrtyK94lPr6cqQWEFIlPjFNH_i2Y_HG-j-VeThYimYvQ-TINuyqDs7poRyO8wS70KYb9rCU4?purpose=inline)

![Volumetric Smoke Shader - Page 13 - Community Showcases - Unity Discussions](https://images.openai.com/static-rsc-4/NkaT5ChPia0asvMxcLA9zNcbPOOPS9H7xF9pv9Z_7TqZr-IywL5b2DNIgW1vCf847PCBdXksMtT5myRIfLldYzHK2FwnV6-7a9HttRus1t7iqHxATsn7oQU-xQ2Uf7VnFbgREUSbvZBG1S_LoD90Vt0qobAYdajU07WOWXi-MSw?purpose=inline)

12

## 1. Particle System nasıl çalışır?

Particle System, belirli kurallara göre parçacık üretir. Her parçacığın kendine ait özellikleri bulunabilir:

- Position: Parçacığın konumu.
    
- Velocity: Hareket hızı ve yönü.
    
- Lifetime: Ne kadar yaşayacağı.
    
- Size: Büyüklüğü.
    
- Color: Rengi.
    
- Rotation: Dönüş açısı.
    

Örneğin bir ateş efekti oluşturduğumuzda parçacıklar aşağıdaki aşamalardan geçer:

Parçacık doğar → hareket eder → boyutu/rengi değişir → ömrü biter ve kaybolur.

## 2. Particle System oluşturma

Unity içerisinde:

Hierarchy → Sağ tık → Effects → Particle System

Yeni bir Particle System oluşturduğunda Inspector panelinde birçok modül göreceksin.

## 3. Particle System temel ayarları

### Main modülü

Particle System'in temel davranışlarını belirler.

|Özellik|Açıklama|
|---|---|
|Duration|Efektin bir döngüsünün süresi|
|Looping|Efektin sürekli tekrarlanması|
|Start Lifetime|Parçacıkların yaşam süresi|
|Start Speed|İlk hareket hızı|
|Start Size|İlk büyüklüğü|
|Start Color|Başlangıç rengi|
|Gravity Modifier|Yer çekiminin etkisi|
|Simulation Space|Parçacıkların hangi koordinat sisteminde hareket edeceği|
|Max Particles|Aynı anda bulunabilecek maksimum parçacık sayısı|

Örnek: `Start Lifetime = 3` verirsek her parçacık doğduktan yaklaşık 3 saniye sonra yok olur.

### Emission modülü

Parçacıkların ne sıklıkla üretileceğini belirler.

- Rate over Time: Saniyede üretilecek parçacık sayısı.
    
- Rate over Distance: Hareket edilen mesafeye göre üretim.
    
- Bursts: Belirli bir anda toplu parçacık üretimi.
    

Örneğin `Rate over Time = 20` ayarlarsak saniyede 20 parçacık üretilir.

### Shape modülü

Parçacıkların nereden ve hangi şekle göre üretileceğini belirler.

![Introduction to Particle Systems - Unity Learn](https://images.openai.com/static-rsc-4/MWhIqN3KSStP2LsmSpTQc5egIPCC11yR2PctzdcU5fpuCLdO22hhFxftrhVDUyX3tytG1jMVxHyBtMQbeTW3OjPsSpgxCyOm1xdG2SHAfNaYvhW7MwKIXQn7GMi4d5ntHe4BLoFYKpwawLMixGC2BFmY58SRny91L2oNN4B64Cg?purpose=inline)

Cone

Koni

![Surface Flow](https://images.openai.com/static-rsc-4/DjfQSyPfK1DBmtSqoeynGO1MS8h0ci-iu6PPD9P7OkndXtrKhQppPrViz9kIT3SkBXbTj97TpGbVq-ocNmEVjPo1eUiMMDuVIY9Gy9jeO1iOWbhds7f5l1Li9l-zDdp78K0Uy-kE_9Sq57etqP7ePpW9Mhwbtccz4cLO4uHxUZA?purpose=inline)

Sphere

Küre

![Create an environmental Particle System - Unity Learn](https://images.openai.com/static-rsc-4/m7YX1xHrFFQ9l0-OsAny2uXJukOgFxejX6Y9Ksx0pJ-1zAzh4ZO93d9wGx2pPFpglxbIANsLO76h3rrmuSHZL_b4zz9SWc5e9AOIthUTbm3tt8KxKmUZGXz8eSMaL7JfBfvh4me3cLihbCqrmZHIFo3NJ8Z-mAUAHx7__LeDdKc?purpose=inline)

Box

Kutu

### Diğer önemli modüller

|Modül|Görevi|
|---|---|
|Velocity over Lifetime|Yaşam süresince hızı değiştirir|
|Color over Lifetime|Zamanla rengini değiştirir|
|Size over Lifetime|Zamanla büyütür veya küçültür|
|Rotation over Lifetime|Parçacıkları döndürür|
|Noise|Rastgele ve doğal hareket sağlar|
|Collision|Parçacıkların yüzeylerle çarpışmasını sağlar|
|Renderer|Parçacıkların nasıl çizileceğini belirler|

## 4. Particle System'i C# ile kontrol etmek

Particle System'i kod üzerinden başlatabilir, durdurabilir ve temizleyebiliriz.

```
using UnityEngine;

public class ParticleController : MonoBehaviour
{
    [SerializeField]
    private ParticleSystem particle;

    void Start()
    {
        particle.Play();
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            particle.Stop();
        }
    }
}
```

Bu örnekte eski Input API kullanılıyor. Projende yeni Input System etkinse aşağıdaki örnek daha uygun:

```
using UnityEngine;
using UnityEngine.InputSystem;

public class ParticleController : MonoBehaviour
{
    [SerializeField]
    private ParticleSystem particle;

    void Update()
    {
        if (Keyboard.current.spaceKey.wasPressedThisFrame)
        {
            particle.Play();
        }

        if (Keyboard.current.sKey.wasPressedThisFrame)
        {
            particle.Stop();
        }
    }
}
```

Inspector üzerinden `particle` alanına Particle System bileşeni olan nesneyi bağlaman gerekir.

## 5. Particle System metotları

|Metot|Açıklama|
|---|---|
|`Play()`|Parçacık üretimini başlatır|
|`Stop()`|Parçacık üretimini durdurur|
|`Pause()`|Sistemi duraklatır|
|`Clear()`|Mevcut parçacıkları temizler|
|`IsAlive()`|Sistemde yaşayan parçacık veya devam eden üretim olup olmadığını kontrol eder|
|`Emit(10)`|Anında 10 parçacık üretir|

Önemli: `Stop()` varsayılan olarak yeni parçacık üretimini durdurur; mevcut parçacıklar yaşam süreleri bitene kadar kalabilir.

Mevcut parçacıkları da hemen kaldırmak için:

```
particle.Stop(
    true,
    ParticleSystemStopBehavior.StopEmittingAndClear
);
```

## 6. Örnek: Karakter öldüğünde patlama efekti

```
using UnityEngine;

public class PlayerHealth : MonoBehaviour
{
    [SerializeField]
    private ParticleSystem deathEffect;

    public void Die()
    {
        ParticleSystem effect = Instantiate(
            deathEffect,
            transform.position,
            Quaternion.identity
        );

        effect.Play();

        Destroy(gameObject);
    }
}
```

Burada:

1. `Instantiate()` ile patlama efektinin bir kopyası oluşturulur.
    
2. `transform.position` sayesinde efekt karakterin konumunda oluşur.
    
3. `Play()` efekti başlatır.
    
4. `Destroy(gameObject)` karakteri yok eder.
    

Efekt bağımsız bir nesne olarak oluşturulduğu için karakter silinse bile çalışmaya devam edebilir. Ancak efektin de sonradan temizlenmesi gerekir; bunun için `Stop Action = Destroy` ve `Looping = false` kullanılabilir.

## 7. Performans açısından dikkat edilmesi gerekenler

Particle System kullanırken özellikle mobil oyunlarda:

- Gereksiz yüksek `Max Particles` değerlerinden kaçın.
    
- Çok sayıda transparan parçacığın üst üste çizilmesini sınırla.
    
- Sürekli çalışan efektler yerine gerektiğinde `Play()` kullan.
    
- Gerekmiyorsa Collision modülünü kapat.
    
- Aynı efektleri sık oluşturuyorsan Object Pooling kullan.
    

## 8. Mini uygulama: Patlama efekti

Patlama parametreleri

Örnek ayarlar

Start Lifetime

2.0 sn

Start Speed

10

Burst Count

70

Bu değerlerle Unity Inspector üzerinden bir patlama efekti oluşturabilirsin. Emission içindeki Rate over Time değerini 0, Burst sayısını 70, Start Lifetime değerini 2.0 ve Start Speed değerini 10 yap. Looping özelliğini kapat.

Ayarları kopyala

Kısacası: Particle System görsel efektlerin parçacık üretimini ve davranışını yönetir. `Main` temel özellikleri, `Emission` üretim miktarını, `Shape` üretim bölgesini, diğer modüller ise parçacıkların yaşamları boyunca nasıl davranacağını belirler.

Bir sonraki önemli konu Particle System ile VFX Graph arasındaki farktır. Particle System genellikle daha basit ve CPU tabanlı efektlerde kullanılırken VFX Graph, GPU tabanlı yüksek parçacık sayılı efektler için tasarlanmıştır.