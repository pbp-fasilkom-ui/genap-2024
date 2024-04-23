# Tutorial 8: Flutter Networking, Authentication, and Integration

Pemrograman Berbasis Platform (CSGE602022) — diselenggarakan oleh Fakultas Ilmu Komputer Universitas Indonesia, Semester Genap 2023/2024

---

## Tujuan Pembelajaran

Setelah menyelesaikan tutorial ini, mahasiswa diharapkan untuk dapat:

- Memahami struktur dan pembuatan model pada Flutter.
- Memahami cara mengambil, mengolah, dan menampilkan data dari web service.
- Memahami _state management_ dasar menggunakan Provider pada Flutter.
- Dapat melakukan autentikasi dengan web service Django dengan aplikasi Flutter.

## Model pada Flutter

Pada tutorial kali ini, kita akan memanggil _web service_ dan menampilkan hasil yang didapatkan ke halaman Flutter yang kita buat. Akan tetapi, sebelum melakukan pemanggilan _web service_, kita perlu mendefinisikan model yang akan kita gunakan ketika melakukan pemanggilan _web service_. Model pada Flutter menggunakan prinsip _class_ seperti layaknya yang sudah dipelajari pada DDP2 bagian OOP.

> Kode di bawah ini adalah contoh, tidak wajib diikuti, tetapi sangat disarankan dibaca karena konsepnya akan digunakan pada bagian-bagian selanjutnya.

Berikut merupakan contoh _class_ pada Flutter.

```dart
class Mobil {
    Mobil({
        this.id,
        this.brand,
        this.model
        this.color
    });

    int id;
    String brand;
    String model;
    String color;
}
```

Catatan: Jika kamu mengalami _error_ saat membuat _class_, tambahkan _keyword_ `required` pada setiap parameter _class_ pada bagian _constructor_.

Sampai saat ini, kita telah berhasil membuat _class_. Selanjutnya, kita akan menambahkan beberapa kode sehingga terbentuk sebuah model `Mobil`. `Mobil` ini merupakan suatu model yang merepresentasikan respons dari pemanggilan _web service_.

Import `dart:convert` pada bagian paling atas file.

```dart
import 'dart:convert';
...
```

Pada _class_ `Mobil`, tambahkan kode berikut.

```dart
factory Mobil.fromJson(Map<String, dynamic> json) => Mobil(
    id: json["id"],
    brand: json["brand"],
    model: json["model"],
    color: json["color"],
);

Map<String, dynamic> toJson() => {
    "id": id,
    "brand": brand,
    "model": model,
    "color": color,
};
```

Tambahkan kode berikut di luar _class_ `Mobil`.

```dart
Mobil mobilFromJson(String str) => Mobil.fromJson(json.decode(str));
String mobilToJson(Mobil data) => json.encode(data.toJson());
```

Pada akhirnya, kode akan terbentuk seperti berikut untuk menampilkan satu objek `Mobil` dari _web service_.

```dart
import 'dart:convert';

Mobil mobilFromJson(String str) => Mobil.fromJson(json.decode(str));
String mobilToJson(Mobil data) => json.encode(data.toJson());

class Mobil {
    Mobil({
        this.id,
        this.brand,
        this.model,
        this.color,
    });

    int id;
    String brand;
    String model;
    String color;

    factory Mobil.fromJson(Map<String, dynamic> json) => Mobil(
        id: json["id"],
        brand: json["brand"],
        model: json["model"],
        color: json["color"],
    );

    Map<String, dynamic> toJson() => {
        "id": id,
        "brand": brand,
        "model": model,
        "color": color,
    };
}
```

### Penjelasan

Terdapat beberapa kode-kode tambahan seperti _method_ `toJson` dan `fromJson` di dalam _class_ `Mobil`. Hal tersebut disebabkan ketika kita me-_request_ suatu _web service_ dengan _method_ **GET**, umumnya kita mendapatkan hasil pemanggilan berupa JSON. Oleh karena itu, kita perlu melakukan konversi data dengan _method_ `fromJson` agar Flutter mengenali JSON tersebut sebagai objek _class_ `Mobil`. Selain itu, terdapat juga _method_ `toJson` yang akan digunakan ketika kita melakukan pengiriman data ke _web service_ (seperti **POST** atau **PUT**).

Berikut adalah contoh respons dari _web service_ dengan _method_ **GET** yang dapat dikonversi ke _class_ model `Mobil`.

