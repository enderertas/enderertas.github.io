# Flexible

Flexible class'ı ile veri tabanında ekstra tablolar oluşturmadan esnek bir şekilde banner, navigation ve yeni bir component geliştirebilirsiniz.

## Flexible 'ın kullanılabilir hazır input alanları

```php
    const OPTION_FIELD__TEXT_INPUT = 'text'; // Text input alanları oluşturur
    const OPTION_FIELD__CHECKBOX = 'checkbox'; // Checkbox alanları oluştutur
    const OPTION_FIELD__DROP_DOWN_LIST = 'dropDownList'; // Selectbox oluşturur, ekstra kod yazımı gerektirir
    const OPTION_FIELD__ICON_PICKER = 'iconPicker'; //Svg Icon seçim alanları oluşturur
    const OPTION_FIELD__VECTOR_ICON_PICKER = 'vectorIconPicker'; //Vector Icon seçim alanları oluşturur
    const OPTION_FIELD__HTML_EDITOR = 'htmlEditor'; //Html editör alanları oluşturur
    const OPTION_FIELD__URL = 'url'; // Url seçim ekranı oluşturur
    const OPTION_FIELD__BLOGS = 'blogs'; // Blog listesi seçim alanı
    const OPTION_FIELD__USERS = 'users'; // Kullanıcı seçim alanı oluşturur
    const OPTION_FIELD__MULTI_SELECT2 = 'multiSelect2'; // Çoklu seçim alanı oluşturur, ekstra kod gerektirir
    const OPTION_FIELD__BLOCK_CONTENT = 'blockcontent'; // Hangi diğer block'un içinde kullanılacağı seçim alanını oluşturur
    const OPTION_FIELD__COLOR = 'color'; // Renk kodu seçim alanını oluşturur
    const OPTION_FIELD__IMAGE = 'image'; // Fotoğraf yükleme alanları oluşturur
    const OPTION_FIELD__NUMBER = 'number'; // Sayısal değer alanı oluşturur
    const OPTION_FIELD__PASSWORD_INPUT = 'passwordInput'; // Parola alanları oluşturur
    const OPTION_FIELD__PLACE = 'place'; // Ülke,il,ilçe gibi alanları gruplanmış bir şekilde getirir, array olarak çıktı verir
    const OPTION_FIELD__PRODUCTS = 'products'; // Ürün adlarını listeler, select2 olarak çoklu seçime sunar
    const OPTION_FIELD__PRODUCT_TAGS = 'productTags'; // Ürün etiketlerini listeler, select2 olarak çoklu seçime sunar
    const OPTION_FIELD__SELECT2 = 'select2'; // Tekli seçim alanı oluşturur, ekstra kod gerektirir
    const OPTION_FIELD__TEXT_AREA = 'textarea'; // Yazı alanı oluşturur
    const OPTION_FIELD__TITLE = 'title'; // Başlık alanları oluşturur
    const OPTION_FIELD__DATE_TIME = 'datetime'; // Tarih alanı oluşturur
    const OPTION_FIELD__BRAND = 'brand'; // Marka seçim alanları oluşturur
``` 


>Not: Bazı input türleri ek kodlamalar talep edebilir.

Eğer OPTION_FIELD__DROP_DOWN_LIST kullanılacaksa Child Model'in içene bu property adıyla
bir list fonksiyonu tanımlanmalı yani şu şekilde. 
```php
          public function list_propertyName()
          {
               return [
                   'value' => 'text'
               ];
          }
```
Bunu yapmazsanız zaten debug olarak sizi uyaracaktır. Yapmanızı zorunlu kılacaktır.

