## ComponentOptions

namespace `app/common/models/ComponentOptions`

> ComponentOptions ile veri tabanında ekstra tablolar oluşturmadan esnek bir şekilde block geliştirebilirsiniz.

> Örnek

* ComponentOptions klasörüne eklenilecek olan block'un adı (TypographyHeading.php) yazılır
* ComponentOption extends edilir
* Public olarak controller id ve action id yazılır
* Geri kalanı diğer model yapılarıyla aynıdır
* ```app/common/views/``` yoluna gidilir ```heading``` adında klasör oluşturulur
* Hepsi yapıldıktan sonra comer paneline gidilir, sayfalar->yapıyı güncelle butonuna tıklanır

```php
class TypographyHeading extends ComponentOption 
{
public $controllerId = "typography";
public $actionId = "heading";
...
```