```json
{
    "id": 1,
    "brand": "Honda",
    "model": "Civic",
    "color": "Yellow"
}
```

Lalu, bagaimana jika respons dari _web service_ berupa kumpulan objek JSON? Sebenarnya sama saja dengan kode di atas, hanya saja terdapat pengubahan pada _method_ `mobilFromJson` dan `mobilToJson`.

Kodenya adalah sebagai berikut.

```dart
List<Mobil> mobilFromJson(String str) => List<Mobil>.from(json.decode(str).map((mobil) => Mobil.fromJson(mobil)));

String mobilToJson(List<Mobil> data) => json.encode(List<dynamic>.from(data.map((mobil) => mobil.toJson())));
```

Berikut adalah contoh respons dari _web service_ dengan _method_ **GET** yang dapat dikonversi ke model `Mobil`.

```json
[
    {
        "id": 1,
        "brand": "Honda",
        "model": "Civic",
        "color": "Yellow"
    },
    {
        "id": 2,
        "brand": "Toyota",
        "model": "Supra",
        "color": "Red"
    }
]
```

## Fetch Data dari Web Service pada Flutter

Pada saat pengembangan aplikasi, ada kalanya kita perlu mengambil data eksternal dari luar aplikasi kita (Internet) untuk ditampilkan di aplikasi kita. Tutorial ini bertujuan untuk memahami cara melakukan _fetching data_ dari sebuah _web service_ pada Flutter.

Secara umum terdapat beberapa langkah ketika ingin menampilkan data dari _web service_ lain ke aplikasi Flutter, yaitu:

1. Menambahkan dependensi `http` ke proyek; dependensi ini digunakan untuk bertukar HTTP _request_.

2. Membuat model sesuai dengan respons dari data yang berasal dari _web service_ tersebut.

3. Membuat _http request_ ke _web service_ menggunakan dependensi `http`.

4. Mengkonversikan objek yang didapatkan dari _web service_ ke model yang telah kita buat di langkah kedua.

5. Menampilkan data yang telah dikonversi ke aplikasi dengan `FutureBuilder`.

Penjelasan lebih lanjut dapat dibaca pada tautan berikut: <http://docs.flutter.dev/cookbook/networking/fetch-data#5-display-the-data>.

## State Management Dasar menggunakan Provider

`Provider` adalah sebuah pembungkus di sekitar `InheritedWidget` agar `InheritedWidget` lebih mudah digunakan dan lebih dapat digunakan kembali. `InheritedWidget` sendiri adalah kelas dasar untuk widget Flutter yang secara efisien menyebarkan informasi ke widget lainnya yang berada pada satu _tree_.

Manfaat menggunakan `provider` adalah sebagai berikut.

- Mengalokasikan _resource_ menjadi lebih sederhana.
- _Lazy-loading_.
- Mengurangi _boilerplate_ tiap kali membuat _class_ baru.
- Didukung oleh Flutter Devtool sehingga `provider` dapat dilacak dari Devtool.
- Peningkatan skalabilitas untuk _class_ yang memanfaatkan mekanisme _listen_ yang dibangun secara kompleks.

Untuk mengetahui `provider` secara lebih lanjut, silakan buka halaman _package_ Provider: <http://pub.dev/packages/provider>

## Tutorial: Integrasi Autentikasi Django-Flutter

### Setup Autentikasi pada Django untuk Flutter

Ikuti langkah-langkah berikut untuk melakukan integrasi sistem autentikasi pada **Django**.

1. Buatlah `django-app` bernama `authentication` pada project Django yang telah kamu buat sebelumnya.

2. Tambahkan `authentication` ke `INSTALLED_APPS` pada _main project_ `settings.py` aplikasi Django kamu.

    > Hint: Apabila lupa cara untuk langkah 1 dan 2, coba baca lagi Tutorial 1.

3. Jalankan perintah `pip install django-cors-headers` untuk menginstal _library_ yang dibutuhkan. Jangan lupa untuk menyalakan _virtual environment_ Python terlebih dahulu.

4. Tambahkan `corsheaders` ke `INSTALLED_APPS` pada _main project_ `settings.py` aplikasi Django kamu.

5. Tambahkan `corsheaders.middleware.CorsMiddleware` pada _main project_ `settings.py` aplikasi Django kamu.

