# Backend

> bakcend


[Başlangıç](/getStarted)

    vagrant up && vagrant ssh

# GridView

GridView; Sıralama, sayfalama ve ayrıca verileri filtreleme gibi özellikler sağlar. Temel kullanım şuna benzer:

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

## GridView Sıralama Listesini Değiştirme

> İlgili GridView'in Searchmodel'ine Gidilir ve Gerekli Kod Yazılır Örnek:

```php
 public function search($params)
{ $query = ProductView::find();

        $dataProvider = new ActiveDataProvider([
            'query' => $query,
        ]);

//ilgili kodu dataproviderin hemen altına yazabilirsiniz

        $dataProvider->sort->defaultOrder = [
            'id' => SORT_DESC
        ];

...
```

## Responsive GridView

GridView'de ki Columnlara genişletilebilir (Collapsible) özelliği verebilmek için kullanım şu şekildedir:

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
                'headerOptions' => ['data-hide' => 'phone,tablet'], // Gizlemek istediğiniz cihazları belirtiniz
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

## Aynı Sayfada İki ( Çoklu ) GridWrap Kullanımının Detayları

```php
<?php
..
GridWrap::begin([    //GridWrap'ı Başlatalım
    'title' => $this->title, //GridWrap'ın Detaylarını Ekleyebiliriz Bu Array İçine (ActionTool, Başlık gibi)
])

?>
<div class="share-cart-update">  // Grid İçeriği
    <?= $this->render('_form', [
        'model' => $model,
    ]) ?>

</div>
<?php
GridWrap::end(); //GridWrapı Sonlandır

GridWrap::begin([ // Diğer GridWrap
    'title' => \Yii::t('er', 'Update Share Cart Item'),  //GridWrap'ın Detaylarını Ekleyebiliriz Bu Array İçine (ActionTool, Başlık gibi)
    'toolbar' => Html::a(Yii::t('er', 'Create Share Cart Item'), ['share-cart-item/create', 'share_cart_id' => $model->id], ['class' => 'btn btn-success']),
    'noPadding' => true
])
?>
<div class="share-cart-item-index">

    <?php // echo $this->render('_search', ['model' => $searchModel]); ?>

    <?= GridView::widget([ // Grid İçeriği
         'searchModel' => $searchModel,
         'dataProvider' => $dataProvider, //Uyarıları iptal etmek için çalışılan dosyanın controller'ına tanıtmak gerekir
        'columns' => [

            'product_id:product',
            'amount',

            [
                'class' => 'yii\grid\ActionColumn',
                'template' => '{update} {delete}',
                'urlCreator' => function ($action, $model, $key, $index) {
                    return Yii::$app->getUrlManager()->createUrl(['/promotion/share-cart-item/' . $action, 'id' => $model->id]);
                }
            ],
        ],
    ]); ?>

</div>
<?php
GridWrap::end();
?>
```

**Hata Veren filterModel dataProvider düzeltmeleri için izlemeniz gereken yol**

* Hata veren gridin controller actionIndex'ine gidilir ve oradan aşağıdakiler kopyalanır:

```php
$searchModel = new ShareCartItemSearch();
$dataProvider = $searchModel->search(Yii::$app->request->post());
```

* Update alanında çalışılacağı için bu kopyalananlar çalışılacak alanın controller'ında update kısmına gerekli yere
  yapıştırılır.
* Ardından render kısmına belirtilen şekilde düzenlenir

```php
        return $this->render('update', [
            'model' => $model,
            'modelShareCartItem' => $modelShareCartItem
        ]);
```

* İlk yapıştırdığımız kodlar üzerinde şu şekilde değişiklik yapıyoruz

```php
        $modelShareCartItem['searchModel'] = new ShareCartItemSearch();
        $modelShareCartItem['dataProvider'] = $modelShareCartItem['searchModel']
```

* Bu şekilde model parametrelerimizi gönderdik
* Şimdi sayfayı yenilediğimizde dataProvider hatası alacaksınız bunu düzeltmek için çalışılan view kısmına yeni
  eklediğimiz dataProvider ve searchModeli tanıtmamız gerekecek

```php
         'dataProvider' => $modelShareCartItem['dataProvider'],
        'filterModel' => $modelShareCartItem['searchModel'],
``` 

## setToolbar (panel düzeltmeleri)

