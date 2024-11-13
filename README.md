# Tugas 7

## Jelaskan apa yang dimaksud dengan stateless widget dan stateful widget, dan jelaskan perbedaan dari keduanya.

StatefulWidget adalah widget yang dinamis dan dapat berubah.
StatelessWidget adalah widget yang tidak akan berubah.
StatefulWidget digunakan ketika kita ingin widgetnya mampu berubah dari waktu ke waktu, sedangkan kita memakai StatelessWidget jika widgetnya tidak perlu diubah atau diupdate sewaktu-waktu.

# Sebutkan widget apa saja yang kamu gunakan pada proyek ini dan jelaskan fungsinya.

- Scaffold: Menyediakan struktur dasar halaman (AppBar dan Body)
- AppBar: Menampilkan bar di bagian atas layar aplikasi
- Padding: Memberikan ruang di sekeliling widget
- Column: Menyusun widget di dalamnya secara vertikal
- Row: Menyusun widget di dalamnya secara horizontal
- SizedBox: Memberikan jarak antara widget di atas dan widget di bawahnya.
- GridView.count: Menyusun widget di dalamnya dalam bentuk grid dengan jumlah kolom yang ditentukan.
- Card: Membuat kotak kartu dengan bayangan di bawahnya, biasanya digunakan untuk menampilkan informasi singkat.
- Material: Menentukan tampilan kartu dengan warna, bayangan, dan efek material
- InkWell: Menambahkan efek klik pada widget dan memberikan aksi tertentu ketika diklik
- Container: Untuk mengatur layout, padding, ukuran, dan warna latar belakang
- Icon: Menampilkan ikon dengan desain material
- Text: Menampilkan teks di layar
- SnackBar: Menampilkan pesan sementara di bagian bawah layar ketika item diklik
- InfoCard: Menampilkan informasi card
- ItemHomePage: Menyimpan data item yang akan ditampilkan di itemCard
- ItemCard: Card untuk menampilkan item

## Apa fungsi dari setState()? Jelaskan variabel apa saja yang dapat terdampak dengan fungsi tersebut.

setState() digunakan untuk memberi tahu framework bahwa ada perubahan pada data atau variabel yang perlu diperbarui di UI.
Variabel yang dapat terdampak adalah variabel yang bukan variabel final

## Jelaskan perbedaan antara const dengan final.

- Final digunakan jika kita ingin mengunci nilai setelah inisialisasi namun nilainya baru ditentukan saat running program. Sedangkan, const digunakan jika nilainya benar-benar diketahui sejak awal (dari saat compile)

## Langkah Implementasi Program (Tugas 7)

1. Buat proyek Flutter baru dan masuk ke direktori proyek tersebut:

```
flutter create bing_chilling_creamery
cd bing_chilling_creamery

```

2. Buat Struktur dan Widget Dasar
   Buat file baru menu.dart di dalam direktori lib/. Masukkan kode UI utama aplikasi ke dalam file ini.
   Pindahkan kelas MyHomePage dari main.dart ke menu.dart. Tambahkan import 'menu.dart'; di file main.dart agar halaman dapat dikenali.
3. Ubah Tema Warna Aplikasi
   Buka main.dart dan ubah bagian tema aplikasi. Untuk membuat tema yang konsisten, gunakan kode berikut pada MaterialApp:

```
theme: ThemeData(
  colorScheme: ColorScheme.fromSwatch(primarySwatch: Colors.pink).copyWith(secondary: Colors.green),
),
```

4. Buat Kelas InfoCard untuk Menampilkan Informasi Es Krim
   Tambahkan kelas InfoCard di menu.dart untuk menampilkan informasi tentang es krim, seperti nama, harga, deskripsi, dan rating. Contoh kode:

```
class InfoCard extends StatelessWidget {
  final String title;
  final String content;

  const InfoCard({super.key, required this.title, required this.content});

  @override
  Widget build(BuildContext context) {
    return Card(
      elevation: 2.0,
      child: Container(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          children: [
            Text(title, style: const TextStyle(fontWeight: FontWeight.bold)),
            const SizedBox(height: 8.0),
            Text(content),
          ],
        ),
      ),
    );
  }
}

```

5. Buat Kelas ItemHomepage untuk Mengelola Tombol Menu
   Buat kelas ItemHomepage di menu.dart untuk menyimpan data tombol menu, seperti nama tombol dan ikon:

```
class ItemHomepage {
  final String name;
  final IconData icon;
  final Color color;

  ItemHomepage(this.name, this.icon, this.color);
}

```

6. Tambahkan Kelas ItemCard untuk Menampilkan Tombol Menu
   Tambahkan ItemCard di menu.dart untuk menampilkan tombol dengan ikon dan warna yang berbeda. Saat ditekan, tombol akan menampilkan SnackBar dengan pesan:

