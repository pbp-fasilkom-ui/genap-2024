# Tutorial 1: Pengenalan Aplikasi Django dan _Model-View-Template_ (MVT) pada Django

Pemrograman Berbasis Platform (CSGE602022) — diselenggarakan oleh Fakultas Ilmu Komputer Universitas Indonesia, Semester Genap 2023/2024

---

## Tujuan Pembelajaran

Setelah menyelesaikan tutorial ini, mahasiswa diharapkan untuk dapat:

- Mengerti konsep MVT pada aplikasi Django
- Mengerti bagaimana alur Django menampilkan sebuah halaman HTML
- Mengerti konfigurasi _routing_ yang ada pada `urls.py`
- Memahami kaitan _models_, _views_ dan _template_ pada Django
- Memahami pembuatan _unit test_ pada _framework_ Django

## Ringkasan Hasil Tutorial 0

Untuk membantumu mengerjakan tutorial 1 dengan baik, kami mengharapkan hasil pengerjaan tutorial 0 sebagai berikut.

1. Pada komputer lokal, terdapat direktori utama `book-tracker` yang telah diinisiasi sebagai repositori lokal.
2. Pada direktori utama `book-tracker` tersebut, terdapat beberapa berkas dan subdirektori berikut.

   - Subdirektori `env`.
   - Subdirektori proyek `book_tracker`. Berbeda dengan direktori utama, subdirektori ini terbentuk setelah menjalankan perintah

     ```bash
     django-admin startproject book_tracker .
     ```

   - Berkas `.gitignore`.
   - Berkas `manage.py`.
   - Berkas `requirements.txt`.
   - (Opsional) Berkas `db.sqlite3`.

   Struktur proyek `book-tracker` pada direktori lokal adalah sebagai berikut.

   ![Struktur Direktori Lokal](https://media.discordapp.net/attachments/1133956580728127550/1203381902141296740/image.png?ex=65d0e3bb&is=65be6ebb&hm=200cc2f7bf3d40278be86246531d714577743507c2baee953ce89298423433da&=&format=webp&quality=lossless)

3. Pada repositori GitHub, pastikan repositori `book-tracker` memiliki berkas dan direktori berikut.

   - Direktori proyek `book_tracker`. Direktori ini hasil menjalankan perintah

     ```bash
     django-admin startproject book_tracker .
     ```

   - Berkas `.gitignore`.
   - Berkas `manage.py`.
   - Berkas `requirements.txt`.

   Struktur proyek `book-tracker` pada repositori GitHub adalah sebagai berikut.

   ![Struktur Repositori Github](https://media.discordapp.net/attachments/1133956580728127550/1203382472805449789/image.png?ex=65d0e443&is=65be6f43&hm=286a1e40dd7bcd6e686a3cbc551315d3a6fa87ea4770cc25f90226fbede863e9&=&format=webp&quality=lossless)

## Pengenalan Mengenai Konsep MVT

Dalam dunia pengembangan web, terdapat berbagai konsep dan arsitektur yang membantu dalam merancang dan mengembangkan aplikasi. Salah satu konsep yang umum digunakan adalah MVT (_Model-View-Template_).

### Apa Itu Konsep MVT?

![MVT Diagram](https://cdn.discordapp.com/attachments/1142468662461214771/1146996248268775484/3._Python_Django_-_Modul_2_Page2_Image5.jpg)

**MVT** adalah singkatan dari **_Model-View-Template_**. MVT adalah sebuah konsep arsitektur yang digunakan dalam pengembangan web untuk memisahkan komponen-komponen utama dari sebuah aplikasi. Konsep ini memungkinkan pengembang web untuk mengorganisasi dan mengelola kode dengan lebih terstruktur.

### Apa Itu Model?

**_Model_** merupakan komponen dalam konsep MVT yang bertanggung jawab untuk mengatur dan mengelola data dari aplikasi. Model mewakili struktur data dan logika aplikasi yang berada di belakang tampilan. Model menghubungkan aplikasi dengan basis data dan mengatur interaksi dengan data tersebut.

### Apa Itu _View_?

**_View_** merupakan komponen yang menangani logika presentasi dalam konsep MVT. _View_ mengontrol bagaimana data yang dikelola oleh model akan ditampilkan kepada pengguna. Dalam konteks MVT, _view_ berperan sebagai pengatur tampilan dan mengambil data dari model untuk disajikan kepada pengguna.

### Apa Itu _Template_?

**_Template_** adalah komponen yang berfungsi untuk mengatur tampilan atau antarmuka pengguna. _Template_ memisahkan kode HTML dari logika aplikasi. Dalam MVT, _template_ digunakan untuk merancang tampilan yang akhirnya akan diisi dengan data dari model melalui _view_.

### Hubungan Antara Komponen-komponen MVT

Secara ringkas, konsep MVT berjalan dalam kerangka berikut:

- **_Model_**: Menyimpan data dan logika aplikasi.
- **_View_**: Menampilkan data dari model dan menghubungkannya dengan _template_.
- **_Template_**: Menentukan tampilan antarmuka pengguna.

### Manfaat MVT

- **Pemisahan Tugas**

  MVT memisahkan tugas antara logika aplikasi, tampilan, dan data, sehingga memungkinkan pengembang untuk bekerja pada setiap komponen secara terpisah.

- **Kode yang Mudah Dikelola**

  Dengan pemisahan tugas yang jelas, kode menjadi lebih terorganisir dan mudah dikelola.

- **Penggunaan Kembali**

  Kode dapat digunakan kembali dalam berbagai bagian aplikasi yang berbeda.

- **Skalabilitas**

  Struktur MVT mendukung skalabilitas dengan memungkinkan pengembangan paralel pada setiap komponen.

**Catatan:**

- Konsep MVT sangat terkait dengan _framework_ Django dalam pengembangan web dengan bahasa pemrograman Python.
- Dalam praktiknya, pemahaman yang baik mengenai konsep MVT akan membantu kamu dalam merancang aplikasi web yang lebih terstruktur dan mudah dikelola.

## Tutorial: Membuat Aplikasi Django beserta Konfigurasi Model

Dalam tutorial ini, akan dijelaskan mengenai konsep aplikasi dan proyek dalam Django.

**Apa Itu Proyek dan Aplikasi dalam Django?**

- **Proyek (_Project_)** adalah keseluruhan proyek web yang kamu bangun dengan menggunakan Django. **Proyek berisi berbagai aplikasi** yang berfungsi secara bersama untuk menciptakan situs web atau aplikasi web yang lengkap.

- **Aplikasi (_Apps_)** adalah unit modular yang melakukan tugas-tugas spesifik dalam suatu proyek Django. Setiap aplikasi dapat memiliki model, tampilan, _template_, dan URL yang terkait dengannya. Aplikasi memungkinkanmu untuk membagi fungsionalitas proyek menjadi bagian-bagian terpisah yang dapat dikelola secara independen.

Sebelum dimulai, kamu perlu mengingat kembali bahwa direktori utama adalah direktori **terluar**, sedangkan direktori proyek adalah direktori **di dalam** direktori utama. Perlu diingat bahwa keduanya memiliki nama yang hampir sama. Direktori utama memiliki nama **`book-tracker`**, sementara itu direktori proyek memiliki nama **`book_tracker`**. Perbedaan hanya terletak pada penggunaan _dash_ (`-`) dan _underscore_ (`_`).

### Langkah 1: Persiapan Awal

1. Buka direktori utama **`book-tracker`**.

   - Sebelum memulai, pastikan kamu berada di direktori utama **`book-tracker`** yang telah dibuat pada tutorial sebelumnya.
   - Di dalam direktori ini, kamu akan melanjutkan pengembangan proyek Django.

2. Buka terminal atau _command prompt_ dan pastikan direktori kerja kamu adalah direktori utama **`book-tracker`**.

3. Aktifkan _virtual environment_ yang telah dibuat sebelumnya. Jalankan perintah berikut sesuai dengan sistem operasi perangkat kamu.

   - **Windows:**

     ```bash
     env\Scripts\activate.bat
     ```

   - **Unix (Linux & Mac OS):**

     ```bash
     source env/bin/activate
     ```

### Langkah 2: Membuat Aplikasi `main` dalam Proyek _Book Tracker_

Kamu akan membuat aplikasi baru bernama `main` dalam proyek _book tracker_.

1. Jalankan perintah berikut untuk membuat aplikasi baru.

   ```shell
   python manage.py startapp main
   ```

   Setelah perintah di atas dijalankan, akan terbentuk direktori baru dengan nama `main` yang akan berisi struktur awal untuk **aplikasimu**.

   > Mungkin kamu cukup bingung dengan istilah direktori **utama**, direktori **proyek**, atau direktori **aplikasi**. Akan tetapi, seiring berjalannya waktu, kamu pasti bisa!

2. Daftarkan aplikasi `main` ke dalam proyek.

   - Buka berkas `settings.py` di dalam direktori proyek `book_tracker`.
   - Temukan variabel `INSTALLED_APPS`.
   - Tambahkan `'main'` ke dalam daftar aplikasi yang ada.

     ```python
     INSTALLED_APPS = [
         ...,
         'main',
         ...
     ]
     ```

Dengan melakukan langkah-langkah tersebut, kamu telah mendaftarkan aplikasi `main` ke dalam proyek _book tracker_ kamu sehingga proyekmu sudah mengenali bahwa terdapat aplikasi `main` yang dapat dijalankan.

## Tutorial: Implementasi _Template_ Dasar

Kamu akan membuat _template_ `main.html` di dalam direktori baru bernama `templates` pada aplikasi `main`. _Template_ ini akan digunakan untuk menampilkan data _book tracker_ kamu.

> Saat ini, aplikasi _book tracker_ belum menampilkan data apapun. Data akan ditampilkan pada tutorial selanjutnya.

### Langkah 1: Membuat dan Mengisi Berkas `main.html`

Sebelum mulai, mari pahami secara singkat mengenai HTML. HTML (_Hypertext Markup Language_) adalah penanda bahasa yang digunakan untuk membuat struktur dan tampilan konten pada halaman web.

> Hint: Kamu akan mempelajari HTML lebih lanjut di tutorial 4.

1. **Buat direktori baru** bernama `templates` di dalam direktori aplikasi `main`.

2. Di dalam direktori `templates`, **buat berkas baru** bernama `main.html` dengan isi sebagai berikut. Gantilah nilai yang sesuai dengan data kamu.

   ```html
   <h1>Book Tracker Page</h1>

   <h5>Name:</h5>
   <p>Pak Bepe</p>
   <!-- Ubahlah sesuai dengan nama kamu -->
   <h5>Class:</h5>
   <p>PBP A</p>
   <!-- Ubahlah sesuai dengan kelas kamu -->
   ```

3. Buka berkas HTML di _web browser_.

   - Sebelum dihubungkan dengan aplikasi, cobalah membuka berkas `main.html` di _web browser_-mu.
   - Perlu dicatat bahwa pada tahap ini **hanya untuk memeriksa** tampilan dasar HTML dan **belum terhubung dengan Django.**
   - Berikut merupakan contoh tampilan HTML yang diharapkan.

     ![main.html](https://media.discordapp.net/attachments/1133956580728127550/1203386239424921610/image.png?ex=65d0e7c5&is=65be72c5&hm=14853f354f14337fa0459ad5f7d569817ecb7eae18b82c773e44812d095a6ae1&=&format=webp&quality=lossless)

## Tutorial: Implementasi Model Dasar

### Langkah 1: Mengubah Berkas `models.py` dalam Aplikasi `main`

Pada langkah ini, kamu akan mengubah berkas `models.py` yang terdapat di dalam direktori aplikasi `main` untuk mendefinisikan model baru.

1. Buka berkas `models.py` pada direktori aplikasi `main`.

2. Isi berkas `models.py` dengan kode berikut.

   ```python
   from django.db import models

   class Book(models.Model):
       name = models.CharField(max_length=255)
       date_added = models.DateField(auto_now_add=True)
       page = models.IntegerField()
       description = models.TextField()
   ```

   **Penjelasan Kode:**

   - `models.Model` adalah kelas dasar yang digunakan untuk mendefinisikan model dalam Django.
   - `Book` adalah nama model yang kamu definisikan.
   - `name` (nama), `date_added` (tanggal tambah), `page` (halaman), dan `description` (deskripsi) adalah atribut atau _field_ pada model. Setiap _field_ memiliki tipe data yang sesuai seperti `CharField`, `DateField`, `IntegerField`, dan `TextField`.

### Langkah 2: Membuat dan Mengaplikasikan Migrasi Model

**Apa itu migrasi model?**

- Migrasi model adalah cara Django melacak perubahan pada model basis data kamu.
- Migrasi ini adalah instruksi untuk mengubah struktur tabel basis data sesuai dengan perubahan model yang didefinisikan dalam kodemu.

**Bagaimana cara melakukan migrasi model?**

1. Jalankan perintah berikut pada terminal atau _command prompt_ di **direktori utama** untuk membuat migrasi model.

   ```shell
   python manage.py makemigrations
   ```

   > `makemigrations` menciptakan berkas migrasi yang berisi perubahan model yang belum diaplikasikan ke dalam basis data.

2. Masih di **direktori utama**, jalankan perintah berikut untuk menerapkan migrasi ke dalam basis data lokal.

   ```shell
   python manage.py migrate
   ```

   > `migrate` mengaplikasikan perubahan model yang tercantum dalam berkas migrasi ke basis data.

**Setiap kali kamu melakukan perubahan pada _model_**, seperti menambahkan atau mengubah atribut, **kamu perlu melakukan migrasi** untuk merefleksikan perubahan tersebut.

## Tutorial: Menghubungkan _View_ dengan _Template_

Dalam tutorial ini, kamu akan menghubungkan _view_ dengan _template_ menggunakan Django. Langkah-langkah ini akan menjelaskan bagaimana membuat fungsi _view_ `show_main` dalam aplikasi `main` dan me-_render_ _template_ dengan data yang telah diambil dari model.

### Langkah 1: Mengintegrasikan Komponen MVT

Kamu akan mengimpor modul yang diperlukan dan membuat fungsi _view_ `show_main`.

1. Buka berkas `views.py` yang terletak di dalam berkas aplikasi `main`.

2. Tambahkan baris-baris impor berikut di bagian paling atas berkas apabila belum ada.

   ```python
   from django.shortcuts import render
   ```

   **Penjelasan Kode:**

   `from django.shortcuts import render` berguna untuk mengimpor fungsi _render_ dari modul `django.shortcuts`. Fungsi _render_ digunakan untuk me-_render_ tampilan HTML dengan menggunakan data yang diberikan.

3. Tambahkan fungsi `show_main` di bawah impor:

   ```python
   def show_main(request):
       context = {
           'name': 'Pak Bepe',
           'class': 'PBP A'
       }

       return render(request, "main.html", context)
   ```

   **Penjelasan Kode:**

   - `def show_main(request)` merupakan deklarasi fungsi `show_main`, yang menerima parameter `request`. Fungsi ini akan mengatur permintaan HTTP dan mengembalikan tampilan yang sesuai.
   - `context` adalah _dictionary_ yang berisi data yang akan dikirimkan ke tampilan. Pada konteks ini, dua data disertakan, yaitu:

     - `name`: Data nama.
     - `class`: Data kelas.

   - `return render(request, "main.html", context)` berguna untuk me-_render_ tampilan `main.html` dengan menggunakan fungsi `render`. Fungsi `render` mengambil tiga argumen:

     - `request`: Ini adalah objek permintaan HTTP yang dikirim oleh pengguna.
     - `main.html`: Ini adalah nama berkas _template_ yang akan digunakan untuk me-_render_ tampilan.
     - `context`: Ini adalah _dictionary_ yang berisi data yang akan diteruskan ke tampilan untuk digunakan dalam penampilan dinamis.

## Langkah 2: Modifikasi _Template_

Sekarang, kamu akan mengubah _template_ `main.html` agar dapat menampilkan data yang telah diambil dari _model_.

1. Buka berkas `main.html` yang telah dibuat sebelumnya dalam direktori `templates` pada direktori `main`.

2. Ubah nama dan kelas yang sebelumnya dibuat secara statis menjadi kode Django yang sesuai untuk menampilkan data.

   ```html
   ...
   <h5>Name:</h5>
   <p>{{ name }}</p>
   <p></p>
   <h5>Class:</h5>
   <p>{{ class }}</p>
   ```

   **Penjelasan Kode:**

   Sintaks Django `{{ name }}` dan `{{ class }}` digunakan untuk menampilkan nilai dari variabel yang telah didefinisikan dalam `context`.

## Tutorial: Mengonfigurasi _Routing_ URL

Kamu akan mengatur _routing_ URL agar aplikasi `main` dapat diakses melalui _web browser_.

### Langkah 1: Mengonfigurasi _Routing_ URL Aplikasi `main`

1. Buatlah berkas **baru** bernama`urls.py` **di dalam** direktori `main`.
2. Isi `urls.py` dengan kode berikut.

   ```python
   from django.urls import path
   from main.views import show_main

   app_name = 'main'

   urlpatterns = [
       path('', show_main, name='show_main'),
   ]
   ```

   **Penjelasan Kode dalam `urls.py` pada Aplikasi `main`:**

   - Berkas `urls.py` pada aplikasi `main` bertanggung jawab untuk mengatur rute URL yang terkait dengan aplikasi `main`.
   - Impor `path` dari `django.urls` untuk mendefinisikan pola URL.
   - Gunakan fungsi `show_main` dari modul `main.views` sebagai tampilan yang akan ditampilkan ketika URL terkait diakses.
   - Nama `app_name` diberikan untuk memberikan nama unik pada pola URL dalam aplikasi.

### Langkah 2: Mengonfigurasi _Routing_ URL Proyek

Kamu akan menambahkan rute URL dalam `urls.py` **pada proyek** untuk menghubungkannya ke tampilan `main`.

1. Buka berkas `urls.py` **di dalam direktori proyek `book_tracker`, bukan yang ada di dalam direktori aplikasi `main`**.
2. Impor fungsi `include` dari `django.urls`.

   ```python
   ...
   from django.urls import path, include
   ...
   ```

3. Tambahkan rute URL seperti berikut untuk mengarahkan ke tampilan `main` di dalam variabel `urlpatterns`.

   ```python
   urlpatterns = [
       ...
       path('', include('main.urls')),
       ...
   ]
   ```

   **Penjelasan:**

   - Berkas `urls.py` pada proyek bertanggung jawab untuk mengatur rute URL dalam skala proyek.
   - Fungsi `include` digunakan untuk mengimpor rute URL dari aplikasi lain (dalam hal ini, dari aplikasi `main`) ke dalam berkas `urls.py` proyek.
   - _Path_ URL `''` akan diarahkan ke rute yang didefinisikan dalam berkas `urls.py` aplikasi `main`. Path URL dibiarkan berupa string kosong agar halaman aplikasi `main` dapat diakses secara langsung.

> Sebagai bayangan, apabila kamu menggunakan _path_ `'main/'` pada contoh di atas, maka kamu perlu mengakses halaman `http://localhost:8000/main/` untuk mengakses halaman aplikasi `main`. Karena _path_ yang ditentukan adalah `''`, maka kamu dapat mengakses aplikasi `main` melalui URL `http://localhost:8000/` saja.

4. Jalankan proyek Django dengan menjalankan perintah `python manage.py runserver` pada terminal atau _command prompt_ di direktori utama.

5. Bukalah <http://localhost:8000/> di _web browser_ favoritmu untuk melihat halaman yang sudah kamu buat.

Dengan langkah-langkah di atas, kamu telah berhasil mengimplementasikan tampilan dasar dalam aplikasi `main` dan menghubungkannya dengan rute URL proyek. Pastikan kamu memahami setiap langkah dan informasi yang diberikan untuk mengaktifkan tampilan dalam proyek Django-mu

### Apa bedanya `urls.py` pada aplikasi dan `urls.py` pada proyek?

- Berkas `urls.py` pada aplikasi mengatur rute URL spesifik untuk fitur-fitur dalam aplikasi tersebut
- `urls.py` pada proyek mengarahkan rute URL tingkat proyek dan dapat mengimpor rute URL dari berkas `urls.py` aplikasi-aplikasi, memungkinkan aplikasi dalam proyek Django untuk bersifat modular dan terpisah.

## Tutorial: Pengenalan Django _Unit Testing_

_Unit testing_ dapat digunakan untuk mengecek apakah kode yang dibuat bekerja sesuai dengan keinginan. Hal ini juga berguna ketika kamu melakukan perubahan kode. Dengan menggunakan tes, kamu bisa mengecek apakah perubahan yang dilakukan dapat menyebabkan perilaku yang tidak diinginkan pada aplikasi.

### Langkah 1: Membuat Unit _Test_

1. Bukalah berkas `tests.py` pada direktori aplikasi `main`.
2. Isi `tests.py` dengan kode berikut.

   ```python
   from django.test import TestCase, Client

   class mainTest(TestCase):
       def test_main_url_is_exist(self):
           response = Client().get('')
           self.assertEqual(response.status_code, 200)

       def test_main_using_main_template(self):
           response = Client().get('')
           self.assertTemplateUsed(response, 'main.html')
   ```

   **Penjelasan:**

   - `test_main_url_is_exist` adalah tes untuk mengecek apakah _path_ URL `''` dapat diakses. _Path_ `''` artinya hanya `http://localhost:8000/` tanpa _path_ tambahan di bagian belakang URL.
   - `test_main_using_main_template` adalah tes untuk mengecek apakah halaman `''` di-_render_ menggunakan _template_ `main.html`.

### Langkah 2: Menjalankan _Test_

1. Jalankan tes dengan menggunakan perintah berikut.

   ```shell
   python manage.py test
   ```

2. Jika tes berhasil, akan mengeluarkan informasi berikut.

   ```text
   Found 2 test(s).
    Creating test database for alias 'default'...
    System check identified no issues (0 silenced).
    ..
    ----------------------------------------------------------------------
    Ran 2 tests in 0.006s

    OK
    Destroying test database for alias 'default'...
   ```

**Selamat!** Kamu telah berhasil menulis Django _Test_ dan menjalankannya.

## Penutup

1. Pada akhir tutorial ini, struktur direktori lokalmu akan terlihat seperti ini.

   ![Struktur Direktori Lokal](https://media.discordapp.net/attachments/1133956580728127550/1203392570441601144/image.png?ex=65d0edaa&is=65be78aa&hm=5823abe8aaa676b508f7ce69333a78f18667f9c1ced302b9a5656ea7dd4a77a3&=&format=webp&quality=lossless)

   > Huruf U menandakan bahwa terdapat berkas baru yang belum dilacak oleh Git. Huruf M menandakan bahwa terdapat berkas yang telah dilacak sebelumnya yang dimodifikasi.

2. Sebelum melakukan langkah ini, **pastikan struktur direktori lokal sudah benar**. Selanjunya, lakukan `add`, `commit` dan `push` untuk memperbarui repositori GitHub.
3. Jalankan perintah berikut untuk melakukan `add`, `commit` dan `push`.

   ```shell
   git add .
   git commit -m "<pesan_commit>"
   git push origin <branch_utama>
   ```

   - Ubah `<pesan_commit>` sesuai dengan keinginan. Contoh: `git commit -m "tutorial 1 selesai"`.
   - Ubah `<branch_utama>` sesuai dengan nama branch utamamu. Contoh: `git push origin main` atau `git push origin master`.

4. Berikut struktur direktori GitHub setelah kamu menyelesaikan tutorial ini.

   ![Struktur Repositori Github](https://media.discordapp.net/attachments/1133956580728127550/1203394340903198780/image.png?ex=65d0ef50&is=65be7a50&hm=e715e7cbe57c194f9f88b25126841b4a52ce0fb2dd74a8190bedb9850c9057d5&=&format=webp&quality=lossless&width=255&height=543)

## Referensi Tambahan

- [Django Unit Testing](https://docs.djangoproject.com/en/4.2/topics/testing/)

## Kontributor

- Muhammad Nabil Mu'afa
- Muhammad Iqbal Dwitama

## Credits

Tutorial ini dikembangkan berdasarkan [PBP Ganjil 2024](https://github.com/pbp-fasilkom-ui/ganjil-2024) yang ditulis oleh Tim Pengajar Pemrograman Berbasis Platform 2023/2024. Segala tutorial serta instruksi yang dicantumkan pada repositori ini dirancang sedemikian rupa sehingga mahasiswa yang sedang mengambil mata kuliah Pemrograman Berbasis Platform dapat menyelesaikan tutorial saat sesi lab berlangsung.