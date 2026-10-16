
Unity'de `Time.time`, **oyun başladığından beri geçen süreyi saniye cinsinden** verir.

```
Debug.Log(Time.time);
```

Örneğin oyun çalışmaya başladıktan sonra değerler kabaca şöyle ilerler:

```
0.0
0.5
1.0
2.7
5.4
10.2
...
```

Yani:

> **`Time.time` = Oyun başlayalı kaç saniye geçti?**

### Basit kullanım

```
void Update()
{
    Debug.Log(Time.time);
}
```

Oyun 5 saniyedir çalışıyorsa:

```
5.023
```

gibi bir değer görebilirsin.

---

## `Time.time` ile zaman kontrolü

Asıl kullanışlı olduğu nokta budur. Örneğin bir işlemi **3 saniye sonra** yapmak istiyoruz:

```
void Update()
{
    if (Time.time > 3f)
    {
        Debug.Log("3 saniye geçti!");
    }
}
```

Ama burada önemli bir problem var.

`Update()` her frame çalıştığı için 3 saniyeden sonra:

```
3.01 → çalışır
3.02 → çalışır
3.03 → çalışır
3.04 → çalışır
...
```

Sürekli çalışır.

Bir kere çalıştırmak istiyorsak:

```
bool calisti = false;

void Update()
{
    if (Time.time > 3f && !calisti)
    {
        Debug.Log("3 saniye geçti!");
        calisti = true;
    }
}
```

---

## En yaygın kullanım: Cooldown

Mesela oyuncu **2 saniyede bir ateş edebilsin.**

```
public float cooldown = 2f;
private float nextFireTime = 0f;

void Update()
{
    if (Input.GetKeyDown(KeyCode.Space) && Time.time >= nextFireTime)
    {
        Fire();

        nextFireTime = Time.time + cooldown;
    }
}

void Fire()
{
    Debug.Log("Ateş!");
}
```

Mantığı çok önemli.

İlk ateşi oyun başladıktan **10. saniyede** yaptığını düşün:

```
Time.time = 10
cooldown = 2
```

Şunu hesaplıyoruz:

```
nextFireTime = Time.time + cooldown;
```

yani:

```
nextFireTime = 10 + 2;
nextFireTime = 12;
```

Artık:

```
Time.time >= nextFireTime
```

şartının sağlanması için oyunun **12. saniyeye ulaşması gerekir.**

```
10.0  ❌
10.5  ❌
11.0  ❌
11.9  ❌
12.0  ✅ Ateş edebilir
```

Bu yapı oyunlarda çok kullanılır:

```
Silah ateş hızı
Skill cooldown
Düşman saldırı aralığı
Spawn süreleri
Hasar alma bekleme süresi
Animasyon zamanlaması
```

---

## `Time.time` ile `Time.deltaTime` farkı

Bunları karıştırmamak önemli:

||`Time.time`|`Time.deltaTime`|
|---|---|---|
|Anlamı|Oyun başlayalı geçen toplam süre|Son frame'in ne kadar sürdüğü|
|Örnek|`15.72 saniye`|`0.016 saniye`|
|Sürekli artar mı?|Evet|Hayır|
|Hareket hesabında?|Genellikle hayır|Evet|
|Cooldown/zaman kontrolünde?|Evet|Kullanılabilir ama farklı yöntemle|

Örneğin:

```
transform.Translate(Vector3.forward * speed * Time.deltaTime);
```

Burada `deltaTime` kullanıyoruz çünkü:

> **Bu frame'de ne kadar hareket etmeliyim?**

sorusunu soruyoruz.

Ama:

```
if (Time.time >= nextFireTime)
```

burada:

> **Oyunun zamanı belirlediğim noktaya ulaştı mı?**

sorusunu soruyoruz.

### Aklında şöyle tut

```
Time.time
↓
⏱️ Kronometrede şu anda kaçıncı saniyedeyiz?

Time.deltaTime
↓
⏱️ Bir önceki frame'den bu frame'e kaç saniye geçti?
```

Örneğin oyun 60 FPS çalışıyor ve başlayalı 10 saniye olmuşsa yaklaşık:

```
Time.time
// 10.0

Time.deltaTime
// 0.0167
```

Bir sonraki frame'de:

```
Time.time
// 10.0167

Time.deltaTime
// 0.0167
```

Dolayısıyla **`Time.time` toplam zamanı**, **`Time.deltaTime` ise iki frame arasındaki küçük zaman parçasını** temsil eder.

Bir sonraki mantıklı konu olarak `Time.timeScale`a geçebiliriz; özellikle oyunu **pause etme, slow-motion yapma ve `Time.time` / `deltaTime` ile ilişkisini** anlamak için önemli.

Time.time kullanımını pekiştir

- Time.timeScale ile pause ve slow-motion
- Time.time ile tekrar eden zamanlayıcı