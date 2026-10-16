
`Time.fixedDeltaTime`, Unity'de iki fizik güncellemesi (`FixedUpdate`) arasında geçen sabit simülasyon süresini saniye cinsinden ifade eden özelliktir.

Unity'nin fizik motoru, Rigidbody hareketleri, çarpışmalar ve kuvvet hesaplamaları bu zaman aralığına göre çalışır.

```
Time.fixedDeltaTime
```

Unity'de varsayılan değeri genellikle:

\[ \boxed{0.02\text{ saniye}=20\text{ ms}} \]

Bu da saniyede yaklaşık 50 fizik güncellemesi anlamına gelir.

## 1. FixedUpdate ve fixedDeltaTime ilişkisi

```
void FixedUpdate()
{
    Debug.Log(Time.fixedDeltaTime);
}
```

Varsayılan ayarlarda konsolda genellikle `0.02` yazdırılır.

Burada iki kavramı ayırmalıyız:

- `FixedUpdate()`: Fizik güncellemesinde çalışan metottur.
    
- `Time.fixedDeltaTime`: Fizik güncellemeleri arasındaki simülasyon süresidir.
    

Fizik güncelleme sıklığını hesaplamak için:

\[ \text{Fizik Güncelleme Sayısı}=\frac{1}{\text{fixedDeltaTime}} \]

Örneğin:

\[ \frac{1}{0.02}=50 \]

Yani saniyede 50 fizik adımı hedeflenir. Gerçek çalışma sırasında performans ve Unity'nin zaman yönetimi nedeniyle bir render frame'inde sıfır, bir veya birden fazla `FixedUpdate()` çalışabilir.

## 2. fixedDeltaTime değerinin fizik güncellemelerine etkisi

Fizik zaman aralığı

## 0.035 sn

Saniyedeki fizik adımı

# 29 Hz

Her adımın süresi

# 35 ms

1 saniyedeki fizik adımları (şematik)

29 adım

Değeri küçülttükçe fizik daha sık hesaplanır, ancak CPU maliyeti artabilir.

|fixedDeltaTime|Fizik güncellemesi|
|---|---|
|`0.01`|100 Hz|
|`0.02`|50 Hz|
|`0.025`|40 Hz|
|`0.04`|25 Hz|
|`0.05`|20 Hz|

## 3. Time.deltaTime ve Time.fixedDeltaTime farkı

Önceki konumuzda `Time.deltaTime` öğrenmiştik. İkisi arasındaki temel fark şudur:

|Özellik|Time.deltaTime|Time.fixedDeltaTime|
|---|---|---|
|Kullanım|Render frame süresi|Fizik adım süresi|
|Değer|FPS'ye göre değişir|Genellikle sabittir|
|Yaygın kullanım|`Update()`|`FixedUpdate()`|
|Amaç|FPS bağımsız hareket|Sabit zaman adımlı fizik|

Örneğin:

```
void Update()
{
    transform.Translate(
        Vector3.forward * 5f * Time.deltaTime
    );
}
```

Burada nesne, saniyede yaklaşık 5 Unity birimi hareket eder.

Aynı işlemi `FixedUpdate()` içerisinde yaparsak:

```
void FixedUpdate()
{
    transform.Translate(
        Vector3.forward * 5f * Time.fixedDeltaTime
    );
}
```

Yine saniyede yaklaşık 5 birim hareket eder. Ancak fizik nesnelerinin hareketinde `Transform.Translate()` yerine Rigidbody'nin fizik tabanlı yöntemlerini tercih etmeliyiz.

Önemli ayrıntı: `FixedUpdate()` içerisinde `Time.deltaTime` da normal koşullarda sabit fizik zaman adımını döndürür. Yine de `Time.fixedDeltaTime` yazmak, hangi süreyi kullandığımızı daha açık gösterir.

## 4. AddRelativeForce() ile fixedDeltaTime ilişkisi

Bir önceki konumuzda `AddRelativeForce()` metodunu öğrenmiştik.

```
void FixedUpdate()
{
    rb.AddRelativeForce(
        Vector3.forward * 10f,
        ForceMode.Force
    );
}
```

Burada `Time.fixedDeltaTime` ile çarpmıyoruz.

Neden?

Çünkü `ForceMode.Force` kullanıldığında Unity, kuvvetin fizik adımı boyunca oluşturacağı hız değişimini kendisi hesaplar.

Newton'un ikinci yasası:

\[ F=m\cdot a \]

İvme:

\[ a=\frac{F}{m} \]

Hız değişimi:

\[ \Delta v=a\cdot\Delta t \]

Burada \(\Delta t\), fizik adımının süresidir.

### Matematiksel örnek

Varsayalım:

- Kuvvet: 10 N
    
- Kütle: 2 kg
    
- fixedDeltaTime: 0.02 saniye
    

İvme:

\[ a=\frac{10}{2}=5\text{ m/s}^2 \]

Tek fizik adımındaki hız değişimi:

\[ \Delta v=5\cdot0.02=0.1\text{ m/s} \]

