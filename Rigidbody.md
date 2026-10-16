
Normalde bir objenin `Transform` değerlerini değiştirerek onu hareket ettirebilirsin:

```
transform.position += Vector3.forward;
```

Ama bu hareketi sen doğrudan yapıyorsun. Unity’nin fizik motoru objenin **kütlesini, yer çekimini, momentumunu, kuvvetleri** hesaba katmıyor.

GameObject’e `Rigidbody` eklediğinde ise obje fizik simülasyonuna dahil olur:

**GameObject + Rigidbody → Fiziksel nesne**

Örneğin havadaki bir küreye Rigidbody verirsen, yer çekimi nedeniyle aşağı düşebilir.

---

## Rigidbody'nin temel mantığı

Şöyle düşünebilirsin:

```
Transform
   ↓
Objenin nerede olduğunu söyler.

Collider
   ↓
Objenin fiziksel şeklini/sınırını belirler.

Rigidbody
   ↓
Objenin fizik kurallarına göre nasıl hareket edeceğini belirler.
```

Örneğin:

```
Player
├── Transform
├── Mesh Renderer
├── Capsule Collider
└── Rigidbody
```

Burada `Capsule Collider` oyuncunun fiziksel sınırını, `Rigidbody` ise hareket/fizik davranışını sağlar.

## En önemli Rigidbody özellikleri

### Mass

Objenin **kütlesidir**.

```
Mass = 1
Mass = 10
Mass = 100
```

Örneğin aynı kuvveti uygularsan:

```
rb.AddForce(Vector3.forward * 10);
```

kütlesi düşük obje daha kolay hızlanır.

Temel fizik:
$$
\ F = m \cdot a \
$$
Dolayısıyla:
$$
\ a = \frac{F}{m} \\
$$

10 N kuvvet uygularsak:

```
Mass = 1  → ivme = 10
Mass = 2  → ivme = 5
Mass = 10 → ivme = 1
```

---

### Use Gravity

Yer çekiminin objeye uygulanıp uygulanmayacağını belirler.

```
Use Gravity ✓
```

ise obje aşağı düşer.

```
Use Gravity ✗
```

ise Unity'nin yer çekimi Rigidbody'ye uygulanmaz.

Unity'nin varsayılan yer çekimi yaklaşık:

$$
\ g = -9.81\,m/s^2 \
$$
Y eksenindedir.

---

### Linear Damping

Objenin hareket ederken hız kaybetmesini sağlar. Eski Unity sürümlerinde buna `Drag` deniyordu.

Örneğin bir topu ittiğini düşün:

```
Damping düşük
→ uzun süre kayar

Damping yüksek
→ hızlı yavaşlar
```

Bir nevi hareket direnci gibi düşünebilirsin.

---

### Angular Damping

Aynı mantığın **dönme hareketine** uygulanmış halidir.

```
Angular Damping düşük
→ obje uzun süre döner

Angular Damping yüksek
→ dönmesi hızlı durur
```

---

## Rigidbody ile hareket

Örneğin:

```
public class PlayerMovement : MonoBehaviour
{
    public Rigidbody rb;

    void FixedUpdate()
    {
        rb.AddForce(Vector3.forward * 10);
    }
}
```

Burada objenin konumunu doğrudan değiştirmiyoruz.

Şunu söylüyoruz:

> Objeye ileri yönde kuvvet uygula.

Fizik motoru geri kalanını hesaplıyor.

---

## AddForce

En önemli Rigidbody metotlarından biridir.

```
rb.AddForce(Vector3.forward * 10);
```

Buradaki:

```
Vector3.forward
```

şudur:

```
(0, 0, 1)
```

Yani Z yönünde kuvvet uygulanır.

Örneğin:

```
rb.AddForce(Vector3.up * 5);
```

Yukarı doğru kuvvet uygular.

Bu, zıplama gibi sistemlerde kullanılabilir.

---

## Velocity / linearVelocity

Rigidbody'nin o anda hangi hızla ve yönde hareket ettiğini temsil eder.

Yeni Unity sürümlerinde:

```
rb.linearVelocity
```

kullanılır.

Örneğin:

```
rb.linearVelocity = new Vector3(0, 0, 5);
```

demek:

```
X = 0
Y = 0
Z = 5 m/s
```

Yani obje ileri doğru **5 unit/s** hızla hareket eder. Unity projelerinde varsayılan ölçek yaklaşımında 1 unit genellikle 1 metre kabul edilir.

Burada `AddForce` ile önemli bir fark var:

