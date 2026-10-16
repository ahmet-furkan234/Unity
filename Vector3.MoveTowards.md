
Temel kullanımı:

```
Vector3.MoveTowards(current, target, maxDistanceDelta);
```

Üç parametre alır:

```
current           → Şu anki konum
target            → Gitmek istediğimiz konum
maxDistanceDelta  → Bu adımda en fazla ne kadar ilerleyeceği
```

Örneğin:

```
transform.position = Vector3.MoveTowards(
    transform.position,
    target.position,
    speed * Time.deltaTime
);
```

Bu kod:

> Nesnenin mevcut konumunu al → hedefe doğru ilerlet → yeni konumu tekrar nesneye ata.

---

## Basit örnek

Bir düşmanın oyuncuya doğru gitmesini istiyoruz:

```
using UnityEngine;

public class Enemy : MonoBehaviour
{
    public Transform player;
    public float speed = 5f;

    void Update()
    {
        transform.position = Vector3.MoveTowards(
            transform.position,
            player.position,
            speed * Time.deltaTime
        );
    }
}
```

Burada:

```
transform.position
```

başlangıç noktası.

```
player.position
```

hedef nokta.

```
speed * Time.deltaTime
```

ise bu frame'de ne kadar hareket edeceğimiz.

---

## Matematiksel mantığı

Şöyle düşün:

```
Enemy:  (0, 0, 0)

Player: (10, 0, 0)
```

ve:

```
speed = 2f;
```

olsun.

Bu durumda düşman saniyede yaklaşık **2 Unity unit** ilerler:

```
Başlangıç:

Enemy                         Player
  ↓                              ↓
  0----1----2----3----4----5----...----10


1 saniye sonra:

       Enemy                    Player
         ↓                         ↓
  0----1----2----3----4----5----...----10


2 saniye sonra:

                 Enemy          Player
                   ↓               ↓
  0----1----2----3----4----5----...----10
```

Yaklaşık:

```
t = 0s  → x = 0
t = 1s  → x = 2
t = 2s  → x = 4
t = 3s  → x = 6
t = 4s  → x = 8
t = 5s  → x = 10
```

---

## Neden `Time.deltaTime` kullanıyoruz?

Şunu yazarsak:

```
Vector3.MoveTowards(
    transform.position,
    player.position,
    speed
);
```

`speed` **her frame'de** uygulanır.

60 FPS'de:

```
5 × 60 = 300 unit/saniye
```

gibi aşırı hızlı bir hareket ortaya çıkabilir.

Bunun yerine:

```
speed * Time.deltaTime
```

kullanıyoruz.

60 FPS'de yaklaşık:

```
Time.deltaTime ≈ 0.0167

5 × 0.0167
≈ 0.0835 unit/frame
```

60 frame sonunda:

```
0.0835 × 60
≈ 5 unit
```

Yani:

```
speed = 5f;
```

yaklaşık **5 unit/saniye** anlamına gelir.

---

## Neden direkt `transform.position` değiştirmiyoruz?

Şunu yapabilirsin:

```
transform.position = player.position;
```

Ama bu hareket değildir.

Nesne direkt hedefe **ışınlanır**:

```
Enemy ---------------------- Player
  ↓

          ↓ tek frame

                         Enemy
                         Player
```

`MoveTowards` ise:

```
Enemy → → → → → → → → Player
```

şeklinde hedefe yaklaştırır.

---

## `MoveTowards` hedefi geçer mi?

En güzel özelliklerinden biri budur:

**Hayır.**

Diyelim:

```
Mevcut konum = 9
Hedef         = 10
Hareket       = 5
```

Normal matematikte:

```
9 + 5 = 14
```

olabilir.

Ama `MoveTowards` hedefin 10 olduğunu bildiği için:

```
9 → 10
```

yapar.

```
9 → 14
```

yapmaz.

Yani hedefe ulaştığında orada durur.

---

## `MoveTowards` aslında ne döndürüyor?

Bu çok önemli.

Şunu yazmak tek başına nesneyi hareket ettirmez:

```
Vector3.MoveTowards(
    transform.position,
    player.position,
    speed * Time.deltaTime
);
```

Çünkü `MoveTowards` **Transform'u değiştiren bir komut değildir**.

Yeni bir `Vector3` hesaplayıp **geri döndürür**.

Bu yüzden:

```
Vector3 newPosition = Vector3.MoveTowards(
    transform.position,
    player.position,
    speed * Time.deltaTime
);

transform.position = newPosition;
```

yapmamız gerekir.

Genellikle bunu tek satırda yazıyoruz:

```
transform.position = Vector3.MoveTowards(
    transform.position,
    player.position,
    speed * Time.deltaTime
);
```

Mantık:

```
Mevcut Position
      ↓
MoveTowards()
      ↓
Yeni Position hesaplanır
      ↓
transform.position'a atanır
      ↓
GameObject hareket eder
```

---

## `Translate` ile `MoveTowards` farkı

Daha önce gördüğümüz `Translate` ile önemli bir fark var.

`Translate`:

```
transform.Translate(Vector3.forward * speed * Time.deltaTime);
```

şunu söyler:

> Bu **yönde** hareket et.

`MoveTowards`:

```
transform.position = Vector3.MoveTowards(
    transform.position,
    target.position,
    speed * Time.deltaTime
);
```

şunu söyler:

> Bu **hedefe doğru** hareket et.

Yani kabaca:

```
Translate
    ↓
Yön önemli
"İleri git."


MoveTowards
    ↓
Hedef önemli
"Şuraya git."
```

Örneğin sürekli hareket eden bir karakter için:

```
transform.Translate(...)
```

mantıklı olabilir.

Bir düşmanın oyuncuyu takip etmesi için:

```
Vector3.MoveTowards(...)
```

çok daha kullanışlıdır.

### Notluk özet

```
transform.position = Vector3.MoveTowards(
    transform.position,     // mevcut konum
    target.position,        // hedef konum
    speed * Time.deltaTime  // bu frame'deki hareket miktarı
);
```

**`Vector3.MoveTowards` = mevcut bir noktayı hedef noktaya, belirlediğimiz maksimum adım miktarı kadar yaklaştırır.**

Ayrıca hedefi geçmez ve doğrudan nesneyi hareket ettirmez; **yeni bir `Vector3` döndürür**, bu yüzden sonucu `transform.position`'a atarız.

MoveTowards kullanımını ilerlet

- Hedefe varınca durdur
- Oyuncuyu takip ettir