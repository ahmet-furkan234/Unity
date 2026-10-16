
`AddRelativeForce()`, Unity'de bir nesnenin Rigidbody bileşenine, nesnenin kendi yerel (Local) koordinat sistemine göre kuvvet uygulayan metottur.

`Transform.Translate()` nesnenin konumunu doğrudan değiştirirken, `AddRelativeForce()` fizik motorunu kullanarak nesneyi hareket ettirir.

Özellikle araba, uçak, roket, uzay gemisi ve fizik tabanlı karakter hareketlerinde kullanılır.

## 1. AddRelativeForce() nasıl kullanılır?

```
rb.AddRelativeForce(Vector3.forward * 10f);
```

Bu kod nesnenin kendi ileri yönüne doğru kuvvet uygular.

Şematik üstten görünüm: Nesne döndüğünde ileri yönü de değişir.

## 2. Temel kullanım örneği

```
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    [SerializeField] private float force = 10f;

    private Rigidbody rb;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void FixedUpdate()
    {
        rb.AddRelativeForce(Vector3.forward * force);
    }
}
```

Kodun çalışma mantığı:

1. `GetComponent<Rigidbody>()` nesnenin Rigidbody bileşenini alır.
    
2. `Vector3.forward` yerel `(0, 0, 1)` yönünü belirtir.
    
3. `force` uygulanacak kuvvetin büyüklüğüdür.
    
4. `AddRelativeForce()` fizik motoruna kuvvet uygular.
    
5. `FixedUpdate()` fizik güncellemeleriyle uyumlu çalışmasını sağlar.
    

Bu örnekte kuvvet her fizik adımında uygulanır. Bu nedenle nesne, sürtünme ve diğer fiziksel etkiler izin verdiği sürece hızlanabilir.

## 3. AddForce() ve AddRelativeForce() farkı

İki metot arasındaki temel fark koordinat sistemidir.

AddForce()

Dünya koordinatına göre kuvvet uygular.

AddRelativeForce()

Nesnenin kendi koordinatına göre kuvvet uygular.

Örneğin nesneyi Y ekseninde 90° döndürdüğümüzü düşünelim:

```
rb.AddForce(Vector3.forward * 10f);
```

Nesne hangi yöne bakarsa baksın dünya koordinatındaki `+Z` yönüne kuvvet uygulanır.

```
rb.AddRelativeForce(Vector3.forward * 10f);
```

Nesne döndüğü için bu kez kendi ileri yönüne, yani dünya koordinatlarında `+X` yönüne kuvvet uygulanır.

## 4. ForceMode nedir?

`AddRelativeForce()` metodunun ikinci parametresi `ForceMode` değeridir.

```
rb.AddRelativeForce(
    Vector3.forward * 10f,
    ForceMode.Force
);
```

Unity'de dört farklı ForceMode bulunur:

|ForceMode|Açıklama|
|---|---|
|`Force`|Sürekli kuvvet uygular, kütleden etkilenir.|
|`Acceleration`|İvme uygular, kütleden bağımsızdır.|
|`Impulse`|Tek seferlik ani itme uygular, kütleden etkilenir.|
|`VelocityChange`|Doğrudan hız değişimi sağlar, kütleden bağımsızdır.|

### ForceMode.Force

```
rb.AddRelativeForce(
    Vector3.forward * 10f,
    ForceMode.Force
);
```

Newton'un ikinci yasasına dayanır:

$$
F=m\cdot a \
$$

Buradan:

$$
a=\frac{F}{m} 
$$

Örneğin 10 N kuvvet uygulandığında:

|Kütle|İvme|
|---|---|
|1 kg|10 m/s²|
|2 kg|5 m/s²|
|5 kg|2 m/s²|

Kütle arttıkça aynı kuvvetin oluşturduğu ivme azalır.

### ForceMode.Acceleration

```
rb.AddRelativeForce(
    Vector3.forward * 10f,
    ForceMode.Acceleration
);
```

Burada `10`, 10 m/s² ivme anlamına gelir. Kütle ne olursa olsun aynı ivme uygulanır.

### ForceMode.Impulse

```
rb.AddRelativeForce(
    Vector3.up * 5f,
    ForceMode.Impulse
);
```

