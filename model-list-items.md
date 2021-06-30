# ModelListItems

```app/common/components/classes/ModelListItems.php ```

> Çok fazla tekrarlanan liste yapıları varsa bu alanda tanıtılması gerekir, eğer bir liste kullanılacaksa bu alanda var mı kontrol edilmelidir.

## ModelListItems Örnek Kullanımı

```php
    public static function list_effect()
    {
        return [
            'fade-up' => \Yii::t('er', 'Yukarıya'),
            'fade-down' => \Yii::t('er', 'Aşağıya'),
            'fade-right' => \Yii::t('er', 'Sağa'),
            'fade-left' => \Yii::t('er', 'Sola'),
            'zoom-in' => \Yii::t('er', 'Yaklaş'),
            'zoom-out' => \Yii::t('er', 'Uzaklaş'),
            'flip-right' => \Yii::t('er', 'Sağa Dönder'),
            'flip-left' => \Yii::t('er', 'Sola Dönder'),
        ];
    }
```

> Yukarıdaki örnekteki gibi en basit şekilde bir liste kullanımı sunulmuştur.

## Kullanılacak olan modelden çağırılması

```php
    public static function list_effect()
    {
        return ModelListItems::list_effect();
    }
```

> Yukarıdaki örnekteki kullanılacak olan modeldeki dropdown liste çağırılması gösterilmiştir.

## İçindekiler

* list_effect : data-aos scriptinin bir kısım efektlerini kapsar
* list_margin : Kenar Mesafesi Seçim alanlarında kullanılır
* list_padding : Kenar Boşluğu Seçim alanlarında kullanılır
* sides
* sizes
* getColorClasses : Bootstrap renkleri alanlarında kullanılır parametre alır bu parametre text, button, gibi bootstrap 
  ön ek alanlarıdır.
* getProductItemTemplate : Tema seçim alanlarında kullanılır
* getProductItemTemplatePathByTemplateName
* getVisibility : üye, ziyaretçi ve herkese görünür mü seçimlerinde kullanılır