```
class ItemCard extends StatelessWidget {
  final Color color;
  final ItemHomepage item;

  const ItemCard(this.item, this.color, {super.key});

  @override
  Widget build(BuildContext context) {
    return Material(
      color: color,
      borderRadius: BorderRadius.circular(12),
      child: InkWell(
        onTap: () {
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(SnackBar(content: Text("Kamu telah menekan tombol ${item.name}!")));
        },
        child: Container(
          padding: const EdgeInsets.all(8),
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: [
              Icon(item.icon, color: Colors.white, size: 30.0),
              const SizedBox(height: 3.0),
              Text(item.name, textAlign: TextAlign.center, style: const TextStyle(color: Colors.white)),
            ],
          ),
        ),
      ),
    );
  }
}
```

7. Tampilkan Semua Widget di MyHomePage
   Dalam MyHomePage, gunakan InfoCard untuk menampilkan informasi es krim dan ItemCard untuk tombol menu. Berikut contoh Widget build:

```
@override
Widget build(BuildContext context) {
  return Scaffold(
    appBar: AppBar(
      title: const Text('Bing Chilling', style: TextStyle(color: Colors.white, fontWeight: FontWeight.bold)),
      backgroundColor: Theme.of(context).colorScheme.primary,
    ),
    body: Padding(
      padding: const EdgeInsets.all(16.0),
      child: Column(
        children: [
          Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly,
            children: [
              InfoCard(title: 'Nama', content: 'Es Krim'),
              InfoCard(title: 'Harga', content: '16000'),
              InfoCard(title: 'Deskripsi', content: 'Lorem ipsum'),
              InfoCard(title: 'Rating', content: '10'),
            ],
          ),
          const SizedBox(height: 16.0),
          Center(
            child: Column(
              children: [
                const Text('Welcome to Bing Chilling Creamery', style: TextStyle(fontWeight: FontWeight.bold, fontSize: 18.0)),
                GridView.count(
                  crossAxisCount: 3,
                  shrinkWrap: true,
                  children: [
                    ItemCard(ItemHomepage("Lihat Es Krim", Icons.shopping_cart, Colors.pink.shade400), Colors.pink.shade400),
                    ItemCard(ItemHomepage("Tambah Es Krim", Icons.add, Colors.pink.shade300), Colors.pink.shade300),
                    ItemCard(ItemHomepage("Logout", Icons.logout, Colors.pink.shade200), Colors.pink.shade200),
                  ],
                ),
              ],
            ),
          ),
        ],
      ),
    ),
  );
}
```

8. Push perubahan ke github.

# Tugas 8

## Apa kegunaan const di Flutter? Jelaskan apa keuntungan ketika menggunakan const pada kode Flutter. Kapan sebaiknya kita menggunakan const, dan kapan sebaiknya tidak digunakan?

Const digunakan untuk mendefinisikan widget/objek sebagai nilai yang tetap selama runtime aplikasi. Keuntungannya adalah penghematan memori dan meningkatkan efisiensi aplikasi sebab Flutter akan tahu bahwa objek const hanya perlu dibuat sekali dan tidak akan pernah diubah.
Gunakan const untuk widget atau objek yang tidak berubah.
Jangan gunakan const ketika widget atau objeknya akan berubah-ubah.

## Jelaskan dan bandingkan penggunaan Column dan Row pada Flutter. Berikan contoh implementasi dari masing-masing layout widget ini!

Column digunakan jika kita ingin menyusun objek-objek secara vertikal, sedangkan row untuk menyusun objek-objek secara horizontal.
Contoh column:

```
Column(
    crossAxisAlignment: CrossAxisAlignment.start,
    children: [
      Text('Nama: $_name'),
      Text('Harga: $_price'),
      Text('Deskripsi: $_description'),
      Text('Rating: $_rating')
    ],
  ),
```

Contoh row:

```
Row(
  mainAxisAlignment: MainAxisAlignment.spaceEvenly,
  children: [
    InfoCard(title: 'Nama', content: name),
    InfoCard(title: 'Harga', content: price),
    InfoCard(title: 'Deskripsi', content: description),
    InfoCard(title: 'Rating', content: rating),
  ],
),
```

## Sebutkan apa saja elemen input yang kamu gunakan pada halaman form yang kamu buat pada tugas kali ini. Apakah terdapat elemen input Flutter lain yang tidak kamu gunakan pada tugas ini? Jelaskan!

Elemen input flutter yang digunakan:
TextFormField, ElevatedButton

Elemen input flutter yang TIDAK digunakan:
Checkbox, Switch, Slider, DatePicker, TimePicker, DropdownButton

## Bagaimana cara kamu mengatur tema (theme) dalam aplikasi Flutter agar aplikasi yang dibuat konsisten? Apakah kamu mengimplementasikan tema pada aplikasi yang kamu buat?

Digunakan properti theme dalam aplikasi yang mencakup warna primer, sekunder, dll. untuk mengatur konsistensi tema. Dan iya, tema sudah diimplementasikan pada aplikasi ini.

## Bagaimana cara kamu menangani navigasi dalam aplikasi dengan banyak halaman pada Flutter?

Menggunakan Navigator dan Drawer

## Langkah Implementasi Program (Tugas 8)

1. Menambahkan Drawer untuk Navigasi