Ani bir itme sağlar. Patlama, çarpışma veya tek seferlik fırlatma hareketlerinde kullanılabilir.

### ForceMode.VelocityChange

```
rb.AddRelativeForce(
    Vector3.up * 5f,
    ForceMode.VelocityChange
);
```

Kütleden bağımsız olarak hızı anlık değiştirir. Bu örnekte nesnenin yerel yukarı yönünde 5 m/s hız değişimi oluşturulur.

## 5. FixedUpdate() neden kullanılır?

Unity'de fizik işlemleri için genellikle `FixedUpdate()` kullanılır.

```
void FixedUpdate()
{
    rb.AddRelativeForce(Vector3.forward * 10f);
}
```

- `Update()` her render frame'inde çalışır.
    
- `FixedUpdate()` sabit fizik zaman adımlarında çalışır.
    
- `AddRelativeForce()` fizik motoruyla çalıştığı için `FixedUpdate()` içerisinde kullanılması önerilir.
    

Önemli: `ForceMode.Force` veya `Acceleration` kullanırken `Time.deltaTime` ile ayrıca çarpmamız gerekmez. Unity fizik zaman adımını kendi hesaplar.

## 6. Uygulamalı örnek: W tuşuyla ileri hareket

Önceki konumuz olan `UnityEngine.InputSystem` ile birleştirelim.

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerMovement : MonoBehaviour
{
    [SerializeField] private float force = 15f;

    private Rigidbody rb;

    void Awake()
    {
        rb = GetComponent<Rigidbody>();
    }

    void FixedUpdate()
    {
        if (Keyboard.current == null)
            return;

        if (Keyboard.current.wKey.isPressed)
        {
            rb.AddRelativeForce(
                Vector3.forward * force,
                ForceMode.Force
            );
        }

        if (Keyboard.current.sKey.isPressed)
        {
            rb.AddRelativeForce(
                Vector3.back * force,
                ForceMode.Force
            );
        }
    }
}
```

W tuşuna basıldığında ileri, S tuşuna basıldığında geri yönde kuvvet uygulanır.

Burada önemli bir ayrıntı var: S tuşuna basmak nesneyi anında geri hareket ettirmez. Öncelikle mevcut ileri hızını azaltabilir, ardından ters yönde hızlandırabilir. Çünkü hareket fizik kurallarına bağlıdır.

## 7. Kuvvet ve ivme hesaplayıcısı

Aşağıdaki hesaplayıcıda kuvvet ve kütleyi değiştirerek oluşacak ivmeyi görebilirsin.

Kuvvet (N)

## 40 N

Rigidbody kütlesi (kg)

## 8 kg

Oluşan ivme (ForceMode.Force)

# 5.00 m/s²

$$
(a=\frac{40}{8}=5.00)
$$

Hesaplama yalnızca uygulanan kuvvetin oluşturduğu ivmeyi gösterir; yer çekimi, sürtünme ve diğer kuvvetler dahil değildir.

## 8. AddRelativeForce() kullanırken dikkat edilmesi gerekenler

- Nesnede `Rigidbody` component'i bulunmalıdır.
    
- `Rigidbody.isKinematic` aktifse standart kuvvet uygulamalarıyla hareket etmez.
    
- `Rigidbody` kütlesi `Force` ve `Impulse` modlarında hareketi etkiler.
    
- Nesne döndüğünde uygulanan yerel kuvvetin dünya üzerindeki yönü değişir.
    
- Sürekli kuvvet uygulamak nesnenin hızlanmasına neden olabilir.
    
- `Rigidbody` üzerinden fiziksel hareket sağlarken aynı nesnenin `Transform.position` değerini sürekli doğrudan değiştirmek fizik simülasyonuyla çakışabilir.
    

Kısaca: `AddRelativeForce()` nesneye kendi yönüne göre fiziksel kuvvet uygular. `AddForce()` ise verilen kuvvet vektörünü dünya koordinatlarında yorumlar.

Bu konudan sonra `Rigidbody.linearVelocity`, `AddTorque()` ve `AddRelativeTorque()` öğrenmek faydalı olacaktır. Böylece bir nesnenin hem hızını hem de fiziksel dönüş hareketini kontrol edebilirsin.