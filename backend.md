# Backend

> bakcend


[Başlangıç](/getStarted)

    vagrant up && vagrant ssh

# Filter

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

![](https://lh3.googleusercontent.com/Nsh4yYhwSSaA2C95N4dOTHKYFymX3nqCV5-aUYCbOdUiF87lsJ3v0T1ndhiDhTrilUrNsMDHiCqEYZW0d7fSobTtM2auuswPglL0ErOsh-ejec9kUeMh5oFtT0Cm7DN4655W5m4DIUey0g9dIeJuOOPNkSHOj7K_g27IpQssbnjPQYpeAgJNhpR5vpLi8YGzOvAAi6GzDbwQTg23bl6fbopvH9YKItrwRpCzmBmZE3DEJk1sHYn2dRGUiLGQ44YInrCGQAmHsm25wEWlo2FKPFXsVi2miFRA2Zs3mL61gmJgTWz_Ibut47QX4KGMD-ypgufg7vMtfG5PTHnK4Btt3j12PrKzsoFXDxkdcRFp9F6LVtRN0QI7MVXqOlaJcJtwzlkpMd9AcCeHQV92lr_QryS7YwcqBVHyXaIsLFMZ5VxIpQHWUMd5Cca3HHo83MiLt7tDobX2VtEL7gTaWNk_z-urk9yFc_9HD3B9Pk6bnwSiTHZt09sgLmjxWmM_ag2Q0OA351MJMJjJURMfu_Ju6mNNAHBmsbXhNNVMjnY-MiHbZdxsB7VjBR4x9Is2k63iV150UXlQWUiWIGuWq7vTksQiaOjTLonZ6jweth6GUAZOlCTITjPGDZTYAWssO40iywCI93XmymmBks-d1Wj2AFyHIP_eQvANUs8JPI1fhHvC84DM1dGkbIrsqjXj=w2880-h828-no?authuser=0)
  
Bu işlemler tamamlandıktan sonra böyle bir görüntü elde edilir.  

