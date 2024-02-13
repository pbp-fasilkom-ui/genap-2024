# Tutorial 2: Form dan Data Delivery

Pemrograman Berbasis Platform (CSGE602022) — diselenggarakan oleh Fakultas Ilmu Komputer Universitas Indonesia, Semester Genap 2023/2024

---

## Tujuan Pembelajaran​

Setelah menyelesaikan tutorial ini, mahasiswa diharapkan untuk dapat:

- Mengetahui `XML` dan `JSON` sebagai salah satu metode _data delivery_
- Memahami cara kerja submisi data yang diberikan oleh pengguna menggunakan elemen `form`
- Memahami cara mengirimkan data menggunakan format `XML` dan `JSON`
- Memahami cara mengambil data spesifik berdasarkan `ID`

## Pengenalan Data Delivery

Dalam mengembangkan suatu _platform_, ada kalanya kita perlu mengirimkan data dari satu _stack_ ke _stack_ lainnya. Data yang dikirimkan bisa bermacam-macam bentuknya. Beberapa contoh format data yang umum digunakan antara lain HTML, XML, dan JSON. Implementasi _data delivery_ dalam bentuk HTML sudah kamu pelajari pada tutorial sebelumnya. Pada tutorial ini akan diajarkan terkait XML dan JSON.

## XML (Extensible Markup Language)

XML adalah singkatan dari _eXtensible Markup Language_. XML didesain menjadi _self-descriptive_, jadi dengan membaca XML tersebut kita bisa mengerti informasi apa yang ingin disampaikan dari data yang tertulis. XML digunakan pada banyak aplikasi web maupun _mobile_, yaitu untuk menyimpan dan mengirimkan data. XML hanya berisi informasi yang dibungkus di dalam _tag_. Kita perlu menulis program untuk mengirim, menerima, menyimpan, atau menampilkan informasi tersebut.

Contoh Format XML:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<person>
    <name>Alice Johnson</name>
    <age>25</age>
    <address>
        <street>123 Main St</street>
        <city>Los Angeles</city>
        <zip>90001</zip>
    </address>
