
`GetComponent`, bir GameObject'in üzerindeki **component'e kod içerisinden ulaşmamızı** sağlar.

Az önce öğrendiğimiz yapıyı düşün:

```
Player
├── Transform
├── Mesh Filter
├── Mesh Renderer
├── Capsule Collider
├── Rigidbody
└── PlayerMovement (Script)
```

`PlayerMovement` scriptinin içerisinden Rigidbody'ye ulaşmak istiyoruz.

İşte burada:

```
GetComponent<Rigidbody>()
```

kullanabiliriz.

Anlamı:

> Bu GameObject'in üzerindeki `Rigidbody` component'ini bul ve bana ver.

---

# Temel kullanım

Örneğin:

```
Rigidbody rb;

void Start()
{
    rb = GetComponent<Rigidbody>();
}
```

Burada adım adım şu oluyor:

```
GetComponent<Rigidbody>()
             ↑
       Aradığımız component
```

Unity scriptin bulunduğu GameObject'e bakar:

```
Player
├── Transform
├── Mesh Renderer
├── Capsule Collider
├── Rigidbody       ← BULDUM
└── PlayerMovement
```

ve o Rigidbody'nin referansını döndürür.

Biz de:

```
rb = ...
```

ile bu referansı `rb` değişkenine kaydediyoruz.

---

# `<Rigidbody>` kısmı nedir?

Şuradaki:

```
GetComponent<Rigidbody>()
```

`<Rigidbody>` hangi tür component'i istediğimizi belirtir.

Örneğin:

```
GetComponent<Rigidbody>();
GetComponent<BoxCollider>();
GetComponent<MeshRenderer>();
GetComponent<AudioSource>();
```

Hepsinde mantık aynı:

```
GetComponent<T>()
             ↑
       Aranacak tür
```

Buradaki yapı C#'taki **generic** yapıdır.

Yani `GetComponent` aslında kabaca:

> Bana `T` türündeki component'i getir.

der.

---

# Neden değişkene atıyoruz?

Şunu yazabiliriz:

```
GetComponent<Rigidbody>().AddForce(Vector3.forward * 10);
```

Bu çalışabilir.

Ama sürekli Rigidbody kullanacaksak her seferinde:

```
GetComponent<Rigidbody>()
```

yazmak istemeyiz.

Bunun yerine bir kere alırız:

```
private Rigidbody rb;

void Start()
{
    rb = GetComponent<Rigidbody>();
}
```

Sonra:

```
rb.AddForce(...);
rb.linearVelocity = ...;
rb.mass = ...;
```

şeklinde kullanırız.

Mantık:

```
GameObject
    ↓
GetComponent<Rigidbody>()
    ↓
Rigidbody bulundu
    ↓
rb değişkenine kaydet
    ↓
Artık rb üzerinden kullan
```

---

# Tam örnek

Örneğin fizik tabanlı basit hareket:

```
using UnityEngine;

public class Mover : MonoBehaviour
{
    private Rigidbody rb;

    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    void FixedUpdate()
    {
        rb.AddForce(Vector3.forward * 10);
    }
}
```

Burada:

```
private Rigidbody rb;
```

Rigidbody referansını tutacak değişken.

```
rb = GetComponent<Rigidbody>();
```

Player'ın Rigidbody'sini buluyor.

```
rb.AddForce(...)
```

bulduğumuz Rigidbody'yi kullanıyor.

---

# GetComponent nerede arama yapıyor?

Çok önemli nokta:

```
GetComponent<Rigidbody>()
```

varsayılan olarak **scriptin bulunduğu aynı GameObject'te** arar.

Şöyleyse:

```
Player
├── Rigidbody
└── PlayerMovement
```

ve `PlayerMovement` içinde:

```
GetComponent<Rigidbody>();
```

kullanırsan Rigidbody bulunur. ✅

Ama şöyleyse:

```
Player
│
├── Rigidbody
│
└── Model
    └── PlayerMovement
```

`PlayerMovement`, `Model` üzerinde olduğu için:

```
GetComponent<Rigidbody>()
```

`Player`ın Rigidbody'sini doğrudan bulmaz.

Çünkü aynı GameObject üzerinde değildir.

---

# Bulamazsa ne olur?

Örneğin:

```
Player
├── Capsule Collider
└── PlayerMovement
```

ama script içerisinde:

```
rb = GetComponent<Rigidbody>();
```

yaptın.

Player'da Rigidbody yok.

Bu durumda `rb`:

```
null
```

olur.

Sonrasında:

```
rb.AddForce(Vector3.forward);
```

dersen büyük ihtimalle:

```
NullReferenceException
```

alırsın.

Çünkü aslında şunu yapmaya çalışıyorsun:

```
rb
↓
null

null.AddForce(...)
```

Ortada kullanılacak Rigidbody yok.

---

# TryGetComponent

Bunun daha güvenli bir alternatifi de vardır:

```
if (TryGetComponent<Rigidbody>(out Rigidbody rb))
{
    rb.AddForce(Vector3.forward * 10);
}
```

Mantığı:

> Rigidbody varsa getir ve işlemi yap.

Yoksa `if` bloğuna girmez.

Örneğin:

