
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

Tablo ismimiz *Movie*, yani geçerli ve uygun bir C# tanımlayıcısı. Öyleyse *Movie* olarak kullanabiliriz.

Bu isim ayrıca diğer sınıf isimlerinin belirlenmesinde de kullanılacak. Örneğin sayfa kontrolcümüz (page controller) *MovieController* olarak isimlendirilecek.

Ayrıca sayfa adresimizde bu isimden belirlenecek. Bu örnekle listeleme/giriş sayfamız  be at */MovieDB/Movie* adresinde olacak.


### Permission Key 

In Serenity, access control to resources (pages, services etc.) are controlled by permission keys which are simple strings. Users or roles are granted these permissions.

Our Movie page will be only used by administrative users (or maybe later content moderators) so let's set it to *Administration* for now. By default, in Serene template, only the *admin* user has this permission.


### Generating Code for First Page

After setting parameters as shown in the image above, click *Generate Code for Entity* button. Sergen will generate several files and include them in MovieTutorial.Web and MovieTutorial.Script projects.

Now you can close Sergen, and return to Visual Studio.

As projects are modified, Visual Studio will ask if you wan't to reload changes, click Reload All.

*REBUILD the Solution* and then press *F5* to launch application.

> Please make sure you REBUILD SOLUTION, by right clicking solution name and clicking rebuild. Some users report that they got an empty page after generating code. It's due to script project is not built properly. You should specially rebuild MovieTutorial.Script project. It's output is placed under MovieTutorial.Web/Scripts/site folder on rebuild.

> Another option is to add a project dependency. To make Script project also build when Web project is run, right click MovieTutorial.Web project, click *Build Dependencies -> Project Dependencies* and check *MovieTutorial.Script* under *Dependencies* tab.

Use *admin* as username, and *serenity* as password to login.

When you are greeted with Dashboard page, you will notice that there is a new section, *MovieDB* on the bottom of left navigation. 

Click to expand it and click Movie to open our first generated page.

![Movies First Generation](img/movies_first_generation.png)

Now try adding a new movie, than try updating and deleting it.

Sergen generated code for our table, and it just works without writing a single line of code.

> This doesn't mean i don't like writing code. In contrast, i love it. Actually i'm not a fan of most designers and code generators. The code they produce is usually unmanagable mess. 

> Sergen just helped us here for initial setup which is required for layered architecture and platform standards. We would have to create about 10 files for entity, repository, page, endpoint, grid, form etc. Also we needed to do some setup in a few other places.

> Even if we did copy paste and replace code from some other page, it would be error prone and take about 5-10 mins.

> The code files Sergen generates has minimum code with the absolute basics. This is thanks to the base classes in Serenity that handles the most logic. Once we generate code for some table, we'll probably never use Sergen again (for this table), and modify this generated code to our needs. We'll see how.