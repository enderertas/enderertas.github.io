## SeoUrl::to

* Veriyi çekme tekniği farklı olduğundan dolayı sadece view içinde kullanılır
* İlk parametresi actionId (Frontend-> ControllerMap'teki ya da paneldeki sayfalarda bulunan actionId'dir)

> PhpstormProjects/****-frontend/app/frontend/components/ControllerMap.php

```
SeoUrl::to('homepage', []);
SeoUrl::to('homepage');

// /tr/ana-sayfa
```

```
SeoUrl::to('filterCategory', ['c' => '1']);

// /tr/boya-kategorisi
```

*
    2. alabildiği parametre ekstra parametrelerdir, Id gibi.
*
    3. parametre lang parametresidir. Gösterilecek alan kullanıcının seçeceği dile göre değilde, farklı dile
       gösterilecekse bu parametreyi kullanabilirsiniz.

## SeoUrl::toWithParams

* SeoUrl'de geçerli bütün durumlar burada da geçerlidir.
* Burada 3. parametre olarak dışarıdan parametre alabilir

> Kullanım Örneğ Şu Şekildedir

```
SeoUrl::toWithParams('filterCategory', ['c'=>'2'], ['b' => '2']);

// /tr/akrilik-boya?b=2

```

## SeoUrl::urlTo($value->url)

**Panel alanında kullanılan link verme durumlarında kullanılır.**

## SeoUrl::toInController("consumerLogin")

Sadece Controller içinde kullanılır cachelenmez daha maliyetli bir kullanımdır. **Gerekmedikçe kullanılmamalıdır.**

## \common\models\SeoUrl::strToClear($model->name)

Var olan stringi url formatında temizler