6. Tambahkan beberapa variabel berikut ini pada _main project_ `settings.py` aplikasi Django kamu.

    ```python
    CORS_ALLOW_ALL_ORIGINS = True
    CORS_ALLOW_CREDENTIALS = True
    CSRF_COOKIE_SECURE = True
    SESSION_COOKIE_SECURE = True
    CSRF_COOKIE_SAMESITE = 'None'
    SESSION_COOKIE_SAMESITE = 'None'
    ```

7. Buatlah sebuah metode _view_ untuk login pada `authentication/views.py`.

    ```python
    from django.contrib.auth import authenticate, login as auth_login
    from django.http import JsonResponse
    from django.views.decorators.csrf import csrf_exempt

    @csrf_exempt
    def login(request):
        username = request.POST['username']
        password = request.POST['password']
        user = authenticate(username=username, password=password)
        if user is not None:
            if user.is_active:
                auth_login(request, user)
                # Status login sukses.
                return JsonResponse({
                    "username": user.username,
                    "status": True,
                    "message": "Login sukses!"
                    # Tambahkan data lainnya jika ingin mengirim data ke Flutter.
                }, status=200)
            else:
                return JsonResponse({
                    "status": False,
                    "message": "Login gagal, akun dinonaktifkan."
                }, status=401)

        else:
            return JsonResponse({
                "status": False,
                "message": "Login gagal, periksa kembali email atau kata sandi."
            }, status=401)
   	```

8. Buat _file_ `urls.py` pada folder `authentication` dan tambahkan URL _routing_ terhadap fungsi yang sudah dibuat dengan _endpoint_ `login/`.

    ```python
    from django.urls import path
    from authentication.views import login

    app_name = 'authentication'

    urlpatterns = [
        path('login/', login, name='login'),
    ]
    ```

9. Terakhir, tambahkan `path('auth/', include('authentication.urls')),` pada file `book_tracker/urls.py`.

### Integrasi Sistem Autentikasi pada Flutter

Untuk memudahkan pembuatan sistem autentikasi, tim asisten dosen telah membuatkan _package_ Flutter yang dapat dipakai untuk melakukan kontak dengan _web service_ Django (termasuk operasi `GET` dan `POST`).

