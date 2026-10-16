
Object Pooling, Unity'de sürekli nesne oluşturup yok etmek yerine, önceden oluşturulmuş nesneleri tekrar tekrar kullanmamızı sağlayan bir optimizasyon tekniğidir.

Özellikle senin Galaxy Strike oyunundaki mermiler, düşmanlar ve patlama efektleri için oldukça kullanışlıdır.

## 1. Object Pooling Nasıl Çalışır?

Normalde `Instantiate()` ve `Destroy()` kullanarak nesneleri oluşturur ve yok ederiz.

Instantiate / Destroy yöntemi

`Instantiate()` → Mermi oluştur

Mermiyi kullan

`Destroy()` → Mermiyi yok et

Her atışta yeni nesne oluşturulur.

Object Pooling yöntemi

Önceden 20 mermi oluştur

`SetActive(true)` → Mermiyi kullan

`SetActive(false)` → Mermiyi havuza geri koy

Aynı mermiler tekrar kullanılır.

Object Pooling'in temel amacı, sık yapılan nesne oluşturma ve yok etme işlemlerinin maliyetini azaltmaktır.

## 2. Unity'de ObjectPool Sınıfı

Unity, `UnityEngine.Pool` namespace'i içerisinde hazır bir `ObjectPool<T>` sınıfı sunar.

```
using UnityEngine.Pool;
```

Bu sınıf sayesinde kendi havuz yönetim sistemimizi sıfırdan yazmamıza gerek kalmaz.

Önemli metotları:

|Metot|Açıklama|
|---|---|
|`Get()`|Havuzdan nesne alır|
|`Release(obj)`|Nesneyi havuza geri verir|
|`Clear()`|Havuzdaki kullanılmayan nesneleri temizler|
|`CountActive`|Kullanımda olan nesne sayısı|
|`CountInactive`|Havuzda bekleyen nesne sayısı|
|`CountAll`|Havuzun yönettiği toplam nesne sayısı|

## 3. Galaxy Strike İçin Mermi Havuzu Oluşturalım

İlk olarak bir `BulletPool.cs` scripti oluşturalım.

```
using UnityEngine;
using UnityEngine.Pool;

public class BulletPool : MonoBehaviour
{
    [SerializeField] private GameObject bulletPrefab;
    [SerializeField] private Transform firePoint;

    private ObjectPool<GameObject> pool;

    void Awake()
    {
        pool = new ObjectPool<GameObject>(
            createFunc: CreateBullet,
            actionOnGet: OnGetBullet,
            actionOnRelease: OnReleaseBullet,
            actionOnDestroy: OnDestroyBullet,
            collectionCheck: true,
            defaultCapacity: 20,
            maxSize: 100
        );
    }

    private GameObject CreateBullet()
    {
        GameObject bullet = Instantiate(bulletPrefab);
        bullet.SetActive(false);
        return bullet;
    }

    private void OnGetBullet(GameObject bullet)
    {
        bullet.transform.SetPositionAndRotation(
            firePoint.position,
            firePoint.rotation
        );

        bullet.SetActive(true);
    }

    private void OnReleaseBullet(GameObject bullet)
    {
        bullet.SetActive(false);
    }

    private void OnDestroyBullet(GameObject bullet)
    {
        Destroy(bullet);
    }

    public GameObject GetBullet()
    {
        return pool.Get();
    }

    public void ReturnBullet(GameObject bullet)
    {
        pool.Release(bullet);
    }
}
```

### Kodun mantığı

`createFunc`

```
createFunc: CreateBullet
```

Havuzda kullanılabilir nesne bulunmadığında yeni bir mermi oluşturur.

`actionOnGet`

```
actionOnGet: OnGetBullet
```

Mermi havuzdan alındığında çalışır. Merminin konumunu ayarlar ve aktif hale getirir.

`actionOnRelease`

```
actionOnRelease: OnReleaseBullet
```

Mermi havuza geri bırakıldığında çalışır. Mermiyi pasif hale getirir.

`actionOnDestroy`

```
actionOnDestroy: OnDestroyBullet
```

Havuz kapasitesi doluyken geri verilen fazla nesneleri yok etmek için kullanılır.

## 4. Mermiyi Ateşleyelim

