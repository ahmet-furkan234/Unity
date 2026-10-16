
**Collider**, Unity’de bir GameObject’in **fiziksel sınırlarını** belirleyen component’tir.

Basitçe:

> Rigidbody objenin **nasıl hareket edeceğini**, Collider ise objenin **nerede çarpışacağını** belirler.

Örneğin ekranda bir küp görüyorsun:

```
┌───────────────┐
│               │
│     KÜP       │
│               │
└───────────────┘
```

Küpün görüntüsünü `Mesh Renderer` oluşturur. Ancak fizik motorunun "bu nesnenin sınırı burada" diyebilmesi için **Collider** gerekir.

```
Görüntü:
██████████

Collider:
┌────────┐
│████████│
└────────┘
```

Collider normalde oyuncuya görünmez.

---

# Collider neden gerekli?

İki tane küp düşün:

```
Küp A  → → →       Küp B

[ A ]                [ B ]
```

Collider yoksa Unity fizik motoru açısından bu objelerin fiziksel sınırı yoktur.

A objesi B'nin içerisinden geçebilir:

```
        ↓

      [ A/B ]

        ↓

[ B ]          [ A ]
```

Collider eklediğimizde fizik motoru temas noktasını algılayabilir:

```
[ A ] → → → [ B ]
            ↑
         Çarpışma
```

---

# Collider türleri

Unity 3D'de birkaç temel Collider bulunur.

En önemlileri:

```
Box Collider
Sphere Collider
Capsule Collider
Mesh Collider
```

## 1. Box Collider

Kutu şeklinde fiziksel sınır oluşturur.

```
┌──────────────┐
│              │
│    Object    │
│              │
└──────────────┘
```

Örneğin:

- duvar
- zemin
- kutu
- masa
- bina

gibi nesnelerde kullanılır.

Inspector'da temel olarak:

```
Box Collider

Center   X Y Z
Size     X Y Z
```

bulunur.

### Center

Collider'ın objeye göre konumudur.

```
Center = (0,0,0)
```

Collider objenin merkezindedir.

Mesela:

```
Object
████████

Collider
┌──────┐
│██████│
└──────┘
```

Center'ı değiştirirsen Collider'ı kaydırabilirsin.

---

### Size

Collider'ın büyüklüğüdür.

Örneğin:

```
Size

X = 2
Y = 1
Z = 3
```

Collider'ın X, Y ve Z eksenlerindeki boyutlarını belirler.

---

# 2. Sphere Collider

Küre şeklinde Collider'dır.

```
      _____
   .-'     '-.
  /           \
 |      ●      |
  \           /
   '-._____.-'
```

Örneğin:

- top
- gezegen
- yuvarlak nesne

gibi objelerde kullanılır.

Inspector'daki önemli değerleri:

```
Center
Radius
```

`Radius`, kürenin yarıçapıdır.

```
Radius = 1
```

ise:

\[ çap = 2 \]

olur.

---

# 3. Capsule Collider

Kapsül şeklindedir.

```
    ___
  /     \
 |       |
 |       |
 |       |
  \_____/
```

Özellikle **karakterlerde** çok kullanılır.

İnsan modeline tam uymasa bile karakter hareketleri için oldukça pratiktir.

```
     O
    /|\
    / \

Collider:

    ___
  /     \
 |   O   |
 |  /|\  |
 |  / \  |
  \_____/
```

Neden insan şeklinde karmaşık Collider kullanmıyoruz?

Çünkü fizik hesaplaması gereksiz yere karmaşıklaşabilir.

Capsule Collider:

- basittir,
- performanslıdır,
- köşelere daha az takılır,
- karakter hareketine uygundur.

Temel özellikleri:

```
Center
Radius
Height
Direction
```

---

# 4. Mesh Collider

Burada Collider doğrudan objenin **3D mesh'ine** göre oluşturulabilir.

Örneğin düzensiz bir kaya:

```
       ___
   ___/   \_
 _/         \
/            \__
\_______________/
```

Box Collider kullanırsan:

