# Yerelleştirme (Localization)  

String ifadelerin geleceği yerlere aşağıdaki kod satırıyla yerelleştirmeyi gerçekleştirebilirsiniz. 
```php
Yii::t('er', 'Çeviri Yapılacak Metin');
```
```php
Yii::t('er', '$ceviri_ifadesi');
```

# Format Ekleme (Formatter)  

>Para birimi, saat, tarih ya da Id'li ifadelerin okunulabilirliğini değiştirir.
  
## Gridlerde kullanımı
```php
<?= DetailView::widget([
        'model' => $model,
        'attributes' => [
            'id:zone', //bu şekilde kullanılır
                    ],
                ]); ?>
```
## Standart kullanımı
```php
    echo Yii::$app->formatter->asZone(5);
```  
## Yeni Formatter Oluşturma

Eğer bir formatter oluşturmak isteniyorsa `app/backend/components/_extends/Formatter.php` yoluna gidilir ve 

```php
    public function asZone($value) //oluşturulacak olan formatter fonksiyonunun adı as ile başlar
    {
        if ($value) {
            $model = SiteZone::find()->select(['name'])->where([ //hangi modelden besleneceği belirtilir
                'id' => $value
            ])->cache(ActiveRecord::CACHE_DURATION_6_HOURS)->one();
            if ($model) {
                return Html::a("($value) " . $model->name, ['/site-zone/update', 'id' => $value]); //return gerçekleştirilir
            }
        }
    }
```  

Daha detaylı bilgi için [Yii Dökümantasyonunu inceleyiniz.](https://www.yiiframework.com/doc/api/2.0/yii-i18n-formatter#:~:text=Formatter%20is%20configured%20as%20an,extension%20has%20to%20be%20installed.)









