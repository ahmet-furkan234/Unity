
`Quaternion.Euler()`, Unity'de bir nesnenin X, Y ve Z eksenlerindeki dönüş açılarını (derece cinsinden) Quaternion türüne dönüştüren fonksiyondur.

Unity, nesnelerin dönüşlerini (`rotation`) Quaternion olarak saklar. Ancak biz dönüşleri genellikle 0°, 90°, 180° gibi Euler açılarıyla ifade ederiz.

### 1. Kullanımı

```
Quaternion.Euler(x, y, z);
```

|Parametre|Açıklama|
|---|---|
|`x`|X ekseni etrafındaki dönüş|
|`y`|Y ekseni etrafındaki dönüş|
|`z`|Z ekseni etrafındaki dönüş|

![Navigation in the Editor - 2019.1 & 2019.2 - Unity Learn](https://images.openai.com/static-rsc-4/o-8W7MsbhtfsyqRsM_2q1kq52O8kH5lwHmAQim9D512RR1YpX_lLcrv-VynRI8v4hxfS3dOauPQNgUDGoNpussq7O6_6onx8DKnrw_EoxIQ0CHwH5LMBFDUy9QERdWEFFh7TL_PvVMU7TerzeDpHG1bf9lcfVp19umMclO0RKSE?purpose=inline)

[learn.unity.com](https://learn.unity.com/course/introduction-to-3d-interactive-development-workshop-project-files/tutorial/navigation-in-the-editor-2019-3)

Unity'de X, Y ve Z eksenleri etrafındaki dönüşler.

### 2. Basit kullanım örneği

```
void Start()
{
    transform.rotation = Quaternion.Euler(0f, 90f, 0f);
}
```

Bu kod çalıştığında nesne Y ekseni etrafında 90 derece döndürülmüş bir yönelime sahip olur.

Örneğin:

```
Quaternion.Euler(90f, 0f, 0f); // X ekseni
Quaternion.Euler(0f, 90f, 0f); // Y ekseni
Quaternion.Euler(0f, 0f, 90f); // Z ekseni
```

### 3. İnteraktif dönüş örneği

Aşağıdaki değerleri değiştirerek bir nesnenin Euler açılarına göre nasıl döndüğünü inceleyebilirsin.

X

0°

Y

-180°

Z

-70°

`Quaternion.Euler({rx}f, {ry}f, {rz}f)`

Dönüşü sıfırla

### 4. Quaternion.Euler ile sürekli döndürme

Bir nesnenin sürekli dönmesini sağlayabiliriz.

```
using UnityEngine;

public class RotateObject : MonoBehaviour
{
    [SerializeField] float rotationSpeed = 90f;

    void Update()
    {
        transform.rotation *= Quaternion.Euler(
            0f,
            rotationSpeed * Time.deltaTime,
            0f
        );
    }
}
```

Nasıl çalışıyor?

- `rotationSpeed`: Saniyede kaç derece döneceğini belirler.
    
- `Time.deltaTime`: Dönüşü zamana bağlı hâle getirir.
    
- `Quaternion.Euler()`: Verilen açılardan bir dönüş oluşturur.
    
- `*=`: Mevcut dönüşe yeni dönüşü ekler. Burada sağ taraftaki dönüş yerel eksenlere göre uygulanır.
    

### 5. Quaternion.Euler ve transform.Rotate farkı

İki yöntem de nesneleri döndürmek için kullanılabilir.

```
// Belirli bir yönelime ayarlar
transform.rotation = Quaternion.Euler(0, 90, 0);

// Mevcut dönüşe 90 derece ekler
transform.Rotate(0, 90, 0);
```

Aralarındaki temel fark:

|Quaternion.Euler|transform.Rotate|
|---|---|
|Quaternion dönüş değeri üretir.|Nesneyi doğrudan döndürür.|
|`transform.rotation` ile belirli yönelim atanabilir.|Mevcut yönelime dönüş ekler.|
|Quaternion.Lerp/Slerp ile kullanılabilir.|Sürekli dönüş işlemlerinde pratiktir.|

### 6. Quaternion.Euler ve Quaternion.Slerp birlikte kullanımı

Nesneyi bir yönden başka bir yöne yumuşak biçimde döndürebiliriz.

```
Quaternion targetRotation = Quaternion.Euler(0, 180, 0);

transform.rotation = Quaternion.Slerp(
    transform.rotation,
    targetRotation,
    2f * Time.deltaTime
);
```

Burada nesne, Y ekseninde 180 derecelik hedef yönelime doğru yavaşlayarak yaklaşır.

`Slerp` iki Quaternion arasında küresel interpolasyon yapar. Bu kullanım sabit açısal hız üretmez.

### Notlarına ekleyebileceğin özet

Quaternion.Euler(x, y, z): X, Y, Z eksenlerine ait derece cinsinden Euler açılarını Quaternion dönüşüne çevirir.

- `transform.rotation = Quaternion.Euler(...)` → Nesnenin dünya uzayındaki dönüşünü ayarlar.
    
- `transform.localRotation = Quaternion.Euler(...)` → Nesnenin parent nesnesine göre dönüşünü ayarlar.
    
- `transform.rotation *= Quaternion.Euler(...)` → Mevcut dönüşe yerel eksenlerde dönüş ekler.
    
- `Quaternion.Slerp()` → İki dönüş arasında yumuşak geçiş yapar.
    

Önemli: Quaternion, yalnızca üç açıdan oluşan bir yapı değildir. Unity dönüşleri dört bileşenli `(x, y, z, w)` Quaternion olarak temsil eder. `Quaternion.Euler()` ise bu dönüşü Euler açılarıyla kolayca oluşturmamızı sağlar.