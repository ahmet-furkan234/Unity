
`Mathf.Clamp()`, Unity'de bir sayının belirlediğimiz minimum ve maksimum değerler arasında kalmasını sağlayan fonksiyondur.

Örneğin oyuncunun canının 0'ın altına düşmesini veya 100'ün üzerine çıkmasını engellemek için kullanılabilir.

### 1. Kullanımı

```
Mathf.Clamp(value, min, max);
```

|Parametre|Açıklama|
|---|---|
|`value`|Kontrol edilecek değer|
|`min`|Minimum değer|
|`max`|Maksimum değer|

Örnek:

```
float health = 120f;

health = Mathf.Clamp(health, 0f, 100f);

Debug.Log(health); // 100
```

- Değer 0'dan küçükse → 0 döndürür.
    
- Değer 0–100 arasındaysa → Aynı değeri döndürür.
    
- Değer 100'den büyükse → 100 döndürür.
    

### 2. İnteraktif örnek

Kaydırıcıyı hareket ettirerek `Mathf.Clamp(value, 0, 100)` sonucunu görebilirsin.

Value

## 150

Minimum: 0

Maksimum: 100

Clamp sonucu

# 100

Değer maksimum sınırı aştı, sonuç 100.

### 3. Unity'de hareket sınırlandırma

Örneğin oyuncunun X ekseninde yalnızca -5 ile +5 arasında hareket etmesini istiyoruz.

```
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [SerializeField] float speed = 5f;

    void Update()
    {
        float movement = Input.GetAxis("Horizontal");

        Vector3 position = transform.position;

        position.x += movement * speed * Time.deltaTime;

        position.x = Mathf.Clamp(position.x, -5f, 5f);

        transform.position = position;
    }
}
```

Bu örnekte oyuncu sağa veya sola hareket edebilir ancak X koordinatı hiçbir zaman -5'in altına veya +5'in üzerine çıkamaz.

### 4. Mathf.Clamp ve Mathf.Clamp01 farkı

```
Mathf.Clamp(1.5f, 0f, 1f); // 1

Mathf.Clamp01(1.5f);       // 1
```

`Mathf.Clamp01()` değeri otomatik olarak 0 ile 1 arasında sınırlar.

Özellikle `Lerp`, animasyon geçişleri ve ilerleme çubuklarında kullanışlıdır.

### Notlarına ekleyebileceğin özet

Mathf.Clamp(value, min, max): Bir sayıyı belirlenen minimum ve maksimum değerler arasında sınırlar. Değer sınırları aşarsa en yakın sınır değerini döndürür. Orijinal değişkeni kendiliğinden değiştirmez; sonucu değişkene atamak gerekir.

Mathf.Clamp01(value): Sayıyı yalnızca 0–1 arasında sınırlar.

Önemli: `Mathf.Clamp()` nesneyi doğrudan hareket ettirmez veya durdurmaz. Yalnızca kendisine verilen sayısal değeri sınırlar.