_Package_ dapat diakses melalui tautan berikut: [pbp_django_auth](http://pub.dev/packages/pbp_django_auth)

Ikuti langkah-langkah berikut untuk melakukan integrasi sistem autentikasi pada **Flutter**.

1. Instal _package_ yang telah disediakan oleh tim asisten dosen dengan menjalankan perintah berikut di Terminal. Jalankan pada direktori _root_ dari proyek Flutter kamu.

	```bash
	flutter pub add provider
	flutter pub add pbp_django_auth
	```

2. Untuk menggunakan _package_ tersebut, kamu perlu memodifikasi _root widget_ untuk menyediakan `CookieRequest` _library_ ke semua _child widgets_ dengan menggunakan `Provider`.

	Sebagai contoh, jika aplikasimu sebelumnya seperti ini:

	```dart
	class MyApp extends StatelessWidget {
		const MyApp({Key? key}) : super(key: key);

		@override
		Widget build(BuildContext context) {
			return MaterialApp(
				title: 'Flutter App',
				theme: ThemeData(
					colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
					useMaterial3: true,
				),
				home: MyHomePage(),
			);
		}
	}
	```

    Ubahlah menjadi:

	```dart
	class MyApp extends StatelessWidget {
		const MyApp({Key? key}) : super(key: key);

		@override
		Widget build(BuildContext context) {
			return Provider(
				create: (_) {
					CookieRequest request = CookieRequest();
					return request;
				},
				child: MaterialApp(
					title: 'Flutter App',
					theme: ThemeData(
						colorScheme: ColorScheme.fromSeed(seedColor: Colors.indigo),
						useMaterial3: true,
					),
					home: MyHomePage(),
				),
			);
		}
	}
	```

    Hal ini akan membuat objek `Provider` baru yang akan membagikan _instance_ `CookieRequest` dengan semua komponen yang ada di aplikasi.

3. Buatlah berkas baru pada folder `screens` dengan nama `login.dart`.

4. Isilah berkas `login.dart` dengan kode berikut.

    ```dart
    import 'package:book_tracker/screens/menu.dart';
    import 'package:flutter/material.dart';
    import 'package:pbp_django_auth/pbp_django_auth.dart';
    import 'package:provider/provider.dart';

    void main() {
        runApp(const LoginApp());
    }

    class LoginApp extends StatelessWidget {
        const LoginApp({super.key});

        @override
        Widget build(BuildContext context) {
            return MaterialApp(
                title: 'Login',
                theme: ThemeData(
                    primarySwatch: Colors.blue,
                ),
                home: const LoginPage(),
            );
        }
    }

    class LoginPage extends StatefulWidget {
        const LoginPage({super.key});

        @override
        State<LoginPage> createState() => _LoginPageState();
    }

    class _LoginPageState extends State<LoginPage> {
        final TextEditingController _usernameController = TextEditingController();
        final TextEditingController _passwordController = TextEditingController();

        @override
        Widget build(BuildContext context) {
            final request = context.watch<CookieRequest>();
            return Scaffold(
                appBar: AppBar(
                    title: const Text('Login'),
                ),
                body: Container(
                    padding: const EdgeInsets.all(16.0),
                    child: Column(
                        mainAxisAlignment: MainAxisAlignment.center,
                        children: [
                            TextField(
                                controller: _usernameController,
                                decoration: const InputDecoration(
                                    labelText: 'Username',
                                ),
                            ),
                            const SizedBox(height: 12.0),
                            TextField(
                                controller: _passwordController,
                                decoration: const InputDecoration(
                                    labelText: 'Password',
                                ),
                                obscureText: true,
                            ),
                            const SizedBox(height: 24.0),
                            ElevatedButton(
                                onPressed: () async {
                                    String username = _usernameController.text;
                                    String password = _passwordController.text;

                                    // Cek kredensial
                                    // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
   								    // Untuk menyambungkan Android emulator dengan Django pada localhost,
                                    // gunakan URL http://10.0.2.2/
                                    final response = await request.login("http://<APP_URL_KAMU>/auth/login/", {
                                        'username': username,
                                        'password': password,
                                    });

                                    if (request.loggedIn) {
                                        String message = response['message'];
                                        String uname = response['username'];
                                        if (context.mounted) {
                                            Navigator.pushReplacement(
                                                context,
                                                MaterialPageRoute(builder: (context) => MyHomePage()),
                                            );
                                            ScaffoldMessenger.of(context)
                                                ..hideCurrentSnackBar()
                                                ..showSnackBar(
                                                    SnackBar(content: Text("$message Selamat datang, $uname.")));
                                        }
                                    } else {
                                        if (context.mounted) {
                                            showDialog(
                                                context: context,
                                                builder: (context) => AlertDialog(
                                                    title: const Text('Login Gagal'),
                                                    content:
                                                        Text(response['message']),
                                                    actions: [
                                                        TextButton(
                                                            child: const Text('OK'),
                                                            onPressed: () {
                                                                Navigator.pop(context);
                                                            },
                                                        ),
                                                    ],
                                                ),
                                            );
                                        }
                                    }
                                },
                                child: const Text('Login'),
                            ),
                        ],
                    ),
                ),
            );
        }
    }
    ```

5. Pada _file_ `main.dart`, pada Widget `MaterialApp(...)`, ubah `home: MyHomePage()` menjadi `home: LoginPage()`

6. Jalankan aplikasi Flutter kamu dan cobalah untuk login.

## Pembuatan Model Kustom

Dalam membuat model yang menyesuaikan dengan data JSON, kita dapat memanfaatkan website [Quicktype](http://app.quicktype.io/) dengan tahapan sebagai berikut.

1. Bukalah _endpoint_ `JSON` yang sudah kamu buat sebelumnya pada tutorial 2.

2. Salinlah data `JSON` dan buka situs web [Quicktype](http://app.quicktype.io/).

3. Pada situs web Quicktype, ubahlah _setup name_ menjadi `Book`, _source type_ menjadi `JSON`, dan _language_ menjadi `Dart`.

4. Tempel data JSON yang telah disalin sebelumnya ke dalam _textbox_ yang tersedia pada Quicktype.

5. Klik pilihan `Copy Code` pada Quicktype.

    Berikut adalah contoh hasilnya.

    ![Contoh Quicktype](../static/img/contoh_quicktype.png)

Setelah mendapatkan kode model melalui Quicktype, buka kembali proyek Flutter dan buatlah folder baru `models/` pada subdirektori `lib/`. Buatlah file baru pada folder tersebut dengan nama `book.dart`, dan tempel kode yang sudah disalin dari Quicktype.

## Penerapan Fetch Data dari Django Untuk Ditampilkan ke Flutter

### Menambahkan Dependensi HTTP

Untuk melakukan perintah _HTTP request_, kita membutuhkan _package_ tambahan yakni _package_ [http](http://pub.dev/packages/http).

1. Lakukan `flutter pub add http` pada terminal proyek Flutter untuk menambahkan _package_ `http`.

2. Pada file `android/app/src/main/AndroidManifest.xml`, tambahkan kode berikut untuk memperbolehkan akses Internet pada aplikasi Flutter yang sedang dibuat.

    ```xml
    ...
        <application>
        ...
        </application>
        <!-- Required to fetch data from the Internet. -->
        <uses-permission android:name="android.permission.INTERNET" />
    ...
    ```

### Melakukan Fetch Data dari Django

1. Buatlah berkas baru pada direktori `lib/screens` dengan nama `list_book.dart`.

2. Pada file `list_book.dart`, impor _library_ yang dibutuhkan. Ubahlah <APP_NAME> sesuai dengan nama proyek Flutter yang kalian buat.

    ```dart
    import 'package:flutter/material.dart';
    import 'package:http/http.dart' as http;
    import 'dart:convert';
    import 'package:<APP_NAME>/models/book.dart';
    ...
    ```

3. Salinlah potongan kode berikut dan _paste_ pada `list_book.dart`. Jangan lupa untuk mengimpor file atau modul yang diperlukan.

    ```dart
    ...
    import 'package:<APP_NAME>/widgets/left_drawer.dart';

    class BookPage extends StatefulWidget {
        const BookPage({Key? key}) : super(key: key);

        @override
        State<BookPage> createState() => _BookPageState();
    }

    class _BookPageState extends State<BookPage> {
    Future<List<Book>> fetchBook() async {
        // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
        var url = Uri.parse(
            'http://<URL_APP_KAMU>/json/');
        var response = await http.get(
            url,
            headers: {"Content-Type": "application/json"},
        );

        // melakukan decode response menjadi bentuk json
        var data = jsonDecode(utf8.decode(response.bodyBytes));

        // melakukan konversi data json menjadi object Book
        List<Book> listBook = [];
        for (var d in data) {
            if (d != null) {
                listBook.add(Book.fromJson(d));
            }
        }
        return listBook;
    }

    @override
    Widget build(BuildContext context) {
        return Scaffold(
            appBar: AppBar(
                title: const Text('Book'),
            ),
            drawer: const LeftDrawer(),
            body: FutureBuilder(
                future: fetchBook(),
                builder: (context, AsyncSnapshot snapshot) {
                    if (snapshot.data == null) {
                        return const Center(child: CircularProgressIndicator());
                    } else {
                        if (!snapshot.hasData) {
                            return const Column(
                                children: [
                                    Text(
                                        "Tidak ada data buku.",
                                        style: TextStyle(
                                            color: Color(0xff59A5D8),
                                            fontSize: 20
                                        ),
                                    ),
                                    SizedBox(height: 8),
                                ],
                            );
                        } else {
                            return ListView.builder(
                                itemCount: snapshot.data!.length,
                                itemBuilder: (_, index) => Container(
                                    margin: const EdgeInsets.symmetric(
                                        horizontal: 16,
                                        vertical: 12
                                    ),
                                    padding: const EdgeInsets.all(20.0),
                                    child: Column(
                                        mainAxisAlignment: MainAxisAlignment.start,
                                        crossAxisAlignment: CrossAxisAlignment.start,
                                        children: [
                                            Text(
                                                "${snapshot.data![index].fields.name}",
                                                style: const TextStyle(
                                                    fontSize: 18.0,
                                                    fontWeight: FontWeight.bold,
                                                ),
                                            ),
                                            const SizedBox(height: 10),
                                            Text("${snapshot.data![index].fields.page}"),
                                            const SizedBox(height: 10),
                                            Text("${snapshot.data![index].fields.description}")
                                        ],
                                    ),
                                ),
                            );
                        }
                    }
                }),
            );
        }
    }
    ```

4. Tambahkan halaman `list_book.dart` ke `widgets/left_drawer.dart` dengan menambahkan kode berikut.

    ```dart
    // Kode ListTile Menu
    ...
    ListTile(
        leading: const Icon(Icons.library_books_rounded),
        title: const Text('Daftar Buku'),
        onTap: () {
            // Route menu ke halaman buku
            Navigator.push(
                context,
                MaterialPageRoute(builder: (context) => const BookPage()),
            );
        },
    ),
    ...
    ```

5. Ubah fungsi tombol `Lihat Buku` pada halaman utama agar mengarahkan ke halaman `BookPage`. Kamu dapat melakukan _redirection_ dengan menambahkan `else if` setelah kode `if(...){...}` di bagian akhir `onTap: () { }` yang ada pada file `widgets/tracker_card.dart`

    ```dart
    ...
    else if (item.name == "Lihat Buku") {
        Navigator.push(context,
            MaterialPageRoute(
                builder: (context) => const BookPage()
            ),
        );
    }
    ...
    ```

6. Impor _file_ yang dibutuhkan saat menambahkan `BookPage` ke `left_drawer.dart` dan `tracker_card.dart`.

7. Jalankan aplikasi dan cobalah untuk menambahkan beberapa `Book` di situs web kamu. Kemudian, coba lihat hasilnya melalui halaman `Daftar Buku` yang baru saja kamu buat di aplikasi Flutter

## Integrasi Form Flutter Dengan Layanan Django

Langkah-langkah berikut akan dilakukan pada kode proyek **Django**.

1. Buatlah sebuah fungsi _view_ baru pada `main/views.py` aplikasi Django kamu dengan potongan kode berikut. Jangan lupa juga untuk menambahkan import-import yang diperlukan pada bagian atas kode.

    ```python
    from django.views.decorators.csrf import csrf_exempt
    import json
    from django.http import JsonResponse
    ...
    @csrf_exempt
    def create_book_flutter(request):
        if request.method == 'POST':

            data = json.loads(request.body)

            new_book = Book.objects.create(
   			    user = request.user,
                name = data["name"],
                page = int(data["page"]),
                description = data["description"]
            )

            new_book.save()

            return JsonResponse({"status": "success"}, status=200)
        else:
            return JsonResponse({"status": "error"}, status=401)
    ```

2. Tambahkan _path_ baru pada `main/urls.py` dengan kode berikut.

    ```python
    path('create-flutter/', create_book_flutter, name='create_book_flutter'),
    ```

3. Jalankan ulang (dan _deploy_ ulang) aplikasi kamu. Apabila kamu telah men-_deploy_ aplikasimu, maka data akun dan transaksi akan hilang setelah di-_redeploy_.

Langkah-langkah berikut akan dilakukan pada kode proyek **Flutter**.

1. Hubungkan halaman `trackerlist_form.dart` dengan `CookieRequest` dengan menambahkan baris kode berikut.

    ```dart
    ...
    @override
    Widget build(BuildContext context) {
        final request = context.watch<CookieRequest>();

        return Scaffold(
        ...
    ```

2. Ubahlah perintah pada `onPressed: ()` _button_ tambah menjadi kode berikut.

    ```dart
    ...
    onPressed: () async {
        if (_formKey.currentState!.validate()) {
            // Kirim ke Django dan tunggu respons
            // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
            final response = await request.postJson(
                "http://<URL_APP_KAMU>/create-flutter/",
                jsonEncode(<String, String>{
                    'name': _name,
                    'page': _page.toString(),
                    'description': _description,
                // TODO: Sesuaikan field data sesuai dengan aplikasimu
                }),
            );
            if (context.mounted) {
                if (response['status'] == 'success') {
                    ScaffoldMessenger.of(context)
                        .showSnackBar(const SnackBar(
                    content: Text("Buku baru berhasil disimpan!"),
                    ));
                    Navigator.pushReplacement(
                        context,
                        MaterialPageRoute(builder: (context) => MyHomePage()),
                    );
                } else {
                    ScaffoldMessenger.of(context)
                        .showSnackBar(const SnackBar(
                        content:
                            Text("Terdapat kesalahan, silakan coba lagi."),
                    ));
                }
            }
        }
    },
    ...
    ```

3. Lakukan _quick fix_ pada baris-baris yang bermasalah untuk mengimpor _file_ yang dibutuhkan.

4. Jalankan ulang aplikasi dan coba untuk menambahkan transaksi baru dari aplikasi Flutter kamu.

## Implementasi Fitur Logout

Langkah-langkah berikut akan dilakukan pada kode proyek Django.

1. Buatlah sebuah metode _view_ untuk logout pada `authentication/views.py`.

    ```python
    from django.contrib.auth import logout as auth_logout
    ...
    @csrf_exempt
    def logout(request):
        username = request.user.username

        try:
            auth_logout(request)
            return JsonResponse({
                "username": username,
                "status": True,
                "message": "Logout berhasil!"
            }, status=200)
        except:
            return JsonResponse({
            "status": False,
            "message": "Logout gagal."
            }, status=401)
    ```

2. Tambahkan _path_ baru pada `authentication/urls.py` dengan kode berikut.

    ```python
    from authentication.views import logout
    ...
    path('logout/', logout, name='logout'),
    ```

Langkah-langkah berikut akan dilakukan pada kode proyek Flutter.

1. Buka _file_ `lib/widgets/tracker_card.dart` dan tambahkan potongan kode berikut. Selesaikan masalah impor _library_ setelah menambahkan potongan kode ke dalam _file_ tersebut.

    ```dart
    ...
    @override
    Widget build(BuildContext context) {
        final request = context.watch<CookieRequest>();
        return Material(
            ...
    ```

2. Ubahlah perintah `onTap: () {...}` pada widget `Inkwell` menjadi `onTap: () async {...}` agar widget `Inkwell` dapat melakukan proses logout secara asinkronus.

3. Tambahkan kode berikut ke dalam `async {...}` di bagian akhir:

    ```dart
    ...
    // statement if sebelumnya
    // tambahkan else if baru seperti di bawah ini
    else if (item.name == "Logout") {
        final response = await request.logout(
            // TODO: Ganti URL dan jangan lupa tambahkan trailing slash (/) di akhir URL!
            "http://<APP_URL_KAMU>/auth/logout/");
        String message = response["message"];
        if (context.mounted) {
            if (response['status']) {
                String uname = response["username"];
                ScaffoldMessenger.of(context).showSnackBar(SnackBar(
                    content: Text("$message Sampai jumpa, $uname."),
                ));
                Navigator.pushReplacement(
                    context,
                    MaterialPageRoute(builder: (context) => const LoginPage()),
                );
            } else {
                ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(
                        content: Text(message),
                    ),
                );
            }
        }
    }
    ...
    ```

4. Jalankan ulang aplikasi dan coba untuk lakukan logout.

## Akhir Kata

Selamat! Kamu telah menyelesaikan Tutorial 8! Semoga dengan tutorial ini, kalian dapat memahami mengenai _model_, _fetch_ data, _state management_ dasar, dan integrasi Django-Flutter dengan baik. 😄

1. Pelajari dan pahami kembali kode yang sudah kamu tuliskan di atas dengan baik. **Jangan lupa untuk menyelesaikan semua TODO yang ada!**

2. Lakukan `add`, `commit` dan `push` untuk memperbarui repositori GitHub.

	```shell
	git add .
	git commit -m "<pesan_commit>"
	git push -u origin <branch_utama>
	```

	- Ubah `<pesan_commit>` sesuai dengan keinginan. Contoh: `git commit -m "tutorial 7 selesai"`.
	- Ubah `<branch_utama>` sesuai dengan nama branch utamamu. Contoh: `git push -u origin main` atau `git push -u origin master`.

## Referensi Tambahan

- [Fetch Data From the Internet](http://docs.flutter.dev/cookbook/networking/fetch-data)
- [How to create models in Flutter Dart](http://thegrowingdeveloper.org/coding-blog/how-to-create-models-in-flutter-dart)
- [Simple app state management | Flutter](http://docs.flutter.dev/development/data-and-backend/state-mgmt/simple)
- [Flutter State Management with Provider](http://blog.devgenius.io/flutter-state-management-with-provider-5a57eca108f1)
- [Pengenalan State Management Flutter dan Jenis-jenisnya](http://caraguna.com/pengenalan-state-management-flutter/)

## Kontributor

- Muhammad Nabil Mu'afa
- Muhammad Iqbal Dwitama

## Credits

Tutorial ini dikembangkan berdasarkan [PBP Ganjil 2024](http://github.com/pbp-fasilkom-ui/ganjil-2024) yang ditulis oleh Tim Pengajar Pemrograman Berbasis Platform 2023. Segala tutorial serta instruksi yang dicantumkan pada repositori ini dirancang sedemikian rupa sehingga mahasiswa yang sedang mengambil mata kuliah Pemrograman Berbasis Platform dapat menyelesaikan tutorial saat sesi lab berlangsung.