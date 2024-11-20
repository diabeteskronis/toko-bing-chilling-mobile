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

# Tugas 9

### Mengapa kita perlu membuat model untuk pengambilan/pengiriman data JSON

Model diperlukan untuk mendefinisikan struktur data yang akan diterima atau dikirim.

### Fungsi library http

Library http digunakan untuk melakukan request HTTP ke server. Di sini, digunakan untuk mengambil atau mengirim data JSON dari/ke server Django.

### Fungsi CookieRequest dan Kenapa Perlu Dibagikan ke Semua Komponen di Aplikasi Flutter

CookieRequest berfungsi menangani permintaan HTTP yang melibatkan cookie autentikasi dan menjaga status login user. Perlu dibagikan ke semua komponen aplikasi agar seluruh aplikasi dapat mengakses session dan cookie yang sama.

### Mekanisme Pengiriman Data dari Input hingga Tampil pada Flutter

1. User mengisi form di Flutter.
2. Data dikirim ke server Django menggunakan request.
3. Django memproses data dan mengembalikan respons (biasanya JSON).
4. Data yang diterima diparsing menjadi objek menggunakan model.
5. Data ditampilkan di Flutter melalui widget seperti FutureBuilder atau ListView.

### Mekanisme Autentikasi (login, register, logout)

1. Login: User mengirimkan username dan password melalui form login di Flutter. Data ini diteruskan ke Django melalui request POST. Django memverifikasi kredensial dan mengembalikan respons sukses atau gagal, beserta status login.

2. Register: User mengisi form registrasi dengan username dan password. Data dikirimkan ke Django. Jika valid, Django membuat akun baru dan mengirimkan respons sukses.

3. Logout: Saat user menekan tombol logout, Flutter mengirimkan request ke Django untuk menghentikan sesi aktif. Django menghapus session dan mengirimkan respons sukses, yang menandakan user telah logout.

## Langkah Implementasi (Tugas 9)

1. Mengimplementasikan Autentikasi Akun pada Proyek Flutter

Pada proyek Django yang sudah dibuat sebelumnya, buat app baru bernama authentication. Lalu tambahkan view untuk register:

```
@csrf_exempt
def register(request):
    if request.method == 'POST':
        data = json.loads(request.body)
        username = data['username']
        password1 = data['password1']
        password2 = data['password2']

        if password1 != password2:
            return JsonResponse({"status": False, "message": "Passwords do not match."}, status=400)

        if User.objects.filter(username=username).exists
```

Tambahkan routenya untuk register:

```
from authentication.views import register

urlpatterns = [
    path('register/', register, name='register'),
]

```

Buat berkas register.dart pada folder screens/ dan gunakan pbp_django_auth untuk mengirim data ke endpoint registrasi:

```
ElevatedButton(
  onPressed: () async {
    String username = _usernameController.text;
    String password1 = _passwordController.text;
    String password2 = _confirmPasswordController.text;

  // UNTUK RETRIEVE PRODUK
    final response = await request.postJson(
        "http://127.0.0.1:8000/auth/register/",
        jsonEncode({
          "username": username,
          "password1": password1,
          "password2": password2,
        }));

    if (response['status'] == 'success') {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Registrasi berhasil!')),
      );
      Navigator.pushReplacement(
        context,
        MaterialPageRoute(builder: (context) => const LoginPage()),
      );
    } else {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text(response['message'])),
      );
    }
  },
  child: const Text('Register'),
)
```

Tambahkan view baru untuk login pada authentication/views.py:

```
@csrf_exempt
def login(request):
    username = request.POST['username']
    password = request.POST['password']
    user = authenticate(username=username, password=password)
    if user is not None and user.is_active:
        auth_login(request, user)
        return JsonResponse({"status": "success", "message": "Login sukses!"}, status=200)
    return JsonResponse({"status": "error", "message": "Login gagal!"}, status=401)
```

Jangan lupa untuk tambahkan URL login pada authentication/urls.py:

```
from authentication.views import login

urlpatterns = [
    path('login/', login, name='login'),
]
```

