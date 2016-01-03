
# Film Arayüzünün Özelleştirilmesi


### Alan Başlıklarının Değiştirilmesi

Film grid'i ve formumuzda, *Runtime (Süre)* isimli bir alanımız bulunuyor. Bu alan, dakika (minutes) cinsinden bir tamsayı girilmesini bekliyor fakat bu bilgi başlığından anlaşılamıyor. Daha açıklayıcı olması açısından *Runtime (mins)* yani olarak değiştirelim.

> Kolon başlığını *Süre (dk)* olarak da değiştirebilirdik, ancak örneklerin ingilizce kılavuz ile senkronize gitmesini istediğimizden, terimleri aynı şekilde kullanıyoruz.

Bunu yapmak için birden fazla yöntem mevcut. Seçeneklerimiz içinde sunucu tarafı form tanımı, sunucu tarafı kolon tanımları, istemci tarafında grid kodundan direk değiştirmek vs. bulunuyor. Ama en iyisi bunu merkezi noktadan, yani entity (row) tanımından yapmak, bu şekilde alan başlığı heryerde değişecektir.

Sergen, Movie tablomuz için kod ürettiğinde, MovieRow isimli bir entity sınıfı da oluşturdu. Dosyayı *MovieTutorial.Web/Modules/MovieDB/Movie/MovieRow.cs* konumunda bulabilirsiniz.

Altta, Runtime özelliği (property) için bu dosyadan bir alıntı bulunmakta:

```cs
namespace MovieTutorial.MovieDB.Entities
{
    // ...
    [ConnectionKey("Default"), DisplayName("Movie"), InstanceName("Movie"), 
     TwoLevelCached]
    public sealed class MovieRow : Row, IIdRow, INameRow
    {
        // ...
        [DisplayName("Runtime")]
        public Int32? Runtime
        {
            get { return Fields.Runtime[this]; }
            set { Fields.Runtime[this] = value; }
        }
        //...
    }
}
```

Entity (ya da row) yapılarına daha sonra değineceğiz. Şu an için hedefimize odaklanalım ve property'nin DisplayName niteliğini *Runtime (mins)" olarak değiştirelim:

```cs
namespace MovieTutorial.MovieDB.Entities
{
    // ...
    [ConnectionKey("Default"), DisplayName("Movie"), InstanceName("Movie"), 
     TwoLevelCached]
    public sealed class MovieRow : Row, IIdRow, INameRow
    {
        // ...
        [DisplayName("Runtime (mins)")]
        public Int32? Runtime
        {
            get { return Fields.Runtime[this]; }
            set { Fields.Runtime[this] = value; }
        }
        //...
    }
}
```

Şimdi solution'ı derleyin ve uygulamayı çalıştırın. Alan başlığının hem grid hem de diyalogta değişmiş olduğunu göreceksiniz.

> Kolon başlığında "..." bulunmasının nedeni, yeterince geniş olmaması, ancak üzerine gelince hint (ipucu) olarak tam başlık ta görüntüleniyor. Bunu nasıl çözeceğimize de az sonra bakacağız.

![Filmler Runtime Kolonu (Dk)](img/movies_runtime_mins.png)


### Kolon Başlık ve Genişliğinin Ezilmesi

Şimdilik herşey iyi gibi, peki ya grid kolonlarında başka formda başka bir başlık göstermek isteyeseydik ne yapacaktık? Elbette karşılık gelen dosyada bu niteliği ezebiliriz.

Öncelikle kolonlarda yapalım. *MovieRow.cs* dosyasının yanında, *MovieColumns.cs* kaynak dosyasını bulabilirsiniz:

```cs
namespace MovieTutorial.MovieDB.Columns
{
    // ...

    [ColumnsScript("MovieDB.Movie")]
    [BasedOnRow(typeof(Entities.MovieRow))]
    public class MovieColumns
    {
        [EditLink, DisplayName("Db.Shared.RecordId"), AlignRight]
        public Int32 MovieId { get; set; }
        //...
        public Int32 Runtime { get; set; }
    }
}
```

