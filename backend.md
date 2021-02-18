# Backend

> bakcend


[Başlangıç](/getStarted)

    vagrant up && vagrant ssh

# GridView 

GridView;  Sıralama, sayfalama ve ayrıca verileri filtreleme gibi özellikler sağlar. Temel kullanım şuna benzer:

> İmport
 ```php
    use backend\components\widgets\GridView;
 ```  

 ```php
    <?= GridView::widget([
        'dataProvider' => $dataProvider,
        'filterModel' => $searchModel,
        'columns' => [
            ['class' => 'yii\grid\SerialColumn'],
            'id',
            'title',
            'updated_at',
            'created_at',

            ['class' => 'yii\grid\ActionColumn'],
        ],
    ]); ?>
 ```  
Temel görünümü şuna benzer:
![](assets/gridview.png)

## Responsive GridView

GridView'de ki Columnlara genişletilebilme(Collapsible) özelliği verebilmek için kullanım şu şekildedir:

 ```php
            [
                'attribute' => 'id', //attribute adı yazılır
                'headerOptions' => ['data-class' => 'expand'], //expand classı verilir
            ],
 ```  
Hangi Columnların gizleneceğini ayarlama ise şu şekildedir:
 ```php
            [
                'attribute' => 'is_active', //attribute adı yazılır
                'headerOptions' => ['data-hide' => 'phone,tablet'], // cihazlar belirtilir
            ],
 ```  
Sonuc şu şekildedir:

![](assets/responsive_gridview.png)

# GridWrap 

Panel üstünde sayfalardaki ayrımları kolay yapabilmeyi amaçlar. View içinde kullanılır. İç içe kullanıma uygundur.
GridWrap sayesinde aşağıdaki gibi bir görüntü elde edebilirsiniz. GridWrap default olarak her sayfada çalışır.

![](assets/gridwrap.png)
 
> İmport
 ```php
    use backend\components\widgets\GridWrap;
 ```

> Örnek Kullanımlar   
 ```php
GridWrap::begin([
    'title' => \Yii::t('er', 'Bu İşlem ile Teslimi Yapılması Gereken Ürünler'),
    'size' => GridWrap::SIZE_XS12_SM12_MD6_LG6,
    'color' => 'red',
    'noPadding' => true
]);
echo Html::tag('div', 'content');
GridWrap::end();
 ```   

## setNoPadding (panel düzeltmeleri)

Filtreleme oluşturmadan önce 

```php
   use backend\components\widgets\GridWrap;
   use backend\components\widgets\GridView;
```
```php
GridWrap::setNoPadding();
```
```php
GridWrap::setToolbar([
    Html::a(Yii::t('er', 'Create Place Province'), ['create'], ['class' => 'btn btn-success'])
]);
```


 yapılarını eklememiz gerekmektedir.  Sonrasında çalışılan dosyanın searchModeline gidilir ve kod yapısında belirtilmiş kısma  
 ```php
 use Search;
```
 eklenir.  
 
```php
class PlaceProvinceSearch extends PlaceProvince
 {
     use Search;
     /**
      * {@inheritdoc}
      */
     public function rules()
     {
         return [
             [['id'], 'integer'],
             [['country_code', 'name'], 'safe'],
         ];
     }
```
*use Search; ın ekleneceği alan*

![](https://github.com/enderertas/enderertas.github.io/blob/main/assets/filter1.png?raw=true)
  
Bu işlemler tamamlandıktan sonra böyle bir görüntü elde edilir.

# Filtreleme (Filter)


Eğer filtreleme oluşturulmak isteniyorsa,
Daha önce use Search; oluşturduğumuz alanın altına bunları ekliyoruz ve use Search; kısmını siliyoruz daha sonra `    public $optionFields = [];
` içinde düzenlemeler yapacağız.
  ```php
    public $filters = [];
    public $filterNumberCondition = [];
    public $optionFields = [];
```
```php
            [['filterNumberCondition', 'filters'], 'safe'],
```
Yukarıdaki kodu da `public function rules()`'un içine ekliyoruz.  

Sırada neleri aratacağımız var.  
ID aratma örneği
```php
    public $optionFields = [
    'id' => Flexible::OPTION_FIELD__NUMBER,
];
```   
aynı dosya içindeki
`        $this->load($params);
`  kodundan sonra 
 ```php
        BackendFilterCache::create($this, $query);
```
kodunu ekleyiniz ve eğer integer bir değer filtreliyorsanız searchModeldeki `            'id' => $this->id,
` kısmını silin.
```php
  $query->andFilterWhere([
            'id' => $this->id,
        ]);
```

Son olarak controller yapısına gidip $dataProvider'e post methodu ekleyiniz.
**Tebrikler.**  

>Not: Filtre hazırlarken olmaması gereken bir hata ile karşılaşıyorsanız 
```sql
TRUNCATE `evimdehobi-db`.`backend_filter_cache`
```  
SQL sorgusunu çalıştırın.

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
            'id':zone, //bu şekilde kullanılır
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









