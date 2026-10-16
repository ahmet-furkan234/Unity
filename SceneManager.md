
`SceneManager`, Unity'de sahneleri (Scene) yönetmek için kullanılan bir sınıftır.

Bir oyunda sahneler arasında geçiş yapmak, mevcut sahneyi yeniden başlatmak, yeni sahne yüklemek veya sahneyi kaldırmak için kullanılır.

Örneğin bir mobil oyunda:

- Ana Menü → Oyun
    
- Oyun → Game Over
    
- Game Over → Yeniden Başlat
    
- Oyun → Sonraki Bölüm
    

Bu işlemleri `SceneManager` ile gerçekleştirebiliriz.

## 1. SceneManager Nasıl Kullanılır?

`SceneManager`, `UnityEngine.SceneManagement` namespace'i içerisinde bulunur.

```
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    public void StartGame()
    {
        SceneManager.LoadScene("GameScene");
    }
}
```

Kodun çalışma mantığı:

1. `using UnityEngine.SceneManagement;` → SceneManager sınıfına erişmemizi sağlar.
    
2. `SceneManager.LoadScene()` → Belirtilen sahneyi yükler.
    
3. `"GameScene"` → Yüklenecek sahnenin adıdır.
    

Burada `GameScene` isimli sahne yüklenir ve varsayılan olarak önceki sahne kaldırılır.

## 2. SceneManager Metotları

| Metot                     | Görevi                               |
| ------------------------- | ------------------------------------ |
| `LoadScene()`             | Sahne yükler                         |
| `LoadSceneAsync()`        | Sahneyi asenkron yükler              |
| `GetActiveScene()`        | Aktif sahneyi getirir                |
| `GetSceneByName()`        | İsme göre sahne bilgisini getirir    |
| `GetSceneByBuildIndex()`  | Build indeksine göre sahneyi getirir |
| `GetSceneAt()`            | Yüklü sahnelerden birini getirir     |
| `SetActiveScene()`        | Aktif sahneyi değiştirir             |
| `UnloadSceneAsync()`      | Yüklenmiş sahneyi kaldırır           |
| `MoveGameObjectToScene()` | GameObject'i başka sahneye taşır     |

## 3. LoadScene() – Sahne Yükleme

Sahne yüklemek için en sık kullanılan metottur.

```
SceneManager.LoadScene("Level1");
```

Sahne ismi yerine Build Index de kullanılabilir:

```
SceneManager.LoadScene(1);
```

Örneğin sahnelerimizin indeksleri şöyle olsun:

|Build Index|Sahne|
|---|---|
|0|MainMenu|
|1|Level1|
|2|Level2|
|3|GameOver|

Bu durumda:

```
SceneManager.LoadScene(2);
```

`Level2` sahnesini yükler.

Önemli: Sahnelerin Unity'de Build Profiles içerisindeki Scene List'e (eski sürümlerde Build Settings) eklenmiş olması gerekir.

## 4. GetActiveScene() – Aktif Sahneyi Öğrenme

Şu anda aktif olan sahnenin bilgilerini döndürür.

```
Scene currentScene = SceneManager.GetActiveScene();

Debug.Log(currentScene.name);
Debug.Log(currentScene.buildIndex);
```

Örneğin Level1 sahnesindeysek:

```
Level1
1
```

Burada `Scene`, sahne bilgilerini temsil eden bir struct'tır.

Sık kullanılan özellikleri:

- `name` → Sahnenin adı
    
- `buildIndex` → Build indeksi
    
- `isLoaded` → Sahnenin yüklenip yüklenmediği
    
- `path` → Sahnenin dosya yolu
    
- `rootCount` → Sahnedeki kök GameObject sayısı
    

## 5. Mevcut Sahneyi Yeniden Başlatma

Özellikle oyuncu öldüğünde veya bölüm başarısız olduğunda kullanılır.

```
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    public void RestartGame()
    {
        SceneManager.LoadScene(
            SceneManager.GetActiveScene().buildIndex
        );
    }
}
```

Çalışma mantığı:

1. `GetActiveScene()` mevcut sahneyi bulur.
    
2. `.buildIndex` sahnenin indeksini alır.
    
3. `LoadScene()` aynı sahneyi yeniden yükler.
    

Böylece sahne başlangıç durumuna döner. Ancak `DontDestroyOnLoad` ile korunan nesneler ve statik değişkenler otomatik sıfırlanmaz.

## 6. Sonraki Bölüme Geçme

```
public void NextLevel()
{
    int currentLevel = SceneManager.GetActiveScene().buildIndex;

    SceneManager.LoadScene(currentLevel + 1);
}
```

Örneğin:

`Level1 (1)` → `Level2 (2)` → `Level3 (3)`

Burada dikkat edilmesi gereken nokta, sonraki indeksin gerçekten mevcut olmasıdır.

