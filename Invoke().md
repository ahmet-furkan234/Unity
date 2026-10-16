
Unity'de `Invoke()`, belirli bir metodu belirlediğimiz süre kadar gecikmeli çalıştırmamızı sağlayan bir `MonoBehaviour` metodudur.

Örneğin, bir düşmanın 3 saniye sonra yok olmasını, bir kapının 5 saniye sonra kapanmasını veya bir patlamanın gecikmeli gerçekleşmesini sağlayabiliriz.

## 1. Invoke() Kullanımı

```
Invoke("MethodAdi", gecikmeSuresi);
```

|Parametre|Açıklama|
|---|---|
|`methodName`|Çalıştırılacak metodun adı|
|`time`|Metodun kaç saniye sonra çalışacağı|

Örnek:

```
using UnityEngine;

public class Player : MonoBehaviour
{
    void Start()
    {
        Invoke(nameof(SelamVer), 3f);
    }

    void SelamVer()
    {
        Debug.Log("Merhaba Unity!");
    }
}
```

Invoke çalışma zaman çizelgesi

Start() çağrıldı

0.0 saniye

SelamVer() bekliyor...

Sıfırla

Kaydırıcıyla zamanı ilerleterek 3. saniyedeki çağrıyı görebilirsin.

Çalışma mantığı:

1. Oyun başladığında `Start()` çalışır.
    
2. `Invoke()` metodu 3 saniye sonrasına planlar.
    
3. Unity bu sırada oyunu çalıştırmaya devam eder.
    
4. Süre dolduğunda `SelamVer()` çalıştırılır.
    

Önemli: `Invoke()` oyunu veya ana thread'i 3 saniye durdurmaz. Yalnızca metodun çalışmasını geciktirir.

## 2. Invoke() ile GameObject Yok Etme

```
using UnityEngine;

public class Enemy : MonoBehaviour
{
    void Start()
    {
        Invoke(nameof(Die), 5f);
    }

    void Die()
    {
        Destroy(gameObject);
    }
}
```

Bu örnekte düşman oluşturulduktan 5 saniye sonra yok edilir.

Aslında yalnızca nesneyi gecikmeli yok etmek istiyorsak daha kısa bir yöntem de vardır:

```
Destroy(gameObject, 5f);
```

## 3. InvokeRepeating() – Sürekli Tekrarlama

`Invoke()` bir metodu bir kez çalıştırırken, `InvokeRepeating()` belirli aralıklarla tekrar tekrar çalıştırır.

```
InvokeRepeating("MethodAdi", ilkGecikme, tekrarAraligi);
```

Örnek:

```
using UnityEngine;

public class EnemySpawner : MonoBehaviour
{
    void Start()
    {
        InvokeRepeating(nameof(SpawnEnemy), 2f, 3f);
    }

    void SpawnEnemy()
    {
        Debug.Log("Yeni düşman oluşturuldu!");
    }
}
```

InvokeRepeating zaman çizelgesi

- 0. saniye: `Start()` çalışır.
        
    
- 2. saniye: İlk düşman oluşturulur.
        
    
- 5. saniye: İkinci düşman oluşturulur.
        
    
- 8. saniye: Üçüncü düşman oluşturulur.
        
    
- 11. saniye: Dördüncü düşman oluşturulur.
        
    

Bu işlem iptal edilene veya ilgili davranış sona erene kadar devam eder.

## 4. CancelInvoke() – Zamanlanmış Çağrıyı İptal Etme

`CancelInvoke()` henüz çalışmamış `Invoke()` çağrılarını ve devam eden `InvokeRepeating()` tekrarlarını iptal eder.

```
void Start()
{
    InvokeRepeating(nameof(SpawnEnemy), 2f, 3f);
}

void Update()
{
    if (Input.GetKeyDown(KeyCode.Space))
    {
        CancelInvoke(nameof(SpawnEnemy));
    }
}

void SpawnEnemy()
{
    Debug.Log("Düşman oluşturuldu!");
}
```

Burada Space tuşuna basıldığında düşman oluşturma işlemi durdurulur.