Yani uygulanan kuvvet, her fizik adımında hızı yaklaşık `0.1 m/s` artırır (başka etkiler yoksa).

50 fizik adımında:

\[ 50\cdot0.1=5\text{ m/s} \]

Bir saniye sonunda hız yaklaşık 5 m/s artar.

Dolayısıyla aşağıdaki kullanım yanlıştır:

```
// ForceMode.Force için gereksiz çarpım
rb.AddRelativeForce(
    Vector3.forward * 10f * Time.fixedDeltaTime
);
```

Çünkü zaman faktörünü ikinci kez ekleyerek uygulanan kuvveti gereksiz yere azaltmış oluruz.

## 5. fixedDeltaTime nereden değiştirilir?

Unity Editor üzerinden:

Edit → Project Settings → Time → Fixed Timestep

![Unity deltatime value to csv file - Scripting - Unity Discussions](https://images.openai.com/static-rsc-4/pokFFYtqKsyXrEQjQzKI_WnRCHnOwQKm63KXSwj1XpCbQAvMM1CevguP2MVJUVMwmBTYteAKiKC2HXpVdlujxjtCub3AfvoVKYveHuP9_RUHPtz7Np3JZBIK-RNqSPwTpmDSxF54-zWIDAkp2ByCrKlMsYXOlOCLFIeKZ333DuM?purpose=inline)

[discussions.unity.com](https://discussions.unity.com/t/unity-deltatime-value-to-csv-file/909530)

Buradaki `Fixed Timestep` değeri, `Time.fixedDeltaTime` ile ilişkilidir.

Kod üzerinden de değiştirebiliriz:

```
void Awake()
{
    Time.fixedDeltaTime = 0.01f;
}
```

Böylece hedef fizik güncelleme sıklığı 100 Hz olur.

Ancak bu ayar tüm projeyi etkiler. Sadece bir Rigidbody için geçerli değildir.

## 6. Uygulamalı örnek: Rigidbody ile hareket

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerMovement : MonoBehaviour
{
    [SerializeField] private float speed = 5f;

    private Rigidbody rb;
    private Vector2 moveInput;

    void Awake()
    {
        rb = GetComponent<Rigidbody>();
    }

    void Update()
    {
        if (Keyboard.current == null)
        {
            moveInput = Vector2.zero;
            return;
        }

        moveInput = Vector2.zero;

        if (Keyboard.current.wKey.isPressed)
            moveInput.y += 1;

        if (Keyboard.current.sKey.isPressed)
            moveInput.y -= 1;

        if (Keyboard.current.aKey.isPressed)
            moveInput.x -= 1;

        if (Keyboard.current.dKey.isPressed)
            moveInput.x += 1;

        moveInput = moveInput.normalized;
    }

    void FixedUpdate()
    {
        Vector3 direction = new Vector3(
            moveInput.x,
            0f,
            moveInput.y
        );

        Vector3 movement =
            direction * speed * Time.fixedDeltaTime;

        rb.MovePosition(rb.position + movement);
    }
}
```

Burada:

1. `Update()` içerisinde klavye girişlerini okuyoruz.
    
2. `Vector2.normalized` ile çapraz hareket hızını dengeliyoruz.
    
3. `FixedUpdate()` içerisinde Rigidbody hareketini gerçekleştiriyoruz.
    
4. `Time.fixedDeltaTime` ile her fizik adımında gidilecek mesafeyi hesaplıyoruz.
    
5. `Rigidbody.MovePosition()` ile hedef konumu belirliyoruz.
    

Bu örnek, konum tabanlı Rigidbody hareketini gösterir. Özellikle kinematik Rigidbody için uygundur; kuvvet ve ivme tabanlı hareket istiyorsak `AddForce()` kullanmak daha doğru olabilir.

## 7. Önemli notlar

- `Time.fixedDeltaTime`, fizik adımının simülasyon süresini belirtir.
    
- Varsayılan değeri genellikle `0.02` saniyedir.
    
- Küçük değer, daha sık fizik hesaplaması anlamına gelir.
    
- Büyük değer, daha seyrek fizik hesaplaması anlamına gelir.
    
- `AddForce()` ve `AddRelativeForce()` kullanırken kuvveti ayrıca `fixedDeltaTime` ile çarpmamalıyız.
    
- `MovePosition()` gibi mesafe hesapladığımız işlemlerde `speed * Time.fixedDeltaTime` kullanabiliriz.
    
- `FixedUpdate()` sabit fizik adımlarında çalışır ancak render FPS'siyle bire bir eşleşmez.
    
- `Time.timeScale` değiştirildiğinde fizik güncellemelerinin gerçek zamandaki sıklığı da etkilenebilir.
    

Kısaca: `Time.deltaTime` render frame'leri arasındaki süreyi, `Time.fixedDeltaTime` ise fizik adımları arasındaki simülasyon süresini temsil eder.

Bir sonraki konu olarak `Rigidbody.MovePosition()` veya `Rigidbody.linearVelocity` üzerinden ilerlemek, fizik tabanlı hareket mantığını daha iyi kavramanı sağlayacaktır.