```
AddForce
→ kuvvet uygular
→ kütle ve fizik hesaplamaları devreye girer
→ hız zamanla değişebilir

linearVelocity
→ hızı doğrudan belirlersin
```

---

## Is Kinematic

Çok önemli bir ayardır.

Normal durumda:

```
Is Kinematic = false
```

Rigidbody fizik motoru tarafından kontrol edilir.

```
Yer çekimi
Kuvvet
Çarpışmalar
Momentum
↓
Rigidbody hareket eder
```

Ama:

```
Is Kinematic = true
```

yaparsan Rigidbody artık fizik kuvvetleri tarafından normal şekilde hareket ettirilmez.

Bunun yerine hareketi sen kontrol edersin.

Örneğin hareketli platformlar, kapılar veya animasyon tarafından kontrol edilen fizik objelerinde kullanılabilir.

---

## Constraints

Objenin belirli eksenlerde hareket etmesini veya dönmesini engeller.

Örneğin karakterin devrilmesini istemiyorsun:

```
Freeze Rotation

X ✓
Y
Z ✓
```

Böylece karakter sağa veya öne doğru devrilmez ama Y ekseninde dönmeye devam edebilir.

3D karakter controller yaparken oldukça sık kullanılır.

---

## Collision Detection

Objenin çarpışmalarının nasıl hesaplanacağını belirler.

Örneğin çok hızlı hareket eden küçük bir mermi düşün:

```
Duvar |   ● ---------->
```

Obje bir fizik frame'inde duvarın önünde, sonraki frame'de duvarın arkasında olabilir. Böyle olunca çarpışmayı kaçırabilir.

Bu nedenle Unity farklı collision detection seçenekleri sunar.

Genel olarak:

```
Discrete
→ normal objeler
→ daha ucuz

Continuous seçenekleri
→ hızlı hareket eden objeler
→ çarpışma kaçırma ihtimalini azaltır
→ daha maliyetli
```

---

## Interpolate

Fizik hesaplaması ile ekranda çizilen frameler arasındaki hareketin daha yumuşak görünmesine yardımcı olur.

Çünkü fizik genellikle:

```
FixedUpdate()
```

üzerinden ilerlerken görüntü:

```
Update()
```

ritminde çizilir.

Interpolate özellikle oyuncu gibi takip edilen Rigidbody objelerinde görülen küçük titremeleri azaltabilir.

---

## Neden FixedUpdate kullanıyoruz?

Rigidbody fizik işlemleri genellikle:

```
void FixedUpdate()
{
    rb.AddForce(...);
}
```

içerisinde yapılır.

Çünkü `FixedUpdate` fizik simülasyonunun sabit zaman adımıyla çalışmasına uygun tasarlanmıştır.

Örneğin:

```
void FixedUpdate()
{
    rb.AddForce(Vector3.forward * 10);
}
```

Rigidbody tabanlı hareketlerde temel yaklaşım budur.

---

## Transform ile Rigidbody arasındaki temel fark

Şunu yaparsan:

```
transform.position += Vector3.forward;
```

kabaca:

> Objeyi buraya taşı.

diyorsun.

Ama:

```
rb.AddForce(Vector3.forward * 10);
```

dediğinde:

> Objeye bu yönde kuvvet uygula.

diyorsun.

İkinci durumda Unity fizik motoru:

```
Mass
Gravity
Velocity
Collision
Damping
Force
Momentum
```

gibi fiziksel etkenleri hesaba katabilir.

Bu yüzden Rigidbody kullanıyorsan objeyi sürekli `Transform` üzerinden hareket ettirmek yerine Rigidbody'nin fizik API'lerini kullanmak genellikle daha doğru yaklaşımdır.

### Kısa özet

```
Rigidbody
   │
   ├── Mass              → Kütle
   ├── Use Gravity       → Yer çekimi
   ├── Linear Damping    → Hareket direnci
   ├── Angular Damping   → Dönme direnci
   ├── Is Kinematic      → Fizik mi, manuel kontrol mü?
   ├── Constraints       → Eksenleri kilitle
   ├── Collision Detection → Çarpışma hesaplama şekli
   └── Interpolate       → Görsel hareketi yumuşatma

Önemli kodlar:

AddForce()              → Kuvvet uygula
linearVelocity          → Hızı oku/değiştir
MovePosition()          → Rigidbody'yi konuma taşı
MoveRotation()          → Rigidbody'yi döndür
```

Unity fiziğini öğrenirken **Rigidbody → Collider → AddForce → ForceMode → FixedUpdate → collision/trigger** sırasıyla ilerlemek mantıklı olur.