Not: Bu örnek eski Input API'sini kullanır. Projen yalnızca yeni Input System ile yapılandırılmışsa `Keyboard.current.spaceKey.wasPressedThisFrame` kullanılabilir.

İki farklı kullanım vardır:

```
CancelInvoke(); // Bu bileşendeki bütün Invoke çağrılarını iptal eder.

CancelInvoke(nameof(SpawnEnemy)); // Yalnızca SpawnEnemy çağrılarını iptal eder.
```

## 5. IsInvoking() – Metot Zamanlanmış mı?

`IsInvoking()` bir metodun çalıştırılmak üzere zamanlanıp zamanlanmadığını kontrol eder.

```
void Start()
{
    Invoke(nameof(Attack), 5f);

    if (IsInvoking(nameof(Attack)))
    {
        Debug.Log("Saldırı zamanlandı!");
    }
}

void Attack()
{
    Debug.Log("Saldırı gerçekleşti!");
}
```

`IsInvoking()` bir `bool` döndürür.

- `true`: Metot için bekleyen veya tekrarlanan bir çağrı vardır.
    
- `false`: Metot zamanlanmamıştır.
    

## 6. Invoke() Dezavantajları

1. Parametre gönderilemez.

Aşağıdaki kullanım geçersizdir:

```
Invoke(nameof(TakeDamage), 2f);

void TakeDamage(int damage)
{
    // Invoke parametreyi sağlayamaz.
}
```

Parametre kullanmak istiyorsak bir aracı metot oluşturabiliriz:

```
void Start()
{
    Invoke(nameof(ApplyDamage), 2f);
}

void ApplyDamage()
{
    TakeDamage(25);
}

void TakeDamage(int damage)
{
    Debug.Log($"Hasar: {damage}");
}
```

2. Metot adı string olarak kullanılır.

```
Invoke("Attak", 2f); // Yazım hatası!
```

Bu tür hataları azaltmak için `nameof()` kullanmak daha güvenlidir:

```
Invoke(nameof(Attack), 2f);
```

3. Karmaşık zamanlama işlemlerinde yetersiz kalabilir.

Örneğin, iki saniye beklemek, bir animasyonu çalıştırmak, tekrar beklemek ve ardından başka bir işlem yapmak istiyorsak `Coroutine` daha uygundur.

## 7. Invoke() vs Coroutine vs Update()

|Özellik|Invoke|Coroutine|Update|
|---|---|---|---|
|Gecikmeli çalışma|Evet|Evet|Elle yapılır|
|Tekrarlama|InvokeRepeating|Evet|Evet|
|Parametre kullanımı|Hayır|Evet|Evet|
|Çok aşamalı zamanlama|Sınırlı|Evet|Elle yapılır|
|Her frame çalışma|Hayır|Mümkün|Evet|

## 8. Time.timeScale ile İlişkisi

`Invoke()` ve `InvokeRepeating()` Unity'nin ölçeklenmiş oyun zamanını kullanır.

Örneğin:

```
Time.timeScale = 0f;

Invoke(nameof(Attack), 3f);
```

Oyun zamanı durdurulduğu için 3 saniyelik gecikleme ilerlemez. `timeScale` tekrar artırıldığında zamanlama devam eder.

Gerçek zaman üzerinden beklemek istiyorsak Coroutine içerisinde `WaitForSecondsRealtime` kullanabiliriz.

## Özet

|Metot|Görevi|
|---|---|
|`Invoke()`|Metodu gecikmeli çalıştırır|
|`InvokeRepeating()`|Metodu belirli aralıklarla tekrarlar|
|`CancelInvoke()`|Zamanlanan çağrıları iptal eder|
|`IsInvoking()`|Zamanlanmış çağrı olup olmadığını kontrol eder|

Unity öğrenirken aklında tutman gereken: `Invoke()` basit zamanlama işlemleri için oldukça kullanışlıdır. Ancak daha karmaşık işlemler için bir sonraki önemli konu Coroutine, IEnumerator ve yield return yapılarıdır.