Buat file left_drawer.dart di dalam folder widgets pada direktori lib/.
Tambahkan kelas LeftDrawer untuk membuat drawer navigasi, dengan opsi untuk mengarahkan pengguna ke halaman utama (MyHomePage) dan halaman formulir tambah item (CreamEntryFormPage).
Pastikan untuk menambahkan header dengan nama aplikasi, serta ikon dan label untuk setiap opsi navigasi.

```
import 'package:flutter/material.dart';
import 'package:toko_bing_chilling/screens/menu.dart';
import 'package:toko_bing_chilling/creamentry_form.dart';


class LeftDrawer extends StatelessWidget {
  const LeftDrawer({super.key});

  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: ListView(
        children: [
          DrawerHeader(
            decoration: BoxDecoration(color: Theme.of(context).colorScheme.primary),
            child: const Column(
              children: [
                Text('Bing Chilling Creamery', style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold, color: Colors.white)),
                Padding(padding: EdgeInsets.all(8)),
                Text("Where the best ice creams lie.", style: TextStyle(color: Colors.white)),
              ],
            ),
          ),
          ListTile(
            leading: const Icon(Icons.home_outlined),
            title: const Text('Halaman Utama'),
            onTap: () {
              Navigator.pushReplacement(context, MaterialPageRoute(builder: (context) => MyHomePage()));
            },
          ),
          ListTile(
            leading: const Icon(Icons.add),
            title: const Text('Tambah Es Krim'),
            onTap: () {
              Navigator.push(context, MaterialPageRoute(builder: (context) => CreamEntryFormPage()));
            },
          ),
        ],
      ),
    );
  }
}
```

2. Menambahkan Drawer ke Halaman Utama

Di dalam file menu.dart, tambahkan drawer ke dalam Scaffold dari MyHomePage dengan mengimpor LeftDrawer.
Drawer akan memberikan pengguna opsi untuk berpindah ke halaman utama atau halaman tambah item.

```
import 'package:flutter/material.dart';
import 'package:toko_bing_chilling/widgets/left_drawer.dart';

class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Bing Chilling Creamery'),
        backgroundColor: Theme.of(context).colorScheme.primary,
      ),
      drawer: const LeftDrawer(),
      body: // Konten halaman utama
    );
  }
}
```

3. Membuat Halaman Formulir Tambah Item Baru

Buat file baru bernama creamentry_form.dart di dalam folder screens.
Tambahkan struktur dasar halaman menggunakan Scaffold dengan AppBar dan Drawer yang sudah dibuat sebelumnya.

```
import 'package:flutter/material.dart';
import 'package:toko_bing_chilling/widgets/left_drawer.dart';

class CreamEntryFormPage extends StatefulWidget {
const CreamEntryFormPage({super.key});

    @override
    State<CreamEntryFormPage> createState() => _CreamEntryFormPageState();

}

class \_CreamEntryFormPageState extends State<CreamEntryFormPage> {
final \_formKey = GlobalKey<FormState>();

    @override
    Widget build(BuildContext context) {
      return Scaffold(
        appBar: AppBar(title: const Text('Tambah Es Krim Baru')),
        drawer: const LeftDrawer(),
        body: Form(key: _formKey, child: // Konten form),
      );
    }

}
```

4. Menambahkan Input Fields dan Validasi pada Formulir

Tambahkan empat elemen input pada halaman CreamEntryFormPage: Name, Harga, Deskripsi, dan Rating.
Pastikan setiap elemen input divalidasi agar tidak kosong, dan cocok dengan tipe data atributnya.

```
TextFormField(
decoration: InputDecoration(labelText: "Name"),
validator: (value) => value == null || value.isEmpty ? "Isian tidak boleh kosong!" : null,
),
// Tambahkan TextFormField untuk Harga, Deskripsi, dan Rating dengan validasi masing-masing
```

5. Menambahkan Tombol Save dan Pop-up Konfirmasi

Tambahkan tombol Save yang saat ditekan akan menampilkan pop-up dengan data yang diinputkan oleh pengguna.

```
ElevatedButton(
  onPressed: () {
    if (_formKey.currentState!.validate()) {
      showDialog(
        context: context,
        builder: (context) => AlertDialog(
          title: Text('Data berhasil disimpan'),
          content: Column(children: [
            Text('Nama: $_name'),
            Text('Harga: $_price'),
            Text('Deskripsi: $_description'),
            Text('Rating: $_rating'),
          ]),
          actions: [TextButton(onPressed: () => Navigator.pop(context), child: Text('OK'))],
        ),
      );
    }
  },
  child: Text("Save"),
),
```

6. Menghubungkan Tombol "Tambah Item" pada Halaman Utama ke Form Tambah Item

Pada halaman utama (menu.dart), tambahkan navigasi untuk membuka halaman CreamEntryFormPage menggunakan Navigator.push().

```
if (item.name == "Tambah Es Krim") {
  Navigator.push(context, MaterialPageRoute(builder: (context) => CreamEntryFormPage()));
}
```
