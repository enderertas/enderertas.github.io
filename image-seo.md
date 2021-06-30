#ImageSeo Kullanımı

```namespace common\models\ImageSeo; ```

> Image'ların alt değerini otomatik ayarlamak için kullanılır bu yapıyla beraber frontend tarafında ekstradan alt değeri belirlemeye gerekmez ya da panel üzerinden alt değer inputu oluşturmanıza gerek kalmayabilir.


## Örnek Kullanım

* Klasör içine kullanılacak olan modelin adında php class açılır örneğin CarouselBasic.php

```php
namespace common\models\ImageSeo; // namespace belirlenir


class CarouselBasic extends ImageSeo // image seo extends edilir
{

    public function variables()
    {
        return [
            "title" => \Yii::t('er', "Başlık"),  // modeldeki hangi alan alt değer olacaksa onun adı yazılır 
        ];
    }

    public function template()
    {
        return '{title}'; // son olarak template fonksiyonu içinde return ettirilir.
    }
}
```

> Yukarıdaki işlemi gerçekleştirdikten CarouselBasic modelindeki title alt değer olarak belirlenmiş olacaktır ve frontend tarafında ekstra bir iş yapmaya gerek kalmayacaktır.

