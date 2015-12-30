# Giriş


## Serenity Platformu Nedir?

Serenity, açık kaynak teknolojiler üzerine inşaa edilmiş bir ASP.NET MVC / Javascript uygulama platformudur.

Spagetti kodlardan uzak durma, tekrarlı işlerde harcanan süreyi azaltma ve en iyi yazılım tasarım desenlerini takip ederek uygulama geliştirmeyi kolaylaştırmayı hedefler.


## Bu Platform Kim ve Ne İçin?

Serenity, birçok veri giriş ekranından oluşan iş uygulamaları ya da dışarı açık web sitelerinin yönetim arayüzleri için biçilmiş kaftandır.

Tabi ki, bu başka uygulama tipleri için de kullanılamayacağı anlamına gelmez. Script versiyonlama, paketleme, önbellekleme, kod üretimi, akıcı sql oluşturucu gibi birçok özelliği her türlü uygulama için faydalı olabilir.


## Serenity ile İlgili Bilgi Kaynakları

Bu kılavuzu ve öğreticilerini okuduktan sonra daha fazla bilgi için aşağıdaki kaynaklardan faydalanabilirsiniz.

<dl>

  <dt>Github Kod Deposu:</dt>
  <dd><a href='https://github.com/volkanceylan/Serenity'>https://github.com/volkanceylan/Serenity</a></dd>

  <dt>Problemleriniz, Öneri ve Sorularınız İçin</dt>
  <dd><a href='https://github.com/volkanceylan/Serenity/issues'>https://github.com/volkanceylan/Serenity/issues</a></dd>
  
  <dt>Değişiklik Kütüğü:</dt>
  <dd><a href='https://github.com/volkanceylan/Serenity/blob/master/CHANGELOG.md'>https://github.com/volkanceylan/Serenity/blob/master/CHANGELOG.md</a></dd>

  <dt>Serene Örnek Uygulama Şablonu:</dt>
  <dd><a href='https://visualstudiogallery.msdn.microsoft.com/559ec6fc-feef-4077-b6d5-5a99408a6681'>https://visualstudiogallery.msdn.microsoft.com/559ec6fc-feef-4077-b6d5-5a99408a6681</a></dd>

  <dt>Öğreticiler İçin Örnek Kaynak Kodları:</dt>
  <dd><a href='https://github.com/volkanceylan/Serenity-Tutorials'>https://github.com/volkanceylan/Serenity-Tutorials</a></dd>


</dl>


## İsmi Ne Anlama Geliyor?

Serenity'nin sözlük manasına baktığınızda huzur, rahatlık ve sakinlik kelimelerini bulursunuz.

Bunlar bizim Serenity'yi geliştirirken hedeflediklerimizle örtüşen kavramlar. Umarız, Serenity'yi kurup kullanmaya başladıktan sonra siz de bu şekilde hissedersiniz...

## Sağladığı Özelliklerden Bazıları

* Modüler, servis tabanlı bir web uygulama modeli
* İstenen SQL tablosu için başlangıç amaçlı servis ve kullanıcı arayüzü kodlarını üreten bir yardımcı uygulama (Sergen)
* Sunucu tarafında T4 tabanlı kod üretimiyle, script tarafındaki bileşenlere (widget) intellisense / derleme zamanı kontrol desteğiyle erişim.
* Script tarafından AJAX servislerine erişirken yine intellisense ve derleme zamanı kontrol sağlamak için T4 tabanlı kod üretimi.
* Nitelik (attribute) tabanlı form yapısı. Arayüz sunucu tarafında basit bir C# sınıfı ile tasarlanır.
* Form, entity ve servis arasında, otomatik, ek işlem gerektirmeyen veri bağlama (data-binding).
* Önbellekleme Yardımcıları (Yerel / Dağıtık)
* Otomatik önbellek validasyonu
* Konfigürasyon Sistemi (saklama ortamı bağımsız. ayarlar, veritabanı, dosya gibi herhangi bir yerde tutulabilir)
* Basit Loglama
* Raporlama (raporlar sadece veri kaynağıdır, gösterime hiçbir bağımlılıkları yoktur, bu yönüyle MVC ye benzer)
* Script paketleme, küçültme (Node / UglifyJS / CleanCSS kütüphanelerini kullanarak) ve içerik versiyonlama (sürekli F5 / tarayıcı önbelleği temizlemeye gerek yok)
* Akıcı SQL Üretici (SELECT/INSERT/UPDATE/DELETE)
*Micro ORM (Dapper da entegre edilmiştir)
* REST benzeri servisler için özelleştirilebilir işleyiciler, entity sınıflarındaki bilgiyi kullanarak çalışır ve otomatik validasyon yapar.
* Nitelik (attribute) tabanlı navigasyon menüsü
* Arayüz Yerelleştirme (yerelleştirilmiş metinler json, entegre kaynak (resource), veritabanı gibi farklı yerlerde tutulabilir)
* Veri Yerelleştirme (özel bir uzantı tablosu yapısıyla, referans tabloları gibi, kullanıcı tarafından girilen veriler de yerelleştirilebilir)
* Script bileşen sistemi (jQueryUI'dan ilham alınıp C#'a uygun düzenlenmiş bir yapı)
* İstemci ve sunucu tarafı doğrulama (validation). jQuery validate plugini kullanır ancak bağımlılık soyutlanır)
* Kayıt tarihçesi kütüğü (CDC kullanılamadığında)
* Veri yönelimli testler için özel sistem
* Dinamik, sanal script dosyaları
* İstemci tarafı HTML şablonları

