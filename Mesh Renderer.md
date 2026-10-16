
**Mesh Renderer**, bir 3D nesnenin **ekranda görünmesini sağlayan** component’tir.

En basit haliyle:

> **Mesh Filter** → Nesnenin şekli ne?  
> **Mesh Renderer** → Bu şekli ekranda nasıl çizelim?

Örneğin Unity'de bir `Cube` oluşturduğunda kabaca şöyle bir yapı vardır:

```
Cube
├── Transform
├── Mesh Filter
├── Mesh Renderer
└── Box Collider
```

Buradaki görevler farklıdır:

```
Transform      → Nerede, hangi açıda, ne büyüklükte?
Mesh Filter    → Hangi 3D geometrisi kullanılacak?
Mesh Renderer  → Bu geometri ekranda nasıl çizilecek?
Box Collider   → Fiziksel sınırı nerede?
```

---

# Önce Mesh nedir?

`Mesh Renderer`ı anlamak için önce **Mesh** kavramını bilmek gerekiyor.

Mesh, 3D nesnenin geometrik yapısıdır.

Bir 3D model aslında çok sayıda:

- Vertex (köşe noktası)
- Edge (kenar)
- Triangle (üçgen)

kullanılarak oluşturulur.

Örneğin çok basitleştirilmiş bir yüzey:

```
A──────B
│\     │
│  \   │
│    \ │
D──────C
```

Bu yüzey iki triangle olarak düşünülebilir:

```
A-B-C
A-C-D
```

GPU en sonunda bu üçgenleri kullanarak modeli çizer.

Yani:

```
Vertex'ler
    ↓
Triangle'lar
    ↓
Mesh
    ↓
3D Model
```

---

# Mesh Filter ne yapıyor?

Mesh Filter, GameObject'in **hangi Mesh'i kullanacağını** tutar.

Örneğin bir Cube seçersen:

```
Mesh Filter
└── Mesh: Cube
```

Sphere seçersen:

```
Mesh Filter
└── Mesh: Sphere
```

Dolayısıyla Mesh Filter:

> "Benim geometrim bu."

der.

Ama tek başına Mesh Filter nesneyi ekrana çizmez.

Bunun için **Mesh Renderer** gerekir.

---

# Mesh Renderer ne yapıyor?

Mesh Renderer, Mesh Filter'daki Mesh'i alır ve Unity'nin rendering sistemi aracılığıyla kameraya çizdirir.

Kabaca:

```
Mesh Filter
    │
    │ Mesh
    ↓
Mesh Renderer
    │
    │ Material / Lighting / Shadow
    ↓
   GPU
    ↓
 Camera
    ↓
 Ekran
```

Yani:

```
Mesh Filter = Ne çizilecek?
Mesh Renderer = Nasıl çizilecek?
```

Bu ayrım çok önemli.

---

# Basit bir örnek

Bir Cube oluşturdun:

```
Cube
├── Transform
├── Mesh Filter
│      └── Cube Mesh
│
├── Mesh Renderer
│      └── Material
│
└── Box Collider
```

Burada:

```
Mesh Filter
→ Küp geometrisini sağlar.

Mesh Renderer
→ Küp geometrisini görüntüler.

Box Collider
→ Küpün fiziksel sınırını oluşturur.
```

---

# Mesh Renderer'ı kapatırsak ne olur?

Inspector'dan:

```
Mesh Renderer
☐ Enabled
```

yaparsan nesne **görünmez olur.**

Ama GameObject ortadan kalkmaz.

Örneğin:

```
Cube
├── Transform       ✓
├── Mesh Filter     ✓
├── Mesh Renderer   ✗
├── Box Collider    ✓
└── Rigidbody       ✓
```

Bu durumda:

> Küp görünmez ama fiziksel olarak hâlâ orada olabilir.

Oyuncu görünmez bir duvara çarpabilir.

```
Player →        █ ← görünmüyor
         DURDU!
```

Çünkü görünürlüğü sağlayan:

```
Mesh Renderer
```

kapalıdır ama:

```
Box Collider
```

hâlâ aktiftir.

Bu bize önemli bir ayrımı gösteriyor:

> **Görüntü sistemi ile fizik sistemi birbirinden ayrıdır.**

---

# Material nedir?

Mesh Renderer'ın en önemli kısımlarından biri **Materials** bölümüdür.

Örneğin:

```
Mesh Renderer

Materials
└── Element 0
      └── RedMaterial
```

Material, yüzeyin nasıl görüneceğini belirler.

Örneğin:

```
Renk
Texture
Parlaklık
Metalik görünüm
Saydamlık
Normal map
```

gibi özellikler Material üzerinden kontrol edilebilir.

Kabaca:

```
Mesh
+
Material
=
Görünen nesne
```

Örneğin aynı Cube Mesh:

```
Cube Mesh + Red Material
→ Kırmızı küp

Cube Mesh + Metal Material
→ Metalik küp

Cube Mesh + Wood Material
→ Ahşap görünümlü küp
```

Geometri değişmedi.

Sadece Material değişti.

---

# Material ile Texture aynı şey değil

Bunlar çok karıştırılır.

**Texture**, genellikle bir görüntüdür.

Örneğin:

```
wood.png
brick.png
grass.png
```