```
┌────────────────┐
│       ___      │
│   ___/   \_    │
│ _/         \   │
│/            \__│
└────────────────┘
```

boş alanlar da fiziksel alan sayılabilir.

Mesh Collider ise şekle daha yakın olabilir:

```
       ___
   ___/   \_
 _/         \
/            \__
\_______________/
 ↑              ↑
 collider mesh'e
 yakın
```

Ancak bunun bedeli vardır:

> Mesh Collider genellikle primitive collider'lardan daha pahalıdır.

Bu yüzden mümkünse:

```
Box Collider
Sphere Collider
Capsule Collider
```

tercih edilir.

---

# Collider + Rigidbody ilişkisi

Bu ikisini ayrı düşünmek önemli.

### Sadece Collider

Örneğin zemin:

```
Ground
└── Box Collider
```

Rigidbody olmak zorunda değildir.

Böyle bir Collider fizik dünyasında **statik** bir engel olarak kullanılabilir.

### Rigidbody + Collider

Oyuncu:

```
Player
├── Rigidbody
└── Capsule Collider
```

Burada oyuncu hareket eden fiziksel bir objedir.

Örneğin:

```
       Player
         ↓
        ( )
        /|\
        / \
         ↓
════════════════════
       Ground
```

Player:

```
Rigidbody
Capsule Collider
```

Ground:

```
Box Collider
```

olabilir.

Yer çekimi oyuncuyu aşağı çeker.

Collider'lar temas eder:

```
Player Collider
      ↓
    ______
   /      \
  | Player |
   \______/
      ↑
──────────────
Ground Collider
```

Fizik motoru çarpışmayı çözer ve oyuncunun zeminin içinden geçmesini engeller.

---

# Collider tek başına hareket ettirmez

Çok önemli:

> Collider'ın görevi hareket ettirmek değildir.

Collider:

```
Nesnenin fiziksel sınırı nerede?
```

sorusuna cevap verir.

Rigidbody:

```
Nesne fizik dünyasında nasıl hareket ediyor?
```

sorusuna cevap verir.

Bu nedenle:

```
Collider ≠ Rigidbody
```

---

# Is Trigger

Collider'ın en önemli özelliklerinden biridir.

Normal Collider:

```
Is Trigger ❌
```

olduğunda fiziksel çarpışma oluşturur.

Örneğin:

```
Player → | WALL |

          DURUR
```

Ama:

```
Is Trigger ✓
```

yaparsan Collider fiziksel bir engelden çok **algılama bölgesi** gibi davranır.

Player içinden geçebilir:

```
Player → | Trigger | → Player
```

Ama Unity:

> Bir obje bu bölgeye girdi.

olayını algılayabilir.

Örneğin oyunlarda:

```
      Coin

      ( $ )
        ↑
   Trigger Area
```

Player buraya girdiğinde:

```
void OnTriggerEnter(Collider other)
{
    Debug.Log("Bir obje trigger'a girdi.");
}
```

çalışabilir.

---

# Trigger nerelerde kullanılır?

Çok yaygındır:

```
Coin toplama
Checkpoint
Kapı açma alanı
Görev başlatma
Hasar bölgesi
Finish çizgisi
NPC konuşma alanı
Düşman algılama alanı
```

Örneğin otomatik kapı:

```
          DOOR

          █ █

    ┌──────────────┐
    │ Trigger Area │
    │              │
    └──────────────┘

Player →
```

Oyuncu Trigger alanına girer:

```
void OnTriggerEnter(Collider other)
{
    if (other.CompareTag("Player"))
    {
        OpenDoor();
    }
}
```

Kapı açılır.

---

# OnCollisionEnter

Trigger kullanmıyorsan gerçek fiziksel çarpışmaları da yakalayabilirsin.

```C#
void OnCollisionEnter(Collision collision)
{
    Debug.Log("Çarpışma oldu!");
}
```

Örneğin:

```
Ball → → → Wall

 O  → → → █

           ↓

OnCollisionEnter()
```

---

# OnCollisionStay ve OnCollisionExit

