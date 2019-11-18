
# LARAVEL NOTLARI

## Faydalanılan Kaynaklar
- [Kısa Kaynak, 20 Video, Laravel for Beginners, By Mr Digital](https://www.youtube.com/watch?v=9ugzTTvNvkM&list=PLgFB6lmeXFOqRC4Sc-RST38jboldiQdds)
- [Uzun Kaynak, 43 Video, Laravel 6 tutorial, By php step by step](https://www.youtube.com/watch?v=ZE_IG7rF2a8&list=PL8p2I9GklV47fi-yiWkfRpbMV8PPaQDH4)
- [Laravel Best Practice (Türkçe)](https://github.com/alexeymezenin/laravel-best-practices/blob/master/turkish.md)


### Faydalı Kaynaklar
- [laravel-cheat-sheet](https://github.com/piotr-jura-udemy/laravel-cheat-sheet)
- [Laravel Cheatsheet](https://learninglaravel.net/cheatsheet/)
- [Laravel for Beginners 2019 by: Mr Digital](https://www.youtube.com/watch?v=9ugzTTvNvkM&list=PLgFB6lmeXFOqRC4Sc-RST38jboldiQdds)
- [Eloquent ORM](https://www.tutsmake.com/laravel-eloquent-cheat-sheet-eloquent-orm/)
- [Laravel Relationships](https://www.tutsmake.com/laravel-eloquent-relationships-inverse-relationships/)
- [Laravel 5 Cheat Sheet](https://learninglaravel.net/cheatsheet/)
- [Geçici eMail Hesabı Hizmeti](https://temp-mail.org/)
- [eMail Test Hizmeti](https://mailtrap.io/)


# KURULUM

### LAMP
```BASH
sudo apt install curl vim git php apache2 mysql-server mysql-client
sudo apt install php-pear php-fpm php-dev php-zip php-curl php-xmlrpc php-gd php-mysql php-mbstring php-xml libapache2-mod-php -y
```

### NodeJS
12.11.2019 tarihi itibariyle NodeJS v13 desteklenmediği için v10 yükleniyor.
```BASH
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
nodejs -v
npm -v
```

### COMPOSER
```BASH
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer
composer global require laravel/installer
```

### PATH Ayarları:
- `sudo vi /etc/environment` veya  `sudo vi ~/profile`
- Şöyle görünebilir:  **PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"**
- Bu PATH değişkeninin başına şu kod eklenir: `$HOME/.config/composer/vendor/bin:`
- PATH="**$HOME/.config/composer/vendor/bin:**/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
- Değişikliğin devreye girmesi için şu komut çalıştırılır: `source /etc/environment`

# LARAVEL

### Yeni bir Laravel Projesine Başlama
- `laravel new /var/www/html/blog`
- veya
- `composer create-project --prefer-dist laravel/laravel /var/www/html/blog`
- `npm install` ile npm paketlerinin yüklenmesi sağlanır

### Yeni Laravel Uygulamamızın ayarları
- **config/app.php** dosyası içinde timezone ve locale için ayarlar vardır.
- `.env` dosyası içinde şu sahalar doldurulur:
- `APP_NAME="Laravel Blog Uygulaması"`
- `APP_URL=http://localhost.development`   DİKKAT: Sonunda '/' karakteri YOK!
- `APP_DEBUG=true`

### APP KEY değiştirme/oluşturma
- KEY tanımı, `.env` adlı dosya içinde yer alır.
- `.env` adlı bir dosya yoksa `.env.example` adlı dosya kopyalanarak oluşturulur
- KEY oluşturma komutu: `php artisan key:generate`
- Eğer, key oluşturulmazsa uygulama güvenli olmaz.

### DB için ön hazırlık
- `vi app/Providers/AppServiceProvider.php`
- Dosya başına: `use Illuminate\Support\Facades\Schema;`
- `boot()` bölümüne: `Schema::defaultStringLength(191);`  Böylece "Specified key was too long" hatası engellenecektir

### PROJE DİZİNİMİZ:
- `/var/www/html/lara/blog`

### Proje için APACHE ayarı
- `vi /etc/apache2/sites-enabled/000-default.conf`
- **Şunu ekle:**
```
<VirtualHost *:80>
	ServerName laravel.development
	DocumentRoot /var/www/html/lara/blog/public
</VirtualHost>
```
**VEYA**
```
Yukarıdaki VirtualHost tanımını içeren yeni bir dosyayı aynı dizinde oluştur. 
Örnek: laravel-blog-deneme.conf içeriği:
<VirtualHost *:80>
	ServerName laravel.development
	DocumentRoot /var/www/html/lara/blog/public
</VirtualHost>
```
- Devreye girmesi için servisi tekrar başlat: `sudo service apache2 restart`
- Veya Devreye girmesi için ayarları tekrar okut: `sudo service apache2 reload`
- NOT: Windows'da bunun için [Host File Editor](https://hostsfileeditor.com/) adlı program kullanılabilir

### Gerekli dizinlere YAZMA yetkisi verilmesi
```BASH
cd /var/www/html/lara/
sudo chmod -R g+w storage/
sudo chmod -R g+w bootstrap/cache/
sudo chown -R $USER:www-data /var/www/html/lara/
```

### Proje için HOSTS ayarı
- `vi /etc/hosts`
- **Şunu ekle:**
- `127.0.0.1     laravel.development`

### Tarayıcıdan Test Edelim
- http://laravel.development

### Yerel laravel sunucusunu başlatma
- Proje dizininde olduğunuza emin olun! 
- Doğru dizinde değilseniz şöyle bir hata alabilirsiniz: **Could not open input file: artisan**
```BASH
cd /var/www/html/lara/blog
php artisan serve
http://127.0.0.1:8000
```

# VERİTABANI

### DB İçin Bağlantı Kurulması
- Adminer'e girilir ve `laravel_blog` adında yeni bir DB oluşturulur
- Proje dizinine geçilir
- `.env` dosyası içinde şu sahalar doldurulur:
- `DB_DATABASE=laravel_blog`
- `DB_USERNAME=root`
- `DB_PASSWORD=root`
- Bir controller oluşturulur: `php artisan make:controller UserController`
- ve dosya başına şu kod eklenir: `use Illuminate\Support\Facades\DB;`
- En iyi ayar için, Model adı SINGLE/TEKİL, tablo adı ve controller PLURIAL/ÇOĞUL olmalıdır


```
public function checkDb() {
	return DB::select("select * from users"); // Yöntem 1
	
	$user = DB::select("select * from users"); // Yöntem 2
	// print_r($user);
	return $user;   // cevabı jSON formatta döner
}
```

### SQLite kullanmak istenirse DB Ayarı
- BOŞ sqlite oluşturmak için: `touch database/myBlogs.sqlite`
- **config/database.php** içine şunlar yazılır:
- DB_CONNECTION=sqlite
- DB_DATABASE=/absolute/path/to/myBlogs.sqlite

# Migration
- Migration, veri tabanı için **Sürüm Yönetim Sistemi** olarak düşünülebilir
- Migration yardımı ile **Table** oluşturma: `php artisan make:migration --create=company`
- Böylece, **app/data/migration** dizini altına yeni bir dosya eklenir
- Oluşan dosya açılıp tablo sutunları tanımlaıp kaydedilir.
- `php artisan migrate` komutu çalıştırılarak tablonun oluşturulması sağlanır
- `php artisan migrate:reset` komutu, DB içindeki tüm table'ları siler!
- 

### Migration hazırlama
- Faydalı Kaynak: https://kodumunblogu.net/detail/laravelde-veritabani-iliskileri-eloquent-relationships-islemleri
- **ORM**: Object Relational Mapper ile DB işlemleri kolayca yapılır
- **MODEL**: DB içindeki TABLO olarak düşünülmelidir
- `User` modeli, veritabanındaki `users` adlı tabloyu işaret eder.
- `Post` modeli, veritabanındaki `posts` adlı tabloyu işaret eder.
- **MIGRATION**: Veritabanındaki Tablonun Yapısıdır (Table Structure)
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
- Tanımladığımız **users** tablosunu işleme alabilmek için:
- Proje dizinine geçilir ve `php artisan migrate` komutu uygulanır
- Bu işlemle birlikte daha önceki tablolar/veriler silinir!!!

Tabloda değişiklik olursa, 
- önce database/migrations dizinine gidilir
- `php artisan migrate:rollback` yapılır. DİKKAT! Tablodaki tüm data silinir!
- database/migrations/xxxxxx_xxx_create_users_table.php içinde değişiklikler yapılır.
- Proje dizinine geçilir ve `php artisan migrate` komutu uygulanır

# MODEL

### Projeye yeni bir Tablo (model) ekleme
- `php artisan make:model Post -mc` yapılır. Dikkat!!! Çoğul değil!! (s yok, Posts değil)
- Böylece hem Model, migration ve controller dosyaları oluşturulmuş olur. Örnek:
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

### Diğer tablolarla ilişkilendirme
- Bir tablonun başka bir tablo ile olan bağlantısı için **belongTo()** kullanılır.
- Örneğin **Posts** modeli içinde şu satır yazılarak, Posts tablosu ile Users tablosu bağlanmış olur:
```PHP
	public function user()
	{
		return $this->belongsTo('App\User');
	}
```
- Bu şekilde **belongsTo** ile bağlanan veriyi view içinde kullanırken: {{ $post->user->name }}

### Model Binding
- Route tanımı:
```PHP
Route::get('/users/show/{$id}', function () {
    return view('Users@index');
});
```
- Normal kullanım:
```PHP
public function showUsers($id) {
    $user = User::findOrFail($id); // ilgili ID'ye sahip kaydı getir
    return view('home', compact('user')); // view'i çağır
}
```
- Data binding yaparak kullanım:
```PHP
public function showUsers(User $id) {  // ÖNEMLİ! Böyle yapınca, User adlı model'deki $id'ya sahip kayıt (Yani Tablodaki PRIMARY KEY) OTOMATİK gelir. Buna Model Binding denir
    return view('home', compact('id'));
}
```
- `showUsers(User $id)` cağrısında, User adlı model'deki $id'ya sahip kayıt OTOMATİK gelir. Buna **Model Binding** denir

- slug'dan yola çıkarak ID'ye erişmek için
```PHP
    public function getRouteKeyNameForArticle()
    {
        return 'slug'; 
    }
```
- MODEL'de bu şekilde kullanım şu anlama gelir: 
- `Article::where('slug', $article)->first();`


# ROUTE

### Rooting tanımı sonrasında 404 hatası oluşursa:
- `sudo a2enmod rewrite` komutu çalıştırılır
- `vim  /etc/apache2/apache2.conf` komutu ile açılan dosyada **<Directory /var/www/>** bölümünde **AllowOverride None** ifadesi  **AllowOverride All**  olarak değiştirilir.
```
<Directory /var/www/>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```
- Değişikliklerin devreye girmesi için: `sudo service apache2 restart`

### Rooting normal çalışırsa:
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

// Route ile çağrının view ile cavaplanması
Route::view('users', 'users');


// Controller'in parametre ile çağrılması:
// DİKKAT: Route'da kullanılan isim ile Controller'daki isim AYNI OLMALI!
Route::get('/users/user/{id}', 'UserController@showUsers'});

// Controller'in BAZEN parametre ile çağrılması:
Route::get('/users/user/{id?}', function ($id=0) {
    return view('sayfalar.home', compact('id'));
});

// Route için isim verilmesi:
Route::get('/users/user/{id}', 'UserController@showUsers'})->name("KullaniciGoster");
// Bunu view içinde kullanmak için: <a href={{ route('KullaniciGoster') }}>GÖSTER</a>

// Bir yere yöneltilen akışın başka yere yönelndirilmesi
Route::redirect('/urunler', '/coksatanlar'});



// Controller tanımı:
public function showUsers($id) {
    $user = User::findOrFail($id); // ilgili ID'ye sahip kaydı getir
    return view('home', compact('user')); // view'i çağır
}

```


# VIEW

- Ekranda görüntülenen çıktı view ile sağlanır
- View isimlendirmesi KÜÇÜK HARF ile başlamalıdır.
- Route'ın sonucunda bir sayfa gösterilecekse, bu sayfa view ile yapılır
- **resources/views** dizini altındadırlar
- **SAYFAADI.blade.php** olarak isimlendirilir
- Örnekteki "isim" adlı değişkeni gösterecek view dosyası içeriği:
```
Merhaba {{ $isim }}    // escape ederek yazar
Merhaba {!! $isim !!}  // escape ETMEDEN yazar (XSS açığı oluşabilir)
```

# BLADE

- Master Template
- Blade içinde değişken saha tanımlama: `@yield('BLOGICERIGI')`
- KULLANIMI:
```PHP
@yield("content")   // Şablon dosyada "content" adlı bir alan tanımlar

@extends("includes.master")   // Site ana şablonu

@include("includes.navbar")   // Bağımsız dosyadan NAVBAR çektik

@if(Auth::check())
	// Kullanıcı LOGİN olmuşsa gösterilecek şeyler burada
@endif

@section('BLOGICERIGI')
   {{-- Bölüm içinde gösterilecekler buraya yazılır --}} 
@endsection
```
- Proje dizinine css ekleme ve kullanma:
- Kendi css ve js dosyalarımızı **public** klasörüne minified ederek atmak için:
- webpack.mix.js projede kullanılan css ve js dosyalarının minified edilmesini sağlar
- Kodu compile etmek için: `npm run dev`
- Kod değiştikçe compile etmek için: `npm run watch`
- View içinde kullanım örneği: `<link href="{{ asset('css/blog.css') }}" rel="stylesheet">`
- Blade içindeki JS'de değişkene JSON değer atamak için:
```PHP
<script>
    var DATA = @json($data);
</script>
```



# Projeye özel CSS ve JS dosyaları

- CSS, SCSS ve JS dosyaları **webpack.mix.js** içinde tanımlanır
- webpack.mix.js dosyası içinde kullanılacak dosyaların tanımlanması:
- `mix.js('resources/js/app.js', 'public/js')`
- `.sass('resources/sass/NURI.scss', 'public/css');`
- `.sass('resources/sass/app.scss', 'public/css');`  Kendi CSS dosyalarımızı böyle ekliyoruz
- View içinde kullanım örneği 1: `<link href="{{ asset('css/app.css') }}" rel="stylesheet">`   !! versiyonlama yoksa
- View içinde kullanım örneği 2: `<link href="{{ mix('css/app.css') }}" rel="stylesheet">`   !! versiyonlama varsa
- `nmp run watch` yapılınca mix.js'de belirtilen dosyalar değişikliğe karşı izlemeye alınır, değişiklik olunca otomatik derlenir.
- `nmp run dev` yapılınca mix.js'de belirtilen dosyalar development ortamı için derlenir
- `nmp run prog` yapılınca mix.js'de belirtilen dosyalar production ortamı için minified edilerek derlenir
- css ve js dosyalar için VERSION eklemek için webpack.mix.js dosyası tanımı:
- `mix.js('resources/js/app.js', 'public/js').version()`
- `.sass('resources/sass/NURI.scss', 'public/css').version();`
- `.sass('resources/sass/app.scss', 'public/css').version();`
- Böylece, üretilen dosya çağrılırken xxx.css?123123132123 gibi bir şekilde çağrılır cache'lenmesinin önüne geçilir.





# CONTROLLER

- DB'den veriyi çeker ve View içine yerleştirir
- Controller adı BÜYÜK HARFLER başlar
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

- Controller'den View'a SONUÇ bildirerek çağrı yapma: `return redirect()->back()->with('success', 'Kayıt başarılı');`
- View'den, bu çağrının karşılanması: 
```PHP
	@if(session()->has('success'))
		{{ session()->get('success') }} // Session değişkeni kontrolü
	@endif
```
- Yukarıdaki kod bağımsiz bir view olarak sisteme kaydedilerek BAŞARILI mesajı verilecek her yerde `@include('validation')` edilerek kullanılabilir.

**app/http/request** klasörüne bu request için controller oluşturma: `php artisan make:request CreateUser` Böylece, form gönderme sonrasındaki her kontrol bağımsız olarak bu dosyada yapılabilir. Bu dosyanın **Rules** başlığı kuralları tanımlar, **authorize** bölümü ise kurallar sağlandığında nasıl bir cevap döneceğini tanımlar

## API Yazma
- Controller, cevabını API için kullanmak istersek jSON formatında dönebiliriz:
```PHP
public function getUserInfo($id) {
	
    return ["id"=>$id, "fname"=>"Nuri", "lname"=>"Akman"]; // Sonuç jSON formatındadır.
}
```


## REQUEST
- Controller içinden REQUEST detaylarına şu şekilde erişiriz:
```PHP
public function test(Request $request) 
{
	dd($request->input());      // Tüm GET veya POST??? parametrelerini listeler
	print_r($request->input());      // Tüm GET veya POST??? parametrelerini listeler
	dd($request->input('id'));  // URL'deki id adlı sahanın değerini alır
	dd($request->path());  // URL'deki yol değerini gösterir
	dd($request->url());  // URL'in tam değerini gösterir
	dd($request->method());  // Yönetimi gösteri: POST, GET; vb.vb.
	if( $request->isMethod('GET') ) echo "GET ile çağrıldı";
	dd($request->query());  // URL'deki parametreleri gösterir
}
```


### Controller içinden tablodan veri çekme
- Sayfa başına ekle: `use App\User`   (Veri çekeceğimiz tablo için)
- **UserController.php** örnek içeriği: (class içeriği)
```PHP
use App\User  // Dosyanın başına bu satır yazılır !!!
public function ShowUsers() {
	$arrKullanicilar = User::get(); // Tüm tablo içeriğini getirir // use App\User yazdığımız için böyle kullandık
	// VEYA: $arrKullanicilar = \App\User::get(); // Tüm tablo içeriğini getirir

	$arrKullanicilar = User::get(); // Tüm tablo içeriğini getirir
    $arrKullanicilar = User::latest()->get(); // Son kayıttan başlayarak getirir // Order by updated_by DESC
    $arrKullanicilar = User::latest('published_at')->get(); // Son kayıttan başlayarak tüm satırları getirir // Order by published_ad DESC
    $arrKullanicilar = User::take(5)->latest('published_at')->get(); // Son kayıttan başlayarak son 5 satırı getirir // Order by published_ad DESC
	$arrKullanicilar = User::all(); // Tüm tablo içeriğini getirir
    $arrKullanicilar = User::take(3); // Sadece 3 kayıt getirir
	$arrKullanicilar = User::where('adi', 'nuri')->get(); // where adi = 'nuri'
	$arrKullanicilar = User::paginate(20); // Her ekranda 20 kayıt olacak biçimde getir
    return view("sayfalar.kullanicilar", compact('arrKullanicilar') );
}
```

- Yukarıdaki kodun, Blade şablonu üzerinden listelenmesi için:
```PHP
@section('content')
	
	@foreach($arrKullanicilar as $Kullanici)

		{{ $kullanici->adi }}     {{ $kullanici->soyadi }}

		<a href="{{ route('edituser', $kullanici->adi) }}">Edit User</a> <br>

	@endforeach

	{{ $arrKullanicilar->links() }}  // Paginating yapıldığında, diğer sayfaların erişim linklerini göstermesi için
	{{ $arrKullanicilar->links('vendor.paginations.bootstrap-4') }}  // Paginating için kullanılacak view'i belirledik

@endsection
```
- Yukarıdaki Bootstrap pagination view'lerini oluşturabilmek için:
- `php artisan vendor:publish --tag=laravel-pagination`


# Pagination
- Verinin ekrana sayfa sayfa listelenmesini sağlar
- 


# FACTORY
Veritabanına dummy kayıt ekleme işini yapar.

### Dummy kayıt oluşturma
- Önce sınıf oluşturalım: `php artisan make:factory PostFactory`
- **database/factories/PostFactory.php** oluştu.
- Kullanılabilecek bazı faker'ler:
```
    'name' => $faker->name,
    'email' => $faker->unique()->safeEmail,
    'email_verified_at' => now(),
    'password' => '$2y$10$92IXUNpkjO0rOQ5byMi.Ye4oKoEa3Ro9llC/.og/at2.uheWG/igi', // password
    'remember_token' => Str::random(10),
    'title' => $faker->sentence,
    'body' => $faker->paragraph,
```
- `php artisan tinker`
- `factory('App\User', 10)->create();`
- 10 set USER oluşturmak için:
- `$users = factory('App\User', 10)->create();`
- Her kullanıcı için 10 adet de POST oluşturmak için:
- `$users->each(function ($user){ factory('App\Post', 10)->create([user_id]=>$user->id)};);`






# Model'de ilave kontroller
- protected guarded = ['id']; // Bu saha update edilemez
- protected fillable = ['title', 'body']; // Sadece bu sahalar update edilebilir.
- **419 - Page Expired** hatasını önlemek için FORM etiketi içine içine şu yazılır: `@csrf`
- Insert İçin 
- Update İçin 
- Delete İçin 

# FORM Validation
```PHP
	$request()->validate([
		'fname' >= 'required|min:3|max:50',
		'lname' >= 'required',
		'email' >= 'required|email',
	// veya:
	//	'email' >= 'required|email|unique:user,email,' . request()->id ,
	]);

	// Bu satıra gelmesi, validation'ı geçtiği anlamına gelir
	$user = new User;
	$user->fname = $request->fname;
	$user->lname = $request->lname;
	$user->email = $request->email;
	$user->save();
	return redirect()->back()->with('success', 'Kayıt başarılı');


    // Yukardaki kod bloğunun alternatif kullanımı:
    User::create([
        'fname' = $request->fname,
        'lname' = $request->lname,
        'email' = $request->email ]);


```

# Validations on View
```PHP
	@if(session()->has('success'))
		{{ session()->get('success') }} // Session değişkeni kontrolü
	@endif

	@if(session()->has('danger'))
		{{ session()->get('danger') }} // Session değişkeni kontrolü
	@endif

	@if($errors()->any())
		// $request->validate() tarafında oluşan hatalar burada yakalanır...
		<ul>
		@foreach($errors()->all() as $error)
			<li>{{ $error }}</li> // Tüm hataları ekrana listeler
		@endforeach
		</ul>
	@endif


	<form method="post" action="{{ route('createUser') }}">
		<input type="text" name="fname" value="{{old('fname')}}" required> // Hata sonrası forma eski değerleri doldurabilmek için
		@error('fname')
		<div>{{ $message }}</div>  // Bu bölüm sadece "fname" sahası için bir hata oluşursa gösterecektir
		@enderror

		<input type="text" name="lname" class="{{ $error->has('lname') ? 'has-error' : '' }}" value="{{old('lname')}}" required> // YÖNTEM 1: Hata sonrası forma eski değerleri doldurabilmek için
        <input type="text" name="lname" class="{{ @error('lname') has-error @enderror" value="{{old('lname')}}" required> // YÖNTEM 2: Hata sonrası forma eski değerleri doldurabilmek için
		@if($error->has('lname'))
		<div>{{ $message }}</div>  // Bu bölüm sadece "lname" sahası için bir hata oluşursa gösterecektir
		@endif

		<input type="text" name="email" value="{{old('email')}}" required> // Hata sonrası forma eski değerleri doldurabilmek için
		@error('email')
		<div>{{ $message }}</div>  // Bu bölüm sadece "email" sahası için bir hata oluşursa gösterecektir
		@enderror
	</form>

```



# HELPERS

- auth()
- request()

# Password HASH

- passwords Sahaları HASH'lenerek veritabanında saklanmalı. Bunun için:
- İlgili model'in controller dosyasının başına: `use Illuminate\Support\Facades\Hash;`
- Kullanımı:
```PHP
use Illuminate\Support\Facades\Hash; // Dosyanın başına yaz!

	$user = new User;
	$user->fname = $request->fname;
	$user->lname = $request->lname;
	$user->password = Hash::make($request->password);
	$user->save();

```

# Authentication, Login, Logout

## AuthController İçin Yapılacaklar
- `php artisan make:controller AuthController` ile controller dosyası oluşturulur
- Controller içindeki **login** ve **signin** için tanımlama yapılır:
```PHP
use Illuminate\Support\Facades\Auth; // Dosyanın başına yaz!

public function login() 
{
	return view('login');  // login formunu gösterelim
}

public function logout() 
{
	Auth::logout()
	return redirect()->back();  // login formunu gösterelim
}

public function signin(Request $request) { 
	$request->validate([
		'email'    => 'required',
		'password' => 'required',
	]);

	if( Auth::attempt([
		'email'    => $request->email,
		'password' => $request->password,
		]) 
	  ) 
	{
		return redirect()->intended( route("userWelcome") ); // intended, Login sayfasına nereden yöneltildiyse o sayfaya gitmesini sağlar!!!
	}
	return redirect()->back().with("danger", "Login is incorrect");

}
```

## Sayfaların korumaya alınması
- İlgili Controller dosyaları açılır. Örnek: **UserController.php**
- Class tanımına şu constructor eklenir:
```PHP
public function __construct()
{
	$this->middleware('auth'); // __construct tanımında bu kod olan sayfalara giriş yapılması şartı aranır
}
```

## Hazırlanacak view'ler
- Login Sayfası: **login** Login için nereye gidileceği **app/Http/Middleware/Authenticate.php** dosyasına belirtilir.
- Login sonrası karşılama Sayfası: **userWelcome**

## Hazırlanacak route'lar
```PHP
	Route::get('/login', 'AuthController@login'})->name('login')->middleware('guest');
	Route::get('/signin', 'AuthController@signin'})->name('signin');
	Route::post('/logout', 'AuthController@logout'})->name('logout');
```

## Login View Dosyası
- Yetkilendirmesi olmayan (giriş yapmamış) kullanıcıyı "login" sayfasına göndermemiz lazım.
- Bunu için "/login" için bir **route** tanımı yapılır:
- `Route::get('/login', 'AuthController@login'})->name('login');` // KORUMA YOK
- **login.blade.php** dosyası içinde bir login formu hazırlanır
- Form'un tanımı: `action="{{ route('signin') }}" method="post" `
- **Ayrıca,**
- Giriş yapmış bir kişi tekrar login sayfasına girmesin. Bunun yerine kişiyi bir yere gönderelim. 
- Bunun için **app/Http/Middleware/RedirectIfAuthenticated.php** dosyası açılır:
- `return redirect('/userWelcome');` Oturum açtıysa ve login sayfasına tekrar gidiyorsa gitmesin! buraya yönlendirelim


## @csrf
- Cross Site Request Forgery anlamına gelir.
- Form içine özel bir kod ekler.
- Böylece, bizim sayfalarımızın başka bir uygulama içinden çağrılıp, doldurulup, gönderilmesi engellenmiş olur.

## Logout için blade
```
@if(Auth::check())
	<form method="post" action="{{ route('logout') }}">
		@csrf
		<button type="submit">Logout</button>
	</form>
@endif
```


# MIDDLEWARE

- Http requestinin Controller'a gitmesi öncesinde bir **filtre** gibi çalışır.
- `php artisan make:middleware AgeChecker` diyerek oluşturulur
- 3 Tür Middleware vardır:
  - Global Middleware
  - Grouped Middleware
  - Routed Middleware
- Global Middleware **/app/Http/Kernel.php** içinde **$middleware[]** altında tanımlanır
- Grouped Middleware **/app/Http/Kernel.php** içinde **$middlewareGroups[]** altında tanımlanır
- Root tanımı yaparken hangi Middleware'e tabi tutulacağı seçilebilir.
- Örnek: `Route::get('/login', 'AuthController@login'})->name('login')->middleware('guest');`


# SESSION

- $request->session()->set( 'info', $request->input() );
- $DEGISKEN = $request->session()->get( 'info' );
- **FLASH SESSION**, Sadece 1 defa kullanılabilen session değişkenidir
- Flash Session Doldurma: `$request->session()->flash('data', 'mesaj/veri');`
- Flash Session Kullanma: `{{ Session::get('data') }}`


# ÖNEMLİ!

- .env dosyasında değişiklik olursa: Varsa, "php artisan serve" kapatılıp tekrar başlatılmalıdır


# Query Builder
 
```PHP
	use Illuminate\Support\Facades\DB;   // !!! Dosyanın başına yaz!

	$users = DB::table("user")->get();
	print_r($users);


	$users = DB::table("user")
				->where("name", "NURİ")
				->get(); // name="NURİ" olan kayıtları getirir

	$users = DB::table("user")->find(3); // id'si 3 olan kayıt
	
	$users = DB::table("user")->count(); // Kayıt sayısı alma
	$users = DB::table("user")->avg('id'); // "id" adlı sayısal alandaki verilerin ortalamasını verir
	$users = DB::table("user")->sum('id'); // "id" adlı sayısal alandaki verilerin toplamını verir
	

	$users = DB::table("user")
				->where("name", "NURİ")
				->delete(); // Kayıt silme
				->delete(4); // 4 Nolu ID'ye sahip satırı siler

	$users = DB::table("user")
				->insert([
					"fname" => "NURİ",
					"lname" => "AKMAN",
					"email" => "nuri@gmail.com"
				]); // Kayıt ekleme

	$users = DB::table("user")
				->where("name" => "NURİ")
				->update([
					"fname" => "nuri",
					"lname" => "akman"
				]]); // Kayıt güncelleme


```

# Query Builder - Joins

```PHP
	use Illuminate\Support\Facades\DB;   // !!! Dosyanın başına yaz!
/*
	company tablosu:
	id
	name
	user_id

	user tablosu:
	id
	fname
	lname
*/

	$users = DB::table("user")
	->join("company", "user.id", "company/user_id")
	->select("company.name", "user.fname")
	->where("user.fname", "nuri")
	->get();
	print_r($users);
	
	$users = DB::table("user")
	->join("company", "user.id", "company/user_id")
	->select("company.name", "user.fname")
	->where("company.name", "samsung")
	->get();
	print_r($users);


    $users = DB::table("user")
    ->select("fname", "lname")
    ->toSql();  // SQL OLARAK NE YADIĞIMIZI GÖREBİLİRİZ
	
```


# Localization (Multi-language)

- /config/app/ içindeki lang="en" satırı ile dil tercihi yapılır
- Dil seçimi için Route tanımlanır:
```PHP
Route::get('/{lang}', function () {
	App::setlocale($lang)
    return view('welcome');
});
```
- /resource/lang dizini altında ilgili dil için klasör açılır: tr,en,hi gibi
- /resource/lang/tr dizini içinde bir dosya tanımlanır. Örnek: msg.php
/resource/lang/tr/msg.php içeriği
```PHP
	'Welcome'=>'Hoşgeldiniz',
	'Docs' => 'Blgeler',
	'Help'=> 'Yardım'
```
- view içinden bunu kullanabilmek için: `{{  __('msg.Welcome') }}`   msg: dosya adı, Welcome: Kelime


# File Upload

- `<form>` etiketi içnde şuna yer verilmeli: `enctype='multipart/form-dataupload'`
- `$request->file('profil_resmi')->store('upload');`
- Yukarıdaki komut yüklenen **dosyanın /storage/app/upload** dizinine kaydedilmesini sağlar



# Eloquent

- Eloquent sayesinde veritabanı katmanı xxx edilmiş olur. 
- Eloquent sayesinde kod yazarken doğrudan SQL kullanılmaz. 
- Tanım bölümünde DB olarak her ne tanımlanmış olursa olsun (mysql, sqlite, oracle, vb) sorgularımız özel bir ayar gerekmeksizin çalışır
- Adım 1: `/.env` dosyası içinden DB bağlantısına ilişkin tanım yapılır
- `DB_DATABASE=laravel_blog`
- `DB_USERNAME=root`
- `DB_PASSWORD=root`
- Adım 2: `php artisan make:model User` komutu ile bir **Model** dosyası oluşturulur
- Oluşan dosyanın yeri: **/app** dizini altındadır
- En iyi ayar için, Model adı SINGLE/TEKİL, tablo adı ve controller PLURIAL/ÇOĞUL olmalıdır
- Adım 3: `php artisan make:controller Users` komutu ile bir **Model** dosyası oluşturulur
- Oluşan dosyanın yeri: **/app/http/Controllers** dizini altındadır
- **/app/http/Controllers/Users.php** içine şu yazılır:
```PHP
use App\User  // Dosyanın başına bu satır yazılır !!! 
// Bu işlem, Model'i bğlamaktır. Model adı bağlantısı yapılmadan bu modele (Tablo) erişilemez.
function index() {
	return User::all();
	$userInfo = User::
				where('yas', '>', '20')
				->orderBy('sehir', 'desc')
				->take(3)  // Limit
				->get();

	$userInfo = User::find(2);
	return view('db', compact('userInfo'))
}
```
- Adım 4: Route tanımı yapılması
```PHP
Route::get('/db', function () {
    return view('Users@index');
});
```
- Adım 5: View tanımı yapılması
```PHP
	@foreach( userInfo as user)
	{{ user->fname }}, {{ user->lname }}
```
- Adım 6: Eğer, Form verisi INSERT edilecekse:
```PHP
	$user = new User;
	$user->fname = $request->fname;
	$user->lname = $request->lname;
	$user->email = $request->email;
	$Sonuc = $user->save(); //$Sonuc == 1: Başarılı!
	return redirect()->back()->with('success', 'Kayıt başarılı');
```
- Adım 7: Eğer, Form verisi UPDATE edilecekse:
```PHP
	$users = DB::table("user")
				->where("name" => "NURİ")
				->update([
					"fname" => "nuri",
					"lname" => "akman"
				]]); // Kayıt güncelleme

```
- Elequent ile başka bir tablodan veri çekmek için:
- `protected $table='company';`
- Elequent ile tabloda timestamp kullanılmayacağını belirtmek için:
- `protected $timestamps=false;`


# DOMpdf Kullanma
- Proje Sitesi: https://github.com/barryvdh/laravel-dompdf
- Kütüphane, projeye eklenir: `composer require barryvdh/laravel-dompdf`
- Yöntem 1: SERVICE olarak kullanmak için
- **/config/add.php** dosyası içinde **services** başlığının sonuna eklenir:
- `Barryvdh\DomPDF\ServiceProvider::class,`
- Kullanmak İçin Örnek-1:
```PHP
$pdf = App::make('dompdf.wrapper');
$pdf->loadHTML('<h1>Test</h1>');
return $pdf->stream(); // Tarayıcıda PDF açar
```
- Kullanmak İçin Örnek-2:
```PHP
$HTML = '<h1>Test</h1>';
$pdf = App::make('dompdf.wrapper');
$pdf->loadHTML($HTML)->setPaper('a4', 'landscape')->setWarnings(false)->save('myfile.pdf')
return $pdf->download('invoice.pdf'); // Tarayıcıda Donwnload ister
```
- Yöntem 2: ALIAS olarak kullanmak için
- **/config/add.php** dosyası içinde **aliases** başlığının sonuna eklenir:
- `'PDF' => Barryvdh\DomPDF\Facade::class,`
- Kullanmak İçin Örnek:
```PHP
$pdf = PDF::loadView('pdf.invoice', $data);
return $pdf->download('invoice.pdf');
```


# Custom 404 Page

- `404.blade.php` gibi bir dosya hazırlanır.
- `app\Exceptions\Handler.php` içindeki `render()` bloğu içine şu yazılır:
```PHP
	if($this->isHttpException($e)) {
		$code = $e->getStatusCode();
		if($code == 404) return response()->view('404');
	}
```


# gMail üzerinden Mail Gönderme

- .env doyasında ve gmai hesabında yapılacak değişiklikler sonrasında çalışır
- gMail'de **Accounts/Security** sayfasında **Less secure app access** ON yapılır
- `/.env` dosyasındaki ayarlar:
```
MAIL_DRIVER=smtp
MAIL_HOST=smtp.gmail.com
MAIL_PORT=465
MAIL_USERNAME=nuriakman@gmail.com
MAIL_PASSWORD=GMAIL_PAROLAM_BURAYA
MAIL_ENCRYPTION=ssl
```
- Mail Gönderme Örneği
```PHP
	$to_name="Hasan Çiçek"
	$to_email="hasancicek@gmail.com"
	$data=array(
		"name"=>"Nuri Akman",
		"body"=>"Merhaba !!!"
	);
	Mail::send('MAIL_FORMATI_VIEW_ADI', $data, function($message) use ($to_name, $to_email) {
		$message->to($to_email)
		->subject('Laravelden mesaj gönderme örneği');
	});
```
- **MAIL_FORMATI_VIEW_ADI** adlı View içeriği:
```
	<h1>Merhaba {{ $name }}</h1>
	{{ $body }}
```





https://www.udemy.com/course/laravel-6-framework/learn/lecture/16352616#overview

Windows xampp için not: Bölüm 2/8'de açıklandı:

copy server.php index.php
copy public/.htaccess ./

Böylece,  artık PUBLIC dizinine girmeye gerek kalmadan çalışacak







# Regex Tablosu

Başlık|Kodu
---|---
github|a_!
gmail|a_!+
firefox|a_+x4
isimtescil|q_q
ssh|a_!+g
bsm78na|a_+x3.
hm|dr_+x3.
fb|_+x5
twi|q_tw
instgr|q_ig
udem|q_u
stackoverflow|????
iaydin|5369


```HTML

<input list="people-I-love" placeholder="Start typing...">
<datalist id="people-I-love">
  <option value="My wife">
  <option value="Carsten">
  <option value="@imogenf">
  <option value="@stereobooster">
  <option value="@fabien0102">
  <option value="@mpotNotlara devamomin">
  <option value="@mweststrate">
  <option value="@cpojer">
  <option value="@dabit3">
  <option value="@_darkfadr">
</datalist>

```


- Bir çok şey olur: `php artisan make:controller UserController --resource` 
- `php artisan make:model Students -m`
- `php artisan migrate`




# Update Form

- Form Action şöyle olmalı: ` action='{{ route('update', $Student->id) }}' `
- Veya şöyle olmalı: ` action='/update/{{ $Student->id }}' `

# Form Submit (DELETE methodu ile)

- Form Action: `action='POST'`
- Form'un içinde : 
```
{{ @scrf_field }}
{{ method_field('delete') }}
``
**VEYA:**
```
@scrf
@method('DELETE')
```



# Authentication
```PHP
composer require laravel/ui --dev
php artisan ui vue --auth
php artisan migrate
use Illuminate\Support\Facades\Hash; // Dosyanın başına yaz!
use Illuminate\Support\Facades\Auth; // Dosyanın başına yaz!
Auth::id                                  // Oturumu açmış olan kullanıcının ID'di
Auth::user()->password                    // Oturumu açmış olan kullanıcının parolası (Hashli)
Auth::logout()                            // Oturum Kapat
Hash::make($request->password)            // Hash'li hale getirme
Hash::check($girilenParola, $eskiParola)  // Parola karşılaştırması

public function __construct()
{
    $this->middleware('auth'); // __construct tanımında bu kod olan sayfalara giriş yapılması şartı aranır
}

```
- HOME sayfasına akışı yönlendirme: `return redirect()->route('home')->with('success', 'Kayıt başarılı');`
- Son gelinen sayfaya akışı yönlendirme: `return redirect()->back()->with('success', 'Kayıt başarılı');`










# LaraCast

- $var1 => $request["id"] ?? 0;  // "id" YOKSA "0" döner
- abort('404', 'Mesaj'); // 404 vererek program akışını keser

**MODEL OLUŞTURULMAMIŞSA:**
```
// '.\DB' yazarak kullanmak yerine 'DB' yazıp kullanmak için
// dosya başın 'use DB;' yazılır

$post = .\DB::table('posts')->where('slug', $slug)->first();

if( !$post ) {
    abort('404')
}

return view('post'), [
    'post' =>$post
])
```

**MODEL OLUŞTURULMUŞSA (1):**
```
// dosya başın 'use DB;' yazılır
// dosya başın 'use App\Post;' yazılır

$post = Post::where('slug', $slug)->firstOrFail();

return view('post'), [
    'post' =>$post
])
```

**MODEL OLUŞTURULMUŞSA (2):**
```
// dosya başın 'use DB;' yazılır
// dosya başın 'use App\Post;' yazılır

return view('post',[
    'post' => Post::where('slug', $slug)->firstOrFail();
])
```

Yukarıdaki 3 kod bloğu da AYNI işi yapar.



# Migration

- migration dosyası oluşturma:
php artisan make:migration create_posts_table

- migration'ı çalıştırma
php artisan migrate

- Tabloya TITLE sutununu ekleme:
php artisan make:migration add_title_to_posts_table
$table->string('title');

- Tablodan TITLE sutununu silme:
down() bölümüne:
$table->dropColumn('title');

- migration'ı çalıştırma
php artisan migrate

- Son migration'ı geri alma
php artisan migrate:rollback

- Tüm tabloları SİLİP, sıfırdan migration yapma
php artisan migrate:fresh

- Oluşturma sırası: migration, controller, model
- model'i, migration'u (-m) ve controller'ı (-c) oluştur: ÖNEMLİ KOLAYLIK
php artisan make:model Project -mc


# tinker
- Php - Laravel Playground: Komut satırından kod yazıp test etme imkanı sunar.
php artisan tinker
App\ModelAdı::all();  // Tablodaki tüm satırları gösterir


# Site Templati Ayarlama
KAYNAK: https://laracasts.com/series/laravel-6-from-scratch/episodes/15?autoplay=true
Şablon dosyası: https://templated.co/simplework
**index.htm** dışındaki tüm dosyaları **public** dizinine kopyala
layout.blade.php dosyasını oluştur.
**index.htm** içeriğini bu dosyaya yapıştır
```
{{ Request::path() == '/' ? 'menu-active' : '' }}
{{ Request::path() == 'about' ? 'menu-active' : '' }}
**veya**
{{ Request::path('/') ? 'menu-active' : '' }}
{{ Request::is('about') ? 'menu-active' : '' }}
{{ Request::is('urun*') ? 'menu-active' : '' }} // URL' urun* ile devam ediyor
```

# 7 Controller Actions

- index : Listeleme işlemi
- show : Sadece 1 kayıt gösterir
- create : Yeni bir kayıt için gerekli olan View'i hazırlar
- store : Submit edilen create formundan gelen veriyi kaydetme işini yapar
- edit : Edit edilecek kaydı gösteren View'i hazırlar
- update : Submit edilen edit formundan gelen veriyi güncelleme işini yapar
- destroy : Kaydın silinmesi işini yapar




- index : Render of a list of resource
- show : Show a single resource
- create : Show a view to create a noe resource
- store : Persist the new resource
- edit : Show a view to edit existing resource
- update : Persist the edited resource
- destroy : Delete the resource


Yukarıdaki 7 fonksiyonu da hazır olarak sunan komut:
`php artisan make:controller ProjectsController -r -m Project`



# Restful Routing
Metodlar: GET, POST, PUT, DELETE, PATCH
PUT ve PATCH genelde aynı amaç için kullanılırlar
```
GET    /articles
GET    /articles/:id
POST   /articles
PUT    /articles/:id
DELETE /articles/:id

GET    /videos
GET    /videos/create
POST   /videos
GET    /videos/2
GET    /videos/2/edit
PUT    /videos/2
DELETE /videos/2
```


# Reduce Duplication
- KAYNAK: https://laracasts.com/series/laravel-6-from-scratch/episodes/27?autoplay=true
- Controller içinde "validate" için bi metod oluşturulur,
- Bu medod lazım olan yerde kullanılır


# Consider Named Routes
KAYNAK: https://laracasts.com/series/laravel-6-from-scratch/episodes/28?autoplay=true
- Model içinde şu kod yazılır:
```
public function path() {
    return route('articles.show'), $this)
}
```
- Böylece, mesela Update sonrası gidilecek sayfa şu şekilde tanımlanır:
```
    return redirect( $article->path() );
```


TEMPLATE Örnekleri: https://templated.co/simplework




# Database Integrity
- Foreign Key İle tabloları bağlama ve CASCADE etme işlemidir

- **Articles** Modeli tanımı içinde şu yapılır:
```
	$table->bigIncrements('id');
	$table->string('title');
	$table->string('ozet');
	$table->string('body');
	$table->integer('user_id');  // Bu article hangi User tarafından oluşturulmuş takibi için
	$table->string('password');

	$table->foreign('user_id') // Article tablosundaki saha adı
		->references('id')     // Users tablosunda karşılık geldiği saha id
		->on('users')          // karşılık geldiği tablo adı
		-onDelete('cascade');  // Eğer, User tablosunda silme yapılırsa, karşılık gelen Article'lar otomatik silinir!

```



# Elequent Relationships

- Article Model'i içinde:
- `public function user(){ return $this->belongsTo(User::class); }`
- Yukarıda, fonksiyon adı "user()" olarak verildiği için "user" tablosuna "user_id" field'ı ile bağlanacağını varsayar
- `public function author(){ return $this->belongsTo(User::class, 'user_id'); }`
- Yukarıda, fonksiyon adı "author()" olarak verildiği için, 
- normalde "user" tablosuna "author_id" field'ı ile bağlanacağını varsayar.
- Bunu değiştirmek için 'user_id' parametresi eklenir


- User Model'i içinde:
- `public function articles(){ return $this->hasMany(Articles::class); }`
- Şöyle anlamlandırılabilir: `select * from articles where user_id = ?`
- `public function articles(){ return $this->hasMany(Projects::class); }`
- Şöyle anlamlandırılabilir: `select * from Projects where user_id = ?`

- Project Model'i içinde:
- `public function articles(){ return $this->belongsTo(User::class); }`
- Şöyle anlamlandırılabilir: `select * from user where project_id = ?`


$user = User::find(1); // select * from user where user_id = 1
$user->projects;       // select * from projects where user_id = $user->id
$user->projects->first();
KAYNAK: https://laracasts.com/series/laravel-6-from-scratch/episodes/30?autoplay=true

\App\Article::find(1);       // 1 nolu Article'ı bul
\App\Article::find(1)->user; // 1 Nolu article'ın User'ının tüm bilgilerini gösterir
\App\Article::find(1)->user->name; // 1 Nolu article'ın User'ının ADINI gösterir


hasMany  <==> belongsTo

hasOne    Sadece 1 tane   hasOne Profile

hasMany   Çok tane        hasMany Articles

belongsTo Diğerine aittir. Articles, User'a aittir.

manyToMany   


- Model tanımında, "primary key" sahası `$table->bigIncrements('id')` olarak tanımlanır
- Başka bir tablo ile ilişkilendirilecek saha (foreign key) `$table->unsignedBigInteger('user_id')` olarak tanımlanır


```
php artisan migrate:fresh // Veriyi  siler, modeli yeniden oluşturur
php artisan tinker
factory(App\User::class)->create(); // User tablosuna DUMY 1 adet kayıt ekler
factory(App\User::class, 25)->create(); // User tablosuna DUMY 25 adet kayıt ekler
```

Articles için Factory tanımlama (Böylece, articles tablosu için DUMY data üretimi yapacağız)
```
php artisan help make:factory   // Komut hakkında açıklama verir


php artisan make:factory ArticleFactory -m "App\Article" // App\Article modeli için ArticleFactory adında sınıf oluşturur

'user_id'=> $faker->factory(\App\User::class),  // Bu satır için "UserFactory" olması gerekir. Veriyi oradan çekecek.
'title'  => $faker->sentence,
'ozet'   => $faker->paragraph,
'makale' => $faker->paragraph(5),


php artisan make:factory UserFactory -m "App\User" // App\User modeli için UserFactory adında sınıf oluşturur

'name' => $faker->name,
'email' => $faker->unique()->safeEmail,
'email_verified_at' => now(),
'remember_token' >= Str:random(10),


factory(App\Article::class, 5)->create();  // 5 adet Article ve 5 adet de User oluşturur. Yeni oluşturulan User'ın ID'si Yeni oluştutulan Article'da kullanılır. O Article, O User'a ait olur yani.

factory(App\Article::class, 5)->create(['user_id' => 21]);  // 5 adet Article oluşturur, hepsinin de User_id'si 21 nolu kullanıcıdır
factory(App\Article::class, 3)->create(['user_id' => 18]);  // 3 adet Article oluşturur, hepsinin de User_id'si 18 nolu kullanıcıdır
```




















