
`Vector3.Lerp()`, Unity'de iki Vector3 konumu arasında doğrusal interpolasyon (Linear Interpolation) yapmak için kullanılan bir metottur.

Basitçe, bir nesnenin başlangıç ve bitiş konumu arasında nerede bulunacağını hesaplar.

Özellikle hareketli platformlar, kapılar, asansörler ve Oscillating Objects oluştururken kullanılır.

## 1. Metodun yapısı

```
Vector3.Lerp(Vector3 a, Vector3 b, float t);
```

|Parametre|Açıklama|
|---|---|
|`a`|Başlangıç konumu|
|`b`|Hedef konumu|
|`t`|0 ile 1 arasında konumu belirleyen oran|
|Dönüş değeri|Hesaplanan yeni `Vector3`|

`t` değerinin anlamı:

- `t = 0` → Başlangıç konumu.
    
- `t = 0.25` → Yolun %25'i.
    
- `t = 0.5` → Yolun %50'si.
    
- `t = 0.75` → Yolun %75'i.
    
- `t = 1` → Hedef konumu.
    

## 2. İnteraktif Vector3.Lerp örneği

t (movementFactor)

0.33

İlerleme

## 33%

Hesaplanan X

## 4.64

Kaydırıcıyı değiştirdiğinde nesnenin A ile B arasında hangi konuma yerleştiğini görebilirsin.

## 3. Matematiksel çalışma mantığı

`Vector3.Lerp()` şu formülü kullanır:

\[ Lerp(a,b,t)=a+(b-a)\times t \]

Örneğin:

```
Vector3 a = new Vector3(2, 0, 0);
Vector3 b = new Vector3(10, 0, 0);

Vector3 result = Vector3.Lerp(a, b, 0.5f);
```

Hesaplayalım:

$$
2+(10-2)\times0.5=6
$$

Sonuç:

```
result = new Vector3(6, 0, 0);
```

Önemli: `Lerp()` nesneyi kendi başına hareket ettirmez. Sadece verilen `t` değerine göre bir konum hesaplar. Hareket için sonucu `transform.position` gibi bir özelliğe atamamız gerekir.

## 4. Oscillating Objects ile kullanımı

Önceki konumuzdaki `movementFactor` burada devreye giriyor.

```

using UnityEngine;

public class Oscillator : MonoBehaviour
{
    [SerializeField] Vector3 movementVector = new Vector3(5, 0, 0);
    [SerializeField] float speed = 2f;

    Vector3 startingPosition;

    void Start()
    {
        startingPosition = transform.position;
    }

    void Update()
    {
        float rawSinWave = Mathf.Sin(Time.time * speed);
        float movementFactor = (rawSinWave + 1f) / 2f;

        Vector3 targetPosition = startingPosition + movementVector;

        transform.position = Vector3.Lerp(
            startingPosition,
            targetPosition,
            movementFactor
        );
    }
}
```

Burada:

1. `startingPosition` nesnenin başlangıç konumudur.
    
2. `targetPosition` nesnenin ulaşabileceği son konumdur.
    
3. `movementFactor` sinüs fonksiyonuyla 0–1 arasında değişir.
    
4. `Vector3.Lerp()` bu iki konum arasındaki noktayı hesaplar.
    
5. `transform.position` hesaplanan konuma güncellenir.
    

Böylece nesne sürekli ileri-geri hareket eder.

## 5. Lerp neden doğrusal olmasına rağmen yumuşak hareket ediyor?

Burada önemli bir ayrım var:

Lerp doğrusal interpolasyon yapar, ancak hareketin hızını `t` değerinin zaman içinde nasıl değiştiği belirler.

- `t` sabit hızla 0'dan 1'e artarsa nesne sabit hızla hareket eder.
    
- `t` sinüs fonksiyonuyla değişirse nesne uç noktalarda yavaşlar ve yön değiştirir.
    

Yani yumuşak salınımın nedeni `Lerp()` değil, `Mathf.Sin()` fonksiyonudur.

## 6. Lerp ile LerpUnclamped farkı

Unity'de ayrıca `Vector3.LerpUnclamped()` bulunur.

| Özellik    | Lerp              | LerpUnclamped              |
| ---------- | ----------------- | -------------------------- |
| `t = 0.5`  | %50 konum         | %50 konum                  |
| `t = 1`    | Hedef konum       | Hedef konum                |
| `t = 1.5`  | Hedefte kalır     | Hedefi %50 aşar            |
| `t = -0.5` | Başlangıçta kalır | Başlangıcın gerisine gider |

`Vector3.Lerp()` otomatik olarak `t` değerini 0 ile 1 arasına sınırlar (Clamp).

## 7. Lerp ile MoveTowards farkı

|Vector3.Lerp|Vector3.MoveTowards|
|---|---|
|Oran (`t`) kullanır|Maksimum adım mesafesi kullanır|
|İki konum arasında interpolasyon yapar|Hedefe doğru belirli miktarda ilerler|
|Hareket süresi `t` ile kontrol edilir|Hareket hızı kolayca kontrol edilir|
|Salınım ve geçişler için uygundur|Sabit hızlı hareket için uygundur|

Kısaca: `Vector3.Lerp(a, b, t)` iki nokta arasında `t` oranına karşılık gelen konumu hesaplar. `movementFactor` değerini `t` olarak verirsen nesnenin salınım sırasında nerede bulunacağını kontrol etmiş olursun.
