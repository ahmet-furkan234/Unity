
Cinemachine, Unity'de kameranın hedefi nasıl takip edeceğini, nereye bakacağını, ne kadar hızlı döneceğini ve hareketleri ne kadar yumuşak gerçekleştireceğini kontrol eden kamera sistemidir.

Özellikle geliştirdiğin Galaxy Strike oyununda uzay gemisini arkadan takip eden profesyonel bir kamera oluşturmak için oldukça kullanışlıdır.

Cinemachine 3.x sürümündeki ayarlar üzerinden ilerleyelim. Cinemachine 2.x kullanıyorsan bazı ayarların isimleri farklıdır.

## 1. Cinemachine Camera – Temel ayarlar

![Cinemachine Decollider going through objects - Cinemachine - Unity Discussions](https://images.openai.com/static-rsc-4/ev8aNaLHzX2LORdCigZ0A3dSSmpgaePBXvejMQHhTfBDVmw-_EEfovxfQoykJiKgNM8XpG8ZJgsOamSmkeQCNGQXMSY6ei-zE6m8XFYE_eAZEoODMKYG2px9P6fmED_q5_NN7DyMN2Jpvn06JGrjulDgbVUZZ6rd2DQqcM03DXI?purpose=inline)

[discussions.unity.com](https://discussions.unity.com/t/cinemachine-decollider-going-through-objects/1698234)

|Ayar|Açıklama|
|---|---|
|Priority|Birden fazla Cinemachine Camera varsa hangisinin aktif olacağını belirler.|
|Tracking Target|Kameranın takip edeceği Transform nesnesidir.|
|Position Control|Kameranın hedefe göre nasıl konumlanacağını belirler.|
|Rotation Control|Kameranın hangi yöne bakacağını belirler.|
|Lens|Görüş açısı, yakın ve uzak kırpma mesafeleri gibi kamera özelliklerini kontrol eder.|
|Noise|Kamera sarsıntısı ve titreşim efektlerini yönetir.|

## 2. Position Control – Kamera konumu

Kameranın uzay gemisine göre nerede duracağını ve nasıl hareket edeceğini belirleyen en önemli bölümdür.

### Cinemachine Follow

Kameranın hedefi belirli bir mesafeden takip etmesini sağlar.

|Ayar|Açıklama|
|---|---|
|Follow Offset|Kameranın hedefe göre konumu.|
|X Damping|Yatay hareketin yumuşaklığı.|
|Y Damping|Dikey hareketin yumuşaklığı.|
|Z Damping|İleri-geri hareketin yumuşaklığı.|

Örnek:

```
Follow Offset
X = 0
Y = 3
Z = -8

Damping
X = 0.5
Y = 0.5
Z = 0.5
```

Bu ayarlarda kamera geminin 3 birim üstünde ve 8 birim arkasında durmayı hedefler.

Damping mantığı:

- `0`: Kamera hedefi gecikmesiz takip eder.
    
- `0.5`: Hafif yumuşatma uygulanır.
    
- `2`: Kamera daha yavaş tepki verir.
    
- `5`: Belirgin gecikme oluşur.
    

Damping değeri yükseldikçe kamera hedefin hareketine daha geç uyum sağlar.

### Diğer Position Control seçenekleri

|Seçenek|Kullanım amacı|
|---|---|
|Follow|Hedefi belirli bir offset ile takip etmek.|
|Orbital Follow|Hedefin etrafında dönebilen kamera oluşturmak.|
|Third Person Follow|Omuz üstü veya üçüncü şahıs kamera oluşturmak.|
|Position Composer|Hedefi ekranda istenen bölgede tutacak şekilde kamerayı konumlandırmak.|
|Hard Lock To Target|Kamerayı doğrudan hedefin konumuna sabitlemek.|
|Spline Dolly|Kamerayı bir spline rotası boyunca hareket ettirmek.|

## 3. Rotation Control – Kamera dönüşü

Bu bölüm kameranın hangi yöne bakacağını belirler.

### Rotation Composer

Kameranın hedefe bakmasını ve hedefin ekrandaki konumunu kontrol eder.

|Ayar|Açıklama|
|---|---|
|Tracking Target|Kameranın baktığı hedef.|
|Screen Position|Hedefin ekrandaki konumu.|
|Dead Zone|Hedefin hareket etmesine rağmen kameranın tepki vermediği alan.|
|Soft Zone|Kameranın hedefi yumuşak biçimde takip ettiği alan.|
|Damping|Kamera dönüşünün yumuşaklığı.|

Dead Zone ve Soft Zone'un kavramsal gösterimi.

Dead Zone: Uzay gemisi bu bölge içinde hareket ettiğinde kamera yönünü değiştirmek zorunda kalmaz.

Soft Zone: Gemi bu bölgeye geldiğinde kamera yumuşak bir dönüşle onu yeniden hedef konumuna yaklaştırır.

### Diğer Rotation Control seçenekleri

|Seçenek|Açıklama|
|---|---|
|Rotation Composer|Hedefi kadrajda belirli bir konumda tutar.|
|Hard Look At|Kamerayı doğrudan hedefe yönlendirir.|
|Pan Tilt|Kameranın yatay ve dikey dönüşlerini kontrol eder.|
|Same As Follow Target|Hedefin dönüşünü kameraya uygular.|
|POV|Eski Cinemachine 2.x sürümünde kullanıcı girdisiyle kamera yönünü kontrol eder.|

## 4. Lens – Kamera görüş açısı

Lens ayarları kameranın görüntüyü nasıl oluşturacağını belirler.

![Void Crew](https://images.openai.com/static-rsc-4/OCewmRq2DaMlktapmzORf3oBD_tvbOTizIBqQfKFzF1j2een-oy_mqT8UReDePGoTZvGA1GKtMVxyVnhh_wCYA1bvVfQ4pwO8S66HMjfWmOGkE-93z7xkx-BU6pHDt4yy_xasUa7fSgyJXJvngE0WSapSlILE3h-GuA6zOq_Rx4?purpose=inline)

Geniş FOV

Daha geniş çevre görünür, perspektif etkisi artar.

![Space Mercs (2019) | My Games Backlog](https://images.openai.com/static-rsc-4/QDhof-NRUXtKSdMSdbB2xzRDRGL-5SP-9KB_2CGKvHqXzG1c_Fr0s3solJXn2OozMeZiV5IpivhFtk9kS1udnwGwDuVYsJxZ2ifm4M1Xv2JP7v7C4BxVxnAqO68MH1r3lTadH_z18fLqTmMaBAeGUaA7-b723LEP-8lSxbqV5S8?purpose=inline)

Dar FOV

Daha dar alan görünür, uzaktaki nesneler daha büyük görünür.

|Ayar|Açıklama|
|---|---|
|Field of View|Kameranın dikey görüş açısı.|
|Near Clip Plane|Kameranın görüntülemeye başladığı yakın mesafe.|
|Far Clip Plane|Kameranın görüntüleyebildiği uzak sınır.|
|Dutch|Kameranın kendi ileri ekseni etrafındaki eğimi.|

Örneğin `Field of View = 60`, uzay gemisi takip kamerası için iyi bir başlangıç olabilir.

## 5. Noise – Kamera sarsıntısı

Noise, kameraya titreşim eklemek için kullanılır.

Örneğin uzay gemisi hasar aldığında veya bir patlama gerçekleştiğinde kamera sarsılabilir.

- Amplitude Gain: Sarsıntının şiddeti.
    
- Frequency Gain: Sarsıntının ne kadar hızlı gerçekleştiği.
    
- Noise Profile: Kullanılan sarsıntı deseni.
    

Sürekli titreşim istemiyorsan normal uçuş sırasında Amplitude Gain değerini `0` tutabilirsin.

## 6. Galaxy Strike için önerdiğim başlangıç ayarları

Uzay Gemisi Takip Kamerası

Başlangıç profili

Follow Distance (Z)

-8

Height (Y)

3

Damping

0.5

Field of View

60°

Inspector'a uygulayabileceğin örnek değerler:

Position Control

Follow

Follow Offset

(0, 3, -8)

XYZ Damping

0.5

Rotation Control

Rotation Composer

FOV

60°

Ayarları kopyala

Önemli: Geminin dönüşleriyle kameranın da birlikte dönmesini istiyorsan Follow bileşeninin Binding Mode ayarını kontrol et. Hedefin dönüşünü izleyen bir mod seçmen gerekir. Aksi takdirde kamera geminin arkasında sabit kalmayabilir.

Senin oyununda özellikle bir konu önemli: Uzay gemisi Timeline üzerinde çizdiğin rota boyunca ilerliyorsa, kamera da geminin dönüşlerini mi takip etsin, yoksa gemiyi sürekli aynı açıdan mı göstersin? Bu seçim hangi Cinemachine ayarlarını kullanacağımızı doğrudan değiştirir.