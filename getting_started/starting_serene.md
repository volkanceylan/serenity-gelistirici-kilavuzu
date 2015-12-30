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

The sample application includes old and famous Northwind data along with services and user interface to edit it, which is mostly produced by Serenity Code Generator.

### Adding Project Dependency

The output of *Serene1.Script* project (Serene1.Script.Site.js) is copied under *Serene1.Web/scripts/site* on build. 

By default, Visual Studio only builds Serene1.Web project when you press F5 to run. 

> This is controlled by a setting under Visual Studio Options -> Projects and Solutions -> Build And Run -> "Only build startup projects and dependencies on Run". It is not recommended to change it.

To make Script project also build when Web project is run, right click Serene1.Web project, click *Build Dependencies -> Project Dependencies* and check *Serene1.Script* under *Dependencies* tab.

> Unfortunately, there is no way, we can set this dependency in Serene template.


### Troubleshooting Connection Problems

If you are getting a connection error like the following while starting Serene for first time:

> A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: SQL Network Interfaces, error: 50 - Local Database Runtime error occurred. The specified LocalDB instance does not exist.
)



This error might mean that you don't have SQL Server Local DB 2012 installed. This server comes preinstalled with Visual Studio 2012 and 2013. 

In Serene.Web/web.config file there are *Default* and *Northwind* connection entries:

```xml
<connectionStrings>
    <add name="Default" connectionString="Data Source=(LocalDb)\v11.0; 
        Initial Catalog=Serene_Default_v1; Integrated Security=True" 
        providerName="System.Data.SqlClient" />
  </connectionStrings>
```

`(localdb)\v11.0` corresponds to default SQL Server 2012 LocalDB instance.

If you don't have SQL LocalDB 2012, you can install it from:

http://www.microsoft.com/en-us/download/details.aspx?id=29062

Visual Studio 2015 comes with SQL Server 2014 LocalDB. It's default instance name is renamed to MsSqlLocalDB by default. Thus, if you have VS2015, try changing connection strings from `(localdb)\v11.0` to `(localdb)\MsSqlLocalDB`.

```xml
<connectionStrings>
    <add name="Default" connectionString="Data Source=(LocalDb)\MsSqlLocalDB; 
        Initial Catalog=Serene_Default_v1; Integrated Security=True" 
        providerName="System.Data.SqlClient" />
  </connectionStrings>
```

If you still have an error, open an administrative command prompt and type

```bat
> sqllocaldb info
```

This will list localdb instances like:

```
MSSqlLocalDB
test
```

If you don't have MsSqlLocalDB listed, you can create it:

```bat
> sqllocaldb create MsSqlLocalDB
```


If you have another SQL server instance, for example SQL Express, change data source to `.\SqlExpress`:


```xml
<connectionStrings>
    <add name="Default" connectionString="Data Source=.\SqlExpress; 
        Initial Catalog=Serene_Default_v1; Integrated Security=True" 
        providerName="System.Data.SqlClient" />
  </connectionStrings>
```


You can also use another SQL server. Just change the connection string.

> Perform these steps for both Default and Northwind databases. Serene 1.6.4.3+ creates two databases.
