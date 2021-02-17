# Backend

> bakcend


[Başlangıç](/getStarted)

    vagrant up && vagrant ssh

# setNoPadding (panel düzeltmeleri)

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
kodunu ekleyiniz.

Son olarak controller yapısına gidip $dataProvider'e post methodu ekleyiniz.

