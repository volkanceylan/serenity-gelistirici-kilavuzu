
# Öğretici: Film Veritabanı

IMDB benzeri bir site için düzenleme arayüzünü Serenity ile geliştireceğiz.

> Bu öğretici için kaynak kodu şurada bulabilirsiniz: 

> https://github.com/volkanceylan/Serenity-Tutorials/tree/master/MovieTutorial



### *MovieTutorial* İsim Yeni Bir Proje Oluşturma

Visual Studio'da File -> New Project (Dosya -> Yeni Proje) ye tıklayınız. *Serene* şablonunun seçili olduğundan emin olun. Proje ismi olarak *MovieTutorial* yazın ve *OK* e basın.

Solution (çözüm) oluşturulduktan sonra *MovieTutorial.Web* ve *MovieTutorial.Script* isimli iki ayrı proje görüyor olmalısınız.

> *MovieTutorial.Web*'in başlangıç projesi olduğundan emin olunuz (koyu fontlu olmalı). Eğer değilse, proje adına sağ tuşla tıklayın ve *Set As Startup Project (Başlangıç Projesi Olarak Belirle)* yi seçin.



### Bu Proje Dosyaları Nedir?

Serenity uygulamaları genellikle en az iki projeden oluşur. Bir tanesi sunucu tarafı kodlar ile css dosyaları, resimler gibi statik kaynakları içerir (MovieTutorial.Web) ve diğeri de istemci tarafı script kodlarını içerir (MovieTutorial.Script).

MovieTutorial.Script sıradan bir C# sınıf kitaplığı gibi gözükse de içerdiği kod aslında Saltaralle ile Javascript koduna derlenir.

> Script projesinin çıktısı (MovieTutorial.Script.js) derlendiğinde *MovieTutorial.Web* projesinin altındaki *Scripts/site/* klasörüne kopyalanır. Yani aslında çalışma zamanında, sadece MovieTutorial.Web projesi kullanılır. Canlıya almanız gereken proje de Web projesidir.

### Proje Bağımlılığı Eklemek

Visual Studio, F5 e basıp uygulamnızı çalıştırdığınızda, varsayılan olarak sadece başlangıç projesi olan MovieTutorial.Web projesini derler. Script projesini siz özellikle istemedikçe derlemez.

> Bu durum *Visual Studio -> Options -> Projects and Solutions -> Build And Run -> "Only build startup projects and dependencies on Run (Visual Studio Seçenekleri -> Proje ve Çözümler -> Derle ve Çalıştır -> Çalıştırınca sadece başlangıç projesi ve bağımlılıklarını derle)* seçeneği ile kontrol edilmektedir. Ancak değiştirmeniz önerilmez.

Script projesinin de, Web projesi çalıştırıldığında derlenmesini sağlamak için, MovieTutorial.Web projesine sağ tıklayın, *Build Dependencies -> Project Dependencies (Derleme Bağımlılıkları -> Proje Bağımlılıkları)* seçeneğine tıklayın ve *Dependencies (Bağımlılıklar)* altındaki *MovieTutorial.Script* i işaretli hale getirin.

> Malesef, bu ayarı Serene şablonundan bizim yapabilmemiz için bir yol bulunmamakta. Bu ayarı yapmayı atladığınız taktirde, ileride script projesi otomatik derlenmediği için bazı tarayıcı hatalarıyla karşılaşabilirsiniz.
