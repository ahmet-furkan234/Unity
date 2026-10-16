
Unity'de Timeline üzerindeki üç nokta (`...`) menüsünden açtığın Animation penceresi, GameObject'lerin özelliklerini zaman içerisinde değiştirerek animasyon oluşturmanı sağlar.

Örneğin bir küpü 3 saniyede X ekseninde `0` konumundan `10` konumuna hareket ettirebilirsin.

### 1. Animation nasıl çalışır?

Animation sisteminin temelinde Keyframe (Anahtar Kare) bulunur.

Keyframe, bir nesnenin belirli bir zamandaki özellik değerini kaydeder.

Örnek: Küpün X eksenindeki hareketi

0 saniye

1.6 saniye

3 saniye

Zamanı değiştirerek küpün konumunun nasıl değiştiğini incele.

Burada iki Keyframe oluşturduk:

- `0. saniye` → Position X = 0
    
- `3. saniye` → Position X = 10
    

Unity, aradaki konumları otomatik hesaplar. Bu işleme Interpolation (Enterpolasyon) denir.

### 2. Animation ile neleri değiştirebiliriz?

|Özellik|Örnek|
|---|---|
|Position|Nesneyi hareket ettirme|
|Rotation|Nesneyi döndürme|
|Scale|Nesneyi büyütme/küçültme|
|Color|Rengini değiştirme|
|Sprite|2D kare animasyonu|
|Component Properties|Desteklenen bileşen özelliklerini değiştirme|

### 3. Animation penceresindeki önemli kavramlar

- Record (Kırmızı nokta): Animasyon kaydetme modunu açar.
    
- Add Property: Animasyona yeni bir özellik ekler.
    
- Dopesheet: Keyframe'leri zaman çizelgesinde gösterir.
    
- Curves: Animasyonun hızını ve değişim eğrisini düzenler.
    
- Samples: Animasyonun saniyedeki örnekleme hızını belirler.
    
- Preview: Animasyonu düzenleme sırasında önizlemeni sağlar.
    

### 4. Timeline ile Animation arasındaki fark

Animation: Bir nesnenin nasıl hareket edeceğini veya özelliklerinin nasıl değişeceğini belirler.

Timeline: Bu animasyonun ne zaman başlayacağını, ne kadar süreceğini ve diğer animasyonlarla nasıl senkronize edileceğini yönetir.

Örneğin Animation ile kapının açılma hareketini hazırlarsın. Timeline ile kapının 5. saniyede açılmasını sağlarsın.

Önemli: Animation penceresinde hazırladığın animasyonlar genellikle `.anim` uzantılı Animation Clip dosyalarında saklanır. Timeline bu Clip'leri kullanabilir.