Farkettiyseniz, bu kolon tanımlamaları, Movie (film) entity'sini baz alıyor (BasedOnRow niteliğiyle).

Burada yazılabilecek her türlü nitelik, entity sınıfında tanımlanmış nitelikleri ezecektir.

*Runtime* property'sine bir *DisplayName* niteliği ekleyelim:

```cs
namespace MovieTutorial.MovieDB.Columns
{
    // ...

    [ColumnsScript("MovieDB.Movie")]
    [BasedOnRow(typeof(Entities.MovieRow))]
    public class MovieColumns
    {
        [EditLink, DisplayName("Db.Shared.RecordId"), AlignRight]
        public Int32 MovieId { get; set; }
        //...
        [DisplayName("Runtime in Minutes"), Width(150), AlignRight]
        public Int32 Runtime { get; set; }
    }
}
```

Şimdi kolon başlığını, formdan farklı olarak "Runtime in Minutes" olarak değiştirdik. 

Ayrıca, iki nitelik daha ekledik.

Biri, kolon genişliğini 150 piksele çıkarmak için.

> Serenity, siz manuel belirlemezseniz, kolon genişliklerini alan tipleri ve karakter uzunluklarını göz önüne alarak, otomatik olarak tespit eder.

Bir diğeri de kolon metnini sağa yaslamak için (AlignCenter (ortala), AlignLeft (sola yasla) nitelikleri de kullanılabilir).

Tekrar derleyip çalıştıralım, şimdi görünüm şöyle oldu:

![Movies Runtime Column](img/movies_runtime_column.png)

Form başlığı aynı kalırken, kolon başlığı ve görünümü değişti.

> Eğer form başlığını ezmek isteseydik, benzer adımları MovieForm.cs dosyasındaki tanımlamalarda uygulayacaktık.


### Description (Açıklama) ve Storyline (Senaryo) Editör Tiplerininin Değiştirilmesi

Description ve Storyline alanları, Title (başlık) alanına göre biraz daha uzun olabilir. O yüzden editör tiplerini textarea (çok satırlı metin) olarak değiştirelim.

*MovieColumns.cs* ile aynı klasördeki *MovieForm.cs* dosyasını açın:

```cs
namespace MovieTutorial.MovieDB.Forms
{
    //...
    [FormScript("MovieDB.Movie")]
    [BasedOnRow(typeof(Entities.MovieRow))]
    public class MovieForm
    {
        public String Title { get; set; }
        public String Description { get; set; }
        public String Storyline { get; set; }
        public Int32 Year { get; set; }
        public DateTime ReleaseDate { get; set; }
        public Int32 Runtime { get; set; }
    }
}
```

ve iki property'ye de TextAreaEditor niteliğini uygulayın:

```cs
namespace MovieTutorial.MovieDB.Forms
{
    //...
    [FormScript("MovieDB.Movie")]
    [BasedOnRow(typeof(Entities.MovieRow))]
    public class MovieForm
    {
        public String Title { get; set; }
        [TextAreaEditor(Rows = 3)]
        public String Description { get; set; }
        [TextAreaEditor(Rows = 8)]
        public String Storyline { get; set; }
        public Int32 Year { get; set; }
        public DateTime ReleaseDate { get; set; }
        public Int32 Runtime { get; set; }
    }
}
```

Description'a göre (3), Storyline daha uzun olabileceği için biraz daha fazla satır (8) ayarladım.

Derleyip çalıştırdıktan sonra şu görünümdeyiz:

![Filmler TextArea Editörleri](img/movies_textarea_editors.png)

Serene, çeşitli editör tiplerine sahiptir. Bunlardan bazıları alan veri tipine göre otomatik belirlenirken, istediklerinizi siz özel (explicit) olarak seçebilirsiniz.

> Kendi editör tiplerinizi geliştirmeniz de mümkün. Mevcut editörlerden birini baz alabilir, ya da sıfırdan kendi editörlerinizi yazabilirsiniz. Nasıl olacağına ilerleyen bölümlerde değineceğiz.

