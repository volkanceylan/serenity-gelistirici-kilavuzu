# Kullanıcı ve Rol Yönetimi

Serene kullanıcı, rol ve hak yönetimi sistemine sahiptir.

> Bu özellik Serenity'nin kendisinde entegre değildir. Her ne kadar tam özellikli çalışsa da bir örnek olarak hazırlanmıştır. İsterseniz kendinize has kullanıcı yönetiminizi tasarlayıp Serenity ile kullanabilirsiniz. Nasıl yapılacağını ilerleyen bölümlerde göreceğiz.

*Administration / Roles (Sistem Yönetimi / Roller)* sayfasını açıp *Administrators (Yöneticiler)* ve *Translators (Çevirmenler)* rollerini oluşturalım.

*New Role (Yeni Rol)* e tıklayıp *Administrators* yazın ve Save (Kaydet) düğmesine tıklayın.

Aynısını *Translators* için de tekrarlayın.

![Create Admin Role](img/create_admin_role.png)

Then click role *Administrators* to open edit form, and click *Edit Permissons* button to modify its permissions. Check all boxes to grant every permisson to this role, then click *OK*.

![Admin Permissions](img/admin_permissions.png)

Repeat same steps for the *Translations* role but this time grant only the *Administration: Languages and Translations* permission.

Navigate to *Administration / User Management* page to add more users.

Click *admin* user to edit its details.

![Edit Admin User](img/edit_admin_user.png)

Here you can change admin details like username, display name, email.

You can also change its password (which is *serenity* by default) by typing into *Password* and *Confirm Password* inputs and clicking *Update*.

> You can also delete it but this would make your site unusable as you wouldn't be able to login.

*admin* is a special user in Serene, as it has all permissions even if none is explicitly granted to him.

Lets create another one and grant roles / permissions to it.

Close this dialog, click new user and type *translator* as username. Fill in other fields as you'd like, then click *Update*.

![Create Translator User](img/create_translator_user.png)

> You may have noticed there is a *Apply Changes* button with a black disk icon without title, next to *Save*. Unlike *Save*, when you use it, the form stays open, so you can see how your record looks like after saving, also you can edit roles and permissions before closing the form.

Now click *Translator* role to open its edit form and click *Edit Roles*. Grant him *Translators* role and click *OK*.

![Edit Translator Roles](img/edit_translator_roles.png)

> When you grant a role to a user, he gets all permissions granted to the role automatically. By clicking Edit Permissions and you can also grant extra permissions explicitly. But you can't revoke a role permission from a user, unless you remove him from the role.

Now close all dialogs and logout by clicking *admin* on top right of site and clicking *Logout*. 

Try logging in with *translator* and the password you set.

Translator user will only have access to Dashboard, Theme Samples, Languages and Translations pages.

![Translator Logged In](img/translator_logged_in.png)