Buat berkas login.dart dan isi dengan kode:

```
ElevatedButton(
  onPressed: () async {
    String username = _usernameController.text;
    String password = _passwordController.text;

    final response = await request.login("http://127.0.0.1:8000/auth/login/", {
      'username': username,
      'password': password,
    });

    if (request.loggedIn) {
      Navigator.pushReplacement(
        context,
        MaterialPageRoute(builder: (context) => MyHomePage()),
      );
    } else {
      ScaffoldMessenger.of(context).showSnackBar(
        SnackBar(content: Text('Login gagal!')),
      );
    }
  },
  child: const Text('Login'),
)

```

2. Mengintegrasikan Autentikasi Django dengan Proyek Flutter

Gunakan package pbp_django_auth untuk menangani login dan request lain yang membutuhkan autentikasi cookie:

```
import 'package:pbp_django_auth/pbp_django_auth.dart';
import 'package:provider/provider.dart';

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Provider(
      create: (_) => CookieRequest(),
      child: MaterialApp(
        home: LoginPage(),
      ),
    );
  }
}
```

3. Membuat Model Kustom sesuai pada Aplikasi Django

Pada website Quicktype, ambil json model Django dan ubahlah menjadi model untuk flutter, sehingga pada models/bing_entry.dart menjadi:

```
// To parse this JSON data, do
//
//     final product = productFromJson(jsonString);

import 'dart:convert';

List<Product> productFromJson(String str) =>
    List<Product>.from(json.decode(str).map((x) => Product.fromJson(x)));

String productToJson(List<Product> data) =>
    json.encode(List<dynamic>.from(data.map((x) => x.toJson())));

class Product {
  String model;
  String pk;
  Fields fields;

  Product({
    required this.model,
    required this.pk,
    required this.fields,
  });

  factory Product.fromJson(Map<String, dynamic> json) => Product(
        model: json["model"],
        pk: json["pk"],
        fields: Fields.fromJson(json["fields"]),
      );

  Map<String, dynamic> toJson() => {
        "model": model,
        "pk": pk,
        "fields": fields.toJson(),
      };
}

class Fields {
  int user;
  String name;
  int price;
  String description;
  int icecreamrating;

  Fields({
    required this.user,
    required this.name,
    required this.price,
    required this.description,
    required this.icecreamrating,
  });

  factory Fields.fromJson(Map<String, dynamic> json) => Fields(
        user: json["user"],
        name: json["name"],
        price: json["price"],
        description: json["description"],
        icecreamrating: json["icecreamrating"],
      );

  Map<String, dynamic> toJson() => {
        "user": user,
        "name": name,
        "price": price,
        "description": description,
        "icecreamrating": icecreamrating,
      };
}

```

4. Membuat Halaman Daftar Item dari Endpoint JSON

Dafter item sudah terfilter sesuai dari Django jadi yang akan diambil hanya produk-produk yang terkait dengan user tersebut.
Buat berkas screens/list_bingentry untuk menampilkan dafftar item, dengan kodenya:

