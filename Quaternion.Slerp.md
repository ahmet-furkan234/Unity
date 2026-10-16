
`Quaternion.Slerp()`, Unity'de iki dönüş (rotation) arasında küresel interpolasyon yaparak yumuşak bir geçiş sağlayan fonksiyondur.

`Vector3.Lerp()` iki konum arasında geçiş yaparken, `Quaternion.Slerp()` iki dönüş arasında geçiş yapar.

### 1. Kullanımı

```
Quaternion.Slerp(a, b, t);
```

|Parametre|Açıklama|
|---|---|
|`a`|Başlangıç dönüşü|
|`b`|Hedef dönüş|
|`t`|Geçiş oranı (0–1)|

- `t = 0` → Başlangıç dönüşü
    
- `t = 0.5` → İki dönüşün ortası
    
- `t = 1` → Hedef dönüş
    

### 2. İnteraktif örnek

Aşağıdaki kaydırıcıyı hareket ettirerek nesnenin 0° ile 180° arasında nasıl döndüğünü görebilirsin.

Y ekseni etrafındaki dönüşün üstten şematik görünümü.

Geçiş oranı (t)

## 0.00

0° Başlangıç

180° Hedef

`Quaternion.Slerp(start, target, {t.toFixed(2)}f)`

### 3. Unity C# örneği

```
using UnityEngine;

public class RotateObject : MonoBehaviour
{
    [SerializeField] float rotationSpeed = 2f;

    void Update()
    {
        Quaternion targetRotation =
            Quaternion.Euler(0f, 180f, 0f);

        transform.rotation = Quaternion.Slerp(
            transform.rotation,
            targetRotation,
            rotationSpeed * Time.deltaTime
        );
    }
}
```

Nasıl çalışıyor?

1. `Quaternion.Euler(0, 180, 0)` hedef dönüşü oluşturur.
    
2. `transform.rotation` nesnenin mevcut dönüşünü alır.
    
3. `rotationSpeed * Time.deltaTime` geçiş oranını hesaplar.
    
4. `Quaternion.Slerp()` nesneyi hedef dönüşe doğru yaklaştırır.
    

Önemli: Burada başlangıç dönüşü her karede güncellendiğinden nesne hedefe yaklaştıkça yavaşlar. Yani sabit hızla dönmez.

### 4. Belirli sürede dönüş tamamlama

Örneğin nesnenin tam olarak 2 saniyede 180 derece dönmesini istiyorsak:

```
using UnityEngine;

public class RotateObject : MonoBehaviour
{
    Quaternion startRotation;
    Quaternion targetRotation;

    float duration = 2f;
    float elapsedTime = 0f;

    void Start()
    {
        startRotation = transform.rotation;
        targetRotation = Quaternion.Euler(0f, 180f, 0f);
    }

    void Update()
    {
        elapsedTime += Time.deltaTime;

        float t = elapsedTime / duration;

        transform.rotation = Quaternion.Slerp(
            startRotation,
            targetRotation,
            t
        );
    }
}
```

Burada `t`, 2 saniye boyunca 0'dan 1'e ilerler. Böylece dönüş sabit bir interpolasyon oranıyla tamamlanır.

### 5. Quaternion.Lerp ve Quaternion.Slerp farkı

|Özellik|Quaternion.Lerp|Quaternion.Slerp|
|---|---|---|
|İnterpolasyon|Normalize edilmiş doğrusal|Küresel|
|Açısal hız|Genellikle sabit değil|Sabit `t` artışında sabit|
|Performans|Genellikle daha hızlı|Biraz daha maliyetli|
|Kullanım|Basit dönüş geçişleri|Daha tutarlı açısal geçişler|

Her iki fonksiyon da `t` değerini 0–1 arasında sınırlar.

### Notlarına ekleyebileceğin özet

Quaternion.Slerp(a, b, t): İki Quaternion dönüşü arasında küresel interpolasyon yaparak geçiş sağlar.

- `a` → Başlangıç dönüşü.
    
- `b` → Hedef dönüş.
    
- `t` → 0–1 arasında geçiş oranı.
    
- `Time.deltaTime` → Zamana bağlı geçişlerde kullanılır.
    
- `Quaternion.Euler()` → Hedef dönüş oluşturmak için kullanılabilir.
    

Önemli fark: `Vector3.Lerp()` konumlar arasında, `Quaternion.Slerp()` ise dönüşler arasında interpolasyon yapar.

Ek bilgi: Eğer bir nesneyi hedef yöne saniyede belirli bir derece hızla döndürmek istiyorsan `Quaternion.RotateTowards()` daha uygun olabilir.