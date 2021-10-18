# OWL Carousel Kullanımı

>Comer içinde performansı artırmak için owl carousel kullanımı standartından farklıdır

## Owl Carousel İçin View Alanında Yapılması Gerekenler
```php
$loadedFirstFunction = false;  //false ile başlatıyoruz

$items = [];
foreach ($images as $index => $image) {
    $image['class'] = 'img-fluid w-100'; // owl-lazy standart yapıda varsa o classı siliyoruz
//    $image['data-owl-w2i'] = $image['data-w2i']; //standartta olan classları kaldırıyoruz
//    $image['data-w2i-ref'] = ['xs' => 1]; //standartta olan classları kaldırıyoruz
    if (!$loadedFirstFunction) { // functionun bir defa çalıştırmak için kontrolümüzü yapıyoruz
        $image['data-loaded-function'] = 'productImages'; // js de kullanılacak functionun adını belirliyoruz
    }
    $zoomImages = Json::decode($image['data-w2i']);
    $zoomImages = ArrayHelper::getValue($zoomImages, '1000');

//    unset($image['data-w2i']); //eğer böyle bir function kullanılmışsa kaldırıyoruz
    $img = Html::img('', $image);
    $img = Html::a($img, $zoomImages, ['data-fancybox' => 'fb-' . $_modelBlock->id]);
    $items[] = Html::tag('div', $img, ['class' => 'item']);

    $loadedFirstFunction = true; //ve son olarak true gönderiyoruz
}
```
>View alanında yapılması gerekenler sadece bu kadardır. 

## Owl Carousel İçin JS Alanında Yapılması Gerekenler

>Owl Carouselin standart yapısından farklı olarak comer içinde kullanılan js aşağıdaki gibidir.


```
window.productImages = function (owl) { // view kısmında verdiğimiz functionun adını veriyoruz
    owl = owl.parents('.owl-carousel');
    owl.owlCarousel({
        items: 1,
        lazyLoad: false, //lazyLoadı false çeviriyoruz, geri kalan owl carousel ayarlamalar şekli aynıdır.
        lazyLoadEager: 1 
    });
    $.width2Image.owlCarouselLazy(owl);
};

```

## Owl Carousel İçin CSS Alanında Yapılması Gerekenler

>Yukarıdaki işlemleri başarılı bir şekilde gerçekleştirseniz bile owl carosuel standartta olduğu gibi çalışmayacaktır bu yapıya uygun olarak css eklemeniz gerekecektir.

```scss

@import "../../../../web/assets/css/core/_bootstrap";

.carousel.vertical-list {
  .owl-carousel {
    
    &:not(.owl-loaded) {//hangi ölçüde kaç item gösterilmesi gerektiğinin ayarlarıdır.
      display: flex !important;
      .item {
        display: none;
        margin: 0 15px;

        &:nth-child(1),
        &:nth-child(2) {
          width: calc(100% / 2);
          display: block;
        }

        @include bootstrap-respond-to(sm) {  //hangi ölçüde kaç item gösterilmesi gerektiğinin ayarlarıdır.
          &:nth-child(1),
          &:nth-child(2),
          &:nth-child(3),
          {
            width: calc(100% / 3);
            display: block;
          }
        }
        @include bootstrap-respond-to(lg) {//hangi ölçüde kaç item gösterilmesi gerektiğinin ayarlarıdır.
          &:nth-child(1),
          &:nth-child(2),
          &:nth-child(3),
          &:nth-child(4),
          &:nth-child(5),
          {
            width: calc(100% / 5);
            display: block;
          }
        }

      }
    }

  }

}


```
