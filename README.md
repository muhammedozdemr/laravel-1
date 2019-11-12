KAYNAK: https://www.youtube.com/watch?v=9ugzTTvNvkM&list=PLgFB6lmeXFOqRC4Sc-RST38jboldiQdds&index=7


# LARAVEL

## LAMP kurulacak

## NodeJS Kurulacak
- curl -sL https://deb.nodesource.com/setup_13.x | sudo -E bash -
- sudo apt-get install -y nodejs
- nodejs -v
- npm -v

## Composer Kurulumu
- sudo apt install curl vim
- sudo apt install php php-pear php-fpm php-dev php-zip php-curl php-xmlrpc php-gd php-mysql php-mbstring php-xml libapache2-mod-php -y
- curl -sS https://getcomposer.org/installer | php
- sudo mv composer.phar /usr/local/bin/composer
- composer global require laravel/installer

## PATH Tanımı gerekiyor:
- `sudo vi /etc/environment`
- Şöyle görünebilir:  **PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"**
- Bu PATH değişkeninin başına şu kod eklenir: `$HOME/.config/composer/vendor/bin:`
- PATH="**$HOME/.config/composer/vendor/bin:**/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
- Değişikliğin devreye girmesi için şu komut çalıştırılır: `source /etc/environment`

# LARAVEL

## Yeni bir Laravel Projesine Başlama
- `laravel new /var/www/html/blog`
- veya
- `composer create-project --prefer-dist laravel/laravel /var/www/html/blog`

## PROJE DİZİNİMİZ:
- `/var/www/html/lara/blog`

## Proje için APACHE ayarı
- `vi /etc/apache2/sites-enabled/000-default.conf`
- **Şunu ekle:**
- <VirtualHost *:80>
- 	ServerName laravel.development
- 	DocumentRoot /var/www/html/lara/blog/public
- </VirtualHost>
- **VEYA**
- Yukarıdaki VirtualHost tanımını içeren yeni bir dosyayı aynı dizinde oluştur. 
- Örnek: laravel-blog-deneme.conf içeriği:
- <VirtualHost *:80>
- 	ServerName laravel.development
- 	DocumentRoot /var/www/html/lara/blog/public
- </VirtualHost>
- Devreye girmesi için servisi tekrar başlat: `sudo service apache2 restart`
- Veya Devreye girmesi için ayarları tekrar okut: `sudo service apache2 reload`
- NOT: Windows'da bunun için **Host File Editor** adlı program kullanılabilir

## Gerekli dizinlere YAZMA yetkisi verilmesi
```BASH
cd /var/www/html/lara/
chmod -R g+w storage/
chmod -R g+w bootstrap/cache/
chown -R $USER:www-data /var/www/html/lara/
```

## Proje için HOSTS ayarı
- `vi /etc/hosts`
- **Şunu ekle:**
- `127.0.0.1     laravel.development`

## Tarayıcıdan Test Edelim
- http://laravel.development

## Yerel laravel Sunucusunu Başlatma
Proje dizininde olduğunuza emin olun!
```BASH
cd /var/www/html/lara/blog
php artisan serve
http://127.0.0.1:8000
```

## DB İçin Bağlantı Kurulması
- Adminer'e girilir ve `laravel_blog` adında yeni bir DB oluşturulur
- Proje dizinine geçilir
- `.env` dosyası içinde şu sahalar doldurulur:
- `DB_DATABASE=laravel_blog`
- `DB_USERNAME=root`
- `DB_PASSWORD=root`

## SQLite kullanmak istenirse DB Ayarı
- BOŞ sqlite oluşturmak için: `touch database/myBlogs.sqlite`
- **config/database.php** içine şunlar yazılır:
- DB_CONNECTION=sqlite
- DB_DATABASE=/absolute/path/to/myBlogs.sqlite


## Yeni Laravel Uygulamamızın ayarları
- **config/app.php** dosyası içinde timezone ve locale için ayarlar vardır.
- `.env` dosyası içinde şu sahalar doldurulur:
- `APP_NAME="Laravel Blog Uygulaması"`
- `APP_URL=http://localhost.development`   DİKKAT: Sonunda '/' karakteri YOK!
- `APP_DEBUG=true`

## APP KEY değiştirme/oluşturma
- KEY tanımı, `.env` adlı dosya içinde yer alır.
- `.env` adlı bir dosya yoksa `.env.example` adlı dosya kopyalanarak oluşturulur
- KEY oluşturma komutu: `php artisan key:generate`
- Eğer, key oluşturulmazsa uygulama güvenli olmaz.

## DB için ön hazırlık
- `vi app/Providers/AppServiceProvider.php`
- Dosya başına: `use Illuminate\Support\Facades\Scheme;`
- `boot()` bölümüne: `Schema::defaultStringLength(191);`  Böylece "Specified key was too long" hatası engellenecektir