```
import 'package:flutter/material.dart';
import 'package:pbp_django_auth/pbp_django_auth.dart';
import 'package:provider/provider.dart';
import 'package:toko_bing_chilling/models/bing_entry.dart';
import 'package:toko_bing_chilling/widgets/left_drawer.dart';

class BingEntryPage extends StatefulWidget {
  const BingEntryPage({super.key});

  @override
  State<BingEntryPage> createState() => _BingEntryPageState();
}

class _BingEntryPageState extends State<BingEntryPage> {
  Future<List<Product>> fetchBing(CookieRequest request) async {
    final response = await request.get('http://127.0.0.1:8000/json/');

    // Melakukan decode response menjadi bentuk json
    var data = response;

    List<Product> listProduct = [];
    for (var d in data) {
      if (d != null) {
        listProduct.add(Product.fromJson(d));
      }
    }
    return listProduct;
  }

  @override
  Widget build(BuildContext context) {
    final request = context.watch<CookieRequest>();
    return Scaffold(
      appBar: AppBar(
        title: const Text('Products List'),
      ),
      drawer: const LeftDrawer(),
      body: FutureBuilder(
        future: fetchBing(request),
        builder: (context, AsyncSnapshot snapshot) {
          if (snapshot.data == null) {
            return const Center(child: CircularProgressIndicator());
          } else {
            if (!snapshot.hasData) {
              return const Column(
                children: [
                  Text(
                    'Belum product.',
                    style: TextStyle(fontSize: 20, color: Color(0xff59A5D8)),
                  ),
                  SizedBox(height: 8),
                ],
              );
            } else {
              return ListView.builder(
                itemCount: snapshot.data!.length,
                itemBuilder: (_, index) => Container(
                  margin:
                      const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
                  padding: const EdgeInsets.all(20.0),
                  child: Column(
                    mainAxisAlignment: MainAxisAlignment.start,
                    crossAxisAlignment: CrossAxisAlignment.start,
                    children: [
                      Text("Nama: "),
                      Text(
                        "${snapshot.data![index].fields.name}",
                        style: const TextStyle(
                          fontSize: 18.0,
                          fontWeight: FontWeight.bold,
                        ),
                      ),
                      const SizedBox(height: 10),
                      Text("Harga:"),
                      Text("${snapshot.data![index].fields.price}"),
                      const SizedBox(height: 10),
                      Text("Deskripsi:"),
                      Text("${snapshot.data![index].fields.description}"),
                      Text("Rating:"),
                      const SizedBox(height: 10),
                      Text("${snapshot.data![index].fields.icecreamrating}")
                    ],
                  ),
                ),
              );
            }
          }
        },
      ),
    );
  }
}

```

5. Menunjukkan Halaman Detail Produk pada List Produk

Buat berkas baru screens/product_detail.dart, dengan kode sebagai berikut:

```
import 'package:flutter/material.dart';
import 'package:toko_bing_chilling/models/bing_entry.dart';

class ProductDetailPage extends StatelessWidget {
  final Product product; // This will hold the selected product

  const ProductDetailPage({super.key, required this.product});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title:
            Text(product.fields.name), // Show the product name on the app bar
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(
              "Name: ${product.fields.name}",
              style:
                  const TextStyle(fontSize: 24.0, fontWeight: FontWeight.bold),
            ),
            const SizedBox(height: 10),
            Text("Price: ${product.fields.price}"),
            const SizedBox(height: 10),
            Text("Description: ${product.fields.description}"),
            const SizedBox(height: 10),
            Text("Rating: ${product.fields.icecreamrating}"),
          ],
        ),
      ),
    );
  }
}

```

Lalu, modify ListView.builder pada list_bingentry.dart untuk menerima gesture tap dan membawa user ke halaman detail product.

```
            return ListView.builder(
              itemCount: snapshot.data!.length,
              itemBuilder: (_, index) {
                var product = snapshot.data![index];
                return GestureDetector(
                  onTap: () {
                    // Navigate to the ProductDetailPage when tapped
                    Navigator.push(
                      context,
                      MaterialPageRoute(
                        builder: (context) => ProductDetailPage(
                          product: product,  // Pass the selected product
                        ),
                      ),
                    );
                  },
                  child: Container(
                    margin: const EdgeInsets.symmetric(horizontal: 16, vertical: 12),
                    padding: const EdgeInsets.all(20.0),
                    child: Column(
                      mainAxisAlignment: MainAxisAlignment.start,
                      crossAxisAlignment: CrossAxisAlignment.start,
                      children: [
                        Text("Nama: "),
                        Text(
                          "${product.fields.name}",
                          style: const TextStyle(
                            fontSize: 18.0,
                            fontWeight: FontWeight.bold,
                          ),
                        ),
                        const SizedBox(height: 10),
                        Text("Harga: ${product.fields.price}"),
                        const SizedBox(height: 10),
                        Text("Deskripsi: ${product.fields.description}"),
                        Text("Rating: ${product.fields.icecreamrating}")
                      ],
                    ),
                  ),
                );
```