Editörler biraz yükseklik açısından büyüdüğünden, form yüksekliği varsayılan Serenity form yüksekliğini aştı (Sergen başlangıçta 260px olarak belirler) ve şimdi bir dikey kaydırma çubuğumuz var. Onu ortadan kaldıralım.

### CSS (Less) ile İlk Diyalog Yüksekliğinin Ayarlanması

Sergen, film diyaloğumuz için *MovieTutorial.Web/Content/site/site.less* dosyasında bazı CSS stilleri oluşturdu.

Eğer dosyayı açıp, sonuna giderseniz, şunu göreceksiniz:

```cs
/* ------------------------------------------------------------------------- */
/* APPENDED BY CODE GENERATOR, MOVE TO CORRECT PLACE AND REMOVE THIS COMMENT */
/* ------------------------------------------------------------------------- */

.s-MovieDialog {
    > .size { .widthAndMin(650px); }
    .dialog-styles(@h: auto, @l: 150px, @e: 400px);
    .s-PropertyGrid .categories { height: 260px; }
}
```

Rahatça ilk 3 yorum satırını silebilirsiniz (appended by code generator...). Bu sadece, üretilen bu satırları daha uygun bir yere, örneğin bu modüle özel oluşturacağınız bir   *site.movies.less* taşımanız için bir hatırlatma (stilleri modüllere ayırmanız önerilir).

Buradaki kurallar, *.s-MovieDialog* CSS sınıfına sahip HTML elemanlarına uygulanır. Bizim Movie diyaloğumuz varsayılan olarak bu sınıfa sahiptir.

İkinci satırda, diyaloğun 650px genişliğinde olacağı (aynı zamanda minimum 650px olacağını da belirtiyor, fakat bu, diyaloğu boyutlanabilir yaptığımızda anlam kazanacak).

Üçüncü satırda, diyalog yüksekliğinin otomatik (@h: auto), alan başlıklarının 150px (@l: 150px), ve editörlerin de 400px genişliğinde (@e: 400px) olmasını istediğimizi belirtiyoruz.

Burada bazı less yardımcılarından (mixin) faydalandık (widthAndMin and dialog-styles).

Form yüksekliğimiz *s-PropertyGrid .categories { height: 260px; }* satırı ile kontrol ediliyor. Onu 400px olarak değiştirelim ki dikey kaydırma çubuğu (scroll bar) gerekmesin.

```css
.s-MovieDialog {
    > .size { .widthAndMin(650px); }
    .dialog-styles(@h: auto, @l: 150px, @e: 400px);
    .s-PropertyGrid .categories { height: 400px; }
}
```


### Sayfa Başlığının Değiştirilmesi

Sayfamızın başlığı *Movie* olarak gözüküyor. *Movies* yapalım.

*MovieRow.cs* tekrar açın. 

```cs
namespace MovieTutorial.MovieDB.Entities
{
    // ...
    [ConnectionKey("Default"), DisplayName("Movie"), InstanceName("Movie"), 
     TwoLevelCached]
    public sealed class MovieRow : Row, IIdRow, INameRow
    {
        [DisplayName("Movie Id"), Identity]
        public Int32? MovieId
```

*DisplayName* nitelik değerini *Movies* olarak değiştirin. Bu, tablonun isimlendirmesi olarak kullanılır ve genellikle çoğuldur. Bu metin sayfa başlığını da belirler. 

> *MoviePage.Index.cshtml* dosyasından da sayfa başlığını özel olarak değiştirebilirsiniz, ancak bu bilginin diğer noktalarda da kullanılabilmesi için merkezi yerden düzenlenmesini tercih ediyoruz.

*InstanceName (Örnek İsmi)* ise tekil isme karşılık gelir ve grid'in New Record (Yeni Film) düğmesiyle, diyalog başlığını belirlemek için kullanılır (ör. Edit Movie (Film Düzenle)).

```cs
namespace MovieTutorial.MovieDB.Entities
{
    // ...
    [ConnectionKey("Default"), DisplayName("Movies"), InstanceName("Movie"), 
     TwoLevelCached]
    public sealed class MovieRow : Row, IIdRow, INameRow
    {
        [DisplayName("Movie Id"), Identity]
        public Int32? MovieId
```