Üç temel collision eventi vardır:

```
void OnCollisionEnter(Collision collision)
{
    Debug.Log("Temas başladı");
}

void OnCollisionStay(Collision collision)
{
    Debug.Log("Temas devam ediyor");
}

void OnCollisionExit(Collision collision)
{
    Debug.Log("Temas bitti");
}
```

Mantığı:

```
       WALL

        █
Ball →  █   Enter
Ball O  █   Stay
Ball ←  █   Exit
```

Yani:

```
Enter → ilk temas
Stay  → temas devam ettiği sürece
Exit  → temas bittiğinde
```

---

# Trigger'da da aynı sistem var

Trigger için:

```
void OnTriggerEnter(Collider other)
{
}

void OnTriggerStay(Collider other)
{
}

void OnTriggerExit(Collider other)
{
}
```

Mantık yine:

```
         Trigger
      ┌───────────┐

→ O   │           │
      └───────────┘
        ENTER

      ┌───────────┐
      │     O     │
      └───────────┘
        STAY

      ┌───────────┐
      │           │ O →
      └───────────┘
        EXIT
```

---

# Collision ile Trigger farkı

En önemli farkı şöyle ezberleyebilirsin:

|Collision|Trigger|
|---|---|
|Fiziksel temas|Algılama alanı|
|Objeyi engelleyebilir|Objeyi engellemez|
|`OnCollisionEnter`|`OnTriggerEnter`|
|Duvar, zemin vb.|Coin, checkpoint vb.|

Örneğin:

```
DUVAR
Player → █
         ↑
      Collision

COIN
Player → $ → Player
         ↑
       Trigger
```

---

# Collider görünüş değildir

Bu da önemli bir nokta.

Şöyle bir karakterin olabilir:

```
      O
     /|\
     / \
```

ama Collider'ı:

```
     _____
   /       \
  |    O    |
  |   /|\   |
  |   / \   |
   \_______/
```

olabilir.

Oyuncunun gördüğü model ile fizik motorunun kullandığı şekil **aynı olmak zorunda değildir**.

Hatta çoğu zaman aynı değildir.

---

# Bir objede birden fazla Collider olabilir

Örneğin araba:

```
        _________
   ____/         \____
  |                   |
 O                     O
```

Tek bir karmaşık Mesh Collider yerine birkaç Box Collider kullanılabilir:

```
       ┌─────────┐
       │         │
┌──────┴─────────┴──────┐
│                       │
└───────────────────────┘
```

Buna genel olarak **compound collider** yaklaşımı denir.

Hem şekle yaklaşabilirsin hem de çoğu durumda karmaşık Mesh Collider kullanmaktan daha uygun olabilir.

---

# Rigidbody + Collider'ı birlikte düşün

Şimdi önceki konuyla bağlayalım:

```
             GAMEOBJECT
                 │
       ┌─────────┴─────────┐
       ↓                   ↓
   Rigidbody            Collider
       │                   │
       ↓                   ↓
 Nasıl hareket       Fiziksel sınırı
 edecek?             nerede?
       │                   │
       └─────────┬─────────┘
                 ↓
           PHYSICS ENGINE
                 ↓
       Hareket + Çarpışma
```

Örneğin bir top:

```
Ball
├── Transform
├── Mesh Renderer
├── Sphere Collider
└── Rigidbody
```

Burada:

**Transform** → Top nerede?

**Mesh Renderer** → Top nasıl görünüyor?

**Sphere Collider** → Topun fiziksel sınırı nerede?

**Rigidbody** → Top nasıl hareket ediyor?

Bu dört kavramı ayırabilirsen Unity'deki fizik sisteminin temelini büyük ölçüde anlamış olursun.

Bir sonraki konu olarak **Collision vs Trigger'ı kod üzerinde küçük bir Unity örneğiyle** işlemek, ardından `Physics Material` ve `Layer Collision` konularına geçmek iyi bir sıra olur.

Collider konusunu pekiştir

- Collision ve Trigger kodunu yaz
- Physics Material’ı anlat