## Migration hazırlama
- Faydalı Kaynak: https://kodumunblogu.net/detail/laravelde-veritabani-iliskileri-eloquent-relationships-islemleri
- **ORM**: Object Relational Mapper ile DB işlemleri kolayca yapılır
- **MODEL**: DB içindeki TABLO olarak düşünülmelidir
- ` modeli`, `users` adlı tabloyu yönetirken kullanılan addır
- **MIGRATION**: Tablonun Yapısıdır (Table Structure)
- **Artisan**: Laravel'in Komut Satırı arayüzüdür (CLI)
- Laravel, otomatik olarak "User" modelini tanımlar
- Tanımlar sırasında kullanılabilecek [veri tipleri burada](https://laravel.com/docs/6.x/migrations#columns)
- database/migrations/xxxxxx_xxx_create_users_table.php içine ekle:
```PHP
    public function up()
    {
        Schema::create('users', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('fname');
            $table->string('lname')->nulable();
            $table->string('email')->unique();
            $table->string('password');
            $table->timestamps();
        });
    }
```
Diğer dosyalar siliniyor
- Tanımladığımız **users** tablosunu işleme alabilmek için:
- Proje dizinine geçilir ve `php artisan migrate` komutu uygulanır

Tabloda değişiklik olursa, 
- önce database/migrations dizinine gidilir
- `php artisan migrate:rollback` yapılır. DİKKAT! Tablodaki tüm data silinir!
- database/migrations/xxxxxx_xxx_create_users_table.php içinde değişiklikler yapılır.
- Proje dizinine geçilir ve `php artisan migrate` komutu uygulanır

## Projeye yeni bir Tablo (model) ekleme
- `php artisan make:model Post -m` yapılır. Dikkat!!! Çoğul değil!! (s yok, Posts değil)
- Böylece hem Model dosyası oluşturulur hem de migration dosyası. Örnek:
  - app/Post.php
  - migrations/xxxxx.Post.php
- Kullanıcı tablosu ile migration tablosu arasında ilişki tanımlanacaksa bu Model içinde yapılır
database/migrations dizini içindeki xxx_xxx_posts_table'da:
```PHP
    public function up()
    {
        Schema::create('posts', function (Blueprint $table) {
            $table->bigIncrements('id');
            $table->string('title');
            $table->mediumtext('content');
            $table->integer('user_id');  // user: tablo adı, id: o tablodaki id adlı saha
            $table->timestamps();
        });
    }
```
- `php artisan migrate` yapılır
Artık, veritabanında posts (ve users) adlı tabloları görebiliriz


# ROUTE

## Rooting tanımı sonrasında 404 hatası oluşursa:
- `sudo a2enmod rewrite` komutu çalıştırılır
- `vim  /etc/apache2/apache2.conf` komutu ile aşağıdaki bölümün olması sağlanır  **AllowOverride All** olduğuna emin ol!
```
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```
- Değişikliklerin devreye girmesi için: `sudo service apache2 restart`

## Rooting normal çalışırsa:
- `routes/web.php` içinde çalışılır
- `dd("mesaj");` Uygulama akışını keser (die komutu) ve mesajı gösterir
- Route'lar MODEL ve GÖRÜNÜM arasında yer alırlar
- Route'lar Controller'lara gider.
- Örnek route tanımı: `Route::get('/admin/users/{id}', function ($id) {`
- Bir değişken ile View'e yönelecek route tanımı örneği: (Aslında sayfalar/home.blade.php dosyasını çağıracak)
```PHP
Route::get('/home', function () {
	$isim = "Nuri Akman"
    return view('sayfalar.home', compact('isim'));
});


// Controller'in parametre ile çağrılması:
Route::get('/users/user/{id}', UserController@showUsers});

// Controller tanımı:
public function showUsers($id) {
    $user = User::findOrFail($id); // ilgili ID'ye sahip kaydı getir
    return view('home', compact('user')); // view'i çağır
}

```


# View
- Route'ın sonucunda bir sayfa gösterilecekse, bu sayfa view ile yapılır
- **resources/views** dizini altındadırlar
- **SAYFAADI.blade.php** olarak isimlendirilir
- Örnekteki "isim" adlı değişkeni gösterecek view dosyası içeriği:
```
Merhaba {{ $isim }}
```

# Blade
- Master Template
- Blade içinde değişken saha tanımlama: `@yield('BLOGICERIGI')`
- KULLANIMI:
```PHP
@extends("includes.master")
@section('BLOGICERIGI')
   // Bölüm içinde gösterilecekler burada
@endsection
```

# Controller

- DB'den veriyi çeker ve View içine yerleştirir
- `php artisan make:controller UserController`
- Yukarıdaki komut **app/http/Controller** dizini altına **UserController.php** dosyasını oluşturur
- **UserController.php** çağrılırken yazılacak route tanımı:
- `Route::get('/home', 'UserController@ShowUsers')`
- **UserController.php** örnek içeriği: (class içeriği)
```PHP
public function ShowUsers() {
	$sehir="Ankara";
    return view("sayfalar.kullanicilar", compact('sehir') );
}
```
Yukarıdaki kod, **resources/views/sayfalar/kullanicilar.blade.php** sayfasını cevap olarak döner

- User tablosunda kayıt arama:
```PHP
	$arrSonuc = User::where('yas', '27')->first();
    return view("sayfalar.kullanicilar", compact('arrSonuc') );
```

- Olmayan bir kayda erişim:
```PHP
    $arrSonuc = User::find(1); // ID'si 1 olan kaydı getirir
    $arrSonuc = User::findOrFail(1111); // 1111 ID'li kayıt yok. 404 hatası verecek
    return view("sayfalar.kullanicilar", compact('arrSonuc') );
```


## Controller içinden tablodan veri çekme
- Sayfa başına ekle: `use App\User`   (Veri çekeceğimiz tablo için)
- **UserController.php** örnek içeriği: (class içeriği)
```PHP
use App\User

public function ShowUsers() {
	$arrKullanicilar = User:get(); // Tüm tablo içeriğini getirir
    return view("sayfalar.kullanicilar", compact('arrKullanicilar') );
}
```

- Yukarıdaki kodun, Blade şablonu üzerinden listelenmesi için:
```PHP
@section('content')
	
	@foreach($arrKullanicilar as $Kullanici)

		{{ $kullanici->adi }}		{{ $kullanici->soyadi }}   <br>

	@endforeach

@endsection
```
