# Temalar

Serene başlangıçta koyu/mavi bir tema ile görüntülenir. Ekranın sağ üstündeki, kullanıcı adının yanındaki ayarlar simgesine tıklayın ve gelen menüden farklı bir tema seçin.

![Serene Dashboard Yellow Light](img/serene_dashboard_light.png)

Bu özellik BODY tag'i üzerindeki bir CSS sınıfı değiştirilerek sağlanmaktadır.

Eğer kaynak kodu incelerseniz, `<body>` tag'i üzerinde aşağıdakine benzer bir skin sınıfını görebilirsiniz:

```html
<body id="s-DashboardPage" class="fixed sidebar-mini hold-transition skin-blue has-layout-event">
```

Açık sarı temayı seçtiğinizde, bu tag şu şekilde değişir:

```html
<body id="s-DashboardPage" class="fixed sidebar-mini hold-transition skin-yellow-light has-layout-event">
```

Bu hafızada yapıldığından, sayfanın yeniden yüklenmesine gerek yoktur.

Bunun yanında, *"ThemePreference"*" adlı *"yellow-light"* içeriğine sahip bir çerez tarayıcınıza eklenir. Yani, Serene'yi tekrar açtığınızda, tema tercihiniz hatırlanacaktır ve yine açık sarı temayla başlanacaktır.

Bu tema dosyaları Serene.Web projesinin "Content/adminlte/skins/" klasörü altında bulunur. Eğer oraya bakarsanız, şunlara benzer dosya adlarını görebilirsiniz:

```
_all-skins.less
skin.black-light.less
site.blue.less
site.yellow-light.less
site.yellow.less
```

CSS üretimi için LESS kütüphanesinden faydalanıyoruz, bu nedenle CSS yerine LESS dosyalarını düzenlemelisiniz. Projenizi derlediğinizde, LESS dosyaları CSS dosyalarına çevrilir (*Node* tabanlı *Less.js* derleyicisi ile).

Bu işlem, Serene.Web.csproj dosyasında bir derleme adımı olarak tanımlanmıştır:

```xml
...
<Target Name="CompileSiteLess" AfterTargets="AfterBuild">
    <Exec Command="&quot;$(ProjectDir)tools\node\lessc.cmd&quot;
        &quot;$(ProjectDir)Content\site\site.less&quot; &gt;
        &quot;$(ProjectDir)Content\site\site.css&quot;">
    </Exec>
</Target>
...
```

Burada, *site.less* dosyası, aynı klasördeki karşılık gelen *site.css* dosyasına derlenmektedir.

>  http://lesscss.org/ adresinden LESS derleyicisi ve söz dizilimi (syntax) hakkında daha fazla bilgi edinebilirsiniz.
