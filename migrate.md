# Migrate

Yeni bir migrate oluşturma kodu:

```console
php yii migrate/create migrateAdı
```

## Var olan tabloya migrate ile column ekleme işlemi

```console
php yii migrate/create migrateAdı_eklenecekColumAdı
```

```php
    public function safeUp()
    {
        $this->addColumn('profile', 'gender', $this->tinyInteger()->null()->after('last_name'));
        $this->addColumn('profile', 'birthday', $this->timestamp()->null()->after('gender'));
    }
```

```console
php yii migrate
```

