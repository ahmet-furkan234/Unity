
`UnityEngine.InputSystem`, Unity'nin yeni nesil kullanıcı giriş (Input) sistemidir. Klavye, fare, gamepad, dokunmatik ekran ve diğer giriş cihazlarından gelen verileri okumamızı sağlar.

Eski `UnityEngine.Input` sisteminin daha modern, esnek ve cihaz bağımsız alternatifidir.

## 1. Eski ve yeni Input System farkları

|Özellik|Eski Input System|Yeni Input System|
|---|---|---|
|Namespace|`UnityEngine.Input`|`UnityEngine.InputSystem`|
|Klavye|`Input.GetKey()`|`Keyboard.current`|
|Fare|`Input.GetMouseButton()`|`Mouse.current`|
|Hareket|`Input.GetAxis()`|`InputAction`|
|Gamepad|Manuel yapılandırma|Daha kapsamlı destek|
|Tuş değiştirme|Daha zahmetli|Rebinding desteği|
|Olay tabanlı kullanım|Sınırlı|`started`, `performed`, `canceled`|

Önemli: Yeni Input System kullanırken yalnızca `Keyboard.current` gibi sınıflarla sınırlı değiliz. `InputAction` sayesinde aynı hareketi klavye, gamepad veya farklı cihazlarla gerçekleştirebiliriz.

## 2. Input System nasıl kullanılır?

Öncelikle scriptimize şu namespace'i ekliyoruz:

```
using UnityEngine;
using UnityEngine.InputSystem;
```

Böylece `Keyboard`, `Mouse`, `Gamepad` ve `InputAction` gibi sınıflara erişebiliriz.

### Klavyeden giriş almak

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerController : MonoBehaviour
{
    void Update()
    {
        if (Keyboard.current != null &&
            Keyboard.current.spaceKey.wasPressedThisFrame)
        {
            Debug.Log("Space tuşuna basıldı!");
        }
    }
}
```

Burada:

- `Keyboard.current`: Aktif klavye cihazına erişir.
    
- `spaceKey`: Space tuşunu temsil eder.
    
- `wasPressedThisFrame`: Tuşa bu frame içerisinde basılıp basılmadığını kontrol eder.
    

### Klavye tuşlarının kontrol yöntemleri

|Metot / Özellik|Açıklama|
|---|---|
|`isPressed`|Tuş basılı tutulduğu sürece `true`|
|`wasPressedThisFrame`|Tuşa basıldığı frame'de `true`|
|`wasReleasedThisFrame`|Tuşun bırakıldığı frame'de `true`|

Örnek:

```
if (Keyboard.current.wKey.isPressed)
{
    Debug.Log("W basılı tutuluyor");
}

if (Keyboard.current.spaceKey.wasPressedThisFrame)
{
    Debug.Log("Space'e basıldı");
}

if (Keyboard.current.spaceKey.wasReleasedThisFrame)
{
    Debug.Log("Space bırakıldı");
}
```

## 3. Yeni Input System ile karakter hareketi

Eski sistemde şöyle kullanıyorduk:

```
float horizontal = Input.GetAxis("Horizontal");
float vertical = Input.GetAxis("Vertical");
```

Yeni sistemde klavye durumunu doğrudan okuyabiliriz:

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerMovement : MonoBehaviour
{
    [SerializeField] private float speed = 5f;

    void Update()
    {
        if (Keyboard.current == null)
            return;

        Vector3 direction = Vector3.zero;

        if (Keyboard.current.wKey.isPressed)
            direction.z += 1;

        if (Keyboard.current.sKey.isPressed)
            direction.z -= 1;

        if (Keyboard.current.aKey.isPressed)
            direction.x -= 1;

        if (Keyboard.current.dKey.isPressed)
            direction.x += 1;

        direction.Normalize();

        transform.Translate(direction * speed * Time.deltaTime);
    }
}
```

Çalışma mantığı:

1. `Vector3.zero` ile başlangıçta hareket yönünü sıfırlıyoruz.
    
2. W, A, S, D tuşlarından hangisinin basılı olduğunu kontrol ediyoruz.
    
3. `direction` vektörünü hareket yönüne göre değiştiriyoruz.
    
4. `Normalize()` ile çapraz hareketin daha hızlı olmasını engelliyoruz.
    
5. `speed * Time.deltaTime` ile frame hızından bağımsız hareket sağlıyoruz.
    

Bu örnek yerel koordinatlarda hareket eder çünkü `Transform.Translate()` varsayılan olarak `Space.Self` kullanır.