</person>
```

XML di atas sangatlah _self-descriptive_:

- Ada informasi nama (`name`)
- Ada informasi umur (`age`)
- Ada informasi alamat (`address`)
  - Ada informasi jalan (`street`)
  - Ada informasi kota (`city`)
  - Ada informasi kode pos (`zip`)

Dokumen XML membentuk struktur seperti _tree_ yang dimulai dari _root_, lalu _branch_, hingga berakhir pada _leaves_. Dokumen XML **harus mengandung sebuah _root element_** yang merupakan _parent_ dari elemen lainnya. Pada contoh di atas, `<person>` adalah _root element_.

Baris `<?xml version="1.0" encoding="UTF-8"?>` biasanya disebut sebagai **XML Prolog**. XML Prolog bersifat opsional, akan tetapi jika ada maka posisinya harus berada di awal dokumen XML. Pada dokumen XML, **semua elemen wajib memiliki** **closing tag**. **Tag** pada XML sifatnya **case sensitive**, sehingga tag `<person>` dianggap **berbeda** dengan tag `<Person>`.

## JSON (JavaScript Object Notation)

JSON adalah singkatan dari _JavaScript Object Notation_. JSON didesain menjadi _self-describing_, sehingga JSON sangat mudah untuk dimengerti. JSON digunakan pada banyak aplikasi web maupun _mobile_, yaitu untuk menyimpan dan mengirimkan data. Sintaks JSON merupakan turunan dari _Object_ JavaScript. Akan tetapi format JSON berbentuk _text_, sehingga kode untuk membaca dan membuat JSON banyak terdapat dibanyak bahasa pemrograman.

Contoh format JSON:

```json
{
  "name": "Alice Johnson",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "Los Angeles",
    "zip": "90001"
  }
}
```

Data pada JSON disimpan dalam bentuk _key_ dan _value_. Pada contoh di atas yang menjadi _key_ adalah `name`, `age`, dan `address`. _Value_ dapat berupa tipe data primitif (_string, number, boolean_) ataupun berupa objek.

## Pre-Tutorial Notes

Sebelum kamu memulai, serta untuk membantumu mengikuti tutorial 2 dengan baik, kami mengharapkan beberapa hasil berikut dari tutorial 1:

- Struktur direktori `book-tracker` secara lokal adalah sebagai berikut.

![Struktur direktori lokal](https://media.discordapp.net/attachments/1133956580728127550/1203392570441601144/image.png?ex=65d0edaa&is=65be78aa&hm=5823abe8aaa676b508f7ce69333a78f18667f9c1ced302b9a5656ea7dd4a77a3&=&format=webp&quality=lossless)

- Struktur repository `book-tracker` pada GitHub adalah sebagai berikut.

![Struktur Repositori Github](https://media.discordapp.net/attachments/1133956580728127550/1203394340903198780/image.png?ex=65d0ef50&is=65be7a50&hm=e715e7cbe57c194f9f88b25126841b4a52ce0fb2dd74a8190bedb9850c9057d5&=&format=webp&quality=lossless&width=255&height=543)

## Tutorial: Implementasi _Skeleton_ sebagai Kerangka _Views_

Sebelum kita membuat form registrasi, kita perlu membuat suatu _skeleton_ yang berfungsi sebagai kerangka _views_ dari situs web kita. Dengan kerangka _views_ ini, kita dapat memastikan adanya konsistensi dalam desain situs web kita serta memperkecil kemungkinan terjadinya redundansi kode. Pada tutorial ini, kita akan membuat _skeleton_ untuk situs web yang akan kita buat pada tutorial selanjutnya.

1. Buat _folder_ `templates` pada _root folder_ (**direktori utama**) dan buatlah sebuah berkas HTML baru bernama `base.html`. Berkas `base.html` berfungsi sebagai _template_ dasar yang dapat digunakan sebagai kerangka umum untuk halaman web lainnya di dalam proyek. Isilah berkas `base.html` tersebut dengan kode berikut:

   ```html
   {% load static %}
   <!DOCTYPE html>
   <html lang="en">
     <head>
       <meta charset="UTF-8" />
       <meta name="viewport" content="width=device-width, initial-scale=1.0" />
       {% block meta %} {% endblock meta %}
     </head>

     <body>
       {% block content %} {% endblock content %}
     </body>
   </html>
   ```

   Baris-baris yang dikurung dalam `{% ... %}` disebut dengan _template tags_ Django. Baris-baris inilah yang akan berfungsi untuk memuat data secara dinamis dari Django ke HTML.

2. Buka `settings.py` yang ada pada direktori proyek (`book_tracker`) dan carilah baris yang mengandung variabel `TEMPLATES`. Sesuaikan kode yang ada dengan potongan kode berikut agar berkas `base.html` terdeteksi sebagai berkas _template_.

   ```python
   ...
   TEMPLATES = [
       {
           'BACKEND': 'django.template.backends.django.DjangoTemplates',
           'DIRS': [BASE_DIR / 'templates'], # Tambahkan konten baris ini
           'APP_DIRS': True,
           ...
       }
   ]
   ...
   ```

3. Pada subdirektori `templates` yang ada pada direktori `main` (`main/templates/`), ubahlah kode berkas `main.html` yang telah dibuat di tutorial sebelumnya menjadi sebagai berikut.

   ```html
   {% extends 'base.html' %} {% block content %}
   <h1>Book Tracker Page</h1>

   <h5>Name:</h5>
   <p>{{name}}</p>

   <h5>Class:</h5>
   <p>{{class}}</p>
   {% endblock content %}
   ```

   > Perhatikan bahwa kode diatas merupakan kode yang sama dengan kode pada `main.html` pada tutorial sebelumnya. Perbedaannya adalah pada kode diatas, kita menggunakan `base.html` sebagai _template_ utama.

## Tutorial: Membuat Form Input Data dan Menampilkan Data Buku Pada HTML

Sampai saat ini, belum ada data yang ditambahkan dan dimunculkan ke dalam aplikasi. Sekarang, kita akan membuat sebuah _form_ sederhana untuk menginput data buku pada aplikasi sehingga nantinya kamu dapat menambahkan data baru untuk ditampilkan pada halaman utama.

1. Buat berkas baru pada direktori `main` dengan nama `forms.py` untuk membuat struktur _form_ yang dapat menerima data buku baru. Tambahkan kode berikut ke dalam berkas `forms.py`.

   ```python
   from django.forms import ModelForm
   from main.models import Book

   class BookForm(ModelForm):
       class Meta:
           model = Book
           fields = ["name", "page", "description"]
   ```

   **Penjelasan Kode:**

   - `model = Book` untuk menunjukkan model yang akan digunakan untuk _form_. Ketika data dari _form_ disimpan, isi dari _form_ akan disimpan menjadi sebuah objek `Book`.
   - `fields = ["name", "page", "description"]` untuk menunjukkan _field_ dari model Book yang digunakan untuk _form_. _Field_ `date_added` tidak dimasukkan ke _list_ `fields` karena tanggal ditambahkan secara otomatis.

2. Buka berkas `views.py` yang ada pada folder `main` dan tambahkan beberapa _import_ berikut pada bagian paling atas.

   ```python
   from django.shortcuts import render, redirect   # Tambahkan import redirect di baris ini
   from main.forms import BookForm
   from main.models import Book
   ```

3. Masih di berkas yang sama, buat fungsi baru dengan nama `create_book` yang menerima parameter `request`. Tambahkan potongan kode di bawah ini untuk menghasilkan formulir yang dapat menambahkan data buku secara otomatis ketika data di-_submit_ dari _form_.

   ```python
   def create_book(request):
       form = BookForm(request.POST or None)

       if form.is_valid() and request.method == "POST":
           form.save()
           return redirect('main:show_main')

       context = {'form': form}
       return render(request, "create_book.html", context)
   ```

   **Penjelasan Kode:**

   - `form = BookForm(request.POST or None)` digunakan untuk membuat `BookForm` baru dengan memasukkan QueryDict berdasarkan input dari _user_ pada `request.POST`.
   - `form.is_valid()` digunakan untuk memvalidasi isi input dari _form_ tersebut.
   - `form.save()` digunakan untuk membuat dan menyimpan data dari _form_ tersebut.
   - `return redirect('main:show_main')` digunakan untuk melakukan _redirect_ ke fungsi `show_main` pada _views_ aplikasi `main` setelah data _form_ berhasil disimpan.

4. Ubahlah fungsi `show_main` yang sudah ada pada berkas `views.py` menjadi seperti berikut.

   ```python
   def show_main(request):
       books = Book.objects.all()

       context = {
           'name': 'Pak Bepe',
           'class': 'PBP A',
           'books': books
       }

       return render(request, "main.html", context)
   ```

   **Penjelasan Kode:**

   Fungsi `Book.objects.all()` digunakan untuk mengambil seluruh objek `Book` yang tersimpan pada _database_.

5. Buka `urls.py` yang ada pada direktori `main` dan _import_ fungsi `create_book` yang sudah kamu buat tadi.

   ```python
   from main.views import show_main, create_book
   ```

6. Tambahkan _path_ URL ke dalam variabel `urlpatterns` pada `urls.py` di `main` untuk mengakses fungsi yang sudah di-_import_ pada poin sebelumnya.

   ```python
   urlpatterns = [
      ...
      path('create-book', create_book, name='create_book'),
   ]
   ```

7. Buat berkas HTML baru dengan nama `create_book.html` pada direktori `main/templates`. Isi `create_book.html` dengan kode berikut.

   ```html
   {% extends 'base.html' %} {% block content %}
   <h1>Add New Book</h1>

   <form method="POST">
     {% csrf_token %}
     <table>
       {{ form.as_table }}
       <tr>
         <td></td>
         <td>
           <input type="submit" value="Add Book" />
         </td>
       </tr>
     </table>
   </form>

   {% endblock %}
   ```

   **Penjelasan Kode:**

   - `<form method="POST">` digunakan untuk menandakan _block_ untuk _form_ dengan metode POST.
   - `{% csrf_token %}` adalah token yang berfungsi sebagai _security_. Token ini di-_generate_ secara otomatis oleh Django untuk mencegah serangan berbahaya.
   - `{{ form.as_table }}` adalah _template tag_ yang digunakan untuk menampilkan _fields_ form yang sudah dibuat pada `forms.py` sebagai _table_.
   - `<input type="submit" value="Add Book"/>` digunakan sebagai tombol _submit_ untuk mengirimkan _request_ ke _view_ `create_book(request)`.

8. Buka `main.html` dan tambahkan kode berikut di dalam `{% block content %}` untuk menampilkan data produk dalam bentuk _table_ serta tombol "Add New Book" yang akan _redirect_ ke halaman _form_.

   ```html
   ...
   <table>
     <tr>
       <th>Name</th>
       <th>Page</th>
       <th>Description</th>
       <th>Date Added</th>
     </tr>

     {% comment %} Berikut cara memperlihatkan data produk di bawah baris ini
     {%endcomment %} {% for book in books %}
     <tr>
       <td>{{book.name}}</td>
       <td>{{book.page}}</td>
       <td>{{book.description}}</td>
       <td>{{book.date_added}}</td>
     </tr>
     {% endfor %}
   </table>

   <br />

   <a href="{% url 'main:create_book' %}">
     <button>Add New Book</button>
   </a>
   {% endblock content %}
   ```

9. Jalankan proyek Django-mu dengan perintah `python manage.py runserver` dan bukalah <http://localhost:8000/> di browser favoritmu. Coba tambahkan beberapa data produk baru dan seharusnya kamu dapat melihat data yang ditambahkan pada halaman utama aplikasi.

## Tutorial Mengembalikan Data dalam Bentuk XML

1. Buka `views.py` yang ada pada folder `main` dan tambahkan _import_ `HttpResponse` dan `Serializer` pada bagian paling atas.

   ```python
   from django.http import HttpResponse
   from django.core import serializers
   ```

2. Buatlah sebuah fungsi baru yang menerima parameter _request_ dengan nama `show_xml` dan buatlah sebuah variabel di dalam fungsi tersebut yang menyimpan hasil _query_ dari seluruh data yang ada pada `Book`.

   ```python
   def show_xml(request):
       data = Book.objects.all()
   ```

3. Tambahkan _return function_ berupa `HttpResponse` yang berisi parameter data hasil _query_ yang sudah diserialisasi menjadi XML dan parameter `content_type="application/xml"`.

   ```python
   def show_xml(request):
       data = Book.objects.all()
       return HttpResponse(serializers.serialize("xml", data), content_type="application/xml")
   ```

   **Penjelasan Kode:**

   `serializers` digunakan untuk _translate_ objek model menjadi format lain seperti dalam fungsi ini adalah XML.

4. Buka `urls.py` yang ada pada folder `main` dan _import_ fungsi yang sudah kamu buat tadi.

   ```python
   from main.views import show_main, create_book, show_xml
   ```

5. Tambahkan _path url_ ke dalam `urlpatterns` untuk mengakses fungsi yang sudah diimpor tadi.

   ```python
   ...
   path('xml/', show_xml, name='show_xml'),
   ...
   ```

6. Jalankan proyek Django-mu dengan perintah `python manage.py runserver` dan bukalah <http://localhost:8000/xml/> di browser favoritmu untuk melihat hasilnya.

## Tutorial: Mengembalikan Data dalam Bentuk JSON

1. Buka `views.py` yang ada pada folder `main` dan buatlah sebuah fungsi baru yang menerima parameter _request_ dengan nama `show_json` dengan sebuah variabel di dalamnya yang menyimpan hasil _query_ dari seluruh data yang ada pada `Book`.

   ```python
   def show_json(request):
       data = Book.objects.all()
   ```

2. Tambahkan _return function_ berupa `HttpResponse` yang berisi parameter data hasil _query_ yang sudah diserialisasi menjadi JSON dan parameter `content_type="application/json"`.

   ```python
   def show_json(request):
       data = Book.objects.all()
       return HttpResponse(serializers.serialize("json", data), content_type="application/json")
   ```

3. Buka `urls.py` yang ada pada folder `main` dan _import_ fungsi yang sudah kamu buat tadi.

   ```python
   from main.views import show_main, create_book, show_xml, show_json
   ```

4. Tambahkan _path url_ ke dalam `urlpatterns` untuk mengakses fungsi yang sudah diimpor tadi.

   ```python
   ...
   path('json/', show_json, name='show_json'),
   ...
   ```

5. Jalankan proyek Django-mu dengan perintah `python manage.py runserver` dan bukalah <http://localhost:8000/json/> (sesuaikan dengan _path url_ yang dibuat) di browser favoritmu untuk melihat hasilnya.

## Tutorial: Mengembalikan Data Berdasarkan ID dalam Bentuk XML dan JSON

1. Buka `views.py` yang ada pada folder `main` dan buatlah dua fungsi baru yang menerima parameter `_request_` dan `id` dengan nama `show_xml_by_id` dan `show_json_by_id`.

2. Buatlah sebuah variabel di dalam fungsi tersebut yang menyimpan hasil _query_ dari data dengan id tertentu yang ada pada `Book`.

   ```python
   data = Book.objects.filter(pk=id)
   ```

3. Tambahkan _return function_ berupa `HttpResponse` yang berisi parameter data hasil _query_ yang sudah diserialisasi menjadi JSON atau XML dan parameter `content_type` dengan _value_ `"application/xml"` (untuk format XML) atau `"application/json"` (untuk format JSON).

   - XML

     ```python
     def show_xml_by_id(request, id):
         data = Book.objects.filter(pk=id)
         return HttpResponse(serializers.serialize("xml", data), content_type="application/xml")
     ```

   - JSON

     ```python
     def show_json_by_id(request, id):
         data = Book.objects.filter(pk=id)
         return HttpResponse(serializers.serialize("json", data), content_type="application/json")
     ```

4. Buka `urls.py` yang ada pada folder `main` dan _import_ fungsi yang sudah kamu buat tadi.

   ```python
   from main.views import show_main, create_book, show_xml, show_json, show_xml_by_id, show_json_by_id
   ```

5. Tambahkan _path_ URL ke dalam `urlpatterns` untuk mengakses fungsi yang sudah diimpor tadi.

   ```python
   ...
   path('xml/<int:id>/', show_xml_by_id, name='show_xml_by_id'),
   path('json/<int:id>/', show_json_by_id, name='show_json_by_id'),
   ...
   ```

6. Jalankan proyek Django-mu dengan perintah `python manage.py runserver` dan bukalah <http://localhost:8000/xml/[id]/> atau <http://localhost:8000/json/[id]/> di browser favoritmu untuk melihat hasilnya.

   > Catatan: Sesuaikan `[id]` pada URL di atas dengan id objek yang ingin dilihat.

## Tutorial: Penggunaan Postman Sebagai _Data Viewer_

1. Pastikan server kamu sudah berjalan dengan perintah `python manage.py runserver`.

2. Buka Postman dan buatlah sebuah _request_ baru dengan _method_ `GET` dan _url_ <http://localhost:8000/xml/> atau <http://localhost:8000/json/> untuk mengetes apakah data terkirimkan dengan baik.

   > Panduan Instalasi Postman dapat dilihat pada [Laman Resmi Postman](#referensi-tambahan).

   Contoh:
   ![Tampilan Halaman Postman](https://media.discordapp.net/attachments/1133956580728127550/1206218883782934558/image.png?ex=65db35e0&is=65c8c0e0&hm=cc5e093ce311fa05279428527608c5c6de0d4672a461d61eed29dd192eb17888&=&format=webp&quality=lossless)

3. Klik tombol `Send` untuk mengirimkan _request_ tersebut.

4. Kamu akan melihat hasil _response_ dari _request_ tersebut pada bagian bawah Postman.

   ![Sent](https://media.discordapp.net/attachments/1133956580728127550/1206219165132787752/image.png?ex=65db3623&is=65c8c123&hm=e4ca6ef643d6ae651cd9526b61c1e4455cecd5244096b3ed298d1022ee9f788a&=&format=webp&quality=lossless&width=701&height=543)

5. Kamu juga dapat mengubah _url_ menjadi <http://localhost:8000/xml/[id]> atau <http://localhost:8000/json/[id]> untuk mengetes fungsi pengambilan data produk berdasarkan ID.

   ![Json By ID](https://media.discordapp.net/attachments/1133956580728127550/1206219270007038003/image.png?ex=65db363c&is=65c8c13c&hm=b979c68fb739601dc1cdcc5066f552fa28e9941e082ef8b4b949490bb65de2a1&=&format=webp&quality=lossless&width=698&height=543)

## Penutup

1. Setelah menyelesaikan tutorial ini, tampilan halaman web kamu seharusnya terlihat seperti ini.

   ![Tampilan Halaman Web](https://media.discordapp.net/attachments/1133956580728127550/1206220516117975050/image.png?ex=65db3765&is=65c8c265&hm=f010001a6578077a802d40156be31bec1a6b40705817ffc5c792aed8f20c47b3&=&format=webp&quality=lossless)

2. Pada akhir tutorial ini, struktur direktori lokalmu akan terlihat seperti ini.

   ![Struktur Direktori Lokal](https://media.discordapp.net/attachments/1133956580728127550/1206222687773401138/image.png?ex=65db396b&is=65c8c46b&hm=e711796985a77120a5beb95165348f0c587e1f13f8becb04650ac18ebd5eb5c9&=&format=webp&quality=lossless&width=184&height=542)

3. Sebelum melakukan langkah ini, **pastikan struktur direktori lokal sudah benar**. Selanjutnya, lakukan `add`, `commit` dan `push` untuk memperbarui repositori GitHub.

4. Jalankan perintah berikut untuk melakukan `add`, `commit` dan `push`.

   ```shell
   git add .
   git commit -m "<pesan_commit>"
   git push -u origin <branch_utama>
   ```

   - Ubah `<pesan_commit>` sesuai dengan keinginan. Contoh: `git commit -m "tutorial 2 selesai"`.
   - Ubah `<branch_utama>` sesuai dengan nama branch utamamu. Contoh: `git push -u origin main` atau `git push -u origin master`.

## Referensi Tambahan

- [How to install Postman](https://learning.postman.com/docs/getting-started/installation/installation-and-updates/#:~:text=Postman%20is%20available%20on%20the,select%20Download%20for%20your%20platform.)

## Kontributor

- Muhammad Nabil Mu'afa
- Muhammad Iqbal Dwitama

## Credits

Tutorial ini dikembangkan berdasarkan [PBP Ganjil 2024](https://github.com/pbp-fasilkom-ui/ganjil-2024) yang ditulis oleh Tim Pengajar Pemrograman Berbasis Platform 2024. Segala tutorial serta instruksi yang dicantumkan pada repositori ini dirancang sedemikian rupa sehingga mahasiswa yang sedang mengambil mata kuliah Pemrograman Berbasis Platform dapat menyelesaikan tutorial saat sesi lab berlangsung.