```
void Start()
{
    if (TryGetComponent<Rigidbody>(out Rigidbody rb))
    {
        Debug.Log("Rigidbody bulundu!");
    }
    else
    {
        Debug.Log("Rigidbody bulunamadı!");
    }
}
```

---

# GetComponent ile Mesh Renderer örneği

Az önce öğrendiğimiz Mesh Renderer'a bağlayalım.

```
private MeshRenderer meshRenderer;

void Start()
{
    meshRenderer = GetComponent<MeshRenderer>();
}
```

Sonra:

```
meshRenderer.enabled = false;
```

dersek Renderer kapanır.

```
Player
├── Mesh Renderer ← GetComponent bunu buluyor
└── Script
```

Sonuç:

```
Mesh Renderer disabled
        ↓
Nesne görünmez
```

Tekrar:

```
meshRenderer.enabled = true;
```

yaparsak görünür.

---

# GetComponent ve public/SerializeField farkı

Daha önce gördüğümüz `[SerializeField]` ile karıştırmamak önemli.

Şöyle yapabiliriz:

```
[SerializeField]
private Rigidbody rb;
```

Bu durumda Rigidbody'yi Inspector'dan kendimiz sürükleyip bırakabiliriz.

```
Inspector

Mover Script
└── Rb: [Player Rigidbody]
```

Ama:

```
private Rigidbody rb;

void Start()
{
    rb = GetComponent<Rigidbody>();
}
```

yaparsak Unity çalışma sırasında **aynı GameObject'teki Rigidbody'yi kendisi bulur.**

Yani:

```
[SerializeField]
→ Referansı Inspector'dan veriyoruz.

GetComponent
→ Referansı kod içerisinden buluyoruz.
```

İkisi de component'e referans elde etmenin yollarıdır.

---

# GetComponent her frame kullanılmalı mı?

Genellikle şöyle yapmak iyi değildir:

```
void Update()
{
    GetComponent<Rigidbody>().AddForce(...);
}
```

Çünkü `Update()` sürekli çalışır ve component referansını tekrar tekrar istemiş olursun.

Çoğu durumda daha mantıklısı referansı bir kez alıp saklamaktır:

```
private Rigidbody rb;

void Awake()
{
    rb = GetComponent<Rigidbody>();
}
```

Sonra:

```
void FixedUpdate()
{
    rb.AddForce(...);
}
```

Buna **caching** diyebilirsin:

```
Bir kere bul
    ↓
Referansı sakla
    ↓
Tekrar tekrar kullan
```

---

# Neden Awake kullandık?

Component referanslarını almak için sıklıkla:

```
void Awake()
{
    rb = GetComponent<Rigidbody>();
}
```

görürsün.

`Awake`, obje/script hazırlanırken çalışır.

`Start` da kullanılabilir:

```
void Start()
{
    rb = GetComponent<Rigidbody>();
}
```

Başlangıç seviyesinde ikisi de karşına çok çıkar. `Awake` ve `Start` arasındaki **çalışma sırası** konusuna ayrıca geçmek daha doğru olur.

---

# GetComponent sadece Unity'nin hazır componentlerinde kullanılmaz

Kendi yazdığın scriptleri de alabilirsin.

Mesela:

```
Player
├── Rigidbody
├── Health
└── PlayerMovement
```

`Health.cs` kendi scriptin olsun.

`PlayerMovement` içerisinde:

```
Health health;

void Awake()
{
    health = GetComponent<Health>();
}
```

yapabilirsin.

Sonra örneğin:

```
health.TakeDamage(20);
```

kullanabilirsin.

Yani `GetComponent` sadece:

```
Rigidbody
Collider
MeshRenderer
```

için değil, kendi component/scriptlerin için de çalışır.

---

# En önemli mantık

Unity'de scriptler de aslında **Component**'tir.

Bu yüzden:

```
Player
│
├── Rigidbody
│
├── CapsuleCollider
│
├── MeshRenderer
│
├── Health
│
└── PlayerMovement
```

bir GameObject'in üzerine takılmış farklı yetenekler gibi düşünülebilir.

`PlayerMovement`:

```
GetComponent<Rigidbody>()
```

dediğinde:

```
"Benim bulunduğum GameObject'te
 Rigidbody component'i var mı?"
          ↓
         VAR
          ↓
   Referansını döndür
```

demiş olur.

## Kısa not

```
GetComponent<T>()
→ Aynı GameObject üzerindeki T component'ini getirir.

Örnek:
GetComponent<Rigidbody>()
GetComponent<MeshRenderer>()
GetComponent<BoxCollider>()
GetComponent<Health>()

Bulamazsa:
→ null

Genellikle:
Awake/Start → component'i bul
Değişkende   → referansı sakla
Update       → saklanan referansı kullan

Örnek:

private Rigidbody rb;

void Awake()
{
    rb = GetComponent<Rigidbody>();
}

void FixedUpdate()
{
    rb.AddForce(Vector3.forward * 10);
}
```

Burada özellikle anlaman gereken ana fikir şu: **`GetComponent` yeni bir Rigidbody oluşturmuyor. Zaten GameObject üzerinde bulunan Rigidbody'yi bulup onun referansını sana veriyor.**

GetComponent kullanımını pekiştir

- GetComponentInChildren ve GetComponentInParent öğren
- GetComponent ile hareket scripti yaz