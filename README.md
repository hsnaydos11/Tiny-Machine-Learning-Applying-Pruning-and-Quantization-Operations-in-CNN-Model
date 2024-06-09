# Tiny Machine Learning: Applying Pruning and Quantization Operations in CNN Model

## Proje Genel Bakış

Bu proje, bir kamera kullanarak meyveleri (elma, portakal, mandalina ve muz) gerçek zamanlı olarak algılayıp sınıflandıran bir Konvolüsyonel Sinir Ağı (CNN) modeli geliştirmeyi ve optimize etmeyi içerir. Projenin ana amacı, farklı meyve türlerini doğru ve hızlı bir şekilde tanımlayıp saymaktır. Bunu başarmak için, model boyutunu küçültmek ve sınırlı hesaplama kaynaklarına sahip cihazlarda dağıtımı kolaylaştırmak amacıyla budama ve kantifikasyon teknikleri uygulanır.

## İçindekiler

- [Giriş](#giriş)
- [Yöntemler](#yöntemler)
- [Model Tasarımı](#model-tasarımı)
- [Temel Model Sonuçları](#temel-model-sonuçları)
- [Budama](#budama)
- [Budama Sonuçları](#budama-sonuçları)
- [Kantifikasyon](#kantifikasyon)
- [Sonuçlar](#sonuçlar)
- [Gereksinimler](#gereksinimler)
- [Kullanım](#kullanım)

## Giriş

Bu projenin amacı, kullanıcılara kamera tarafından gösterilen çeşitli meyveleri tanıyıp sınıflandırabilen bir derin öğrenme modeli geliştirmektir. Model, öğretildiği meyve türlerini hızlı ve doğru bir şekilde tanımlayabilmelidir. Başlangıç modelinin geliştirilmesinden sonra, budama ve kantifikasyon teknikleri uygulanarak model boyutu küçültülür ve küçük, kaynak kısıtlı cihazlarda dağıtım için uygun hale getirilir.

## Yöntemler

1. **Veri Toplama**: Farklı arka plan ve ışık koşullarında çeşitli meyve görüntülerinden oluşan yeterli bir veri seti toplandı.
2. **Ön İşleme**: Görüntüler yeniden boyutlandırıldı ve normalize edildi. Etiketler şu şekilde belirlendi:
   - "elma": 0
   - "portakal": 1
   - "mandalina": 2
   - "muz": 3
3. **Model Eğitimi**: Veri seti eğitim ve test setlerine bölündü ve bu veriler üzerinde bir CNN modeli eğitildi.

## Model Tasarımı

Bu projede kullanılan CNN modeli şu katmanlardan oluşur:
- 3x3 filtrelere ve ReLU aktivasyonuna sahip konvolüsyon katmanları
- Batch normalizasyon katmanları
- Max pooling katmanları
- Flatten katmanı
- Dropout ile düzenlenen yoğun katmanlar

Model, Adam optimizer ve sparse categorical cross-entropy loss kullanılarak eğitildi. Eğitim süresi ve bellek kullanımı takip edildi.

## Temel Model Sonuçları

Başlangıç CNN modeli 16 batch boyutuyla 10 epoch boyunca eğitildi. Her epoch için kayıp ve doğruluk sonuçları kaydedildi.

## Budama

Budama, modelin ağırlıklarının bir kısmını sıfıra ayarlayarak modelin boyutunu ve karmaşıklığını azaltmayı içerir. Budama işlemi, TensorFlow Model Optimization Toolkit (TFMOT) kullanılarak uygulandı. Model budamadan sonra yeniden eğitildi.

### Budama Sonuçları

Budamadan sonra, modelin parametreleri geçici olarak arttı çünkü budama kaplamaları tarafından eklenen meta veriler vardı. Bu artış, eğitimden sonra yönetilir ve kaldırılır, bu da önemli ölçüde daha küçük ve verimli bir modelle sonuçlanır.

## Kantifikasyon

Kantifikasyon, model boyutunu daha da azaltarak modeli TensorFlow Lite formatına dönüştürür, böylece mobil ve gömülü cihazlar için uygun hale getirir.

### Kantifikasyon Sonuçları

Kantifikasyon sonrası modelin doğruluğu değerlendirildi. Nihai doğruluk ve boyut, başlangıç ve budama sonrası modellerle karşılaştırıldı.

## Sonuçlar

- Başlangıç model boyutu 183.07 MB ve doğruluğu %95 idi.
- Budama, model boyutunu önemli ölçüde azaltırken %88.98 doğruluk sağladı.
- Kantifikasyon, model boyutunu 15.26 MB'a düşürdü, ancak doğruluk %67.51'a düştü.

## Gereksinimler

- TensorFlow
- TensorFlow Model Optimization Toolkit (TFMOT)
- Python 3.x
- NumPy

## Kullanım

1. **Modeli Eğitme**: Sağlanan veri seti ile CNN modelini eğitin.
   ```python
   python train_model.py
