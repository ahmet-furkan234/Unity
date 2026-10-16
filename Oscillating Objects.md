
Oscillating Objects, Unity'de bir nesnenin belirli iki konum arasında veya bir merkez etrafında sürekli ileri-geri hareket etmesidir.

Örneğin:

- Sağa ve sola hareket eden platformlar.
    
- Yukarı ve aşağı hareket eden asansörler.
    
- İleri geri hareket eden tuzaklar.
    
- Havada süzülen nesneler.
    

## 1. Oscillation nasıl çalışır?

Nesne merkez konumunun iki tarafında sürekli hareket eder.

Unity'de bunu yapmanın birkaç yolu vardır. En yaygın yöntemlerden biri `Mathf.Sin()` kullanmaktır.

## 2. Mathf.Sin() ile salınım

`Mathf.Sin()` sinüs fonksiyonudur. Radyan cinsinden bir açı alır ve -1 ile +1 arasında değer üretir.

Sinüs eğrisi: hareket yumuşak biçimde hızlanır, yavaşlar ve yön değiştirir.

### Örnek C# kodu

```

using UnityEngine;

public class Oscillator : MonoBehaviour
{
    [SerializeField] private Vector3 movementVector = new Vector3(5, 0, 0);
    [SerializeField] private float speed = 2f;

    private Vector3 startPosition;

    void Start()
    {
        startPosition = transform.position;
    }

    void Update()
    {
        float movement = Mathf.Sin(Time.time * speed);

        transform.position = startPosition + movementVector * movement;
    }
}
```

### Kodun çalışma mantığı

1. `startPosition`

Nesnenin başlangıç konumunu saklar. Böylece hareket her zaman bu konumun etrafında gerçekleşir.

2. `Time.time * speed`

`Time.time` oyun başladığından itibaren geçen süreyi saniye olarak verir.

`speed` ise salınımın ne kadar hızlı gerçekleşeceğini belirler.

3. `Mathf.Sin()`

Zamana bağlı olarak -1 ile +1 arasında değer üretir.

4. `movementVector * movement`

Hareket yönünü ve maksimum uzaklığı belirler.

Örneğin:

```
movementVector = new Vector3(5, 0, 0);
```

Nesne X ekseninde başlangıç konumunun 5 birim soluna ve 5 birim sağına hareket eder. Toplam hareket aralığı 10 birimdir.

## 3. İnteraktif salınım örneği

Aşağıdaki değerleri değiştirerek salınımın nasıl çalıştığını görebilirsin.

Genlik (mesafe)

4 birim

Hız çarpanı

2

Zaman (simülasyon)

0.00 sn

Zaman kaydırıcısını hareket ettirerek nesnenin konumunu inceleyebilirsin. Hız çarpanı, aynı faza ulaşmak için gereken süreyi değiştirir.

## 4. Salınım süresi ve hızı

Sinüs fonksiyonunun tam bir döngüsü \(2\pi\) radyandır.

Bu nedenle:

$$
T=\frac{2\pi}{speed} 
$$

Örneğin `speed = 2` olduğunda:

$$
T=\frac{2\pi}{2}\approx3.14\text{ saniye}
$$

Yani nesne yaklaşık 3,14 saniyede tam bir ileri-geri hareketini tamamlar.

## 5. Farklı yönlerde salınım

|movementVector|Sonuç|
|---|---|
|`(5, 0, 0)`|Sağ-sol|
|`(0, 3, 0)`|Yukarı-aşağı|
|`(0, 0, 4)`|İleri-geri|
|`(2, 3, 0)`|Çapraz|

## 6. Mathf.Sin ile Mathf.PingPong farkı

Unity'de salınım oluşturmak için `Mathf.PingPong()` da kullanılabilir.

- Mathf.Sin: Nesne uç noktalara yaklaşırken yavaşlar ve yön değiştirirken hızı sıfıra iner.
    
- Mathf.PingPong: Nesne sabit hızla ilerler ve uç noktada aniden yön değiştirir.
    

```

void Update()
{
    float x = Mathf.PingPong(Time.time * 2f, 5f);

    transform.position = startPosition + Vector3.right * x;
}
```

Bu örnekte nesne başlangıç konumundan 5 birim sağa gider, ardından başlangıç noktasına geri döner.

Önemli: Fizik tabanlı nesnelerde (`Rigidbody`) doğrudan `transform.position` değiştirmek yerine fizik sistemine uygun hareket yöntemleri kullanmak daha doğrudur.

Özet: Oscillating Objects ayrı bir Unity bileşeni değildir. Nesnelere kod aracılığıyla kazandırdığımız bir hareket davranışıdır. Sinüs tabanlı salınım özellikle hareketli platformlar, havada süzülen nesneler ve yumuşak hareket eden engeller için kullanışlıdır.