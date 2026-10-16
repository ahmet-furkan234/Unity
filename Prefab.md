

Prefab, Unity'de oluşturduğun bir `GameObject`'i bütün bileşenleri ve ayarlarıyla birlikte kaydedip tekrar tekrar kullanmanı sağlayan bir sistemdir.

Kısaca: Prefab = Yeniden kullanılabilir GameObject şablonu.

Örneğin bir düşman oluşturduğunu düşün. Bu düşmanın `Transform`, `Rigidbody`, `Collider` ve kendi yazdığın `Enemy` scripti var.

Bunları her düşman için yeniden oluşturmak yerine bir Prefab hazırlarsın ve istediğin kadar çoğaltırsın.

## 1. Prefab nasıl çalışır?

![Getting started with NetCode | Unity NetCode | 0.4.0-preview.1](https://images.openai.com/static-rsc-4/yhQ0_R7wpen49rkAWFY1ZQxPCieVNHx1DAhQPdKu6xzf33ZhsXSz-aW1wXPg9NFeL4qD4cK5jKSJ-JfUnRfhKt1I1Bi1Y5BYX0oPKxG7cJCoUXuOAuI6GNlP8dOhQCcpHiINw7XenMOOqV-spmMrDoEjDw49BUJVKdQ_BEnNDfs?purpose=inline)

[docs.unity3d.com](https://docs.unity3d.com/Packages/com.unity.netcode%400.4/manual/getting-started.html)

Unity'de iki temel kavram bulunur:

- Prefab Asset: Project klasöründe saklanan ana şablondur.
    
- Prefab Instance: Bu şablondan sahneye yerleştirdiğin nesnedir.
    

Örneğin `Enemy` adında bir Prefab oluşturduğunda, sahneye 50 tane düşman yerleştirebilirsin.

Ana Prefab üzerinde bir değişiklik yaptığında, ilgili özelliği kendisi değiştirmemiş olan Instance'lar da bu değişikliği alır.

## 2. Prefab nasıl oluşturulur?

1. `Hierarchy` panelinde bir GameObject oluştur.
    
2. Nesneye istediğin Component ve Script'leri ekle.
    
3. GameObject'i `Hierarchy` panelinden `Project` panelindeki bir klasöre sürükle.
    
4. Artık bir Prefab oluşturdun.
    

Prefab'lar genellikle `.prefab` uzantısıyla saklanır.

## 3. Prefab kod ile nasıl oluşturulur?

Unity'de `Instantiate()` metodu kullanılarak Prefab'dan yeni nesneler oluşturulur.

```
using UnityEngine;

public class EnemySpawner : MonoBehaviour
{
    [SerializeField] private GameObject enemyPrefab;

    void Start()
    {
        Instantiate(
            enemyPrefab,
            new Vector3(0, 0, 5),
            Quaternion.identity
        );
    }
}
```

Kodun açıklaması:

- `enemyPrefab`: Inspector üzerinden atanacak Prefab.
    
- `Instantiate()`: Prefab'dan yeni bir GameObject oluşturur.
    
- `new Vector3(0, 0, 5)`: Nesnenin oluşturulacağı konum.
    
- `Quaternion.identity`: Nesnenin dönüşünü sıfır rotasyon olarak ayarlar.
    

## 4. Prefab ile normal GameObject farkı

|Özellik|GameObject|Prefab|
|---|---|---|
|Sahnede bulunabilir|Evet|Evet, Instance olarak|
|Tekrar kullanılabilir|Kopyalanarak|Şablondan oluşturularak|
|Project klasöründe saklanır|Normalde hayır|Evet|
|Ana şablondan güncelleme alır|Hayır|Evet|
|`Instantiate()` ile oluşturulabilir|Evet|Evet|

## 5. Prefab Override nedir?

Bir Prefab'ın sahnedeki kopyasında yaptığın ve ana Prefab'dan farklı olan ayarlara Override denir.

Örneğin:

- Ana Prefab'ın `Scale` değeri `(1,1,1)`.
    
- Sahnedeki bir Instance'ın `Scale` değerini `(2,2,2)` yaptın.
    
- Bu değişiklik yalnızca o Instance üzerinde geçerli olur.
    
- İstersen `Apply` ile değişikliği ana Prefab'a aktarabilir veya `Revert` ile geri alabilirsin.
    

## 6. Prefab neden önemlidir?

Özellikle oyun geliştirirken düşmanlar, mermiler, silahlar, efektler ve toplanabilir eşyalar gibi tekrar kullanılan nesneleri yönetmeyi kolaylaştırır.

Örneğin bir silah sisteminde her ateş edildiğinde:

```
Instantiate(bulletPrefab, firePoint.position, firePoint.rotation);
```

Bu kod, `bulletPrefab` şablonundan yeni bir mermi oluşturur.

Önemli: `Instantiate()` yalnızca Prefab'larda kullanılmaz. Normal GameObject'leri de çoğaltabilir. Prefab'ın asıl avantajı, nesneyi kalıcı bir şablon olarak saklayıp merkezi olarak yönetebilmesidir.

Özet: Prefab bir sınıf (`class`) değildir, ancak nesne yönelimli programlamadaki şablon mantığına benzer. Bir kez oluşturduğun GameObject yapısını defalarca kullanmana ve yönetmene olanak sağlar.