
Unity'de Sound System, oyun içerisindeki ses efektlerini, arka plan müziklerini ve ortam seslerini yönetmek için kullanılan sistemdir.

Örneğin:

- Karakterin yürüme ve zıplama sesleri
    
- Silah ateşleme ve patlama sesleri
    
- Arka plan müzikleri
    
- Butonlara basıldığında çıkan sesler
    
- Oyuncunun uzaklığına göre azalan veya artan sesler
    

Unity'de ses sistemi temel olarak AudioSource, AudioListener ve AudioClip bileşenlerinden oluşur.

## 1. Unity ses sisteminin temel bileşenleri

![Unity Audio Tutorial - Complete Guide - GameDev Academy](https://images.openai.com/static-rsc-4/OSkpFijG3F66-sqH8GTma8Je4oA8Kq-0LmpOsYIsaBKAAgCACM9xX6mokAuVTvPwBBL3CC4IiPZHIScYWPONhq6HzczHtZlVQLXUaPS9qLWn13SydjcxK1KcvowBcKJ5_Qpixvyuu0xfkAq0pn5gh6GP_LYu5k6L9fdwsA9n-ao?purpose=inline)

AudioSource

Sesi oynatan bileşendir. Sesin yüksekliğini, hızını, döngüye girip girmeyeceğini ve 3D konumunu kontrol eder.



AudioListener

Sesleri dinleyen bileşendir. Genellikle Main Camera üzerinde bulunur. Oyuncunun sesleri hangi konumdan duyduğunu belirler.

![Preload Audio Data & How Unity Decides Which Audio Assets to Load to a Scene | by Made Indrayana | Medium](https://images.openai.com/static-rsc-4/g6A6N2WZPw_m296-x6o4ux75KK5nMZK6xAbpLZLNPaKYvd6ODLyNEksACoN2BltPb_23hJ5m_9K2DHEELbD0pFzp-lJhZrITXVPquQtU58KsRsZwvFu6F9xuspQ84f40aQUQgLhPQ9HLkq5x9ITjJeoo4DN5NrSRm9DquKkJy0E?purpose=inline)

AudioClip

Ses dosyasının kendisidir. Örneğin `.wav`, `.mp3` veya `.ogg` dosyaları AudioClip olarak kullanılabilir.

## 2. AudioSource nedir?

`AudioSource`, bir GameObject üzerinden ses oynatmamızı sağlayan Unity bileşenidir.

Örneğin bir karakterin zıpladığında ses çıkarmasını istiyorsak karaktere AudioSource ekleyebiliriz.

```
using UnityEngine;

public class PlayerSound : MonoBehaviour
{
    private AudioSource audioSource;

    void Start()
    {
        audioSource = GetComponent<AudioSource>();
    }

    void Update()
    {
        if (Input.GetKeyDown(KeyCode.Space))
        {
            audioSource.Play();
        }
    }
}
```

Bu örnekte eski Input API'si kullanılmıştır. Projen yalnızca yeni Input System kullanıyorsa giriş kontrolünü ona göre değiştirmelisin.

Kodun çalışma mantığı:

1. `GetComponent<AudioSource>()` ile nesnenin AudioSource bileşenini alıyoruz.
    
2. Oyuncu Space tuşuna bastığında koşul sağlanıyor.
    
3. `audioSource.Play()` çağrılıyor ve ses oynatılıyor.
    

AudioSource üzerinde bir AudioClip atanmış olması gerekir.

## 3. AudioSource metotları

|Metot|Açıklama|
|---|---|
|`Play()`|Sesi oynatır.|
|`Pause()`|Sesi duraklatır.|
|`UnPause()`|Duraklatılan sesi devam ettirir.|
|`Stop()`|Sesi durdurur ve oynatma konumunu sıfırlar.|
|`PlayOneShot()`|Tek seferlik ses efekti oynatır.|
|`PlayDelayed()`|Belirtilen süre sonra sesi başlatır.|

### Play() ve PlayOneShot() farkı

`Play()` AudioSource'a atanmış AudioClip'i oynatır. Aynı AudioSource üzerinde tekrar `Play()` çağrılması mevcut oynatmayı yeniden başlatır.

`PlayOneShot()` ise bir ses efektini, mevcut oynatmayı kesmeden çalabilir.

```
[SerializeField] private AudioSource audioSource;
[SerializeField] private AudioClip jumpSound;

void Jump()
{
    audioSource.PlayOneShot(jumpSound);
}
```

Bu yöntem özellikle zıplama, ateş etme ve çarpışma gibi kısa ses efektlerinde kullanışlıdır.

## 4. AudioSource özellikleri

|Özellik|Açıklama|
|---|---|
|`clip`|Oynatılacak ses dosyası|
|`volume`|Ses yüksekliği (0–1)|
|`pitch`|Sesin hızı ve perdesi|
|`loop`|Sesin sürekli tekrarlanması|
|`mute`|Sesi kapatır|
|`playOnAwake`|Nesne aktif olduğunda otomatik oynatma|
|`spatialBlend`|2D ve 3D ses arasındaki geçiş|
|`isPlaying`|Sesin oynatılıp oynatılmadığı|

Örnek:

```
audioSource.volume = 0.5f;
audioSource.pitch = 1.2f;
audioSource.loop = true;

audioSource.Play();
```

Burada ses yüksekliği `0.5` olarak ayarlanır, ses normalden daha hızlı ve tiz oynatılır, ayrıca sürekli tekrar eder.

## 5. 2D ve 3D ses farkı

![Set of audio icons including headphones, speaker, and sound waves. Lstening, music and technology concept. Vector illustration isolated on white background, flat design](https://images.openai.com/static-rsc-4/HRzmh71YJ0IB9VQxFPf4WBO6US4J9McBFlufx7dt1cJB8LAkIwA0EvxVlHk-Oxi0CiLWLJw0woBxQ8Llf99FeE6T3eHTgqX2u50epdWJqrqCCV0FPdAhukq4_8BfFqHAyQgeuYRSBI88p7wL8k3wekAzACfAI7742g1qaDcdXg4hQa1Re0HEgfMy067QVqe8?purpose=inline)

2D Sound

Ses, oyuncunun dünyadaki konumundan bağımsızdır.

Örnek: Menü müziği, arayüz sesleri.

![VRChatアバターから音を鳴らそう！「VRC Spatial Audio Suroce」解説｜ハカセ](https://images.openai.com/static-rsc-4/YeCPaazUjuY8Klh-Ygg9O3F9YqkGk-IW7rjGQUX5qYxXimWtGMgDhn1XuUAF3bNoQwhrdjZ6aCUzKwS73bLzfM3nKfqdKNu8Xndl6FjKIjnIp2mWbItLxp7NKp6_N_GR7nt_rS8M8K28yYRlqzi2erNlA3ZzgycIK6hyyfIO7Bc?purpose=inline)

3D Sound

Sesin duyulması, oyuncunun konumuna ve ses kaynağına olan uzaklığına bağlıdır.

Örnek: Yaklaşan araba, uzaktaki patlama.

Bu özellik `spatialBlend` ile ayarlanır.

```
audioSource.spatialBlend = 0f; // Tamamen 2D
audioSource.spatialBlend = 1f; // Tamamen 3D
```

3D seslerde `Min Distance`, `Max Distance` ve `Rolloff` ayarları sesin mesafeyle nasıl azaldığını belirler.

## 6. Yeni Input System ile ses oynatma

Unity'nin yeni Input System paketini kullanıyorsan aşağıdaki gibi bir örnek oluşturabilirsin:

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerSound : MonoBehaviour
{
    [SerializeField] private AudioSource audioSource;
    [SerializeField] private AudioClip jumpSound;

    void Update()
    {
        if (Keyboard.current != null &&
            Keyboard.current.spaceKey.wasPressedThisFrame)
        {
            audioSource.PlayOneShot(jumpSound);
        }
    }
}
```

Burada:

- `Keyboard.current` klavyeye erişmemizi sağlar.
    
- `spaceKey.wasPressedThisFrame` Space tuşuna basıldığı karede `true` döner.
    
- `PlayOneShot(jumpSound)` zıplama sesini oynatır.
    
- `[SerializeField]` sayesinde AudioSource ve AudioClip alanlarını Inspector üzerinden atayabiliriz.
    

## 7. Unity'nin diğer ses bileşenleri

Ses sistemi yalnızca AudioSource'dan oluşmaz.

|Bileşen|Görevi|
|---|---|
|`AudioMixer`|Sesleri gruplandırır ve yönetir.|
|`AudioMixerGroup`|Müzik, efekt ve ortam seslerini ayırır.|
|`AudioReverbZone`|Yankı efektleri oluşturur.|
|`AudioLowPassFilter`|Yüksek frekansları azaltır.|
|`AudioHighPassFilter`|Düşük frekansları azaltır.|

Örneğin bir oyunda müzik, ses efektleri ve ortam seslerinin seviyelerini ayrı ayrı kontrol etmek istiyorsan AudioMixer kullanabilirsin.

Özet: Unity ses sisteminde `AudioClip` ses dosyasını tutar, `AudioSource` sesi oynatır, `AudioListener` ise sesi dinler. Daha gelişmiş ses yönetimi için `AudioMixer` kullanılır.

