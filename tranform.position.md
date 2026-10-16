Tabii TkMatE. `transform.position`, GameObject'in **Unity dünyasındaki konumunu** tutar. Türü `Vector3`'tür.

```
transform.position
```

Aslında içerisinde üç değer vardır:

```
transform.position
        │
        └── Vector3
             ├── x
             ├── y
             └── z
```

Örneğin:

```
transform.position = new Vector3(5, 2, 10);
```

Nesneyi doğrudan:

```
X = 5
Y = 2
Z = 10
```

konumuna yerleştirir.

### `Translate` ile en önemli fark

Diyelim karakter şu anda:

```
Position = (10, 0, 5)
```

Şunu yaparsan:

```
transform.position = new Vector3(2, 0, 0);
```

karakter:

```
(10, 0, 5)
     ↓
(2, 0, 0)
```

konumuna **gider**. Yani eski konumunun üzerine ekleme yapmazsın, konumu değiştirirsin.

Ama:

```
transform.Translate(2, 0, 0);
```

kabaca:

```
Eski: (10, 0, 5)
       +
       (2, 0, 0)
       ↓
Yeni: (12, 0, 5)
```

olur.

Yani temel fark:

|Kod|Anlamı|
|---|---|
|`transform.position = ...`|Konumu **ayarla**|
|`transform.Translate(...)`|Mevcut konumdan **hareket ettir**|

### `position` değerini okuyabilirsin

Sadece değiştirmek zorunda değilsin:

```
Vector3 konum = transform.position;

Debug.Log(konum);
```

Örneğin Console:

```
(4.25, 1.00, 8.70)
```

gösterebilir.

Tek bir ekseni de okuyabilirsin:

```
float x = transform.position.x;
float y = transform.position.y;
float z = transform.position.z;
```

Örneğin:

```
Debug.Log(transform.position.x);
```

nesnenin X koordinatını verir.

### `position` ile hareket de yaptırabilirsin

Mesela:

```
void Update()
{
    transform.position += Vector3.forward * 5 * Time.deltaTime;
}
```

Buradaki `+=` önemli.

Aslında:

```
transform.position += Vector3.forward * 5 * Time.deltaTime;
```

şunun kısa halidir:

```
transform.position =
    transform.position + Vector3.forward * 5 * Time.deltaTime;
```

Diyelim:

```
Şu anki position:
(0, 0, 10)

Vector3.forward:
(0, 0, 1)

Eklendi:
(0, 0, 10)
+
(0, 0, 1)
──────────
(0, 0, 11)
```

Bunu her frame yaptığımız için nesne ileri doğru hareket eder.

### Ama önemli bir fark daha var

Şu:

```
transform.position += Vector3.forward * speed * Time.deltaTime;
```

**World Space** yönünü kullanır.

Yani nesneyi döndürsen bile:

```
Vector3.forward = dünyanın +Z yönü
```

değişmez.

Buna karşılık:

```
transform.Translate(Vector3.forward * speed * Time.deltaTime);
```

varsayılan kullanımda nesnenin **Local Space** yönünü dikkate alır.

Bu yüzden karakter döndüğü yöne doğru yürüsün istiyorsan bu fark çok önemlidir.

Örneğin nesne 90° sağa dönmüş olsun:

```
WORLD:

          +Z
           ↑
           |
           |
           ●──────→ +X
          karakter
             →
          baktığı yön
```

Şunların sonucu farklı olabilir:

```
transform.position += Vector3.forward * speed * Time.deltaTime;
```

Dünyanın **+Z yönüne** gider.

Ama:

```
transform.Translate(Vector3.forward * speed * Time.deltaTime);
```

Karakterin **kendi ileri yönüne** gider.

`transform.position`'dan sonra öğrenmen gereken en önemli şey aslında **`Vector3`**. Çünkü Unity hareket sisteminin büyük kısmı `Vector3`, `position`, `rotation` ve `Time.deltaTime` etrafında dönüyor.

Hareketi pekiştirecek örnekler

- position ile sağa-sola hareket