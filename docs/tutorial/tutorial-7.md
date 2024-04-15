# Tutorial 7: Flutter Navigation, Layouts, Forms, and Input Elements

Pemrograman Berbasis Platform (CSGE602022) — diselenggarakan oleh Fakultas Ilmu Komputer Universitas Indonesia, Semester Genap 2023/2024

---

## Tujuan Pembelajaran

Setelah menyelesaikan tutorial ini, mahasiswa diharapkan untuk dapat:

- Memahami navigasi dan _routing_ dasar pada Flutter.
- Memahami elemen _input_ dan _form_ pada Flutter.
- Memahami alur pembuatan _form_ dan data pada Flutter.
- Memahami dan menerapkan _clean architecture_ sederhana

## Navigasi Halaman pada Flutter

Pada saat belajar pengembangan _web_, kalian pasti sudah belajar bahwa dalam sebuah _website_ kita dapat berpindah-pindah halaman sesuai dengan _URL_ yang diakses. Hal yang sama juga berlaku pada pengembangan aplikasi, dimana kita juga dapat melakukan perpindahan dari satu 'halaman' ke 'halaman' yang lainnya. Bedanya, pada sebuah aplikasi, yang kita gunakan untuk berpindah bukanlah dengan mengakses _URL_ yang berbeda.

Untuk mengimplementasikan navigasi pada Flutter, sebenarnya sudah disediakan sistem yang cukup lengkap untuk melakukan navigasi antar halaman. Salah satu cara yang dapat kita gunakan untuk berpindah-pindah halaman adalah dengan menggunakan _widget_ `Navigator`. _Widget_ `Navigator` menampilkan halaman-halaman yang ada kepada layar seakan sebagai sebuah tumpukan (_stack_). Untuk menavigasi sebuah halaman baru, kita dapat mengakses `Navigator` melalui `BuildContext` dan memanggil fungsi yang ada, seperti misalnya `push()`, `pop()`, serta `pushReplacement()`.

> Note: Di dalam Flutter, layar dan halaman seringkali disebut dengan terminologi _route_.

Berikut adalah penjelasan mengenai beberapa penggunaan `Navigator` yang paling sering dijumpai dalam pengembangan aplikasi.

### Push (`push()`)

```dart
...
    if (item.name == "Tambah Buku") {
        Navigator.push(context,
            MaterialPageRoute(builder: (context) => const TrackerFormPage()));
    }
...
```

Method `push()` menambahkan suatu _route_ ke dalam _stack_ _route_ yang dikelola oleh `Navigator`. Method ini menyebabkan _route_ yang ditambahkan berada pada paling atas stack, sehingga _route_ yang baru saja ditambahkan tersebut akan muncul dan ditampilkan kepada pengguna.

### Pop (`pop()`)

```dart
...
    onPressed: () {
        Navigator.pop(context);
    },
...
```

Method `pop()` menghapus _route_ yang sedang ditampilkan kepada pengguna (atau dalam kata lain, _route_ yang berada pada paling atas _stack_) dari _stack_ _route_ yang dikelola oleh Navigator. Method ini menyebabkan aplikasi untuk berpindah dari _route_ yang sedang ditampilkan kepada pengguna ke _route_ yang berada di bawahnya pada _stack_ yang dikelola `Navigator`.

### Push Replacement (`pushReplacement()`)

```dart
...
    onTap: () {
        Navigator.pushReplacement(
        context,
        MaterialPageRoute(
            builder: (context) => MyHomePage(),
        ));
    },
...
```

Method `pushReplacement()` menghapus _route_ yang sedang ditampilkan kepada pengguna dan menggantinya dengan suatu _route_. Method ini menyebabkan aplikasi untuk berpindah dari _route_ yang sedang ditampilkan kepada pengguna ke suatu _route_ yang diberikan. Pada _stack_ _route_ yang dikelola `Navigator`, _route_ lama pada atas _stack_ akan digantikan secara langsung oleh _route_ baru yang diberikan tanpa mengubah kondisi elemen _stack_ yang berada di bawahnya.

