Tabii TkMatE. `Time.deltaTime`, Unity'de özellikle **hareket, zamanlayıcı ve animasyon mantığını FPS'ten bağımsız yapmak** için kullanılan en temel değerlerden biridir.

## `Time.deltaTime` nedir?

`Time.deltaTime`, **bir önceki frame ile şu anki frame arasında geçen süreyi saniye cinsinden verir.**

Örneğin oyun 60 FPS çalışıyorsa yaklaşık:

```
1 saniye / 60 frame = 0.0167 saniye

Time.deltaTime ≈ 0.0167
```

30 FPS:

```
1 / 30 = 0.0333

Time.deltaTime ≈ 0.0333
```

120 FPS:

```
1 / 120 = 0.0083

Time.deltaTime ≈ 0.0083
```

Yani FPS yükseldikçe `deltaTime` küçülür.

---

## Neden ihtiyacımız var?

Şöyle hareket ettirdiğimizi düşün:

```
void Update()
{
    transform.Translate(Vector3.forward * 5f);
}
```

`Update()` **her frame'de bir kere** çalışır.

60 FPS'te:

```
5 × 60 = 300 unit / saniye
```

120 FPS'te:

```
5 × 120 = 600 unit / saniye
```

Bu durumda güçlü bilgisayarda karakter daha hızlı gider!

Çünkü aslında `5` değerini **saniyede 5 unit** olarak değil, **frame başına 5 unit** olarak uyguladık.

---

## `Time.deltaTime` bunu nasıl çözüyor?

Şöyle yazarız:

```
void Update()
{
    transform.Translate(
        Vector3.forward * 5f * Time.deltaTime
    );
}
```

Artık `5f` değerini:

> **Saniyede yaklaşık 5 unit hareket et.**

şeklinde düşünebiliriz.

### 60 FPS örneği

`deltaTime ≈ 0.0167`

Her frame:

```
5 × 0.0167
= 0.0835 unit
```

60 frame boyunca:

```
0.0835 × 60
≈ 5 unit
```

### 30 FPS örneği

`deltaTime ≈ 0.0333`

Her frame:

```
5 × 0.0333
= 0.1665 unit
```

30 frame:

```
0.1665 × 30
≈ 5 unit
```

Sonuç:

```
              Her frame      1 saniyede

30 FPS  →      0.166          ≈ 5 unit
60 FPS  →      0.083          ≈ 5 unit
120 FPS →      0.041          ≈ 5 unit
```

FPS düşükse **frame başına daha fazla**, FPS yüksekse **frame başına daha az** hareket ettiriyoruz.

Böylece toplamda yaklaşık aynı mesafe elde ediliyor.

---

## `deltaTime` aslında ne yapıyor?

Şu kodu:

```
speed * Time.deltaTime
```

şöyle okuyabilirsin:

```
speed = saniyedeki hareket miktarı
deltaTime = bu frame'de geçen saniye miktarı
```

Dolayısıyla:

```
saniyedeki hız × geçen süre = gidilecek mesafe
```

Fizikteki:

\[ Mesafe = Hız \times Zaman \]

mantığıyla aynı.

Örneğin:

```
float speed = 10f;

transform.Translate(
    Vector3.forward * speed * Time.deltaTime
);
```

`speed = 10 unit/s` ise ve bu frame `0.02 saniye` sürdüyse:

\[ 10\times0.02=0.2 \]

Bu frame karakteri `0.2 unit` hareket ettirirsin.

---

## Şimdi önceki öğrendiklerimizi birleştirelim

```
[SerializeField]
private float speed = 5f;

void Update()
{
    float x = Input.GetAxis("Horizontal");
    float z = Input.GetAxis("Vertical");

    Vector3 movement = new Vector3(x, 0, z);

    movement = movement.normalized;

    transform.Translate(
        movement * speed * Time.deltaTime
    );
}
```

Burada bütün parçalar artık anlamlı:

```
Input.GetAxis()
      ↓
Oyuncunun yön girdisini al

Vector3(x, 0, z)
      ↓
Hareket vektörünü oluştur

.normalized
      ↓
Yönü koru, magnitude'u 1 yap

* speed
      ↓
Hareket hızını belirle

* Time.deltaTime
      ↓
Hareketi geçen zamana bağla
(FPS bağımsız yap)

Transform.Translate()
      ↓
Objeyi hareket ettir
```

### Akılda kalacak tanım

> **`Time.deltaTime` = Bir önceki frame ile mevcut frame arasında geçen süre (saniye).**

Ve en önemli kullanım mantığı:

```
movement * speed * Time.deltaTime
```

şu anlama gelir:

> **"Belirlediğim hızla, bu frame'de geçen süre kadar hareket et."**

Bu yüzden `Update()` içerisinde yapılan sürekli hareketlerde `Time.deltaTime`'ı çok sık göreceksin.

Time.deltaTime kullanımını pekiştir

- Zamanlayıcı örneği yap