## 4. Mouse sınıfı

Yeni Input System ile farenin konumunu, tıklamaları ve kaydırma hareketini okuyabiliriz.

```
void Update()
{
    if (Mouse.current == null)
        return;

    if (Mouse.current.leftButton.wasPressedThisFrame)
    {
        Debug.Log("Sol tık!");
    }

    if (Mouse.current.rightButton.isPressed)
    {
        Debug.Log("Sağ tık basılı");
    }

    Vector2 mousePosition =
        Mouse.current.position.ReadValue();

    Debug.Log(mousePosition);
}
```

Burada `ReadValue()` farenin ekran üzerindeki konumunu `Vector2` olarak döndürür.

## 5. InputAction nedir?

Yeni Input System'in en önemli özelliklerinden biri InputAction yapısıdır.

`InputAction`, belirli bir tuşa bağımlı olmadan oyuncunun yapmak istediği eylemi tanımlamamızı sağlar.

Örneğin `Jump` isminde bir eylem tanımladığımızı düşünelim:

Jump (InputAction)

Space

Klavye

A

Gamepad

Tap

Dokunmatik

Farklı cihazlardan gelen girişler aynı Jump eylemini tetikleyebilir.

### InputAction olayları

|Olay|Görevi|
|---|---|
|`started`|Eylem başladığında|
|`performed`|Eylem gerçekleştiğinde|
|`canceled`|Eylem iptal edildiğinde veya sona erdiğinde|

Bu olayların tam olarak ne zaman tetiklendiği, eylemin türüne ve tanımlanan etkileşimlere bağlıdır.

Örnek:

```
using UnityEngine;
using UnityEngine.InputSystem;

public class PlayerJump : MonoBehaviour
{
    private InputAction jumpAction;

    void Awake()
    {
        jumpAction = new InputAction(
            "Jump",
            InputActionType.Button,
            "<Keyboard>/space"
        );

        jumpAction.performed += OnJump;
    }

    void OnEnable()
    {
        jumpAction.Enable();
    }

    void OnDisable()
    {
        jumpAction.Disable();
    }

    void OnDestroy()
    {
        jumpAction.performed -= OnJump;
        jumpAction.Dispose();
    }

    void OnJump(InputAction.CallbackContext context)
    {
        Debug.Log("Oyuncu zıpladı!");
    }
}
```

Burada:

- `new InputAction()`: Yeni bir giriş eylemi oluşturur.
    
- `"<Keyboard>/space"`: Eylemi Space tuşuna bağlar.
    
- `performed += OnJump`: Eylem gerçekleştiğinde `OnJump` metodunu çalıştırır.
    
- `Enable()`: Eylemi aktif hâle getirir.
    
- `Disable()`: Eylemi pasifleştirir.
    
- `Dispose()`: Oluşturduğumuz eylemin kaynaklarını temizler.
    

`OnJump` metodu şu anda yalnızca mesaj yazdırır. Gerçek zıplama için Rigidbody veya karakter hareket sistemine kuvvet/hız uygulamamız gerekir.

## 6. Input System'in iki temel kullanım yaklaşımı

Doğrudan cihaz okuma

`Keyboard.current`, `Mouse.current` gibi sınıflar kullanılır. Öğrenmesi kolaydır ve küçük prototipler için uygundur.

Input Actions / Input Action Asset

Hareket, zıplama ve saldırı gibi eylemler tanımlanır. Tuş atamaları ayrı yönetilebilir. Çoklu platform ve büyük projelerde daha avantajlıdır.

## 7. Önemli notlar

- `UnityEngine.InputSystem`, eski `UnityEngine.Input` ile aynı şey değildir.
    
- Yeni sistemde `Input.GetAxis()` yerine genellikle `InputAction` kullanılır.
    
- `Keyboard.current` yalnızca klavye girişlerini okur.
    
- `InputAction` birden fazla cihazı aynı eyleme bağlayabilir.
    
- `InputAction` kullanırken eylemi etkinleştirmek gerekir.
    
- Yeni sistemde `PlayerInput` component'i kullanılarak Input Actions ile oyuncu scriptleri ilişkilendirilebilir.
    

Öğrenme sırası olarak `Keyboard.current` ve `Mouse.current` mantığını öğrendikten sonra `InputAction`, `Input Action Asset` ve `PlayerInput` konularına geçmek en uygunu. Özellikle mobil oyun geliştirmede cihaz bağımsız giriş yönetimi önemli avantaj sağlar.