Walaupun `push()` dan `pushReplacement()` sekilas terlihat mirip, namun perbedaan kedua _method_ tersebut terletak pada apa yang dilakukan kepada _route_ yang berada pada atas _stack_. `push()` akan menambahkan _route_ baru diatas _route_ yang sudah ada pada atas _stack_, sedangkan `pushReplacement()` menggantikan _route_ yang sudah ada pada atas _stack_ dengan _route_ baru tersebut. Penting juga untuk memperhatikan kemungkinan urutan dan isi dari _stack_, karena jika kondisi _stack_ kosong serta kita menekan tombol **Back** pada gawai, maka sistem akan keluar dari aplikasi tersebut.

Di samping ketiga method `Navigator` di atas, terdapat juga beberapa method lain yang dapat memudahkan routing dalam pengembangan aplikasi, seperti `popUntil()`, `canPop()`, dan `maybePop()`. Silakan mengeksplorasi method-method tersebut secara mandiri. Untuk mengetahui lebih dalam terkait `Navigator`, kalian dapat membaca dokumentasi yang ada pada tautan berikut: <https://api.flutter.dev/flutter/widgets/Navigator-class.html>

## Input dan Form Pada Flutter

Sama halnya dengan sebuah web, sebuah aplikasi juga dapat berinteraksi dengan pengguna melalui input dan form. Flutter memiliki widget Form yang dapat kita manfaatkan untuk menjadi wadah bagi beberapa _input field widget_ yang kita buat. Sama halnya dengan _input field_ pada web, Flutter juga memiliki banyak tipe _input field_, salah satunya widget `TextField`.

Untuk mencoba sampel dari widget Form, jalankan perintah berikut:

```bash
flutter create --sample=widgets.Form.1 form_sample
```

