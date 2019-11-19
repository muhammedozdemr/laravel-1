# Laravel

```
cd /var/www/html/lara
laravel new test
sudo chown -R $USER:www-data *

cd test
sudo chmod -R g+w storage/ 
sudo chmod -R g+w bootstrap/cache
cp server.php index.php
cp public/.htaccess ./
subl .
```

## .env içinde:
- APP_URL ararı
- DB_CONNECTION ayarları
dev/?
## `sudo vi /ect/hosts` içinde:
`127.0.0.1  lara-test.com`


## Apache Ayarları
- `sudo subl /etc/apache2/sites-enabled/test.conf`
İçine şu kod yazılıp kaydedilir:
```
<VirtualHost *:80>
    ServerName lara-test.com
    DocumentRoot /var/www/html/lara/test/public
</VirtualHost>
```
- `sudo service apache2 restart`

## Route Tanımlama
`routes/web.php` dosyası içine ekle:
```
Route::get('/nuri', function () {
    return view('welcome');
});
```

## Tarayıcıda Test
- http://localhost/lara/test/nuri
- http://lara-test.com/nuri




### DB için ön hazırlık
- `vi app/Providers/AppServiceProvider.php`
- Dosya başına: `use Illuminate\Support\Facades\Schema;`
- `boot()` bölümüne: `Schema::defaultStringLength(191);`  Böylece "Specified key was too long" hatası engellenecektir


### Örnek Model Tanımları
```PHP
        Schema::create('kitaps', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('kitapAdi');
            $table->string('kitapAciklama');
            $table->integer('yayinYili');
            $table->integer('yazar_id')->unsigned();
            $table->integer('yayinevi_id')->unsigned();
            $table->timestamps();
        });

        Schema::create('yazars', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('yazarAdi');
            $table->timestamps();
        });

        Schema::create('yayinevis', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('yayineviAdi');
            $table->timestamps();
        });
```

### Dosyaların Oluşturulması
Model, Migration, Factory ve Controller dosyalarının hazırlanması:
```
// Migration ile birlikte Model'lerin oluşturulması
php artisan make:model Kitap -m
php artisan make:model Yazar -m
php artisan make:model Yayinevi -m

// Modeller için factory oluşturulması
php artisan make:factory KitapFactory
php artisan make:factory YayineviFactory
php artisan make:factory YazarFactory

// View'ler için Controller oluşturulması
php artisan make:controller KitapController --resource
php artisan make:controller YazarController --resource
php artisan make:controller YayineviController --resource

```

Migration'ın herşeyi silip sıfırdan çalıştırılması: `php artisan migrate:fresh`



### Temizlik
find . -name "*kitap*" | xargs rm -f











