
`Instantiate()`, Unity'de oyun çalışırken (Runtime) yeni bir nesne oluşturmak veya mevcut bir nesnenin kopyasını üretmek için kullanılan metottur.

Özellikle Prefab nesnelerini oyun sırasında sahneye eklemek için kullanılır.

Örneğin:

- Uzay gemisinden mermi fırlatmak.
    
- Düşman gemileri oluşturmak.
    
- Patlama efektleri üretmek.
    
- Haritaya rastgele nesneler yerleştirmek.
    
- Oyuncu öldüğünde yeniden oluşturmak (Respawn).
    

## 1. Instantiate() Temel Kullanımı

```
Instantiate(original, position, rotation);
```

|Parametre|Açıklama|
|---|---|
|`original`|Oluşturulacak nesne veya Prefab|
|`position`|Nesnenin oluşturulacağı konum (`Vector3`)|
|`rotation`|Nesnenin başlangıç dönüşü (`Quaternion`)|

Örnek:

```
public GameObject enemyPrefab;

void Start()
{
    Instantiate(
        enemyPrefab,
        new Vector3(0, 0, 10),
        Quaternion.identity
    );
}
```

Bu kod:

1. `enemyPrefab` nesnesini kopyalar.
    
2. `(0, 0, 10)` konumunda oluşturur.
    
3. `Quaternion.identity` sayesinde varsayılan dönüşünü kullanır.
    

## 2. Instantiate() ile Prefab Oluşturma

