
# Movie Tablosu İçin Kod Üretilmesi

### Serenity Kod Üretici (Code Generator)

Tablomuzun veritabanında bulunduğundan emin olduktan sonra, Serenity Kod Üreticiyi (sergen.exe) kullanarak ilk giriş arayüzünü oluşturacağız.

Visual Studio, *Package Manager Console (Paket Yöneticisi Konsolu)*'nu, *View (Görünüm) * => *Other Windows (Diğer Pencereler)* => *Package Manager Console (Paket Yöneticisi Konsolu)* na tıklayarak açın.

*sergen* yazıp Enter'a basın.

> Bazen, paket yöneticisi ilk açılışta PATH değişkenini düzgün ayarlayamaz (muhtemel NuGet bug ı) ve *sergen* yazıp çalıştırmayı denediğinizde bir hata alabilirsiniz. Visual Studio'yu yeniden başlatmak genellikle sorunu çözer. 

> Bir başka seçenek te Windows Dosya Gezgininden (Windows Explorer) sergen.exe yi manuel açmaktır. *Solution Explorer* da *MovieTutorial* solution'ına sağ tuşla tılayın, *Open In File Explorer (Dosya Gezgininde Aç)*'a tıklayın. Sergen.exe
*packages\Serenity.CodeGenerator.X.Y.Z\tools* klasöründe bulunmaktadır.

![Movies Code Generator](img/movies_code_generator.png)


### Proje Konumlarının Belirlenmesi

Sergen'i ilk çalıştırdığınızda, Web Project (Web Projesi) ve Script Project (Script Projesi) alanları sizin için otomatik olarak doldurulmuş gelir. 

> Eğer Serene 1.6.2'den daha eski bir sürüm kullanıyorsanız, aşağıdaki adımları takip edin:

> "..." butonlarına tıklayarak Solution klasörünüze gidip web ve script projelerini seçin.

> Bir başka opsiyon, aşağıda verilen değerleri belirlemektir (kopyala yapıştır).

> * ..\\..\\..\\MovieTutorial\\MovieTutorial.Web\\MovieTutorial.Web.csproj
> * ..\\..\\..\\MovieTutorial\\MovieTutorial.Script\\MovieTutorial.Script.csproj
 
> Eğer *MovieTutorial* dan başka bir isim kullandıysanız, ör. *MyMovies*, *MovieTutorial* ı onunla değiştirin.

Bu ayarları bir kez belirleyip, ilk sayfanızı ürettikten sonra, tekrar doldurmak zorunda kalmayacaksınız. Bu seçenekler, solution klasörünüzdeki *Serenity.CodeGenerator.config* dosyasında saklanacak.

Proje konumlarının belirlenmesi önemlidir, çünkü Sergen üreteceği dosyaları projelerinize ancak bu şekilde dahil edebilecektir.

### Kök İsim Alanı (Root Namespace) Seçeneği

Bu seçeneği, kullanmış olduğunuz Solution ismine göre belirleyin, ör. *MovieTutorial*. Eğer proje isimleriniz MyProject.Web ve MyProject.Script ise kök isim alanınız MyProject'dir. Bu kritik önem arzeder, o nedenle farklı birşeye ayarlamadığınızdan emin olunuz. Çünkü Serene şablonu üretilen tüm kodun bu kök isim alanı (root namespace) altında olduğunu varsayar.

Bu seçenek te saklandığından, bir sonraki sefer tekrar doldurmanız gerekmez.


### Bağlantı Dizesinin (Connection String) Seçilmesi

Web proje ismi belirlendiğinde, Sergen bağlantı açılır listesini web.config dosyanızdan okuduğu bağlantı dizeleriyle doldurur. Burada, *Default* ve *Northwind* bağlantıları olmalı. *Default* olanı seçelim.


### Kod Üretilecek Tablonun Belirlenmesi

Sergen tek seferde tek tablo için kod üretebilir. Bağlantı dizesini seçtiğimizde,  o veritabanındaki tablo isimleri listeye getirilir.

*mov.Movie* tablosunu seçin.

### Modül Adının Belirlenmesi

Serenity açısından, modül, ortak bir amacı paylaşan sayfalardan oluşan mantıksal bir gruptur. 

Örneğin, Serene şablonunda, *Northwind* örneğiyle ilgili tüm sayfalar *Northwind* modülüne dahildir. 

Sitenin genel yönetimiyle ilgili sayfalar, örneğin kullanıcılar, roller vs. ise *Administration (Sistem Yönetimi)* modülündedir. 

Bir modül, genellikle bir veritabanı şeması ya da ayrı bir veritabanına karşılık gelir. Fakat tek şema/veritabanında birden fazla modül ya da tam tersi, bir modül ile birden fazla veritabanı kullanmanıza bir engel yoktur.

Bu öğretici için, tüm sayfalarda *MovieDB* (IMDB benzeri) modül adı kullanacağız.

Modül adı oluşturulan sayfaların isim alanı (namespace) ve URL lerinin belirlenmesinde kullanılmaktadır.

Örneğin, yeni üreteceğimiz sayfa, *MovieTutorial.MovieDB* isim alanı altında olacak ve *~/MovieDB/Movie* relatif adresinde bulunacak.


### Connection Key (Bağlantı Anahtarı)

Bağlantı anahtarı, daha önce seçilen bağlantının anahtarı olarak belirlenir. Genellikle bu ayarı değiştirmeniz gerekmez. Default olarak bırakabilirsiniz.


### Varlık Tanımlayıcı (Entity Identifier)

