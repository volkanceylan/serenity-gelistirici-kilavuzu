# Serene'nin Başlatılması

Serene şablonu kullanan projeniz Visual Studio'da oluşturulduktan sonra şuna benzer bir solution görürsünüz:

![Başlangıç Çözüm İçeriği](img/initial_solution_content.jpg)

Bu solution iki proje içerir. *Serene1.Script* ve *Serene1.Web* projeleri (ki bu Serenity uygulamaları için genellikle böyledir).

*Serene1.Web* uygulamanızın sunucu tarafı kodlarını içerirken. *Serene1.Script* istemci (client) tarafında kullanılan script kodlarını içerir.

Script projesi sıradan bir .NET kitaplığı gibi gözükür ama aslında Javascript kodu üretir. Bunun için *Saltarelle* C# => Javascript dönüştürücüsü (https://github.com/erik-kallen/SaltarelleCompiler). kullanılmaktadır.

Web projesi normal bir ASP.NET MVC uygulamasıdır.

Her iki proje de ilgili Serenity NuGet paketlerine referans içermektedir. Yani gerektiğinde paket yöneticisi konsolunu (package manager console) kullanarak güncelleyebilirsiniz.

Serene ilk açılışta ihtiyaç duyduğu veritabanlarını SQL Local DB'de (Visual Studio ile gelen entegre bir SQL sunucu) oluşturur. Yani F5'e basmanız uygulamanın çalışması için yeterlidir, herhangi bir ayar yapmanız gerekmez.

Uygulama başladığında, `admin` kullanıcı adı ve `serenity` şifresini kullanarak giriş yapabilirsiniz. Daha sonra, *Administration / User Management (Yönetim / Kullanıcı Yönetimi)* bölümünden bu şifreyi değiştirebilir ya da farklı kullanıcılar oluşturabilirsiniz.

![Giriş Ekranı](img/login_screen.jpg)

Örnek uygulama, Microsoft'tan eski dostumuz Northwind veritabanını ve bu veriler için Serenity Code Generator ile üretilmiş servis ve kullanıcı arayüzlerini içerir.

### Proje Bağımlılığı (Project Dependency) Tanımlanması

*Serene1.Script* projesinin derlenmesi sonucu oluşan (Serene1.Script.Site.js) dosyası, derleme sonucunda *Serene1.Web/scripts/site* altına kopyalanmaktadır.

Ancak Visual Studio, varsayılan olarak, sadece başlangıç projesi olan *Serene1.Web* i derler.

> Bu durum *Visual Studio Options -> Projects and Solutions -> Build And Run -> "Only build startup projects and dependencies on Run"* seçeneği tarafından kontrol edilmektedir. Değiştirmeniz tavsiye edilmez.

Script projesinin de, Web projesi çalıştırıldığında derlenmesi için, *Serene1.Web* projesine sağ tuşla tıklayın, *Build Dependencies -> Project Dependencies* menüsüne girip, *Dependencies* altındaki *Serene1.Script* i işaretleyin.

Bu ayarlamayı yapmadığınız taktirde, ileride Sergen ile kod üretip, script projesini derlemeden F5 e basarsanız, tarayıcınızda bazı script hatalarıyla karşılaşabilirsiniz.

> Malesef Serene şablonunda bu bağımlılığı bizim ayarlayabilmemiz şu an için mümkün değil.

### Bağlantı Sorunlarını Gidermek

Eğer Serene'yi ilk çalıştırdığınızda şuna benzer bir hata alıyorsanız:

> A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 50 - Local Database Runtime error occurred. The specified LocalDB instance does not exist.
)

Bu hata sisteminizde SQL Server Local DB 2012 nin kurulu olmadığını anlamına gelebilir. Bu sunucu Visual Studio 2012 and 2013 ile birlikte önyüklü olarak gelir. 

Serene.Web/web.config dosyasında *Default* ve *Northwind* bağlantıları bulunmaktadır:

```xml
<connectionStrings>
    <add name="Default" connectionString="Data Source=(LocalDb)\v11.0; 
        Initial Catalog=Serene_Default_v1; Integrated Security=True" 
        providerName="System.Data.SqlClient" />
  </connectionStrings>
```

`(localdb)\v11.0` SQL Server 2012 LocalDB'nin varsayılan örneğidir. (instance).

Eğer SQL LocalDB 2012 ye sahip değilseniz, şu adresten kurabilirsiniz:

http://www.microsoft.com/en-us/download/details.aspx?id=29062

Visual Studio 2015, SQL Server 2014 LocalDB ile birlikte gelmektedir. Bu sürümün varsayılan örneği (instance) ise MsSqlLocalDB olarak değiştirilmiştir. Bu nedenle, eğer VS2015'iniz varsa, sunucu adresini `(localdb)\v11.0` den `(localdb)\MsSqlLocalDB` ye değiştirmeyi deneyin:

```xml
<connectionStrings>
    <add name="Default" connectionString="Data Source=(LocalDb)\MsSqlLocalDB; 
        Initial Catalog=Serene_Default_v1; Integrated Security=True" 
        providerName="System.Data.SqlClient" />
  </connectionStrings>
```

Eğer hala hata alıyorsanız, yönetici olarak bir komut satırı açın ve aşağıdaki komutu yazın:

```bat
> sqllocaldb info
```

Bu mevcut local db örneklerinin bir listesini görüntüleyecektir:

```
MSSqlLocalDB
test
```

Eğer MsSqlLocalDB listelenmezse, şu şekilde oluşturabilirsiniz:

```bat
> sqllocaldb create MsSqlLocalDB
```


Eğer başka bir SQL server tipinde sunucunuz varsa, mesela SQL Express, veri kaynağını `.\SqlExpress` olarak değiştirin:


```xml
<connectionStrings>
    <add name="Default" connectionString="Data Source=.\SqlExpress; 
        Initial Catalog=Serene_Default_v1; Integrated Security=True" 
        providerName="System.Data.SqlClient" />
  </connectionStrings>
```

Başka bir sunucudaki SQL server'ı da kullanabilirsiniz. Sadece bağlantı dizesini uygun şekilde düzenleyin.

> Bu adımları hem Default, hem de Northwind bağlantıları için tekrarlayın. Serene 1.6.4.3+ sürümleri iki ayrı veritabanı oluşturur.
