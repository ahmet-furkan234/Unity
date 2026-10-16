## Vector3 Normalize — Detaylı Not

**Normalize**, bir vektörün **yönünü değiştirmeden toplam büyüklüğünü (Magnitude) 1 yapmak** işlemidir.

Unity'de:

```
Vector3 normalizedVector = vector.normalized;
```

veya mevcut vektörü değiştirmek için:

```
vector.Normalize();
```

### 1. Temel mantığı

Bir `Vector3` üç bileşenden oluşur:

```
Vector3 vector = new Vector3(x, y, z);
```

Örneğin:

```
(3, 0, 4)
```

Burada:

```
X = 3
Y = 0
Z = 4
```

Ancak vektörün **toplam büyüklüğü 3 + 4 = 7 değildir.**

Pisagor kullanılır:

\[ Magnitude = \sqrt{x^2+y^2+z^2} \]

Dolayısıyla `(3,0,4)` için:

\[ \sqrt{3^2+0^2+4^2} \]\[ =\sqrt{9+16} \]\[ =\sqrt{25}=5 \]

Yani:

```
Vector     = (3, 0, 4)
Magnitude  = 5
```

---

### 2. Normalize nasıl hesaplanır?

Normalize işleminin formülü:

\[ NormalizedVector = \frac{Vector}{Magnitude} \]

Yani **vektörün bütün bileşenlerini kendi magnitude değerine böleriz.**

Elimizde:

```
Vector = (3, 0, 4)
Magnitude = 5
```

var.

O zaman:

\[ (3,0,4) / 5 \]

Her bileşeni ayrı ayrı böleriz:

```
X → 3 / 5 = 0.6
Y → 0 / 5 = 0
Z → 4 / 5 = 0.8
```

Sonuç:

```
(0.6, 0, 0.8)
```

Şimdi bunun magnitude değerini tekrar hesaplarsak:

\[ \sqrt{0.6^2+0^2+0.8^2} \]\[ =\sqrt{0.36+0.64} \]\[ =\sqrt1=1 \]

Yani:

```
(3, 0, 4)           Magnitude = 5

      ↓ Normalize

(0.6, 0, 0.8)       Magnitude = 1
```

**Yön değişmedi, oran değişmedi; sadece toplam büyüklük 1 oldu.**

---

## 3. Normalize oranı bozmaz

Önemli noktalardan biri budur.

Örneğin:

```
(1, 0, 4)
```

vektörünü normalize ettiğimizde yaklaşık:

```
(0.242, 0, 0.970)
```

elde ederiz.

İlk durumda X/Z oranı:

\[ 1/4=0.25 \]

Normalize sonrasında:

\[ 0.242/0.970\approx0.25 \]

Yani oran aynı kalır.

```
(1, 0, 4)
    ↓
   Normalize
    ↓
(0.242, 0, 0.970)

YÖN  → aynı
ORAN → aynı
BOYUT → 1
```

---

## 4. Unity'de neden kullanılır?

En klasik örnek **karakter hareketidir.**

```
float x = Input.GetAxis("Horizontal");
float z = Input.GetAxis("Vertical");

Vector3 movement = new Vector3(x, 0, z);
```

Sadece D'ye bastığımızı düşünelim:

```
(1, 0, 0)

Magnitude = 1
```

Sadece W:

```
(0, 0, 1)

Magnitude = 1
```

Ama W + D:

```
(1, 0, 1)
```

Magnitude:

\[ \sqrt{1^2+1^2} =\sqrt2 \approx1.414 \]

Bu nedenle normalize kullanmazsak **çapraz hareketin toplam hızı yaklaşık %41 daha fazla olur.**

Normalize:

```
Vector3 movement = new Vector3(x, 0, z).normalized;
```

W+D için:

```
(1, 0, 1)

     ↓ Normalize

(0.707, 0, 0.707)

Magnitude = 1
```

Böylece:

```
D       → magnitude 1
W       → magnitude 1
W + D   → magnitude 1
W + A   → magnitude 1
S + D   → magnitude 1
```

Karakter her yönde aynı toplam hızla hareket eder.

---

## 5. Normalize hız vermek değildir

Bu ayrım çok önemli.

Normalize:

```
Vector3 direction = movement.normalized;
```

**hız belirlemekten çok yönü standartlaştırır.**

Örneğin:

```
direction = (0.6, 0, 0.8)

Magnitude = 1
```

Sonrasında gerçek hızı biz verebiliriz:

```
Vector3 velocity = direction * 5f;
```

Sonuç:

```
(0.6, 0, 0.8)
       × 5

(3, 0, 4)
```

Magnitude:

```
5
```

Bu yüzden karakter hareketinde çok sık:

```
Vector3 movement = new Vector3(x, 0, z).normalized;

transform.Translate(
    movement * speed * Time.deltaTime
);
```

kullanılır.

Mantığı:

```
Input
  ↓
(1, 0, 1)
  ↓
Normalize
  ↓
(0.707, 0, 0.707)
  ↓
Yön belli + Magnitude 1
  ↓
× speed
  ↓
İstediğimiz gerçek hız
```

---

# Örnek 1 — `(3, 0, 4)`

```
Vector = (3, 0, 4)
```

Magnitude:

\[ \sqrt{3^2+4^2}=5 \]

Normalize:

\[ (3,0,4)/5 \]

Sonuç:

```
(0.6, 0, 0.8)

Magnitude = 1
```

---

# Örnek 2 — Çapraz karakter hareketi

W + D:

```
Horizontal = 1
Vertical   = 1
```

Oluşan Vector3:

```
new Vector3(1, 0, 1);
```

Magnitude:

```
1.414
```

Normalize:

```
(1, 0, 1)
     ↓
(0.707, 0, 0.707)

Magnitude = 1
```

Böylece çapraz hareketin fazladan hızlı olması engellenir.

---

# Örnek 3 — `(10, 0, 0)`

```
Vector = (10, 0, 0)
```

Magnitude:

```
10
```

Normalize:

\[ (10,0,0)/10 \]

Sonuç:

```
(1, 0, 0)
```

Dikkat:

```
(10, 0, 0) → sağ yön, magnitude 10
(1,  0, 0) → sağ yön, magnitude 1
```

**Yön tamamen aynı**, sadece büyüklük değişti.

---

## `.normalized` ve `.Normalize()` farkı

```
Vector3 direction = movement.normalized;
```

`movement` değişmez. Normalize edilmiş **yeni bir değer** elde edilir.

```
movement.Normalize();
```

ise `movement` değişkeninin **kendisini değiştirir.**

Örneğin:

```
Vector3 movement = new Vector3(3, 0, 4);

Vector3 direction = movement.normalized;
```

Sonuç:

```
movement  = (3, 0, 4)
direction = (0.6, 0, 0.8)
```

Ama:

```
Vector3 movement = new Vector3(3, 0, 4);

movement.Normalize();
```

sonrasında:

```
movement = (0.6, 0, 0.8)
```

