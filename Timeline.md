
Timeline, Unity'de nesnelerin hareketlerini, animasyonlarını, seslerini ve kamera geçişlerini zaman çizelgesi üzerinden kontrol etmemizi sağlayan bir sistemdir.

Özellikle oyunlarda sinematik sahneler (Cutscene) oluşturmak için kullanılır.

Örneğin bir oyun başlangıcında:

1. Kamera karaktere yaklaşır.
    
2. Karakter yürümeye başlar.
    
3. Arka planda müzik çalar.
    
4. Karakter bir kapıyı açar.
    
5. Kamera başka bir açıya geçer.
    

Bütün bu olayları Timeline üzerinden belirli zamanlarda çalıştırabiliriz.

## 1. Timeline nasıl görünür?

![Radiator Blog: Practical primer to using Unity Timeline / Playables](https://images.openai.com/static-rsc-4/NlhT8qwGHLIzC-FRP3l-dVbHqakVVNfUZ6XCAio0WERHfDSGM1_DlYaFMhPc5QRqV8XPSG_Q6NFeBwK-IRbW-rCYt238T27W-PkRXtITVF-uYZeujNWFCwpAbDU5l_yHRaN8eUjMcnxZfKim4g6qXxOCizO4wjD7kJ2PAiwyKWU?purpose=inline)

![快来下载我们最新的2D示例项目《失落的地穴（Lost Crypt）》吧 - 技术专栏 - Unity官方开发者社区](https://images.openai.com/static-rsc-4/M7QoZYx6LvsKZCGw8Fm418tnNBp-Hfd4wUajZkadx1msc9D-q5KmtyxM4Wp17Jnp00Wqt2GrB0W2YJ-qdQnCsPtJDDp3cb0moI-vRYc3wDOCTOs-rKTAAjQFx66hK6H18u_guReMFy2j0PaaiFOQ2ztjSLLPPfMpgwsuprD9E6Y?purpose=inline)

![The Unity Library : Tag - 🎬 Timeline](https://images.openai.com/static-rsc-4/f0IxWOcT3oJTndbeoQuNQ58yyLez6mNoDwU9gMM15jmraiWSlqQU2oN68-8Cc9aiAss5FSjXNn5faG0GNKZj-ue5DSsKFBspSGQjz4j97oVLZ8G1jZBlHokEbGp5FFZCIJ2hvHW4nD5IV-AaL_wf6tEsLTNMEdFxRmz0lM-Vn68?purpose=inline)

10

Timeline, video düzenleme programlarına benzer. Yatay eksen zamanı, satırlar ise farklı işlemleri temsil eder.

|Kavram|Açıklama|
|---|---|
|Timeline Asset|Zaman çizelgesinin kaydedildiği dosya|
|Playable Director|Timeline'ı sahnede çalıştıran component|
|Track|Animasyon, ses veya başka bir işlemin kanalı|
|Clip|Track üzerinde belirli bir zaman aralığında çalışan içerik|
|Playhead|O anda hangi zamanda olduğumuzu gösteren çizgi|
|Signal|Belirli bir zamanda olay tetiklemek için kullanılır|

## 2. Timeline nasıl oluşturulur?

1. Unity'de `Window > Sequencing > Timeline` bölümünü aç.
    
2. Hierarchy'de boş bir GameObject oluştur. Adını `CutsceneManager` yap.
    
3. GameObject seçiliyken Timeline penceresinden `Create` butonuna bas.
    
4. Timeline dosyasını kaydet.
    
5. `+` butonuyla Animation Track, Audio Track gibi kanallar ekle.
    
6. Sahnedeki nesneleri ilgili Track'lere bağla.
    

Unity, GameObject üzerine bir Playable Director component'i ekler.

## 3. Timeline C# ile nasıl kontrol edilir?

Timeline'ı kod üzerinden de çalıştırabiliriz.

```
using UnityEngine;
using UnityEngine.Playables;

public class TimelineController : MonoBehaviour
{
    [SerializeField] private PlayableDirector director;

    void Start()
    {
        director.Play();
    }

    public void StopTimeline()
    {
        director.Stop();
    }

    public void PauseTimeline()
    {
        director.Pause();
    }

    public void ResumeTimeline()
    {
        director.Resume();
    }
}
```

Burada:

- `Play()` → Timeline'ı başlatır.
    
- `Stop()` → Timeline'ı durdurur ve başlangıç durumuna döndürür.
    
- `Pause()` → Timeline'ı geçici olarak duraklatır.
    
- `Resume()` → Duraklatılan Timeline'ı devam ettirir.
    

## 4. Timeline ile Animator arasındaki fark

|Özellik|Animator|Timeline|
|---|---|---|
|Karakter yürüme animasyonu|||
|Karakter durumları arasında geçiş||Sınırlı|
|Sinematik sahne|Daha zahmetli||
|Kamera geçişleri|Doğrudan değil||
|Ses ve animasyon senkronizasyonu|Daha zahmetli||
|Oyuncunun anlık hareket kontrolü||Uygun değil|

Önemli: Timeline, Animator'ın alternatifi değildir. İkisi birlikte kullanılabilir.

Örneğin karakterin normal yürüme ve koşma animasyonlarını Animator yönetirken, oyunun başlangıcındaki sinematik sahneyi Timeline yönetebilir.

Kısacası: Animator karakterin nasıl hareket edeceğini, Timeline ise sinematik bir sahnede hangi olayın ne zaman gerçekleşeceğini yönetmek için kullanılır.