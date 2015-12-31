# *Movie (Film)* Tablosunun Oluşturulması

Filmlerin bir listesini saklamak için bir *Movie* tablosuna ihtiyacımız var. Bu tabloyu eski usül teknikleri kullanarak, SQL Management Studio ile manuel oluşturabilirdik. Fakat bizim tercihimiz, *Fluent Migrator* kitaplığından faydalanarak *migration (göç?)* sistemiyle oluşturmak:

> Fluent Migrator, .NET için geliştirilmiş, Ruby on Rails Migrations kitaplığına çok benzeyen bir migration framework'üdür. Migration'lar veritabanı şemasının değiştirilmesi için yapısal bir yoldur ve ilgili her yazılımcı tarafından ayrı ayrı çalıştırılması gereken birçok sql script'i hazırlama yöntemine bir alternatiftir. Migration'lar birden çok veritabanı ortamının evrim geçiren şemalarında karşılaşılan sorunları çözer (örneğin, geliştiricinin yerel veritabanı, test veritabanı ve canlı veritabanı). Veritabanı şema değişiklikleri, C# da yazılan sınıflar olarak tanımlanır ve bir sürüm kontrol sisteminde versiyonlanabilir (check in).

> FluentMigrator ile ilgili daha fazla bilgi için bkz. https://github.com/schambers/fluentmigrator.

*Solution Explorer* ile *MovieTutorial.Web / Modules / Common / Migrations / DefaultDB* klasörünü gidiniz.

> Aşağıdaki görüntü bu öğretici yazılırken alınmıştır. Serene'nin güncel sürümlerinde aynı lokasyonda DefaultDB ve NorthwindDB adında iki klasör bulunmaktadır. Çünkü Northwind veritabanı, Serene'nin kullanıcı, rol gibi bilgileri tuttuğu veritabanından ayrılmıştır ve iki ayrı veritabanı bulunmaktadır. Siz çalışmalarınızı DefaultDB altından yapabilirsiniz.

![Initial Migration Folder](img/movies_migration_initial.png)

Görüntüde üç migration bulunmaktadır. Migration, veritabanı yapısını değiştiren bir DML (veri manupilasyon dili) script i gibidir.

*DefaultDB_20141103_140000_Initial.cs*, bizim *Users (Kullanıcı)* ve *Roles (Roller)* tablolarımızı oluşturan başlangıç migration ıdır.

Aynı klasörde (DefaultDB), adı *DefaultDB_20150915_185137_Movie.cs* olan yeni bir migration dosyası oluşturun. Bunun için mevcut dosyalardan birini kopyalayıp, adını değiştirip, içeriğini güncelleyebilirsiniz.

> Aslında migration dosya adı ve içindeki sınıfın adı sistem açısından önemsizdir, ancak tutarlılık ve düzgün sıralama için bu yapıda (y/a/g/sa/dk/sn) kullanmanız önerilir.

*20150915_185137* bu migration ı yazdığımız zamana *yyyyMMdd_HHmmss* formatında karşılık gelir. Ayrıca bu migration için benzersiz bir anahtar olarak da rol alacaktır.

Hazırlayacağımız migration dosyası şu şekilde bir içeriğe sahip olmalı:

```cs
using FluentMigrator;
using System;

namespace MovieTutorial.Migrations.DefaultDB
{
    [Migration(20150915185137)]
    public class DefaultDB_20150915_185137_Movie : Migration
    {
        public override void Up()
        {
            Create.Schema("mov");

            Create.Table("Movie").InSchema("mov")
                .WithColumn("MovieId").AsInt32().Identity().PrimaryKey().NotNullable()
                .WithColumn("Title").AsString(200).NotNullable()
                .WithColumn("Description").AsString(1000).Nullable()
                .WithColumn("Storyline").AsString(Int32.MaxValue).Nullable()
                .WithColumn("Year").AsInt32().Nullable()
                .WithColumn("ReleaseDate").AsDateTime().Nullable()
                .WithColumn("Runtime").AsInt32().Nullable();    
        }

        public override void Down()
        {
        }
    }
}
```

> Sınıfın isim alanının (namespace) *MovieTutorial.Migrations.DefaultDB* olduğundan emin olun, çünkü Serene sadece bu namespace deki migration ları *Default (varsayılan)* veritabanına uygular.

In *Up()* method we specify that this migration, when applied, will create a schema named *mov*. We will use a separate schema for movie tables to avoid clashes with existing tables. It will also create a table named *Movie* with "MovieId, Title, Description..." fields on it.

We could implement *Down()* method to make it possible to undo this migration (drop movie table and mov schema etc), but for the scope of this sample, lets leave it empty.

> Inability to undo a migration might not hurt much, but deleting a table by mistake could do more damage.

On top of our class we applied a Migration attribute.

```cs
[Migration(20150915185137)]
```

This specifies a unique key for this migration. After a migration is applied to a database, its key is recorded in a special table specific to FluentMigrator ([dbo].[VersionInfo]), so same migration won't be applied again.

> Migration key should be in sync with class name (for consistency) but without underscore as migration keys are Int64 numbers.

> Migrations are executed in the key order, so using a sortable datetime pattern like yyyyMMdd for migration keys looks like a good idea.


### Running Migrations

By default, Serene template runs all migrations in *MovieTutorial.Migrations.DefaultDB* namespace. This happens on application start automatically. The code that runs migrations are in *App_Start/SiteInitialization.cs* file:

```cs

public static partial class SiteInitialization
{
    public static void ApplicationStart()
    {
        // ...
        EnsureDatabase();
    }

    private static void EnsureDatabase()
    {
        // ...
        RunMigrations();
    }

    private static void RunMigrations()
    {
        var defaultConnection = SqlConnections.GetConnectionString("Default");

        // safety check to ensure that we are not modifying another database
        if (defaultConnection.ConnectionString.IndexOf(
            typeof(SiteInitialization).Namespace + @"_Default_v1") < 0)
            return;

        using (var sw = new StringWriter())
        {
            //...
            var runner = new RunnerContext(announcer)
            {
                // ...
                Namespace = "MovieTutorial.Migrations.DefaultDB"
            };

            new TaskExecutor(runner).Execute();
        }
    }

```

> There is a safety check on database name to avoid running migrations on some arbitrary database other than the default Serene database (MovieTutorial_Default_v1). You can remove this check if you understand the risks. For example, if you change default connection in web.config to your own production database, migrations will run on it and you will have Northwind etc tables even if you didn't mean to.

Now press F5 to run your application and create Movie table in default database.


### Verifying That the Migration is Run

Using Sql Server Management Studio or Visual Studio -> Connection To Database, open a connection to MovieTutorial_Default_v1 database in server *(localdb)\v11.0*.

> (localdb)\v11.0 is a LocalDB instance created by SQL Server 2012 LocalDB. 

> If you didn't install LocalDB yet, download it from https://www.microsoft.com/en-us/download/details.aspx?id=29062.

> If you have SQL Server 2014 LocalDB, your server name would be (localdb)\v12.0, so change connection string in web.config file. 

> You could also use another SQL server instance, just change the connection string to target server and remove the migration safety check.

You should see *[mov].[Movies]* table in SQL object explorer.

Also when you view data in *[dbo].[VersionInfo]* table, Version column in the last row of the table should be *20150915185137*. This specifies that the migration with that version number (migration key) is already executed on this database.
