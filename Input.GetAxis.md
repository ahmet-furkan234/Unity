Tabii TkMatE. `Input.GetAxis` Unity'de **klavye, gamepad gibi girişlerden hareket değeri almak** için kullanılan eski Input System metodlarından biridir.

Temel kullanım:

```
float horizontal = Input.GetAxis("Horizontal");
float vertical = Input.GetAxis("Vertical");
```

## `Input.GetAxis("Horizontal")` ne döndürür?

`Horizontal` varsayılan olarak sağ-sol hareketini temsil eder.

```
float x = Input.GetAxis("Horizontal");
```

Kabaca:

```
A / Sol ok    → -1
Tuş yok       →  0
D / Sağ ok    → +1
```

Ama önemli nokta: `GetAxis` değeri genellikle **bir anda -1 veya +1 olmaz**. Yumuşak şekilde değişir:

```
D'ye bastın:

0
0.1
0.3
0.5
0.7
0.9
1.0
```

Tuşu bıraktığında da:

```
1.0
0.8
0.6
0.4
0.2
0
```

Bu yüzden karakter hareketlerinde daha akıcı sonuç verir.

Örneğin:

```
void Update()
{
    float horizontal = Input.GetAxis("Horizontal");

    Debug.Log(horizontal);
}
```

D'ye basarsan Console'da buna benzer değerler görürsün.

### Karakteri hareket ettirelim

Bir önceki konuştuğumuz `Transform.Translate` ile birleştirebiliriz:

```
public class PlayerMovement : MonoBehaviour
{
    [SerializeField]
    private float speed = 5f;

    void Update()
    {
        float horizontal = Input.GetAxis("Horizontal");

        transform.Translate(
            horizontal * speed * Time.deltaTime,
            0,
            0
        );
    }
}
```

Burada zincir şöyle:

```
Klavye
  ↓
Input.GetAxis("Horizontal")
  ↓
-1 ile +1 arasında değer
  ↓
horizontal * speed
  ↓
Time.deltaTime
  ↓
Transform.Translate()
  ↓
Karakter hareket eder
```

Mesela:

```
horizontal = 1;
speed = 5;
Time.deltaTime = 0.016;
```

ise o frame'deki hareket:

```
1 × 5 × 0.016
= 0.08 Unity Unit
```

Sola basarsan:

```
-1 × 5 × 0.016
= -0.08
```

ve karakter ters yönde gider.

### `Vertical` da aynı mantık

```
float vertical = Input.GetAxis("Vertical");
```

Varsayılan olarak:

```
W / Yukarı → +1
S / Aşağı  → -1
```

3D oyunda X-Z düzleminde hareket istiyorsan:

```
void Update()
{
    float x = Input.GetAxis("Horizontal");
    float z = Input.GetAxis("Vertical");

    transform.Translate(
        x * speed * Time.deltaTime,
        0,
        z * speed * Time.deltaTime
    );
}
```

Bunu biraz daha temiz şekilde `Vector3` ile de yazabiliriz:

```
void Update()
{
    float x = Input.GetAxis("Horizontal");
    float z = Input.GetAxis("Vertical");

    Vector3 movement = new Vector3(x, 0, z);

    transform.Translate(movement * speed * Time.deltaTime);
}
```

Buradaki **`Vector3` kısmı Unity hareket sistemini anlamak için önemli**. Sonraki adımda `Vector3`'ün ne olduğunu ve neden `new Vector3(x, 0, z)` yazdığımızı öğrenmek mantıklı olur.

Hareket kodunu biraz daha açalım

- Vector3 ile hareketi açıkla
- GetAxis ile GetAxisRaw farkını göster