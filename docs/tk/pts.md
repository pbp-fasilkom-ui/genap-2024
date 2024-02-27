# Proyek Tengah Semester PBP

**Membuat Situs Web menggunakan Framework Django (Berkelompok)**

---

## Tujuan Pembelajaran Khusus

1. Merancang halaman web
2. Mengimplementasikan *website* menggunakan *framework* Django dengan memenuhi Models, Views, dan Templates 
3. Memanfaatkan *framework* CSS untuk mewujudkan *Responsive Web Design*
4. Mengimplementasikan *Unit Test & deployment* (bonus)

### Catatan

Perlu diperhatikan selain tujuan pembelajaran khusus seperti tertulis di atas, peserta kuliah juga perlu mempelajari dan dilatih beberapa aspek kecendekiaan sebagai calon sarjana. Di antaranya yang relevan dalam kuliah ini adalah keteguhan (*grit*), kemandirian, ketelitian, termasuk juga metakognitif (secara sederhana bisa diartikan kemampuan mengatur strategi belajar yang sesuai dengan dirinya meliputi perencanaan, *monitoring* dan evaluasi proses belajar mandiri), termasuk di dalamnya kemampuan untuk memahami, mengkomunikasikan masalah, diskusi dan bertanya, sehingga peserta kuliah juga perlu siap bersikap positif dengan kondisi-kondisi yang secara tidak langsung atau tidak pasti akan dihadapi dan mungkin dapat menghabiskan banyak waktu. Kondisi tersebut bisa dianggap kendala, seperti keterbatasan sumber daya, *bug tools*, kesulitan teknis atau lainnya. Walaupun dirasakan menyulitkan, perlu diupayakan untuk disikapi dengan positif agar dapat menjadi manfaat terkait aspek kecendekiaan yang perlu dilatih peserta kuliah. Sikap negatif hanya akan memperburuk keadaan dan menghilangkan manfaat tugas ini untuk pembelajaran yang akan dapat dirasakan di kemudian hari. Tim asisten dan dosen melalui sarana yang ada, akan berusaha semampunya melayani pertanyaan, keluhan, dan membantu proses pembelajaran peserta agar peserta bisa menjalani perkuliahan dan belajar semaksimal mungkin.

Sebagai selingan, bila rekan-rekan lelah dan bingung menghadapi *error* yang belum kunjung terselesaikan, berikut ini ada video yang cukup populer dan mudah-mudahan bisa menambah semangat untuk tetap teguh mengerjakan dan berlatih demi kesuksesan di kemudian hari.

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/42-hh-iMJJI" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe><br /><br />

Selamat mengerjakan. 😄

## Aturan Umum Tugas Kelompok

