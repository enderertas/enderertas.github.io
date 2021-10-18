# Yii Tarafında SQL Kullanımı

> SQL sorguları sistemi yorar minumum sorguyla maksimum işlem yapılması önemlidir. Bunun için bir sorguyla bir çok işlemi yapabileceğiniz bir sorgulama örneği gösterilecektir.

## Aynı Anda İki SQL Sorgusu Nasıl Yapılır

```php

    public static function getAlsoBought($limit)
    {
        $productIds = self::getUserListModel()->select(['user']);
        $model = self::find()->select(['user_id', new Expression("count(id) count")])->andWhere([ //burada gördüğünüz gibi iki sorgu birleştirilmiş ve tek bir return işlemi dönmektedir.
            'user_history' => self::find()->select(['user_history'])
                ->andWhere(['<>', 'user_history', 0])
                ->andWhere(['product_id' => $productIds])
        ])
            ->andWhere(['not in', 'user_school_id', $productIds])
            ->groupBy(['user_school_id'])
            ->limit($limit)
            ->orderBy(['count' => SORT_DESC])
            ->asArray()
            ->cache(Flexible::CACHE_DURATION_12_HOURS)
            ->indexBy('user_school_id')
            ->all();
        return array_keys($model);
    }
```

[Detaylar İçin Tıklayınız](https://www.yiiframework.com/doc/guide/2.0/en/db-query-builder)


```php
$stockAlarm = StockAlarm::find()->where([
    'status' => StockAlarm::STATUS_PENDING,
    'product_id' => \common\models\Product::find()->select(['id'])->andWhere(['has_user' => $merchantId])
])->count('id')
```