`PlayerWeapon.cs`:

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerWeapon : MonoBehaviour
{
    [SerializeField] private BulletPool bulletPool;

    void Update()
    {
        if (Keyboard.current.spaceKey.wasPressedThisFrame)
        {
            bulletPool.GetBullet();
        }
    }
}
```

Space tuşuna bastığında:

1. Havuzda boşta mermi varsa o kullanılır.
    
2. Boşta mermi yoksa yeni mermi oluşturulur.
    
3. Mermi `firePoint` konumuna taşınır.
    
4. Mermi aktif hale gelir.
    

Bu örnekte merminin hareketi henüz eklenmedi.

## 5. Mermiyi Havuza Geri Gönderme

Mermilerimizi `Destroy()` ile yok etmek yerine havuza geri göndereceğiz.

`Bullet.cs`:

```
using UnityEngine;
using System.Collections;

public class Bullet : MonoBehaviour
{
    private BulletPool bulletPool;
    private Coroutine returnRoutine;

    public void Initialize(BulletPool pool)
    {
        bulletPool = pool;
    }

    public void Fire()
    {
        if (returnRoutine != null)
            StopCoroutine(returnRoutine);

        returnRoutine = StartCoroutine(ReturnAfterDelay());
    }

    private IEnumerator ReturnAfterDelay()
    {
        yield return new WaitForSeconds(3f);

        returnRoutine = null;
        bulletPool.ReturnBullet(gameObject);
    }

    private void OnDisable()
    {
        if (returnRoutine != null)
        {
            StopCoroutine(returnRoutine);
            returnRoutine = null;
        }
    }
}
```

Bunun çalışması için `BulletPool.cs` içerisindeki iki metodu güncelleyelim:

```
private GameObject CreateBullet()
{
    GameObject bullet = Instantiate(bulletPrefab);

    bullet.GetComponent<Bullet>().Initialize(this);

    bullet.SetActive(false);
    return bullet;
}

private void OnGetBullet(GameObject bullet)
{
    bullet.transform.SetPositionAndRotation(
        firePoint.position,
        firePoint.rotation
    );

    bullet.SetActive(true);
    bullet.GetComponent<Bullet>().Fire();
}
```

Artık mermi 3 saniye sonra yok edilmez, havuza geri döner.

## 6. defaultCapacity ve maxSize Arasındaki Fark

Bu iki parametre sıklıkla karıştırılır.

```
defaultCapacity: 20,
maxSize: 100
```

- `defaultCapacity: 20`: Havuzun başlangıçtaki dahili depolama kapasitesidir. Otomatik olarak 20 mermi oluşturmaz.
    
- `maxSize: 100`: Havuzun saklayabileceği pasif nesnelerin üst sınırıdır. Aynı anda aktif olabilecek mermi sayısını doğrudan sınırlandırmaz.
    

Örneğin aynı anda 120 mermi kullanılıyorsa havuz gerektiğinde 120 mermi oluşturabilir. Bunlar geri dönerken havuz en fazla 100 tanesini saklar; fazlalıklar yok edilir.

## 7. Object Pooling Ne Zaman Kullanılmalı?

|Kullanım alanı|Object Pooling|
|---|---|
|Sürekli ateşlenen mermiler|Önerilir|
|Patlama efektleri|Önerilir|
|Sık oluşturulan düşmanlar|Önerilir|
|Oyunun ana karakteri|Genellikle gerekmez|
|Bir defa oluşturulan harita|Genellikle gerekmez|
|Sık oluşan hasar göstergeleri|Önerilir|

Object Pooling bellek kullanımını mutlaka azaltmaz; hatta havuzda bekleyen nesneler bellek tüketmeye devam eder. Temel avantajı tekrar eden oluşturma/yok etme maliyetini ve buna bağlı performans dalgalanmalarını azaltmasıdır.

## Notlarına ekleyebileceğin özet

Object Pooling: Oyun sırasında sürekli `Instantiate()` ve `Destroy()` yapmak yerine, nesneleri bir havuzda tutarak tekrar kullanmayı sağlayan optimizasyon tekniğidir.

```
using UnityEngine.Pool;

ObjectPool<GameObject> pool;
```

- `Get()`: Havuzdan nesne alır.
    
- `Release()`: Nesneyi havuza geri verir.
    
- `createFunc`: Yeni nesne oluşturur.
    
- `actionOnGet`: Nesne alındığında çalışır.
    
- `actionOnRelease`: Nesne geri verildiğinde çalışır.
    
- `defaultCapacity`: Başlangıç depolama kapasitesi.
    
- `maxSize`: Havuzda saklanabilecek pasif nesne sınırı.
    

Önemli: Havuzdan tekrar alınan nesnelerin konum, hız, sağlık, animasyon ve benzeri durumları gerektiğinde sıfırlanmalıdır.

Bir sonraki aşamada `ObjectPool<T>` ile `LinkedPool<T>` arasındaki farkı veya `Rigidbody` kullanarak havuzdan alınan mermilerin fiziksel olarak fırlatılmasını inceleyebiliriz.