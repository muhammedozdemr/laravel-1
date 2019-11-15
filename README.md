
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
- `sudo vi /etc/environment`
- Şöyle görünebilir:  **PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"**
- Bu PATH değişkeninin başına şu kod eklenir: `$HOME/.config/composer/vendor/bin:`
- PATH="**$HOME/.config/composer/vendor/bin:**/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"
- Değişikliğin devreye girmesi için şu komut çalıştırılır: `source /etc/environment`

# LARAVEL

### Yeni bir Laravel Projesine Başlama
- `laravel new /var/www/html/blog`
- veya
- `composer create-project --prefer-dist laravel/laravel /var/www/html/blog`

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
- Dosya başına: `use Illuminate\Support\Facades\Scheme;`
- `boot()` bölümüne: `Schema::defaultStringLength(191);`  Böylece "Specified key was too long" hatası engellenecektir

### PROJE DİZİNİMİZ:
- `/var/www/html/lara/blog`

### Proje için APACHE ayarı
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

### Gerekli dizinlere YAZMA yetkisi verilmesi
```BASH
cd /var/www/html/lara/
chmod -R g+w storage/
chmod -R g+w bootstrap/cache/
chown -R $USER:www-data /var/www/html/lara/
```

### Proje için HOSTS ayarı
- `vi /etc/hosts`
- **Şunu ekle:**
- `127.0.0.1     laravel.development`

### Tarayıcıdan Test Edelim
- http://laravel.development

### Yerel laravel Sunucusunu Başlatma
Proje dizininde olduğunuza emin olun!
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
- Bir controller oluşturulur ve dosya başına şu kod eklenir: `use Illuminate\Support\Facades\DB;`
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
Diğer dosyalar siliniyor
- Tanımladığımız **users** tablosunu işleme alabilmek için:
- Proje dizinine geçilir ve `php artisan migrate` komutu uygulanır

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
```



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
public function showUsers(User $id) {  // ÖNEMLİ! Böyle yapınca, User adlı model'deki $id'ya sahip kayıt OTOMATİK gelir. Buna Model Binding denir
    return view('home', compact('id'));
}
```
- `showUsers(User $id)` cağrısında, User adlı model'deki $id'ya sahip kayıt OTOMATİK gelir. Buna **Model Binding** denir


# ROUTE

### Rooting tanımı sonrasında 404 hatası oluşursa:
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
Merhaba {{ $isim }}
```

# BLADE

- Master Template
- Blade içinde değişken saha tanımlama: `@yield('BLOGICERIGI')`
- KULLANIMI:
```PHP
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
	$arrKullanicilar = User::all(); // Tüm tablo içeriğini getirir
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






# FORM Nesnesi
TODO: EKSİK
- protected guarded = ['id'];
- protected fillable = ['title', 'body'];
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

		<input type="text" name="lname" value="{{old('lname')}}" required> // Hata sonrası forma eski değerleri doldurabilmek için
		@error('lname')
		<div>{{ $message }}</div>  // Bu bölüm sadece "lname" sahası için bir hata oluşursa gösterecektir
		@enderror

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
- En iyi ayar için, Model adı SINGLE, tablo adı ve controller PLURIAL olmalıdır
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



