# JS ve CSS Kullanımı

> JS ve CSS leri yazmak için genelde assetBundle kullanılması gerekmektedir fakat bazen ana dosyanın içinde de js ve css leri kullanmamız gerekebilir onun için aşağıdaki yapıyı kullanmalısınız.

## JS Kullanımı

```php
use common\components\widgets\JS;


<?php
JS::begin();
?>
    <script>
    // bu alana scriptlerinizi yazabilirsiniz
    </script>
<?php
JS::end();
?>
```

## CSS Kullanımı

```php
use common\components\widgets\CSS;


<?php
CSS::begin();
?>
    <style>
/*Bu alana CSS kodlarınızı yazabilirsiniz */
    </style>
<?php
CSS::end();;
?>
```