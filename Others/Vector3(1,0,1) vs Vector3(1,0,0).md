Tabii TkMatE. Burada önemli konu **Vector3'ün büyüklüğü (magnitude)** ve bunun çapraz harekete etkisi.

### `Vector3(1, 0, 0)`

```
Vector3 movement = new Vector3(1, 0, 0);
```

Bu:

```
X = 1  → sağa
Y = 0
Z = 0
```

demek.

Yani sadece sağa gidiyoruz:

```
        Z
        ↑
        |
        |
        ● ─────→ X
           1
```

Bu vektörün uzunluğu/büyüklüğü **1'dir**.

Matematiksel olarak `Vector3`'ün büyüklüğü:

$$
\sqrt{x^2 + y^2 + z^2}
$$

Dolayısıyla:

$$
\sqrt{1^2 + 0^2 + 0^2} = 1
$$

---

### `Vector3(1, 0, 1)`

Şimdi:

```
Vector3 movement = new Vector3(1, 0, 1);
```

diyelim.

Bu sefer:

```
X = 1 → sağa
Y = 0
Z = 1 → ileri
```

Yani çapraz hareket ediyoruz:

```
              ↗ (1,0,1)
             /
            /
           ● ─────→ X
           |
           |
           ↑ Z
```

Burada kritik nokta şu:

**Vektörün büyüklüğü artık 1 değil.**

Hesaplayalım:

$$
\sqrt{1^2 + 0^2 + 1^2}
= \sqrt{2}
\approx 1.414
$$

Yani:

```
Vector3(1,0,0) → uzunluk = 1

Vector3(1,0,1) → uzunluk = 1.414
```

Bu nedenle karakterin hareket kodu şöyleyse:

```
transform.Translate(movement * speed * Time.deltaTime);
```

sadece D'ye basınca:

```
movement = (1,0,0)
hız       = 1 × speed
```

W + D basınca:

```
movement = (1,0,1)
hız       = 1.414 × speed
```

**Karakter çapraz giderken yaklaşık %41 daha hızlı hareket eder.**

### Çözüm: `normalized`

Unity'de:

```
movement = movement.normalized;
```

dediğimizde Unity vektörün **yönünü değiştirmeden uzunluğunu 1 yapar.**

Yani:

```
(1,0,0)
   ↓ normalized
(1,0,0)
uzunluk = 1
```

ama:

```
(1,0,1)
   ↓ normalized
yaklaşık
(0.707, 0, 0.707)

uzunluk = 1
```

Neden `0.707`?

Çünkü:


$$
\sqrt{0.707^2 + 0.707^2} \approx 1
$$


Böylece:

```
float x = Input.GetAxis("Horizontal");
float z = Input.GetAxis("Vertical");

Vector3 movement = new Vector3(x, 0, z);

movement = movement.normalized;

transform.Translate(
    movement * speed * Time.deltaTime
);
```

artık:

```
D        → hız 1
W        → hız 1
W + D    → hız 1
W + A    → hız 1
S + D    → hız 1
```

olur.

Kısacası **Vector3 sadece yönü değil, aynı zamanda büyüklüğü de taşır.**

```
Vector3(1,0,0)
      ↓
sağa doğru, büyüklük 1


Vector3(1,0,1)
      ↓
sağ + ileri, büyüklük 1.414


Vector3(1,0,1).normalized
      ↓
sağ + ileri, büyüklük 1
```

Bu **yön + büyüklük** mantığını anlarsan sonraki `velocity`, `AddForce`, `transform.forward` gibi Unity konuları çok daha kolay oturur.

Hareketi pekiştirelim

- normalized kodunu satır satır açıkla
- Çapraz hareketi örnekle hesapla