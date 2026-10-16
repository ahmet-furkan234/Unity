
Unity'de Light, sahnedeki nesnelerin aydınlatılmasını, gölgelerin oluşmasını ve ortamın daha gerçekçi görünmesini sağlayan bileşendir.

Light sistemi sayesinde:

- Güneş ve ay ışığı oluşturabiliriz.
    
- Lamba, el feneri ve meşale yapabiliriz.
    
- Nesnelerin gölge oluşturmasını sağlayabiliriz.
    
- Gece ve gündüz sistemi geliştirebiliriz.
    
- Işıkların rengini, şiddetini ve etki alanını değiştirebiliriz.
    

## 1. Unity'de Light türleri

Unity'de temel olarak dört farklı ışık türü bulunur.

![【Unity】官方文档学习-光照系统_unity文档-CSDN博客](https://images.openai.com/static-rsc-4/QmER4Sh6ob3vxTgrF6ngA-7nHUc_BV39JWsuTHW6glK2Q6FqF78-LDQBi8CcMFbs_83PFwFEX9hl5SzXjoL3pXCUFiMZ00iIKfEFmfeqeX_CDqnKI7JOn5HOfCNqG_bP-3lQj37vZVci54HlQET-b6gKbtt4fZKJHP5ccQK6w6o?purpose=inline)

Directional Light (Yönlü ışık)

Güneş veya ay ışığını temsil eder. Işık, bütün sahneye aynı yönden gelir.

- Position önemli değildir.
    
- Rotation ışığın yönünü belirler.
    
- Range özelliği yoktur.
    
- Büyük alanları aydınlatmak için kullanılır.
    

![Types of Light Sources in Unity - Yarsa DevBlog](https://images.openai.com/static-rsc-4/_abpaNGU23fwysqehhh_-hnrs-AhG8fb6iTHgpFEMc45UV5UPRdUM6PmmuCr-b5rrxMbIqJWoMD_vA0TiQLG-zUy74q6HnQvrIoLGetT6DDpVoa97il81S8pRc0CkCN1tcaaZ3gyKUnAG45WkowHeWAHA5Fu-ep6He42TGe_pqk?purpose=inline)

Point Light (Noktasal ışık)

Bir noktadan bütün yönlere ışık yayar.

- Position önemlidir.
    
- Rotation genellikle önemli değildir.
    
- Range ile etki alanı belirlenir.
    
- Ampul, ateş ve meşale için uygundur.
    

![Unity - Manual: Types of light](https://images.openai.com/static-rsc-4/WEIz5KQT_Q-1UAc5qXrPswChPyRfKaJYa_ut4d7jTZj3Nm8oBPEhOX_8wbVjMNp_WgOB2lGoAWJqSjDgFbF70Ab4TgbnTmwhW0NVT8aLHt-GRk2n2oeu-rOhh0TAj8VADeWavBtnod6W9B_OWwyHGotNj3DTVKaKefbW8wdbaAk?purpose=inline)

Spot Light (Konik ışık)

Belirli bir yöne koni şeklinde ışık gönderir.

- Position ve Rotation önemlidir.
    
- Range ışığın mesafesini belirler.
    
- Spot Angle ışığın açısını belirler.
    
- El feneri ve sahne spotları için kullanılır.
    

![Art of Lighting Game Environments in Unity - CG Cookie](https://images.openai.com/static-rsc-4/sAD36WZy_ibvTB1ATm3BAt72ICUB2fV7Ur66hv6aeEcBvxX184gY86R3Bs2Ik7UM4fMnfkuaSBbyxKr8E-xCWfTku75KuJYUJhkDnnNXCueJar5R_6WBPiQkU-3oKrUTLJBQUDzgwmD67_Wo7g8mMWQRae1TtWeZq75dpxjsxTs?purpose=inline)

Area Light (Alan ışığı)

Bir yüzeyden ışık yayıyormuş gibi aydınlatma sağlar.

- Daha yumuşak ışık ve gölgeler için kullanılır.
    
- Pencere ve büyük ışık panellerinde tercih edilir.
    
- Kullanılabilirliği ve gerçek zamanlı desteği Render Pipeline'a bağlıdır.
    

## 2. Light bileşeninin temel özellikleri

Unity'de bir Light oluşturmak için:

Hierarchy → Sağ tık → Light → Point Light

Ardından Inspector üzerinden ayarlarını değiştirebilirsin.

|Özellik|Açıklama|
|---|---|
|Type|Işık türünü belirler|
|Color|Işığın rengini belirler|
|Intensity|Işığın şiddetini belirler|
|Range|Işığın etki mesafesini belirler|
|Spot Angle|Spot Light'ın koni açısını belirler|
|Shadow Type|Gölge oluşturma şeklini belirler|
|Culling Mask|Hangi katmanların aydınlatılacağını belirler (pipeline desteğine bağlı)|

### Intensity ve Range farkı

Bu iki kavram birbirinden farklıdır.

- Intensity: Işığın ne kadar güçlü olduğunu belirler.
    
- Range: Işığın ne kadar uzağa etki edebileceğini belirler.
    

Örneğin:

```
light.intensity = 5f;
light.range = 10f;
```

Işık şiddeti 5, etki mesafesi ise 10 Unity birimi olur. Gerçek görünüm kullanılan Render Pipeline ve ışık birimlerine bağlıdır.

## 3. Light Mode (Işığın çalışma biçimi)

Unity'de ışıkların hesaplanma biçimini belirleyen üç temel mod bulunur.

|Mode|Açıklama|
|---|---|
|Realtime|Işık oyun sırasında dinamik olarak hesaplanır|
|Baked|Işık bilgisi önceden hesaplanıp saklanır|
|Mixed|Gerçek zamanlı ve önceden hesaplanan aydınlatmayı birleştirir|

### Realtime

Hareket eden ışıklar için uygundur. Örneğin karakterin elindeki fener.

Avantajı dinamiktir, ancak performans maliyeti daha yüksek olabilir.

### Baked

Aydınlatma önceden hesaplanır ve genellikle Lightmap içerisinde saklanır.

Özellikle hareket etmeyen çevre nesnelerinde performans avantajı sağlar. Dinamik nesneleri aydınlatmak için ayrıca Light Probe gibi yöntemler gerekebilir.

### Mixed

Statik çevreye önceden hesaplanmış aydınlatma uygularken dinamik nesneler için gerçek zamanlı hesaplamalar da kullanabilir. Davranışı Mixed Lighting ayarlarına bağlıdır.

## 4. Shadows (Gölgeler)

Light bileşeninde gölgeleri kontrol edebilirsin.

- No Shadows: Gölge oluşturmaz.
    
- Hard Shadows: Keskin kenarlı gölgeler oluşturur.
    
- Soft Shadows: Daha yumuşak kenarlı gölgeler oluşturur.
    

![Rotates a Unity directional light based on location and time. Includes time scale, and frame stepping for continuous use. · GitHub](https://images.openai.com/static-rsc-4/VRBTpk9Tkr1ya3OO8oczdO8fFoE-bD1fAtLro0Z1OU8duxwUcLME3pJ1vQW5aGYxPQol6ZkXvzW6JilwbRiGR7SH3FviIcVGZ3nzqHncRsKspmD5vi-AtqDW5u4DsRsXxvXnWk_r6Mmt6rX9qcQ4yaJDI67ik6a-2JcvWGp5yZI?purpose=inline)

Hard Shadows

![Dithered shadows using Blender Internal - Lighting and Rendering - Blender Artists Community](https://images.openai.com/static-rsc-4/6qyeLMn_9J2aj4VuRYQ7MQcpjpfwtHZ0dgOPfYdAuWj0C01jADBNZVIL-3WIOnbNXCYnpZsub6U0st7yrwNY8CafBM4FCvnvqDM5hqKMs23mY87ok6jyN_NBuKykXmIxFvlKxm7qUctg68A_bkTGH37q3eOexifk2TopC77ZNh8?purpose=inline)

Soft Shadows

Soft Shadows daha doğal görünebilir, ancak gölge kalitesine ve donanıma göre daha maliyetli olabilir.

## 5. C# ile Light kontrolü

Unity'de Light bileşenine `GetComponent<Light>()` ile erişebiliriz.

```
using UnityEngine;

public class LightController : MonoBehaviour
{
    private Light myLight;

    void Start()
    {
        myLight = GetComponent<Light>();

        myLight.color = Color.red;
        myLight.intensity = 3f;
        myLight.range = 15f;
    }
}
```

Burada:

- `GetComponent<Light>()` aynı GameObject üzerindeki Light bileşenini alır.
    
- `color` ışığın rengini değiştirir.
    
- `intensity` ışığın şiddetini değiştirir.
    
- `range` etki alanını değiştirir.
    

`range` özelliği Point Light ve Spot Light için kullanılır.

## 6. Örnek: F tuşuyla el fenerini açıp kapatma

Yeni Input System kullanarak basit bir el feneri sistemi oluşturalım.

```
using UnityEngine;
using UnityEngine.InputSystem;

public class Flashlight : MonoBehaviour
{
    [SerializeField]
    private Light flashlight;

    void Update()
    {
        if (Keyboard.current != null &&
            Keyboard.current.fKey.wasPressedThisFrame)
        {
            flashlight.enabled = !flashlight.enabled;
        }
    }
}
```

Çalışma mantığı:

1. Oyuncu F tuşuna basar.
    
2. `wasPressedThisFrame` tuşa o karede basılıp basılmadığını kontrol eder.
    
3. `flashlight.enabled` ışığın açık olup olmadığını belirtir.
    
4. `!` operatörü mevcut Boolean değeri tersine çevirir.
    

Örneğin:

```
flashlight.enabled = true;  // Işık açık
flashlight.enabled = false; // Işık kapalı
```

`enabled = false` olduğunda Light bileşeni devre dışı kalır, ancak GameObject yok edilmez.

## 7. Örnek: Gece ve gündüz sistemi

Directional Light'ın açısını zamanla değiştirerek basit bir güneş hareketi oluşturabiliriz.

```
using UnityEngine;

public class DayNightCycle : MonoBehaviour
{
    [SerializeField]
    private Light sun;

    [SerializeField]
    private float rotationSpeed = 5f;

    void Update()
    {
        sun.transform.Rotate(
            Vector3.right,
            rotationSpeed * Time.deltaTime,
            Space.World
        );
    }
}
```

Burada:

- `Rotate()` ışığın yönünü değiştirir.
    
- `Vector3.right` dünya X eksenini temsil eder.
    
- `rotationSpeed` dönüş hızıdır.
    
- `Time.deltaTime` dönüşü kare hızından bağımsız hâle getirir.
    
- `Space.World` dünya koordinat sisteminde döndürür.
    

Bu örnek güneşin yönünü döndürür. Tam bir gece-gündüz sistemi için ortam aydınlatması, gökyüzü ve diğer ışıkların da ayarlanması gerekir.

## 8. Etkileşimli ışık örneği

Aşağıdaki simülasyonda Point Light'ın şiddetini, etki mesafesini ve rengini değiştirebilirsin.

Intensity

7.0

Range

7 birim

Işık rengi

Sıcak sarı

Beyaz

Mavi

Kırmızı

Varsayılan ayarlara dön

## 9. Light ve Post Processing farkı

Bir önceki konumuzla karşılaştıralım.

|Light|Post Processing|
|---|---|
|Nesnelerin aydınlatılmasını sağlar|Oluşturulan görüntüyü işler|
|Gölge oluşturabilir|Bloom gibi efektler uygular|
|Sahne aydınlatmasını etkiler|Kameranın son görüntüsünü etkiler|
|Point, Spot, Directional türleri vardır|Bloom, Vignette gibi efektleri vardır|

Örneğin bir meşale oluşturduğunda:

- Particle System: Alev ve kıvılcımları oluşturur.
    
- Point Light: Meşalenin çevresini aydınlatır.
    
- Post Processing (Bloom): Parlak alevin çevresinde görsel parlama oluşturur.
    

Üç sistemi birlikte kullanarak çok daha etkileyici efektler oluşturabilirsin.

Önemli performans notu: Özellikle mobil oyunlarda çok sayıda Realtime Light ve gölge oluşturan ışık kullanmak GPU yükünü artırabilir. Sabit ışıklarda Baked Lighting, hareketli ışıklarda ise kontrollü Realtime Lighting kullanmak genellikle daha verimlidir.