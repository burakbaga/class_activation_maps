Class Activation Maps
Class activation maps (CAM), CNN modellerinde sınıflandırma yaparken modelin yaptığı tahminde resmin hangi kısımlarının etkili olduğunu bize gösterir. Learning Deep Features for Discriminative Localization makalesinde sınıflandırma için eğitilmiş Localization yapabilmeleri için herhangi bir çalışma ve eğitim yapılmamış sınıflandırma modellerinin resmin hangi kısmının aradığımız sınıfa ait özellik barındırdığını bize gösterebiliyor. Buda bizlere CNN modellerinin içerisinde de bir çeşit attention mekanizması olduğunu gösteriyor. 

Veri setinin indirilmesi ve Çıkartılması
http://www.vision.caltech.edu/Image_Datasets/Caltech101/ linkinden indirdiğimiz datayı drive ortamına taşıyıp buradan !tar komutu ile drivede belirttiğimiz path'e çıkardık. Veri setinde 101 sınıfa ait resimler bulunuyor. Modeli bu resimler üzerinde deneyeceğiz.
Kullanılacak Kütüphanelerin İmport Edilmesi
Imagelerin Pathlerinin Alınması
Driveda klasörler içerisinde bulunan imagelerin pathlerini almak için glob kütüphanesini kullanalım. glob ile dosya yolur ve data formatı vererek o pathteki ilgili formattaki tüm dataları alabiliriz.
Modeller
Resnet Modeli
Activation mapping ve prediction için resnet modeli kullacağız. Resnet modeli milyonlarca veri ile eğitilmiş ve özellik çıkarmada oldukça başarılı bir model.
Feature Map Modeli
Bu model için resnet modelinin global average pooling layerından bir önceki layer olan "conv5_block3_out" (son aktivasyon) layerını get_layer ile alıyoruz.
Keras Functional API ile yeni bir model oluşturalım. Feature maps çıkarmak için bu modeli kullanacağız.
Son katmanın weightlerini de ayrı bir değişkende tutuyoruz. Modelin predict ettiği sınıfa ait weightleri ve çıkartılan özellikleri dot product çarparak modelin yaptığı tahminde imagein hangi kısmında ki özellikler etkili olduğunu öğrenebiliriz.
Activation Maps Oluşturulması ve Gösterilmesi
Random olarak bir image seçelim. Bu seçtiğimiz image'i resnet modeline göndermek için belli processlerden geçirelim.Özellik çıkarımda kullanacağımız model gönderip predict ettirim. (7x7x2048) boyutunda bir çıktı alıyoruz. Bu bizim feature map verimiz.
Resnet modeline imagei gönderip classification yapmasını sağlıyoruz. Hangi sınıfı predict ettiğini np.argmax ile öğreniyoruz.
Bir değişkende tuttuğumuz weight içerisinden ilgili sınıfa giden weightleri çekiyoruz. Bu weightler ve feature map dot product ile hesaplayıp. 7x7 bir matrix elde ediyoruz.
Bu elde ettiğimiz matriksi upsampling ile büyüterek plot ediyoruz.