## 7. LoadSceneAsync() – Asenkron Sahne Yükleme

`LoadScene()` yerine `LoadSceneAsync()` kullanarak sahneyi asenkron yükleyebiliriz.

```
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    public void LoadGame()
    {
        SceneManager.LoadSceneAsync("Level1");
    }
}
```

Aralarındaki temel fark:

|`LoadScene()`|`LoadSceneAsync()`|
|---|---|
|Normal sahne yükleme|Asenkron sahne yükleme|
|Yükleme sırasında takılma yaşanabilir|Yükleme işlemi karelere yayılabilir|
|Basit geçişler için uygun|Büyük sahneler ve yükleme ekranları için uygun|
|İlerleme takibi sunmaz|`AsyncOperation.progress` ile ilerleme izlenebilir|

Asenkron yükleme, oyunun hiçbir zaman takılmayacağını garanti etmez.

## 8. LoadSceneMode – Sahne Yükleme Modları

SceneManager iki temel yükleme modu sunar.

### Single

```
SceneManager.LoadScene("Level2", LoadSceneMode.Single);
```

Yeni sahneyi yükler ve mevcut sahneleri kaldırır. `DontDestroyOnLoad` nesneleri korunur.

### Additive

```
SceneManager.LoadScene("Level2", LoadSceneMode.Additive);
```

Yeni sahneyi mevcut sahneleri kaldırmadan yükler.

Örneğin bir oyunda:

MainScene

Oyuncu, kamera ve oyun yöneticisi

Level2

Harita, düşmanlar ve çevre nesneleri

İki sahne aynı anda yüklü

Additive yükleme sonucu

Bu yöntem özellikle büyük oyunlarda sahneleri parçalara ayırmak için kullanılır.

## 9. UnloadSceneAsync() – Sahneyi Kaldırma

Additive olarak yüklenmiş sahneleri kaldırmak için kullanılır.

```
SceneManager.UnloadSceneAsync("Level2");
```

Bu işlem sahnedeki GameObject'leri de kaldırır. Ancak kullanılmayan bütün asset'lerin belleği anında boşaltılmayabilir.

## 10. SceneManager Olayları (Events)

SceneManager yalnızca metotlardan oluşmaz. Sahne değişimlerini takip etmek için event'ler de sunar.

|Event|Açıklama|
|---|---|
|`sceneLoaded`|Sahne yüklendiğinde tetiklenir|
|`sceneUnloaded`|Sahne kaldırıldığında tetiklenir|
|`activeSceneChanged`|Aktif sahne değiştiğinde tetiklenir|

Örnek:

```
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    private void OnEnable()
    {
        SceneManager.sceneLoaded += OnSceneLoaded;
    }

    private void OnDisable()
    {
        SceneManager.sceneLoaded -= OnSceneLoaded;
    }

    private void OnSceneLoaded(Scene scene, LoadSceneMode mode)
    {
        Debug.Log("Yüklenen sahne: " + scene.name);
    }
}
```

Burada:

- `+=` → Event'e abone olur.
    
- `-=` → Event aboneliğini kaldırır.
    
- `OnSceneLoaded()` → Sahne yüklendiğinde çalışır.
    

Aboneliği kaldırmak, özellikle sahneler arasında geçiş yapan oyunlarda gereksiz event çağrılarını önlemek açısından önemlidir.

## 11. Uygulamalı Örnek: Game Over Sistemi

Oyuncu öldüğünde Game Over sahnesine geçelim. Ardından oyuncu yeniden başlatma butonuna basarsa ilk bölümü yükleyelim.

```
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    public void PlayerDied()
    {
        SceneManager.LoadScene("GameOver");
    }

    public void RestartGame()
    {
        SceneManager.LoadScene("Level1");
    }

    public void MainMenu()
    {
        SceneManager.LoadScene("MainMenu");
    }
}
```

Bu metotları Unity UI Button bileşeninin `On Click()` olayına bağlayabiliriz.

## Özet

`SceneManager`, Unity'nin sahne yönetim sistemidir.

En çok kullanacağın dört işlem:

```
// Sahne yükle
SceneManager.LoadScene("Level1");

// Mevcut sahneyi yeniden başlat
SceneManager.LoadScene(
    SceneManager.GetActiveScene().buildIndex
);

// Sonraki sahneye geç
SceneManager.LoadScene(
    SceneManager.GetActiveScene().buildIndex + 1
);

// Asenkron sahne yükle
SceneManager.LoadSceneAsync("Level2");
```

Bir sonraki öğrenmeni önerdiğim konu: `DontDestroyOnLoad()`. Çünkü SceneManager ile sahne değiştirirken müzik, oyuncu verileri veya GameManager gibi nesnelerin neden yok olduğunu ve bunları sahneler arasında nasıl koruyacağını anlamanı sağlar.**