Default gelen action toollara daha iyi bir görüntü vermek adına eski actionTool silinir yerine aşağıdaki kullanım
eklenir
> Update Sayfası Örneği

 ```php
GridWrap::setToolbar([
    Html::a('Update', ['update', 'id' => $model->id], ['class' => 'btn btn-primary btn-xs']),
    Html::a('Delete', ['delete', 'id' => $model->id], [
        'class' => 'btn btn-danger btn-xs',
        'data' => [
            'confirm' => Yii::t("er", 'Are you sure you want to delete this item?'),
            'method' => 'post',
        ],
    ])
]);
 ```

Ortaya çıkan görüntü bu şekildedir.
![](assets/actiontool.png)

> View Sayfası Örneği

 ```
GridWrap::setToolbar([
    Html::a(Yii::t('er', 'Create Affiliate'), ['create'], ['class' => 'btn btn-success'])
]);
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

yapılarını eklememiz gerekmektedir. Sonrasında çalışılan dosyanın searchModeline gidilir ve kod yapısında belirtilmiş
kısma

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

Eğer filtreleme oluşturulmak isteniyorsa, Daha önce use Search; oluşturduğumuz alanın altına bunları ekliyoruz ve use
Search; kısmını siliyoruz daha sonra `    public $optionFields = [];
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


> Not: Filtre hazırlarken olmaması gereken bir hata ile karşılaşıyorsanız

```sql
TRUNCATE `evimdehobi-db`.`backend_filter_cache`
```  

SQL sorgusunu çalıştırın.

## Filtreleme De Option Fields'lerin Kullanımı

Option Fields'lerin örnek kullanımı şu şekildedir:

```php
 public $optionFields = [
id'=>Flexible::
        'id' => Flexible::OPTION_FIELD__NUMBER,
        'message' => Flexible::OPTION_FIELD__TEXT_INPUT,
        'type' => Flexible::OPTION_FIELD__SELECT2,
    ];
```

Option Fieldsler için belirlenmiş tüm const değerlerini Flexible class'ı ile çağırabilirsiniz ve hazır const değerleri
şunlardır:

```php
    const OPTION_FIELD__TEXT_INPUT = 'text';
    const OPTION_FIELD__CHECKBOX = 'checkbox';
    const OPTION_FIELD__DROP_DOWN_LIST = 'dropDownList';
    const OPTION_FIELD__ICON_PICKER = 'iconPicker';
    const OPTION_FIELD__VECTOR_ICON_PICKER = 'vectorIconPicker';
    const OPTION_FIELD__HTML_EDITOR = 'htmlEditor';
    const OPTION_FIELD__URL = 'url';
    const OPTION_FIELD__BLOGS = 'blogs';
    const OPTION_FIELD__USERS = 'users';
    const OPTION_FIELD__MULTI_SELECT2 = 'multiSelect2';
    const OPTION_FIELD__BLOCK_CONTENT = 'blockcontent';
    const OPTION_FIELD__COLOR = 'color';
    const OPTION_FIELD__ICON = 'icon';
    const OPTION_FIELD__IMAGE = 'image';
    const OPTION_FIELD__NUMBER = 'number';
    const OPTION_FIELD__PASSWORD_INPUT = 'passwordInput';
    const OPTION_FIELD__PLACE = 'place';
    const OPTION_FIELD__PRODUCTS = 'products';
    const OPTION_FIELD__PRODUCT_TAGS = 'productTags';
    const OPTION_FIELD__SELECT2 = 'select2';
    const OPTION_FIELD__TEXT_AREA = 'textarea';
    const OPTION_FIELD__TITLE = 'title';
    const OPTION_FIELD__DATE_TIME = 'datetime';
    const OPTION_FIELD__BRAND = 'brand';
```

> Not: Bazı input türleri ek kodlamalar talep edebilir.

Eğer OPTION_FIELD__DROP_DOWN_LIST kullanılacaksa Child Model'in içene bu property adıyla bir list fonksiyonu
tanımlanmalı yani şu şekilde.

```php
          public static function list_propertyName()
          {
               return [
                   'value' => 'text'
               ];
          }
```

Bunu yapmazsanız zaten debug olarak sizi uyaracaktır. Yapmanızı zorunlu kılacaktır.

## GridView'de Html kodlarının kullanımı

> Basit Kullanımı Şu Şekildedir:

```php
                      [
                          'attribute' => 'status',
                          'format' => 'raw',
                          'value' => function ($model) {
                              return $model->status == StockAlarm::STATUS_SUCCESS ? Html::tag('p', Yii::t('er', 'Bildirildi'), ['class' => 'btn-xs btn btn-success'])
                                  : Html::tag('p', Yii::t('er', 'Bekliyor'), ['class' => 'btn-xs btn btn-danger']);
                          }
                      ],
```
