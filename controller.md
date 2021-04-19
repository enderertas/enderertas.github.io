# Controller

> Fonksiyonların kullanılacağı asıl alan burasıdır.


## Parametre
Controller'da basit bir şekilde parametre işleme şu şekildedir:



```php
    public function actionIndex($pending = false) //parametre belirtiniz
    {
        $searchModel = new StockAlarmSearch();
        /*
         * Parametreyi manuel listelet
         */
        if ($pending) {
            BackendFilterCache::deleteAllFilter($searchModel);
            $searchModel->status = StockAlarm::STATUS_PENDING;
        }
        $dataProvider = $searchModel->search(Yii::$app->request->post());
        /*
         * parametre filtrelendikten sonra redirect et
         */
        if ($pending) {
            return $this->redirect(Url::current(['pending' => null]));
        }

        return $this->render('index', [
            'searchModel' => $searchModel,
            'dataProvider' => $dataProvider,
        ]);
    }
```

Controller'da üstteki örneğe göre daha kompleks parametre işleme şu şekildedir:

```php
    public function actionIndex($out_of_product = false, $editable_view = null)
    {
        $searchModel = new ProductChildSearch([
        ]);
        /**
         * Stoksuz olanları manuel listelet
         */
        if ($out_of_product) {
            BackendFilterCache::deleteAllFilter($searchModel);
            $searchModel->stock = '0';
            $searchModel->filterNumberCondition['stock'] = '=';
            $searchModel->is_active = 1;
        }
        $dataProvider = $searchModel->search(Yii::$app->request->post());

        /**
         * Stoksuzların filtresini ekledikten sonra redirect et
         */
        if ($out_of_product) {
            return $this->redirect(Url::current(['out_of_product' => null]));
        }

      ..........

```

>Bu işlemlerden sonra filtrelemede sıkıntı yaşıyorsanız kod hiyerarşisini kontrol ediniz.

## Panel Üzerinden Controller ve Sayfa Oluşturma

* Panel üzerinden "Kontrolör Haritası"na gidiniz.
* Yeni controller oluşturunuz.
* Panel üzerinden sayfalar-> sayfa oluştura gidiniz.
* Sayfa tipinde oluşturduğunuz controller'i seçiniz

>Sistem otomatik işlemezse 

```
vagrant ssh
```
Projeninizin dosya yoluna gidin

```
php yii seo-url/one controller-adı
```

```
php yii queue/run
```