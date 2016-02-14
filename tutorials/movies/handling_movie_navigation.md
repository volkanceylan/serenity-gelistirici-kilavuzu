# Film Navigasyonunun Ele Alınması

### Navigasyon Elemanının Yerelleştirilmesi

Sergen, *Movie* tablosu için kod ürettiğinde, ayrıca navigasyon için de bir eleman girdisi oluşturdu. Serene'de navigasyon elemanları özel assembly nitelikleri (attribute) ile tanımlanır.

Aynı klasördeki *MoviePage.cs* dosyasını açınca üst bölümünde aşağıdaki satırı bulacaksınız:

```cs
[assembly:Serenity.Navigation.NavigationLink(int.MaxValue, "MovieDB/Movie", 
    typeof(MovieTutorial.MovieDB.Pages.MovieController))]

namespace MovieTutorial.MovieDB.Pages
{
    using Serenity;
    using Serenity.Web;

```

Bu niteliğin ilk argümanı gösterileceği sırayı belirler. *Movie* kategorisinde tek bir elemanımız olduğu için şimdilik sıralamayla uğraşmamıza gerek yok.

İkinci argüman, "Bölüm Başlığı/Bağlantı Başlığı" formatında navigasyon başlığıdır. Bölüm ve navigasyon elemanları düz bölü (/ - slash) ile birbirinden ayrılır.

Başlığı *Movie Database/Movies* olarak değiştirelim.

```cs
[assembly:Serenity.Navigation.NavigationLink(int.MaxValue, "Movie Database/Movies", 
    typeof(MovieTutorial.MovieDB.Pages.MovieController), icon: "icon-camrecorder")]

namespace MovieTutorial.MovieDB.Pages
{

//..
```

![Navigation Elemanı Başlık ve İkonu](img/movies_navigation_links.png)

Ayrıca navigasyon elemanının ikonunu da *icon-camcorder* olarak değiştirdik. Serene şablonunda iki ikon fontu bulunur, *Simple Line Icons* ve *Font Awesome*. Burada basit çizgi ikonları setinden bir kod kullandık. 

To see list of simple line icons and their css classes, visit link below:

http://thesabbir.github.io/simple-line-icons/

FontAwesome is available here:

https://fortawesome.github.io/Font-Awesome/icons/


### Ordering Navigation Sections

As our *Movie Database* section is auto generated last, it is displayed at the bottom of navigation menu.

We'll move it before Northwind menu.

As we saw recently, Sergen created a navigation item in *MoviePage.cs*. If navigation items are scattered through pages like this, it would be hard to see the big picture (list of all navigation items) and order them easily.

So we move it to our central location which is at *MovieTutorial.Web/Modules/Common/Navigation/NavigationItems.cs*.

Just cut the below lines from *MoviePage.cs*:

```cs
[assembly:Serenity.Navigation.NavigationLink(int.MaxValue, "Movie Database/Movies", 
    typeof(MovieTutorial.MovieDB.Pages.MovieController), icon: "icon-camrecorder")]
```

Move it into *NavigationItems.cs* and modify it like this:

```
using Serenity.Navigation;
using Northwind = MovieTutorial.Northwind.Pages;
using Administration = MovieTutorial.Administration.Pages;
using MovieDB = MovieTutorial.MovieDB.Pages;

[assembly: NavigationLink(1000, "Dashboard", url: "~/", permission: "",
    icon: "icon-speedometer")]

[assembly: NavigationMenu(2000, "Movie Database", icon: "icon-film")]
[assembly: NavigationLink(2100, "Movie Database/Movies", 
    typeof(MovieDB.MovieController), icon: "icon-camcorder")]

[assembly: NavigationMenu(8000, "Northwind", icon: "icon-anchor")]
[assembly: NavigationLink(8200, "Northwind/Customers", 
    typeof(Northwind.CustomerController), icon: "icon-wallet")]
[assembly: NavigationLink(8300, "Northwind/Products", 
    typeof(Northwind.ProductController), icon: "icon-present")]
// ...
```

Here we also declared a navigation menu (Movie Database) with *film* icon. When you don't have an explicitly defined navigation menu, Serenity implicitly creates one, but in this case you can't order menu yourself, or set menu icon.

We assigned it a display order of *2000* so this menu will display just after Dashboard link (1000) but before Northwind menu (8000).

We assigned our *Movies* link a display order value of *2100* but it doesn't matter right now, as we have only one navigation item under *Movie Database* menu yet.

> First level links and navigation menus are sorted according to their display order first, then second level links among their siblings.

Here is how it looks like after these changes:

![Movie Database Moved](img/movies_navigation_moved.png)


### Troubleshooting Some Issues with Visual Studio

In case you didn't notice already, Visual Studio doesn't let you modify code while your site is running. Also your site stops when you stop debugging, so you can't keep browser window open and refresh after rebuilding.

To solve this issue, we need to disable *Edit And Continue* (have no idea why).

Right Click *MovieTutorial.Web* project, click *Properties*, in the Web tab, uncheck *Enable Edit And Continue* under *Debuggers*.

Also, on your site, top blue progress bar (which is a Pace.js animation), keeps running all the time like it is still loading something. It is thanks to the *Browser Link* feature of Visual Studio. To disable it, locate its button in Visual Studio toolbar that looks like a refresh button (next to play icon with browser name like Chrome), click dropdown and uncheck *Enable Browser Link*.

It's also possible to disable it with a web.config setting 

```xml
<appsettings>
    <add key="vs:EnableBrowserLink" value="false" />
</appsettings>
```

> Serene 1.5.4 and later will have this by default, so you might not experience this issue