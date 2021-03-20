# GeneralOptions

> NameSpace app/common/models/GeneralOptions

Sitenin temel ayarlarını kurgulamak için kullanılır. Örneğin:

* Proje Adı
* Logo
* Varsayılan Dil
* Ürün Ayarları
* Email Ayarları
* Meta Etiketleri
* İş Ortağı Ayarları

Bu alanlara benzer yeni özellik geliştirilicekse GeneralOptions'ta çalışmalısınız.

> Demo Örnek

GeneralOptions Hakkında Daha Detaylı Bilgi İçin Bu Demo Örneği İnceleyebilirsiniz.

## GeneralOptions'a Yeni Field Ekleme

```php
            'product' => [
                'label' => Yii::t('er', 'Ürün Ayarları'),
                'fields' => [
                    'params.productConfig.showItemAmount' => [ // params ön ekinden sonra gelen id ile dışarıdan nasıl çağıracağınızı belirlersiniz.
                        'label' => Yii::t('er', 'Ürün Miktarı Kutusu Listelerken Gösterilsin Mi?'),
                        'type' => 'checkbox',
                    ],
                ]
            ]
```

## GeneralOptions'tan View'e Veri Çekme İşlemi

**Yukarıda GeneralOptions'a Eklenmiş Field'ın Çağırma işlemiş Şu Şekildedir.**

```php
$showAmount = GeneralOptions::getValue("params.productConfig.showItemAmount");
```
Eğer CheckBox İşaretliyse 1 Dönecektir.