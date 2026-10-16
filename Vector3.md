`Vector3`, Unity'de **3 boyutlu bir değeri** tutan yapıdır. İçinde üç `float` vardır:

```
Vector3 position = new Vector3(5f, 2f, 10f);
```

Bunun anlamı:

```
X = 5
Y = 2
Z = 10
```

### Neden Vector3 var?

3D dünyada bir nesnenin konumunu tek bir sayı ile ifade edemezsin. Üç eksene ihtiyacın vardır:

```
        Y (+)
        ↑
        |
        |
        ●──────→ X (+)
       /
      /
     Z
```

Unity bu üç değeri tek bir yapı altında toplar:

```
Vector3
 ├── x
 ├── y
 └── z
```

Örneğin bir objenin pozisyonu zaten `Vector3`'tür:

```
Vector3 position = transform.position;
```

Çünkü objenin pozisyonunun X, Y ve Z değerleri vardır.

Tek tek de ulaşabilirsin:

```
Debug.Log(transform.position.x);
Debug.Log(transform.position.y);
Debug.Log(transform.position.z);
```

### `new Vector3()` ne yapıyor?

Şunu yazdığımızda:

```
Vector3 movement = new Vector3(1, 0, 3);
```

yeni bir `Vector3` değeri oluşturuyoruz:

```
movement
   ↓
┌─────────────┐
│ x = 1       │
│ y = 0       │
│ z = 3       │
└─────────────┘
```

Bu bir **pozisyon olmak zorunda değil**. Çok önemli nokta bu.

`Vector3` sadece üç sayıyı temsil eder. Bu üç sayı farklı anlamlarda kullanılabilir.

Mesela **pozisyon**:

```
Vector3 position = new Vector3(10, 2, 5);
```

veya **hareket yönü**:

```
Vector3 direction = new Vector3(1, 0, 0);
```

Burada:

```
X = 1 → sağa git
Y = 0 → yukarı/aşağı gitme
Z = 0 → ileri/geri gitme
```

### `Input.GetAxis` ile neden kullandık?

Az önce:

```
float x = Input.GetAxis("Horizontal");
float z = Input.GetAxis("Vertical");
```

yapmıştık.

Diyelim oyuncu **D + W** basıyor:

```
Horizontal = 1
Vertical   = 1
```

Bunları:

```
Vector3 movement = new Vector3(x, 0, z);
```

içine koyuyoruz.

Sonuç:

```
new Vector3(1, 0, 1)
```

Yani:

```
X = 1 → sağa
Y = 0 → dikey hareket yok
Z = 1 → ileri
```

Dolayısıyla karakter **sağ-ileri çapraz** hareket eder.

Sonra:

```
transform.Translate(movement);
```

dediğimizde Unity bu Vector3'ü hareket miktarı olarak kullanabilir.

Gerçek kullanımda:

```
Vector3 movement = new Vector3(x, 0, z);

transform.Translate(
    movement * speed * Time.deltaTime
);
```

yazarız.

Buradaki mantığı oturtursan Unity'nin büyük bir kısmı daha anlaşılır hale gelir. Çünkü `Vector3` sadece harekette değil; **position, direction, velocity, scale, force** gibi birçok yerde karşına çıkacak.

Bir sonraki önemli konu da `Vector3(1,0,1)` neden `Vector3(1,0,0)`'dan daha hızlı hareket ettirebilir ve `.normalized` neden kullanılır konusu.

Vector3 hareketini pekiştir

- Çapraz hareket sorununu göster
- normalized kullanımını açıkla