Untuk mengetahui lebih lanjut terkait widget Form, kalian dapat membaca dokumentasi pada tautan berikut: [Flutter Form Class](https://api.flutter.dev/flutter/widgets/Form-class.html.)

## Tutorial: Menambahkan Drawer Menu Untuk Navigasi

Untuk mempermudah navigasi di aplikasi Flutter kita, kita dapat menambahkan _drawer menu_. _Drawer menu_ adalah sebuah menu yang muncul dari sisi kiri atau kanan layar. _Drawer menu_ biasanya berisi navigasi ke halaman-halaman lain pada aplikasi.

1. Buka proyek yang sebelumnya telah dibuat pada tutorial 6 dengan menggunakan IDE favoritmu.

2. Buatlah direktori baru bernama `widgets` di subdirektori `lib/`. Kemudian, buat berkas dengan nama `left_drawer.dart`. Tambahkan kode berikut ke dalam berkas tersebut.

	```dart
	import 'package:flutter/material.dart';

	class LeftDrawer extends StatelessWidget {
		const LeftDrawer({super.key});

		@override
			Widget build(BuildContext context) {
			return Drawer(
				child: ListView(
					children: [
						const DrawerHeader(
						// TODO: Bagian drawer header
						),
						// TODO: Bagian routing
					],
				),
			);
		}
	}
	```

3. Berikutnya, tambahkan impor untuk halaman-halaman yang kita ingin masukkan navigasinya ke dalam Drawer Menu. Pada contoh ini, kita akan menambahkan navigasi ke halaman `MyHomePage` dan `TrackerFormPage`.

	```dart
	import 'package:flutter/material.dart';
	import 'package:book_tracker/menu.dart';
	// TODO: Impor halaman TrackerFormPage jika sudah dibuat
	```

4. Setelah berhasil impor, kita akan memasukkan routing untuk halaman-halaman yang kita impor ke bagian `TODO: Bagian routing`. Hapus komentar tersebut dan isi dengan potongan kode berikut.

	```dart
	...
	ListTile(
		leading: const Icon(Icons.home_outlined),
		title: const Text('Halaman Utama'),
		// Bagian redirection ke MyHomePage
		onTap: () {
		Navigator.pushReplacement(
			context,
			MaterialPageRoute(
				builder: (context) => MyHomePage(),
			));
		},
	),
	ListTile(
		leading: const Icon(Icons.library_add_rounded),
		title: const Text('Tambah Buku'),
		// Bagian redirection ke TrackerFormPage
		onTap: () {
		/*
		TODO: Buatlah routing ke TrackerFormPage di sini,
		setelah halaman TrackerFormPage sudah dibuat.
		*/
		},
	),
	...
	```

   	> Apabila kalian _copy-paste_ secara langsung, pastikan ellipsis (tanda "...") di atas dan di bawah kode tidak ikut ter-_copy_.

5. Selanjutnya, kita akan menghias drawer kita dengan memasukkan drawer header di `TODO: Bagian drawer header`. Hapus komentar tersebut dan ganti dengan potongan kode berikut.

	```dart
	...
	const DrawerHeader(
		decoration: BoxDecoration(
			color: Colors.indigo,
		),
		child: Column(
			children: [
				Text(
					'Book Tracker',
					textAlign: TextAlign.center,
					style: TextStyle(
						fontSize: 30,
						fontWeight: FontWeight.bold,
						color: Colors.white,
					),
				),
				Padding(padding: EdgeInsets.all(10)),
				Text("Catat seluruh progress membaca bukumu disini!",
					// TODO: Tambahkan gaya teks dengan center alignment, font ukuran 15, warna putih, dan weight biasa
				),
			],
		),
	),
	...
	```

6. Selamat, kamu telah berhasil membuat _drawer menu_! Sekarang, masukkan drawer tersebut ke halaman yang ingin kamu tambahkan drawer. Untuk poin ini, kami akan berikan contoh untuk memasukkan ke halaman `menu.dart`.

	```dart
	...
	// Impor drawer widget
	import 'package:book_tracker/widgets/left_drawer.dart';
	...
	return Scaffold(
		appBar: AppBar(
			title: const Text(
				'Book Tracker',
			),
			backgroundColor: Colors.indigo,
			foregroundColor: Colors.white,
			),
			// Masukkan drawer sebagai parameter nilai drawer dari widget Scaffold
			drawer: const LeftDrawer(),
	...
	```

7. Selamat, drawer dan navigasi sudah dibuat secara sempurna. Silakan jalankan program untuk melihat hasilnya. Jangan lupa kerjakan `TODO` yang masih ada **sebelum mengumpulkan tutorial** (tutorial yang dikumpulkan sudah **tidak memiliki** `TODO`). **Jangan lupa** juga tambahkan drawer ke halaman `TrackerFormPage` jika halaman tersebut sudah dibuat.

## Tutorial: Menambahkan Form dan Elemen Input

Sekarang, kita akan membuat sebuah form sederhana untuk memasukkan data barang pada aplikasi sehingga nantinya kamu dapat menambahkan data baru untuk ditampilkan.

1. Buat berkas baru pada direktori `lib` dengan nama `trackerlist_form.dart`. Tambahkan kode berikut ke dalam berkas `trackerlist_form.dart`.

	```dart
	import 'package:flutter/material.dart';
	// TODO: Impor drawer yang sudah dibuat sebelumnya

	class TrackerFormPage extends StatefulWidget {
		const TrackerFormPage({super.key});

		@override
		State<TrackerFormPage> createState() => _TrackerFormPageState();
	}

	class _TrackerFormPageState extends State<TrackerFormPage> {
		@override
		Widget build(BuildContext context) {
			return Placeholder();
		}
	}
	```

2. Ubah widget `Placeholder` dengan potongan kode berikut.

	```dart
	Scaffold(
		appBar: AppBar(
			title: const Center(
				child: Text(
				'Form Tambah Buku',
				),
			),
			backgroundColor: Colors.indigo,
			foregroundColor: Colors.white,
			),
			// TODO: Tambahkan drawer yang sudah dibuat di sini
			body: Form(
			child: SingleChildScrollView(),
			),
		);
	```

	**Penjelasan Kode:**

	1. Widget `Form` berfungsi sebagai wadah bagi beberapa _input field widget_ yang nanti akan kita buat.

	2. Widget `SingleChildScrollView` berfungsi untuk membuat _child widget_ di dalamnya menjadi _scrollable_.

3. Buat variabel baru bernama `_formKey` dengan nilai `GlobalKey<FormState>();` lalu tambahkan `_formKey` tersebut ke dalam atribut `key` milik widget `Form`. Atribut `key` akan berfungsi sebagai handler dari form state, validasi form, dan penyimpanan form.

	```dart
	...
	class _TrackerFormPageState extends State<TrackerFormPage> {
		final _formKey = GlobalKey<FormState>();
	...
	```

	```dart
	...
	body: Form(
		key: _formKey,
		child: SingleChildScrollView(),
	),
	...
	```

4. Selanjutnya, kita akan mulai mengisi widget `Form` dengan _field_. Buatlah beberapa variabel untuk menyimpan input dari masing-masing _field_ yang akan dibuat.

	```dart
	...
	class _TrackerFormPageState extends State<TrackerFormPage> {
		final _formKey = GlobalKey<FormState>();
		String _name = "";
		int _page = 0;
		String _description = "";
	...
	```

5. Buatlah _widget_ `Column` sebagai _child_ dari `SingleChildScrollView`.

	```dart
	...
	body: Form(
		key: _formKey,
		child: SingleChildScrollView(
			child: Column()
		),
	...
	```

6. Buatlah widget `TextFormField` yang di-`wrap` oleh `Padding` sebagai salah satu _children_ dari widget `Column`. Setelah itu, tambahkan atribut `crossAxisAlignment` untuk mengatur alignment _children_ dari `Column`.

	```dart
	...
	child: Column(
		crossAxisAlignment: CrossAxisAlignment.start,
		children: [
			Padding(
				padding: const EdgeInsets.all(8.0),
				child: TextFormField(
					decoration: InputDecoration(
						hintText: "Judul Buku",
						labelText: "Judul Buku",
						border: OutlineInputBorder(
						borderRadius: BorderRadius.circular(5.0),
						),
					),
					onChanged: (String? value) {
						setState(() {
						_name = value!;
						});
					},
					validator: (String? value) {
						if (value == null || value.isEmpty) {
						return "Judul tidak boleh kosong!";
						}
						return null;
					},
				),
			),
	...
	```

	**Penjelasan Kode:**

	1. `onChanged` akan dijalankan setiap ada perubahan isi `TextFormField`.
	2. `validator` akan melakukan validasi isi `TextFormField` dan mengembalikan `String` jika terdapat error.
	3. Terdapat implementasi null-safety pada bagian `String?` dan `value!`. Operator `?` berfungsi untuk menandakan bahwa variabel tersebut boleh berisi `String` atau `null`. Sedangkan operator `!` berfungsi untuk menandakan bahwa variabel tersebut dijamin akan tidak akan berisi `null`.

	Untuk mempelajari lebih dalam mengenai `null-safety`, kalian dapat membaca dokumentasi yang ada pada tautan berikut: [Dart Null Safety](https://dart.dev/null-safety/understanding-null-safety)

7. Buatlah dua `TextFormField` yang di-_wrap_ dengan `Padding` sebagai _child_ selanjutnya dari `Column` seperti sebelumnya untuk field `page` dan `description`.

	```dart
	...
	Padding(
		padding: const EdgeInsets.all(8.0),
		child: TextFormField(
			decoration: InputDecoration(
				hintText: "Halaman",
				labelText: "Halaman",
				border: OutlineInputBorder(
				borderRadius: BorderRadius.circular(5.0),
				),
			),
			onChanged: (String? value) {
				setState(() {
				// TODO: Tambahkan variabel yang sesuai
				... = int.tryParse(value!) ?? 0;
				});
			},
			validator: (String? value) {
				if (value == null || value.isEmpty) {
					return "Halaman tidak boleh kosong!";
				}
				if (int.tryParse(value) == null) {
					return "Halaman harus berupa angka!";
				}
				return null;
			},
		),
	),
	Padding(
		padding: const EdgeInsets.all(8.0),
		child: TextFormField(
			decoration: InputDecoration(
				hintText: "Deskripsi",
				labelText: "Deskripsi",
				border: OutlineInputBorder(
					borderRadius: BorderRadius.circular(5.0),
				),
			),
			onChanged: (String? value) {
				setState(() {
				// TODO: Tambahkan variabel yang sesuai
				... = value!;
				});
			},
			validator: (String? value) {
				if (value == null || value.isEmpty) {
				return "Deskripsi tidak boleh kosong!";
				}
				return null;
			},
		),
	),
	...
	```

8. Buatlah tombol sebagai _child_ selanjutnya dari `Column`. Bungkus tombol ke dalam widget `Padding` dan `Align`. Kali ini kita belum menyimpan data ke dalam _database_, namun kita akan memunculkannya pada _pop-up_ yang akan muncul setelah tombol ditekan.

	```dart
	...
	Align(
		alignment: Alignment.bottomCenter,
		child: Padding(
			padding: const EdgeInsets.all(8.0),
			child: ElevatedButton(
				style: ButtonStyle(
					backgroundColor: MaterialStateProperty.all(Colors.indigo),
				),
				onPressed: () {
					if (_formKey.currentState!.validate()) {}
				},
				child: const Text(
					"Save",
					style: TextStyle(color: Colors.white),
				),
			),
		),
	),
	...
	```

## Tutorial: Memunculkan Data

1. Tambahkan fungsi `showDialog()` pada bagian `onPressed()` dari potongan kode yang sebelumnya kamu tambahkan. Munculkan widget `AlertDialog` pada fungsi tersebut. Kemudian, tambahkan juga fungsi untuk _reset_ form. Kode kalian akan terlihat seperti ini.

	```dart
	...
	child: ElevatedButton(
		style: ButtonStyle(
			backgroundColor:
				MaterialStateProperty.all(Colors.indigo),
			),
			onPressed: () {
				if (_formKey.currentState!.validate()) {
					showDialog(
						context: context,
						builder: (context) {
							return AlertDialog(
								title: const Text('Buku berhasil tersimpan'),
								content: SingleChildScrollView(
									child: Column(
										crossAxisAlignment:
											CrossAxisAlignment.start,
										children: [
											Text('Judul: $_name'),
											// TODO: Munculkan value-value lainnya
										],
									),
								),
								actions: [
									TextButton(
										child: const Text('OK'),
										onPressed: () {
											Navigator.pop(context);
											_formKey.currentState!.reset();
										},
									),
								],
							);
						},
					);
				}
			},
			child: const Text(
				"Save",
				style: TextStyle(color: Colors.white),
			),
		),
	...
	```

2. Silakan coba jalankan program kalian dan gunakan form yang telah dibuat, kemudian lihat hasilnya. **Jangan lupa untuk terlebih dahulu menambahkan routing pada drawer untuk bisa mengakses form yang telah kalian buat.**

## Tutorial: Menambahkan Fitur Navigasi pada Tombol

Sampai sini, kita sudah berhasil membuat suatu _drawer_ yang dapat menjalankan fitur navigasi ke halaman lain pada aplikasi, serta suatu halaman _form_. Pada tutorial sebelumnya, kita juga sudah berhasil membuat tiga _button widget_ yang dapat melakukan _action_ tertentu saat ia ditekan. Sekarang, kita akan menambahkan fitur navigasi pada tombol tersebut sehingga saat ditekan, pengguna akan ditampilkan halaman lain.

1. Pada _widget_ `TrackerItem` pada berkas `menu.dart` yang sudah dibuat pada tutorial sebelumnya, akan dibuat agar kode yang terletak pada atribut `onTap` dari `InkWell` dapat melakukan navigasi ke _route_ lain (tambahkan kode tambahan di bawah kode `ScaffoldMessenger` yang menampilkan _snackbar_).

	```dart
	...
	// Area responsif terhadap sentuhan
	onTap: () {
		// Memunculkan SnackBar ketika diklik
		ScaffoldMessenger.of(context)
			..hideCurrentSnackBar()
			..showSnackBar(SnackBar(
				content: Text("Kamu telah menekan tombol ${item.name}!")));

		// Navigate ke route yang sesuai (tergantung jenis tombol)
		if (item.name == "Tambah Buku") {
			// TODO: Gunakan Navigator.push untuk melakukan navigasi ke MaterialPageRoute yang mencakup TrackerFormPage.
		}
	},
	...
	```

   	Perhatikan bahwa pada tombol ini, kita menggunakan `Navigator.push()` sehingga pengguna dapat menekan tombol **Back** untuk kembali ke halaman menu. Selain itu, jika kita menggunakan `Navigator.pop()`, maka kita dapat membuat kode dalam program untuk kembali ke halaman menu.

2. Coba jalankan program kamu, gunakan tombol yang telah dibuat fungsionalitasnya, dan lihatlah apa yang terjadi. Bandingkan dengan apa yang terjadi jika kita melakukan navigasi melalui drawer (tentu saja setelah menyelesaikan seluruh TODO pada drawer).

## Tutorial: _Refactoring File_

Setelah membuat halaman `trackerlist_form.dart`, halaman kita sudah semakin banyak. Dengan demikian, mari kita pindahkan halaman-halaman yang sudah dibuat sebelumnya ke dalam satu folder `screens` untuk mempermudah kita ke depannya.

1. Sebelum mulai, pastikan kamu sudah memiliki **ekstensi Flutter terinstal** di IDE atau _text editor_ yang kamu gunakan.

2. Buatlah berkas baru dengan nama `tracker_card.dart` pada direktori `widgets`.

3. Pindahkan isi widget `TrackerItem` dan `TrackerCard` pada `menu.dart` ke berkas `widgets/tracker_card.dart`.

4. Pastikan untuk mengimpor halaman `trackerlist_form.dart` pada berkas `tracker_card.dart` dan import halaman `tracker_card.dart` pada berkas `menu.dart`.

5. Buatlah folder baru bernama `screens` pada direktori `lib`.

6. Pindahkan file `menu.dart` dan `trackerlist_form.dart` ke dalam folder `screens`. Pastikan pemindahan file dilakukan **melalui IDE atau _text editor_ yang memiliki ekstensi atau _plugin_ Flutter**, bukan melalui _file manager_ biasa (seperti File Explorer atau Finder). Hal ini dilakukan agar IDE atau _text editor_ yang kamu gunakan dapat melakukan _refactoring_ secara otomatis.

Setelah _refactoring file_ dilakukan, seharusnya struktur dari direktori `lib` adalah seperti berikut.

![Struktur file di akhir](lib_structure.png)

## Akhir Kata

Selamat! Kamu telah menyelesaikan Tutorial 7! Semoga dengan tutorial ini, kalian dapat memahami mengenai _navigation_, _forms_, _input_, dan _layouts_ dengan baik. 😄

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

- [Flutter Navigation Basics Cookbook](https://docs.flutter.dev/cookbook/navigation/navigation-basics)
- [Add Drawer to a Screen in Flutter](https://docs.flutter.dev/cookbook/design/drawer)

## Kontributor

- Muhammad Nabil Mu'afa
- Muhammad Iqbal Dwitama

## Credits

Tutorial ini dikembangkan berdasarkan [PBP Ganjil 2024](https://github.com/pbp-fasilkom-ui/ganjil-2024) yang ditulis oleh Tim Pengajar Pemrograman Berbasis Platform 2024. Segala tutorial serta instruksi yang dicantumkan pada repositori ini dirancang sedemikian rupa sehingga mahasiswa yang sedang mengambil mata kuliah Pemrograman Berbasis Platform dapat menyelesaikan tutorial saat sesi lab berlangsung.
