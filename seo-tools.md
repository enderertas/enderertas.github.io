#Pazarlama Araçları

* GoogleAnalytics
* FacebookPixel
* YandexVerification

Gibi araçları tek tuşla entegre edebilmek için bu bölümü kullanıyoruz. Bu bölümde nasıl geliştirme örneği aşağıdadır,

>Google Analytics entegresyonunu Comer'da geliştirme örneği


`app/common/models/MarketingIntegrations/` Bu path yoluna modelimizi ekliyoruz

```php
<?php

namespace common\models\MarketingIntegrations; //namespace'i tanımlıyoruz

use common\models\MarketingIntegration; //MarketingIntegration extends alıyoruz

class GoogleAnalytics extends MarketingIntegration
{

    public $gId; // kullanılacak değerleri giriyoruz Google Analytics için sadece id lazım

    public $optionFields = [
        'gId' => self::OPTION_FIELD__TEXT_INPUT //Flexible yapıdan inputu tanımlıyoruz
    ];

    public function rules()
    {
        $rules = parent::rules();
        $rules[] = [['gId'], 'required']; //zorunlu olduğunu belirtip
        $rules[] = [['gId'], 'string']; // Alınacak değerin türünü giriyoruz
        return $rules;
    }

    public function attributeLabels()
    {
        $parent = parent::attributeLabels(); // label alanlarının değerlerini yazıyoruz
        $parent['gId'] = \Yii::t('er', 'Google Analytics ID (X-XXXXXXX)');
        return $parent;
    }


    /**
     * view : MarketingIntegrations/MESELA/xxx.php ise xxx yazılmalı
     * exceptPages : Şu sayfalarda kodu çalıştırmak istemediğinde kullan
     * pages : Eğer pages yoksa tüm sayfalarda çalıştırır eğer varsa girilen sayfalarda çalıştırır
     * @return array[]
     */
    public function run()
    {
        return [
            [
                'view' => 'all-pages',// Ne durumda kodlar döndürülsün istiyorsunuz onları belirtiyorsunuz
            ],
        ];
    }


}
```
>GoogleAnalytics kodunu bizden tüm sayfaların head kısmına eklememizi istiyor onun için 'all-pages' dedik daha kompleks bir örnek aşağıdadır

```php
...
    public function run()
    {
        return [
            [
                'view' => 'all-pages',
                'exceptPages' => ['orderCompleted']
            ],
            [
                'view' => 'add-to-cart',
                'exceptPages' => ['orderCompleted']
            ],
            [
                'view' => 'order-completed',
                'pages' => ['orderCompleted']
            ]
        ];
    }
...
```

Model işlemi bu kadar şimdi Google'dan aldığımız koda oluşturduğumuz modeldeki parametreleri göndermemiz gerekiyor.

Google Analytics'in head kısmına tanımlamamızı istediği kod şu şekildedir:

```html
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=G-xxxxx"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'G-xxxxx');
</script>
```

Gerekli yerlere modelden alacağımız verileri eklemek için

`app/common/models/MarketingIntegrations/` Bu yola gidiyoruz ve
`/GoogleAnalytics/all-pages.php` Bu dosyaları oluşturuyoruz ve kodu Comer'ın işleyeceği biçime çeviriyoruz:

```html
<?php
/**
 * @var $model \common\models\MarketingIntegrations\GoogleAnalytics
 */
?>
<!-- Global site tag (gtag.js) - Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=<?= $model->gId ?>"></script>
<script>
    window.dataLayer = window.dataLayer || [];

    function gtag() {
        dataLayer.push(arguments);
    }

    gtag('js', new Date());
    gtag('config', '<?= $model->gId ?>');
</script>

```