![Controlling the player – Shooting lasers and moving around - JetBrains Guide](https://images.openai.com/static-rsc-4/m-yH3_h93-q9eD4_eYTvd4LoP7J1U5lbUON7oG4SNAHXVTfkPmOfPKCtn8aIcqI8tWUTzZw3NXIff3QrLHa_miZc3kmWI6B4R4gRbKO0VEE6tGCJgIowUl1Uok0NKztSmORwFQaH1Z3vJTLoHCBULBNeqEyoVwx1rAGZjEdN2BQ?purpose=inline)

![Programmer's Ranch: Unity3D: Space Invaders (Part 3 - Prefabs)](https://images.openai.com/static-rsc-4/1AmoNbacm95alOSfKQ0FfLkFn3zUwbvFAmQKzCzK5gkMkuEnpmT-xR4KCCFmkAkZ19kc5E37-8hzcL6GxgB24uusOy9i62G33Jiobsy5RShQMyDKb4cIdtv41WLARE-NhkvWpf2xwhknMv-wkdUySEk1Md-sHpVvVmFo_ArIx8s?purpose=inline)

![VFX in Unity: Creating Enemy Explosions | by Brian David | Medium](https://images.openai.com/static-rsc-4/ulM1Kz4XqHY83WiLd1tjV5I4Ulm50NnZo04uqgDQkuBLnv82TYvE709ajSjve1TYMV7I_QhL3ygj-UePVbS9KzWw6TIOxxFP3B8z6cZdiisNuEoO0uXB_sRfUF-Ko7ViDafZD8_SsUBX9DSgCChrT7oMJ4KEGqJLwsZW6xlBdxE?purpose=inline)

7

Senin geliştirdiğin Galaxy Strike oyununu düşünelim.

Uzay gemisinin önünden mermi fırlatmak istiyoruz.

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerWeapon : MonoBehaviour
{
    [SerializeField] private GameObject bulletPrefab;
    [SerializeField] private Transform firePoint;

    void Update()
    {
        if (Keyboard.current.spaceKey.wasPressedThisFrame)
        {
            Instantiate(
                bulletPrefab,
                firePoint.position,
                firePoint.rotation
            );
        }
    }
}
```

Burada:

- `bulletPrefab`: Oluşturulacak mermi Prefab'ı.
    
- `firePoint`: Merminin çıkacağı konum ve yön.
    
- `firePoint.position`: Merminin başlangıç konumu.
    
- `firePoint.rotation`: Merminin başlangıç dönüşü.
    

Önemli: `Instantiate()` sadece mermiyi oluşturur. Merminin ileri hareket etmesini kendisi sağlamaz.

## 3. Instantiate() Overload'ları

Instantiate metodunun farklı kullanım şekilleri vardır.

### A. Sadece nesneyi oluşturma

```
Instantiate(enemyPrefab);
```

Nesnenin bir kopyasını oluşturur. Prefab'ın kayıtlı konum ve dönüş değerlerini kullanır.

### B. Konum ve dönüş belirleme

```
Instantiate(
    enemyPrefab,
    transform.position,
    transform.rotation
);
```

Nesneyi mevcut GameObject'in konumunda ve dönüşünde oluşturur.

### C. Parent belirleme

```
Instantiate(enemyPrefab, transform);
```

Oluşturulan nesneyi mevcut nesnenin altına (Child) ekler. Varsayılan olarak orijinal nesnenin dünya konumunu korumaya çalışır.

### D. Konum, dönüş ve Parent belirleme

```
Instantiate(
    enemyPrefab,
    transform.position,
    Quaternion.identity,
    transform
);
```

Hem konumunu hem dönüşünü hem de bağlı olduğu Parent nesnesini belirler.

## 4. Instantiate() Geriye Ne Döndürür?

`Instantiate()` oluşturduğu yeni nesnenin referansını döndürür.

```
GameObject enemy = Instantiate(enemyPrefab);

enemy.name = "Enemy_01";
enemy.transform.position = Vector3.zero;
```

Bu sayede oluşturulan nesne üzerinde işlem yapabiliriz.

Örneğin:

```
GameObject bullet = Instantiate(
    bulletPrefab,
    firePoint.position,
    firePoint.rotation
);

Destroy(bullet, 3f);
```

Bu kod mermiyi oluşturur ve 3 saniye sonra yok eder.

## 5. Instantiate() ve Destroy() İlişkisi

İki metot sıklıkla birlikte kullanılır.

```
Instantiate(bulletPrefab);
Destroy(gameObject);
```

|Metot|Görevi|
|---|---|
|`Instantiate()`|Yeni nesne oluşturur|
|`Destroy()`|Var olan nesneyi yok eder|

Örneğin düşman gemisi patladığında:

```
void Die()
{
    Instantiate(
        explosionPrefab,
        transform.position,
        Quaternion.identity
    );

    Destroy(gameObject);
}
```

Düşman yok edilirken bulunduğu konumda patlama efekti oluşturulur.

## 6. Performans: Instantiate mı Object Pooling mi?

Özellikle Galaxy Strike gibi sürekli mermi oluşturulan oyunlarda bu konu önemlidir.

|Instantiate|Object Pooling|
|---|---|
|Yeni nesne oluşturur|Önceden oluşturulmuş nesneyi kullanır|
|Kullanımı kolaydır|Kurulumu daha karmaşıktır|
|Çok sık kullanımda maliyetli olabilir|Sık oluşturulan nesneler için verimlidir|
|Az sayıda nesne için uygundur|Mermi, düşman ve efektler için uygundur|

`Instantiate()` ve `Destroy()` çok sık çağrıldığında bellek tahsisleri ve Garbage Collector nedeniyle performans sorunlarına yol açabilir.

Bu nedenle yüzlerce mermi kullanılan oyunlarda Object Pooling tercih edilebilir.

## Notlarına ekleyebileceğin özet

Instantiate(): Unity'de Runtime sırasında bir GameObject veya Prefab'ın kopyasını oluşturmak için kullanılır.

```
Instantiate(prefab, position, rotation);
```

- `prefab`: Oluşturulacak nesne.
    
- `position`: Oluşturulacağı konum.
    
- `rotation`: Oluşturulacağı dönüş.
    
- Geriye oluşturulan nesnenin referansını döndürür.
    
- Oluşturulan nesne `Destroy()` ile yok edilebilir.
    
- Çok sık nesne oluşturulacaksa Object Pooling değerlendirilebilir.
    

