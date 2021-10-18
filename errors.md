# Hatalar

>Karşılaşabileceğiniz bazı hatalar burada belirtilmiştir


## Block'um Sayfalar Sayfasına Düşmüyor

Eklediğiniz block sayfalar sayfasında belirtiğiniz controller içinde görünmüyorsa yapmanız gerekenler

* Sayfalar sayfasında yapıyı güncelle butonuna tıklayın ve uyarı düşmezse bir sonraki adımı takip edin

* View yolunun doğru olduğundan emin olun, eğer hatalı view yoluna sahipseniz düzeltme işlemini yapın ve birinci maddeyi tekrar uygulayın değilse bir sonraki aşamaya geçin

* Action'unuzu doğru oluşturduğunuzdan ve isimlendirme kurallarına uygun olduğundan emin olun örnek action oluşturma şekli aşağıdadır. Sorun buradaysa düzeltme işlemini yapıp birinci maddeyi tekrar uygulayın.

```php
    public function actionFilterSort()
    {
        $productFilter = $this::productFilter();

        return $this->render('filterSort/view',['productFilter'=>$productFilter]);

    }

```

## "An error occurred while handling another error... Hatası

> Oluşturduğunuz actionda isimlendirme kuralına dikkat ediniz

```php
    public function actionFilterSort() //camel case kullanıma dikkat ediniz
    {
        $productFilter = $this::productFilter();

        return $this->render('filterSort/view',['productFilter'=>$productFilter]);

    }

```