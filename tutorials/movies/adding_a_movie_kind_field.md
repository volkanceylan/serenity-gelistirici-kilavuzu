# Film Tipi Alanının Eklenmesi

Eğer sinema filmleri ile birlikte diziler ve mini dizileri de *Movie* tablomuzda tutmak isteseydik, bu bilgiyi saklamak için başka bir alana ihtiyacımız olurdu *Kind (Tip)*.

*Movie* tablosunu oluştururken bu alanı tanımlamadığımızdan, veritabanına eklemek için başka bir migration yazacağız.

 *MovieTutorial.Web/Modules/Common/Migrations/DefaultDB/ 
 DefaultDB_20150924_142200_MovieKind.cs*: dosyasında yeni bir migration oluşturun:

```cs
using FluentMigrator;
using System;

namespace MovieTutorial.Migrations.DefaultDB
{
    [Migration(20150924142200)]
    public class DefaultDB_20150924_142200_MovieKind : Migration
    {
        public override void Up()
        {
            Alter.Table("Movie").InSchema("mov")
                .AddColumn("Kind").AsInt32().NotNullable()
                    .WithDefaultValue(1);
        }

        public override void Down()
        {
        }
    }
}
```


### MovieKind Enumeration'ının Tanımlanması

*Kind* alanı *Movie* tablosuna ekledikten sonra, film tiplerini içeren bir listeye ihtiyacımız olacak. *MovieTutorial.Web/Modules/MovieDB/Movie/MovieKind.cs* altında bir enum olarak tanımlayalım:

```cs
using Serenity.ComponentModel;
using System.ComponentModel;

namespace MovieTutorial.MovieDB
{
    [EnumKey("MovieDB.MovieKind")]
    public enum MovieKind
    {
        [Description("Film")]
        Film = 1,
        [Description("TV Series")]
        TvSeries = 2,
        [Description("Mini Series")]
        MiniSeries = 3
    }
}
```


### Kind (Tip) Alanının MovieRow Entity'sine Eklenmesi

Artık Sergen'i kullanmadığımıza göre, *Kind* kolonu için *MovieRow.cs* dosyamıza manuel olarak bir eşleme (mapping) eklemeliyiz. *Runtime* property'sinden sonra aşağıdaki property tanımını ekleyin:

```cs
[DisplayName("Runtime (mins)")]
public Int32? Runtime
{
    get { return Fields.Runtime[this]; }
    set { Fields.Runtime[this] = value; }
}

[DisplayName("Kind"), NotNull]
public MovieKind? Kind
{
    get { return (MovieKind?)Fields.Kind[this]; }
    set { Fields.Kind[this] = (Int32?)value; }
}
```

Ek olarak, Serenity varlık (entity) sistemi için gerekli olan bir Int32Field nesnesi de tanımlamalıyız. *MovieRow.cs* dosyasının sonunda *RowFields* sınıfını bulun ve *Runtime* alanından sonra gelecek şekilde *Kind* alanını eklemek üzere düzenleyin:

```cs
public class RowFields : RowFieldsBase
{
    // ...
    public readonly Int32Field Runtime;
    public readonly Int32Field Kind;

    public RowFields()
        : base("[mov].Movie")
    {
        LocalTextPrefix = "MovieDB.Movie";
    }
}
```


### Film Formuna Tip Seçimi Ekleme

Eğer şimdi projemizi derleyip çalıştırırsak, film (Movie) formunda herhangi bir değişiklik olmadığını farkederiz. Oysa, *Kind* alanı için *MovieRow*'a bir eşleme eklemiştik. Bunun nedeni, formda gösterilip düzenlecek alanlarının *MovieForm.cs* dosyasındaki property tanımlarıyla kontrol edilmekte olmasıdır.

> Row'daki bazı alanlar formda gösterilmeyebilir.

*MovieForm.cs*'i aşağıdaki gibi düzenleyin:

```cs

namespace MovieTutorial.MovieDB.Forms
{
    // ...
    [FormScript("MovieDB.Movie")]
    [BasedOnRow(typeof(Entities.MovieRow))]
    public class MovieForm
    {
        // ...
        public Int32 Runtime { get; set; }
        public MovieKind Kind { get; set; }
    }
}
```

Şimdi solution'ı derleyip çalıştırın. Bir film kaydını düzenlemeye ya da yeni bir tane eklemeye çalıştığınızda, hiçbirşey olmayacaktır. Bu beklenen bir durumdur. Eğer tarayıcınızın geliştirici araçları konsolunu kontrol ederseniz (F12, öğeyi incele vs.) şöyle bir hatayla karşılaşırsınız:

```txt
Uncaught Can't find MovieTutorial.MovieDB.MovieKind enum type!
```

Bu hata, MovieKind enumuration tipinin, istemci tarafında mevcut olmamasından kaynaklanır. Uygulamamızı çalıştırmadan önce T4 şablonlarını dönüştürmemiz gerekirdi.

Now in Visual Studio, click *Build -> Transform All Templates* again.

Rebuild your solution and execute it. Now we have a nice dropdown in our form to select movie kind.

![Movie Kind Selection](img/movies_kind_selection.png)


### Declaring a Default Value for Movie Kind

As *Kind* is a required field, we need to fill it in *Add Movie* dialog, otherwise we'll get a validation error.

But most movies we'll store are feature films, so its default should be this value.

To add a default value for *Kind* property, add a *DefaultValue* attribute like this:

```cs
[DisplayName("Kind"), NotNull, DefaultValue(1)]
public MovieKind? Kind
{
    get { return (MovieKind?)Fields.Kind[this]; }
    set { Fields.Kind[this] = (Int32?)value; }
}
```

Now, in *Add Movie* dialog, *Kind* field will come prefilled as *Film*.