1. Satu kelompok terdiri atas 5-6 orang.
2. Satu kelompok membuat satu repositori Git di dalam satu [organisasi](https://docs.github.com/en/organizations/collaborating-with-groups-in-organizations/about-organizations) yang digunakan oleh seluruh anggota kelompok untuk bekerja sama. Kumpulkan tautan repositori ke Scele.
3. Setiap kelompok dipersilakan mencari ide sendiri mengenai aplikasi yang akan dibuat. Tema aplikasi adalah makanan dan minuman (*food and beverage*). Tema ini dipilih karena tiga alasan:
	- Pada Semester Genap 2023/2024, ada momen bulan Ramadan. Biasanya ada banyak acara bukber (buka puasa bersama) pada bulan Ramadan. Acara-acara bukber tersebut dapat menjadi peluang usaha makanan dan minuman.
    - Setelah bulan Ramadan, ada Hari Raya Idulfitri. Perayaan Hari Raya Idulfitri juga dapat menjadi peluang usaha kue kering khas Idulfitri.
	- Selain momen bulan Ramadan dan Hari Raya Idulfitri, tinggal di lingkungan sekitar kampus juga dapat menjadi peluang usaha katering untuk kegiatan kampus dan untuk mahasiswa yang tidak sempat memasak. 
4. Setiap kelompok mengimplementasikan *dataset* makanan dan minuman dalam bentuk *class* Models dan menyimpan data dari *dataset* tersebut ke dalam database Django. *Dataset* makanan dan minuman harus berisi minimal 100 makanan dan/atau minuman. Sumber *dataset* makanan dan minuman boleh berasal dari mana saja, misalnya dari Kaggle. Contoh URL *dataset* yang dapat digunakan: 
	- [Indonesia food delivery GoFood product list](https://www.kaggle.com/datasets/ariqsyahalam/indonesia-food-delivery-gofood-product-list)
	- [Indonesian Food Recipes](https://www.kaggle.com/datasets/canggih/indonesian-food-recipes)
	- [Indonesian Food and Drink Nutrition Dataset](https://www.kaggle.com/datasets/anasfikrihanif/indonesian-food-and-drink-nutrition-dataset)
5. Setiap anggota kelompok mengerjakan modul yang berbeda. Modul ditentukan oleh kelompok yang disesuaikan dengan ide aplikasi yang sudah didiskusikan dalam kelompok.
6. Tugas kelompok dikumpulkan sebagai kesatuan aplikasi web.

## Aturan Khusus per Anggota Kelompok

1. Menerapkan Models dengan membuat, memanfaatkan yang sudah disediakan Django, atau memanfaatkan yang sudah dibuat oleh anggota kelompok (di modul lain). 
2. Menerapkan Views untuk memproses *request* dan mengolah data untuk menghasilkan respons menggunakan templat HTML maupun mengembalikan respons JSON. 
3. Menerapkan templat HTML dengan kerangka yang sistematis dan efisien, seperti `base.html`, `header.html`, dan `footer.html`.
4. Menerapkan *responsive framework* pada templat HTML (seperti [Bootstrap](https://getbootstrap.com/) atau [Tailwind](https://tailwindcss.com/)).
5. Memiliki halaman *form* yang dapat menerima masukan dari pengguna kemudian diproses oleh Views. Contoh pemrosesan oleh Views adalah *insert* data ke dalam Models, *query* data dari Models, dan *update* data pada Models.
6. Menerapkan JavaScript dengan pemanggilan AJAX.
7. Menerapkan filter informasi bagi pengguna yang sudah *log in* saja. Contohnya adalah data alamat, umur, dan nomor ponsel hanya dapat dilihat oleh pengguna yang sudah *log in* saja.
8. Menerapkan filter pada *dataset* makanan dan minuman yang ditampilkan. Contohnya adalah menampilkan daftar makanan dan minuman berdasarkan harga.

## Tahapan Tugas Kelompok

<table>
    <tr>
        <th>Tahapan dan <em>deliverables</em></th>
        <th>Tenggat Waktu dan Keterangan</th>
    </tr>
    <tr>
        <td>
            <b>Tahap I (40%)</b>
            <ul>
                <li>Pembuatan GitHub kelompok</li>
                <li>README.md pada GitHub yang berisi:</li>
                    <ol>
                        <li>Nama-nama anggota kelompok</li>
                        <li>Deskripsi aplikasi (cerita aplikasi yang diajukan serta kebermanfaatannya)</li>
                        <li>Daftar modul yang akan diimplementasikan</li>
                        <li>Sumber dataset makanan dan minuman</li>
						<li><em>Role</em> atau peran pengguna beserta deskripsinya (karena bisa saja lebih dari satu jenis pengguna yang mengakses aplikasi)</li>
                    </ol>
            </ul>
        </td>
        <td>
            <b>Tenggat Waktu: Rabu, 13 Maret 2024, pukul 23:55 WIB</b>
            <b>Kumpulkan tautan GitHub</b> dengan <em>code base</em> proyek Django yang sudah disiapkan di GitHub ke slot submisi yang tersedia di SCELE.
        </td>
    </tr>
    <tr>
        <td>
            <b>Tahap II (60%)</b>
            <p>(Modul sudah terimplementasi dengan baik)</p>
            <ul>
                <li>Modul aplikasi dari tiap anggota kelompok</li>
                <li><em>URL Mapping</em> untuk modul</li>
                <li><em>Models</em> untuk modul</li>
                <li><em>Views</em> untuk modul</li>
                <li>Terintegrasi sebagai satu kesatuan aplikasi</li>
                <li>Fungsionalitas sesuai dengan rancangan desain</li>
            </ul>
        </td>
        <td>
            <b>Tenggat Waktu: Rabu, 17 April 2024, pukul 23.55 WIB</b>
            <p><b>Kriteria Submisi:</b> Seluruh modul yang dikerjakan oleh setiap anggota kelompok sudah muncul dan dapat diakses pada proyek Django</p>
        </td>
    </tr>
    <tr>
        <td>
            <b>Bonus (5%)</b>
            <ul>
                <li>Unit Test (<em>passed</em>) untuk semua aspek, diharapkan <em>code coverage</em> bisa mencapai minimal 80%</li>
                <li>GitHub Actions (CI/CD) sudah terkonfigurasi hingga <em>deployment</em></li>
                <li><em>Pipeline status</em> dan tautan aplikasi yang sudah di-<em>deploy</em> tersedia di berkas README.md pada GitHub</li>
            </ul>
        </td>
        <td></td>
    </tr>
</table>