Bu genellikle tablo adına karşılık gelir fakat bazen tablo isimlerinde alt çizgi ya da C# için geçersiz karakterler olabilir. Bu nedenle üretilen kodda tablonuzu hangi isimle kullanmak istediğinize karar verebilirsiniz (geçerli bir C# tanımlayıcı olmak şartıyla).

> Serene 1.6.2+ sonrasında entity identifier otomatik olarak tablo isminin Pascal (büyük harf ile başlayan küçük harfle devam eden) isimlendirmesine çevrilmiş bir haline ayarlanır.

Tablo ismimiz *Movie*, yani geçerli ve uygun bir C# tanımlayıcısı. Öyleyse *Movie* olarak kullanabiliriz. Üretilen entity mizin sınıf ismi *MovieRow* olacak.

Bu isim ayrıca diğer sınıf isimlerinin belirlenmesinde de kullanılacak. Örneğin sayfa kontrolcümüz (page controller) *MovieController* olarak isimlendirilecek.

Ayrıca sayfa adresimizde bu isimden belirlenecek. Bu örnekle listeleme/giriş sayfamız be at *~/MovieDB/Movie* adresinde olacak.


### İzin Anahtarı (Permission Key)

Serenity'de kaynaklara erişim (sayfalar, servisler vs.), basit dizeler (string) olan izin anahtarları ile kontrol edilir. Kullanıcı ve rollere bu izinler verilir.

Şu an için Movie sayfamız sadece sistem yöneticileri tarafından kullanılabilecek (daha sonra içerik moderatörlerine çevrilebilir). Bu yüzden izin olarak *Administration:General* belirleyelim. Varsayılan olarak, Serene şablonunda sadece *admin* kullanıcısı bu izne sahiptir.


### İlk Sayfamız İçin Kodun Üretilmesi

Parametreleri resimdeki gibi ayarladıktan sonra *Generate Code for Entity* düğmesine basın. Sergen birkaç dosya (10 civarında) üretip, MovieTutorial.Web and MovieTutorial.Script proejelerine dahil edecektir.

Şimdi Sergen'i kapatıp, Visual Studio'ya geri dönebiliriz.

Projeler değişmiş olduğundan dolayı, Visual Studio tekrar yüklemek isteyip istemediğinizi soracaktır. *Reload All* a tıklayın.

Solution'ı *TEKRAR DERLEYİN (REBUILD)* ve *F5* e basarak uygulamayı başlatın.

> Lütfen *SOLUTION'ı TEKRAR DERLEDİĞİNİZDEN* emin olunuz (solution a sağ tuşla tıklayıp REBUILD i seçerek). Bazı kullanıcılar, kod ürettikten sonra boş bir sayfayla karşılaştıklarını rapor etmektedirler. Bunun nedeni script projesinin düzgün bir şekilde derlenmemiş olmasıdır. Özellikle MovieTutorial.Script projesini tekrar derlemelisiniz. Çıktısı derleme sonucunda MovieTutorial.Web/Scripts/site klasörüne kopyalanmakta ve buradan kullanılmaktadır.

> Bir diğer seçenek, proje bağımlılığı eklemektir. Script projesinin de Web projesi çalıştırıldığında derlenmesi için, MovieTutorial.Web projesine sağ tıklayın, *Build Dependencies (Derleme Bağımlılıkları) -> Project Dependencies (Proje Bağımlılıkları)* nı seçin ve *Dependencies (Bağımlılıklar)* sekmesi altındaki *MovieTutorial.Script* i işaretli hale getirin.

Kullanıcı adı olarak *admin*, şifre olarak *serenity* ile giriş yapın.

Gösterge sayfasıyla karşılaştığınızda, sol taraftaki navigasyonun alt kısmında yeni bir bölüm (*MovieDB*) olduğunu farkedeceksiniz.

Tıklayıp genişletin ve *Movie* bağlantısına basıp ilk ürettiğimiz sayfamızı açın.

![Filmler Sayfası İlk Üretim](img/movies_first_generation.png)

Şimdi bir film ekleyip, sonra güncellemeyi ve silmeyi deneyin.

Sergen'in tablomuz için üretmiş olduğu arayüz, bir satır kod yazmadan çalışır durumda.

> Bu kod yazmaktan hoşlanmadığım şeklinde anlaşılmamalı, tam aksine! Aslında, çoğu tasarım aracı ve kod üreticinin hayranı sayılmam. Genellikle ürettikleri kod, yönetilmesi imkansız bir kargaşadır. 

> Sergen, sadece ilk kurulumu yapmamıza yardımcı oldu. Bu katmanlı mimari ve platform standartları açısından gereklidir. Aksi durumda, bizim entity, repository, sayfa, servis bağlantı noktası, grid, form vs. için 10 civarında dosyayı manuel olarak oluşturmamız gerekecekti. Artı birkaç başka yerlerde de ayarlamalar yapmamız lazımdı.

> Bir başka tablonun kodlarını kopyala yapıştır ile alıp değiştirsek bile bu işlemleri bitirmemiz en azından 5-10 dk alır.

> Sergen'in ürettiği kod dosyaları, olabilecek en temel yapıda ve minimum kod içermektedir. Bu Serenity'deki işin büyük kısmını halleden baz sınıfların sayesindedir. 

> Bir tablo için kod ürettikten sonra, aynı tablo için Sergen'i bir daha kullanmayacağız ve üretilmiş kodu ihtiyacımza göre düzenleyeceğiz (diğer kod üreticilerden farklı olarak).