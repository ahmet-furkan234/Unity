TkMatE, Unity'de `Transform.Translate()` bir **GameObject'i bulunduğu konumdan belirli bir miktar hareket ettirmek** için kullanılır.

Örneğin:

```
void Update()
{
    transform.Translate(1, 0, 0);
}
```

Buradaki değerler:

```
             Y
             ↑
             |
             |
             +------→ X
            /
           /
          Z
```

Yani:

```
transform.Translate(x, y, z);
```

- `x` → sağ/sol
- `y` → yukarı/aşağı
- `z` → ileri/geri

### Örneğin karakteri ileri hareket ettirmek

```
void Update()
{
    transform.Translate(0, 0, 5 * Time.deltaTime);
}
```

veya daha okunaklı hali:

```
void Update()
{
    transform.Translate(Vector3.forward * 5 * Time.deltaTime);
}
```

Burada:

```
Vector3.forward
      ↓
(0, 0, 1)

        × 5
      hızımız

        × Time.deltaTime
      FPS'ten bağımsız hareket
```

Dolayısıyla kabaca:

```
transform.Translate(Vector3.forward * 5 * Time.deltaTime);
```

şu anlama gelir:

> "Bu GameObject'i kendi ileri yönünde saniyede yaklaşık **5 Unity birimi** hareket ettir."

### `transform.position` ile farkı

Şunu yaparsan:

```
transform.position = new Vector3(10, 0, 0);
```

nesneyi **doğrudan `(10,0,0)` konumuna koyarsın.**

Ama:

```
transform.Translate(10, 0, 0);
```

nesneyi **mevcut konumundan 10 birim hareket ettirirsin.**

Mesela nesne:

```
Başlangıç:
X = 20

Translate(10, 0, 0)
        ↓
X = 30
```

Yani `Translate` kelime olarak da **"ötele / taşı"** mantığında.

Bir de önemli detay var: `Translate()` varsayılan olarak nesnenin **kendi yönlerini (local space)** dikkate alabilir. Yani nesneyi döndürdüğünde `Vector3.forward` ile hareketinin yönü de değişebilir. Unity'de karakter hareketinde bunu çok göreceksin.