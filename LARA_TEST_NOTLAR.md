# Laravel

```
cd /var/www/html/lara
laravel new test
sudo chown -R $USER:www-data *

cd test
sudo chmod -R g+w storage/ bootstrap/cache
cp server.php index.php
cp public/.htaccess ./
subl .
```

## .env içinde:
- APP_URL ararı
- DB_CONNECTION ayarları

## /ect/hosts içinde:
`127.0.0.1  lara-test.com`


## Apache Ayarları
- `sudo subl /etc/apache2/sites-enabled/test.conf`
İçine şu kod yazılıp kaydedilir:
```
<VirtualHost *:80>
	ServerName lara-test.com
	DocumentRoot /var/www/html/lara/test
</VirtualHost>
```

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