Material ise bu texture'ı kullanabilir.

```
Texture
   ↓
Material
   ↓
Mesh Renderer
   ↓
Mesh
   ↓
Ekran
```

Örneğin:

```
brick.png
    ↓
Brick Material
    ↓
Cube Mesh
    ↓

🧱 Tuğla görünümlü küp
```

Dolayısıyla:

```
Mesh      → Şekil
Texture   → Görsel veri/resim
Material  → Yüzeyin nasıl çizileceği
Renderer  → Nesneyi çizdirir
```

---

# Shader nerede devreye giriyor?

Bir katman daha aşağı indiğimizde **Shader** vardır.

Material aslında bir Shader kullanır.

```
Mesh
 ↓
Material
 ↓
Shader
 ↓
GPU
 ↓
Pixel
```

Shader GPU'ya kabaca:

> "Bu yüzeyi nasıl çizmelisin?"

der.

Örneğin Shader;

- ışığın yüzeye etkisini,
- rengin nasıl hesaplanacağını,
- texture'ın nasıl kullanılacağını,
- metalikliği,
- transparanlığı

belirleyebilir.

Bu yüzden daha doğru ilişki:

```
Mesh Renderer
      │
      ├── Mesh'i al
      │
      └── Material'ı kullan
               │
               ↓
             Shader
               │
               ↓
              GPU
               │
               ↓
             Ekran
```

---

# Cast Shadows

Mesh Renderer içerisinde göreceğin özelliklerden biridir.

```
Cast Shadows
```

Nesnenin başka yüzeylere gölge oluşturup oluşturmayacağını belirler.

Örneğin:

```
       ☀ Light

          ↓

         Cube
        ┌────┐
        │    │
        └────┘
           \
            \____
             Shadow
```

`Cast Shadows` kapatılırsa küp görünmeye devam eder ama gölge oluşturmayabilir.

---

# Receive Shadows

Bu da nesnenin **başka nesnelerin gölgelerini alıp almayacağını** kontrol eder.

Örneğin zemin:

```
      Cube
       █
       █
       ↓
─────────────────
      Ground
```

Küpün gölgesi Ground üzerinde görünüyorsa Ground bu gölgeyi **receive** ediyor.

Kısaca:

```
Cast Shadow
→ Ben gölge oluşturayım mı?

Receive Shadow
→ Üzerime düşen gölgeler görünsün mü?
```

---

# Mesh Renderer ile Collider arasındaki fark

Burası özellikle önemli:

```
        GameObject
            │
     ┌──────┴──────┐
     ↓             ↓
Mesh Renderer   Collider
     │             │
     ↓             ↓
 Görsel dünya   Fizik dünyası
```

Mesh Renderer:

> Oyuncu bu nesneyi nasıl görecek?

Collider:

> Fizik motoru bu nesnenin sınırını nasıl görecek?

Bu nedenle ikisinin şekli aynı olmak zorunda değildir.

Örneğin karmaşık bir kaya:

```
Mesh Renderer:

       ___
   ___/   \_
 _/         \__
/              \
\_______________/


Collider:

┌────────────────┐
│                │
│                │
└────────────────┘
```

Oyuncu karmaşık kaya modelini görür.

Fizik motoru ise basit bir Box Collider kullanabilir.

Bu performans açısından da faydalıdır.

---

# Renderer kod üzerinden kontrol edilebilir

Örneğin:

```
MeshRenderer renderer;

void Start()
{
    renderer = GetComponent<MeshRenderer>();
}
```

GameObject üzerindeki `MeshRenderer` component'ini almış olduk.

Sonra:

```
renderer.enabled = false;
```

dediğimizde nesnenin Renderer'ını kapatabiliriz.

Nesne görünmez olur.

Tekrar:

```
renderer.enabled = true;
```

yaparsak görünür olur.

---

# Şimdi parçaları birleştirelim

Bir Player düşün:

```
Player
│
├── Transform
│
│   Nerede?
│   Ne kadar büyük?
│   Hangi yöne bakıyor?
│
├── Mesh Filter
│
│   3D şekli ne?
│
├── Mesh Renderer
│
│   Nasıl görünecek?
│
├── Collider
│
│   Fiziksel sınırı nerede?
│
└── Rigidbody
    │
    Fiziksel olarak nasıl hareket edecek?
```

Bunları tek cümleyle ezberleyebilirsin:

```
Transform
→ NEREDE?

Mesh Filter
→ ŞEKLİ NE?

Mesh Renderer
→ NASIL GÖRÜNECEK?

Collider
→ FİZİKSEL SINIRI NE?

Rigidbody
→ NASIL HAREKET EDECEK?
```

Özellikle **Mesh Filter + Mesh Renderer** ikilisini beraber düşün:

```
Mesh Filter
"Şu modeli kullan."

       ↓

Mesh Renderer
"Tamam, onu şu Material ile çizeyim."

       ↓

Camera

       ↓

Ekranda görürsün.
```

Buradan sonra **Material → Texture → Shader** üçlüsüne geçmek mantıklı; çünkü Mesh Renderer'ın "nasıl görünecek?" kısmının asıl detayları orada başlıyor.

Mesh Renderer’ı pekiştirelim

- Material ve Texture farkını öğren
- Shader mantığını öğren