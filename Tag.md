Unity'de **Tag**, bir `GameObject`'un **hangi türden/rolden nesne olduğunu belirtmek için kullanılan etikettir**.

Örneğin sahnende şu nesneler olsun:

```
Player
Enemy
Bullet
Ground
Coin
```

Bunlara uygun tag'ler verebilirsin:

```
Player  → "Player"
Enemy   → "Enemy"
Bullet  → "Bullet"
Ground  → "Ground"
```

### Neden kullanıyoruz?

Örneğin bir mermi başka bir nesneye çarptığında, çarptığı şeyin oyuncu mu, düşman mı olduğunu anlamak isteyebilirsin:

```
void OnCollisionEnter(Collision collision)
{
    if (collision.gameObject.CompareTag("Enemy"))
    {
        Debug.Log("Düşmana çarptım!");
    }
}
```

Buradaki:

```
collision.gameObject.CompareTag("Enemy")
```

şu soruyu soruyor:

> "Çarptığım GameObject'in Tag'i `Enemy` mi?"

Eğer öyleyse `if` içerisine giriyor.

### `gameObject.tag` ile farkı

Şöyle de yazabilirsin:

```
if (collision.gameObject.tag == "Enemy")
{
    Debug.Log("Düşman!");
}
```

Ama Unity'de tercih edilen yöntem:

```
if (collision.gameObject.CompareTag("Enemy"))
```

şeklindedir.

### Tag ile nesne bulmak

Tag kullanarak sahnedeki bir nesneyi de bulabilirsin:

```
GameObject player;

void Start()
{
    player = GameObject.FindGameObjectWithTag("Player");
}
```

Böylece adı `"Player"` olmak zorunda değildir. Örneğin:

```
GameObject adı:     MainCharacter
Tag:                Player
```

olsa bile:

```
GameObject.FindGameObjectWithTag("Player");
```

onu bulabilir.

### Önemli: Bir GameObject'in tek Tag'i vardır

Bir nesne aynı anda:

```
Enemy
Boss
Flying
```

gibi üç farklı tag'e sahip olamaz.

Örneğin Boss için:

```
GameObject: Dragon
Tag: Enemy
```

yapıp, `Boss` olup olmadığını başka bir component/script üzerinden yönetebilirsin.

**Kısaca:**

> **Tag = GameObject'ları rollerine/türlerine göre tanımamızı sağlayan etikettir.**

Özellikle birazdan `OnCollisionEnter`, `OnTriggerEnter` gibi çarpışma konularına geçtiğinde **Tag + Collider + Rigidbody** beraber çok sık karşına çıkacak.

Tag kullanımını pekiştir

- Tag oluşturma ve atama
- Tag ile çarpışma kontrolü