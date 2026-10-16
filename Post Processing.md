
Post Processing, Unity'de kamera tarafından oluşturulan görüntüye sonradan görsel efektler uygulanmasını sağlayan sistemdir.

Oyun içerisindeki ışık, renk, parlaklık, gölge algısı ve atmosferi iyileştirmek için kullanılır.

Örneğin Post Processing ile:

- Güneş ışığının parlamasını sağlayabilirsin.
    
- Oyunun renklerini daha canlı veya karanlık yapabilirsin.
    
- Kameraya sinematik bir görünüm verebilirsin.
    
- Arka planı bulanıklaştırabilirsin.
    
- Ekranın kenarlarını karartabilirsin.
    

![Unity - Manual: Image Effect reference](https://images.openai.com/static-rsc-4/OGe9KKaO6z45AYBiFnXx74Z_o1jA8RZr9vXvua0H8U15xWJwMG0waJCcdj1w4aZXBL__6iiPgYShXdBHx3SZzsskLjBzRoXNJO9xDh5VS0Zp_AnPwosNvt7UUGp-X2VRTBaYBsv7t71k8uipftBpImcURC-M-X6NSFudUxrZ_n0?purpose=inline)

Post Processing olmadan

![7 Tips For Better Lighting in Unity 5](https://images.openai.com/static-rsc-4/UCDOic9x-fXecGyJu1FU2JROLV4GKWhdygWD1PX6MHpjxHTlGCclIGdbgUR1BlYbIjQb5mf8-HPLYoO2w08M2XncWWkGFjh2EI1c0cWcL5PJLMGbJbZA5MWQBgvEFNoehqFu7YbGTa6sVebgTr9yk1hTrj8RNeMkWQRO24TkkcE?purpose=inline)

Post Processing ile

Temsili karşılaştırma: Post Processing sahnenin geometrisini değiştirmeden görüntünün görünümünü etkiler.

## 1. Post Processing nasıl çalışır?

Unity'de görüntü oluşturma sürecini basitleştirirsek:

3D Scene

Objeler, materyaller ve ışıklar

Camera / Rendering

Sahnenin görüntüsü oluşturulur

Post Processing

Bloom, renk düzeltme, bulanıklık vb.

Final Image

Oyuncunun gördüğü görüntü

Post Processing, nesnelerin `Transform`, `Rigidbody` veya `Collider` özelliklerini değiştirmez. Temel olarak görüntü üzerinde çalışır.

## 2. Önemli Post Processing efektleri

### Bloom (Parlama)

![Unity - Manual: Bloom](https://images.openai.com/static-rsc-4/LK2-G9f3nT1s0mPX78Ug65FcCcVbD-a67Te8jVycRqZ74NN8tQLwlCf6TujhVN2ZOr3abwEO1X3YYdd9AnienzRRTxjMltEKeGkZQppAKICnNoDWwZQ0SNDOigQtntfv64uGNgdn2PVsVb_RVhlpQoU-AT7-cMxBhYifi5Udiwo?purpose=inline)

[docs.unity3d.com](https://docs.unity3d.com/es/530/Manual/script-Bloom.html)

Bloom, çok parlak bölgelerin çevresine ışık yayılması efekti ekler.

Örneğin:

- Neon ışıkları
    
- Güneş
    
- Büyü efektleri
    
- Parlayan silahlar
    

Temel ayarlar:

|Özellik|Görevi|
|---|---|
|Threshold|Hangi parlaklıktan sonra Bloom uygulanacağı|
|Intensity|Parlamanın şiddeti|
|Scatter|Parlamanın ne kadar geniş yayılacağı (URP)|

### Vignette (Kenar karartma)

![Unforgiving - A Northern Hymn on Steam](https://images.openai.com/static-rsc-4/rE03fVpFiKN72Ph0WvpmIxqewCEzc44LsPX1lYgtZTBlRaEKwT1XIgsnEpeb6upm7M1MEuI9A-1WBkUbRbwWgH7G7PLEydvgHkdqESQJCVvZiompTeG_du-BKL87jR933bCdzH9twBe4fyaw0NHzayFkPOhzTV50FkHeph50EcQ?purpose=inline)

[store.steampowered.com](https://store.steampowered.com/app/747340/Unforgiving__A_Northern_Hymn/)

Ekranın kenarlarını karartarak oyuncunun dikkatini merkeze yönlendirir.

Özellikle korku oyunlarında veya oyuncunun canı azaldığında kullanılabilir.

### Depth of Field (Alan derinliği)

![Depth of Field | Post Processing | 3.2.2](https://images.openai.com/static-rsc-4/6dJKi8f-rKbrE1oVK3I2FlnKSZLBNAS4WLiH6Gl57dJvULrmCfA76321xTr1I017M-C7YNVaZUk25LoZ3jofavco9Uvsg4ZGUK7ezRUngaQm5k5sZJ32Lr7aYv8v3zQdzNUkeNVYOKQpUnOVafvr5LTPT_iH2bPlSORNQN-3Zjo?purpose=inline)

[docs.unity3d.com](https://docs.unity3d.com/Packages/com.unity.postprocessing%403.2/manual/Depth-of-Field.html)

Kameranın odaklandığı mesafeyi belirler. Odak dışında kalan bölgeler bulanıklaşabilir.

Örneğin karakter net görünürken arka plan bulanık olabilir.

### Motion Blur (Hareket bulanıklığı)

![Cyberpunk futuristic racing car speeding through a glowing neon tunnel at night - SeaArt AI](https://images.openai.com/static-rsc-4/nQM26Cb177wF77RcRyocLJln7Ne4ld6klq6LRxY2XvPsZr9jdlMwrUEtDbQ0Bgue5hmG5kwZLhgSslJpJc-zLRMRBn84cnqwsWnKLxr2myzEnATwKKNAOhRkPeXJ_Cms3c5MjVgVpCowt_5TSWkiq5rc6c7CIQQipu5VPss3z8E?purpose=inline)

[seaart.ai](https://www.seaart.ai/explore/detail/d02ro8te878c73boim9g)

Hareket sırasında görüntüye bulanıklık ekler. Yarış oyunlarında hız hissini artırmak için kullanılabilir.

### Color Adjustments (Renk ayarları)

Sahnenin renk ve parlaklık değerlerini değiştirir.

|Özellik|Açıklama|
|---|---|
|Post Exposure|Genel pozlamayı değiştirir|
|Contrast|Açık ve koyu alanların farkını değiştirir|
|Color Filter|Görüntüye renk tonu uygular|
|Hue Shift|Renk tonlarını kaydırır|
|Saturation|Renklerin canlılığını değiştirir|

## 3. Unity'de Post Processing nasıl eklenir?

Unity'de kullandığın Render Pipeline'a göre yöntem değişir.

|Render Pipeline|Kullanılan sistem|
|---|---|
|Built-in|Post Processing Stack v2|
|URP|Volume sistemi|
|HDRP|Volume sistemi|

Modern Unity URP projelerinde genellikle Volume kullanılır.

### URP ile kurulum

1. Hierarchy üzerinden sağ tıkla.
    
2. Volume → Global Volume oluştur.
    
3. Inspector'dan yeni bir Volume Profile oluştur.
    
4. `Add Override` seçeneğine bas.
    
5. `Post-processing → Bloom` ekle.
    
6. Bloom ayarlarını etkinleştir ve düzenle.
    
7. Kameranın `Rendering → Post Processing` seçeneğinin açık olduğundan emin ol.
    

URP Renderer ayarlarının da Post Processing'i desteklemesi gerekir.

## 4. Global Volume ve Local Volume farkı

### Global Volume

Efektleri bütün sahneye uygular.

Örneğin bütün oyunda Bloom kullanmak istiyorsan Global Volume tercih edebilirsin.

### Local Volume

Efektleri belirli bir bölgeye göre uygular. Bunun için genellikle bir Collider kullanılır.

Örneğin:

- Karakter mağaraya girince ekran kararır.
    
- Su altına girince renkler maviye döner.
    
- Zehirli bölgeye girince görüntü yeşilleşir.
    

Local Volume'un etkisi kamera konumuna ve Volume sistemindeki ayarlara bağlıdır.

## 5. C# ile Post Processing kontrolü

URP'de Volume bileşenine kod üzerinden erişebilirsin.

Örneğin Bloom şiddetini değiştirelim:

```
using UnityEngine;
using UnityEngine.Rendering;
using UnityEngine.Rendering.Universal;

public class PostProcessingController : MonoBehaviour
{
    [SerializeField]
    private Volume volume;

    private Bloom bloom;

    void Start()
    {
        if (volume.profile.TryGet(out bloom))
        {
            bloom.intensity.value = 2f;
        }
    }
}
```

Burada:

- `Volume`: Post Processing ayarlarını tutan bileşendir.
    
- `volume.profile`: Volume Profile'a erişir.
    
- `TryGet(out bloom)`: Bloom efektini bulmaya çalışır.
    
- `bloom.intensity.value`: Bloom şiddetini değiştirir.
    

`TryGet()` başarılı olursa Bloom değişkeni kullanılabilir. Bu örnek URP içindir ve Volume Profile içinde Bloom bulunmalıdır.

## 6. Performansa etkisi

Post Processing, görüntü üzerinde ek hesaplamalar yaptığı için GPU yükünü artırabilir.

|Efekt|Genel performans maliyeti|
|---|---|
|Color Adjustments|Düşük|
|Vignette|Düşük|
|Bloom|Orta|
|Depth of Field|Orta–yüksek|
|Motion Blur|Orta–yüksek|

Bu değerler kullanılan platforma, çözünürlüğe ve efekt ayarlarına göre değişir.

Özellikle mobil oyun geliştirirken Bloom ve Depth of Field gibi efektleri dikkatli kullanmalısın.

## 7. Etkileşimli örnek: Görüntü ayarları

Aşağıdaki örnekte Bloom, parlaklık ve Saturation değerlerinin görüntüyü nasıl etkilediğini yaklaşık olarak görebilirsin.

Bloom

67%

Parlaklık

179%

Saturation

200%

SıfırlaBu görsel temsili bir simülasyondur; Unity'nin gerçek render çıktısı değildir.

## 8. Particle System ile Post Processing farkı

Bir önceki konuyla bağlantı kuralım:

|Particle System|Post Processing|
|---|---|
|Parçacık üretir|Görüntüye efekt uygular|
|Ateş, duman, kıvılcım oluşturur|Bloom, Vignette, Color Adjustments uygular|
|Sahne içerisinde çalışır|Görüntü işleme aşamasında çalışır|
|GameObject olarak kullanılabilir|Volume sistemiyle yönetilebilir|

Örnek: Bir büyü saldırısı yaptığını düşün.

Particle System ile ateş parçacıklarını oluşturursun. Post Processing Bloom ile de bu parçacıkların parlak bölgelerinin ekranda ışık saçıyormuş gibi görünmesini sağlarsın.

Özet: Particle System efektin kendisini oluştururken Post Processing, kameranın gösterdiği görüntünün son görünümünü düzenler.