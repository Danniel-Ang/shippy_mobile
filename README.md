<details open>
<summary> Tugas 7 </summary>

## Tugas 7
#### 1. Jelaskan apa yang dimaksud dengan stateless widget dan stateful widget, dan jelaskan perbedaan dari keduanya.
Jawab: 

Stateless widget adalah widget yang bersifat tetap atau statis, artinya tampilan dan data widget ini tidak akan berubah setelah widget tersebut dibuat. 
Contohnya adalah Text, Icon, dan IconButton, yang hanya menampilkan data atau informasi tanpa memerlukan perubahan kondisi. 
Untuk mengimplementasikannya, kita menggunakan extends StatelessWidget, dan hanya perlu mengimplementasikan metode build untuk membangun tampilan widget.

Stateful widget adalah widget yang bersifat dinamis dan dapat berubah berdasarkan interaksi atau event yang disebabkan oleh pengguna, 
seperti input data atau interaksi dengan tombol. Untuk mengimplementasikannya, kita menggunakan extends StatefulWidget 
dan perlu membuat kelas State terpisah untuk menyimpan kondisi atau status widget tersebut. Kelas State ini memiliki metode setState, 
yang memungkinkan widget untuk memperbarui tampilan ketika data atau kondisi berubah.

Perbedaan Utama:
- Stateless widget tidak memiliki state yang dapat berubah, sedangkan stateful widget memiliki state yang dapat berubah-ubah sesuai dengan interaksi pengguna.
- Stateless widget hanya membutuhkan build method dalam kelasnya, sedangkan stateful widget memerlukan dua kelas: satu untuk widget (StatefulWidget) dan satu untuk state (State), yang menyimpan dan mengatur kondisi.
- Stateless widget umumnya lebih ringan karena tidak memerlukan pembaruan ulang, sedangkan stateful widget mungkin lebih berat karena perlu melacak perubahan state.
- 
#### 2. Sebutkan widget apa saja yang kamu gunakan pada proyek ini dan jelaskan fungsinya.
Jawab:
- MaterialApp: Root aplikasi yang mengatur tema dan halaman utama yang akan ditampilkan.
- Scaffold: Struktur dasar halaman yang menyediakan kerangka kerja dengan AppBar, body, dan lainnya.
- AppBar: Bagian atas halaman.
- Column dan Row: Widget tata letak vertikal dan horizontal yang digunakan untuk menata elemen-elemen pada halaman, seperti kartu informasi dan tombol produk.
- Text: Widget untuk menampilkan teks statis.
- GridView.count: Menampilkan item dalam bentuk grid, di sini saya gunakan untuk menampilkan tombol produk dalam format 3 kolom.
- Card: Tampilan berbentuk kartu dengan bayangan yang digunakan untuk membungkus informasi pengguna dan tombol produk.
- InkWell: Menangani interaksi pengguna dan menampilkan efek ripple saat tombol produk ditekan.
- ScaffoldMessenger dan SnackBar: Menampilkan notifikasi sementara di bagian bawah layar saat tombol ditekan.

#### 3. Apa fungsi dari setState()? Jelaskan variabel apa saja yang dapat terdampak dengan fungsi tersebut.
Jawab: Fungsi ini digunakan untuk memberi tau flutter bahwa terjadi suatu perubahan sehingga
tampilan perlu di build ulang dengan menjalankan metode build() sehingga perubahan data akan langsung tampak pada tampilan.

Adapun variabel yang dapat terdampak oleh setState() adalah variabel yang menyimpan data dinamis. 
Ini adalah variabel dalam objek State yang nilai atau statusnya dapat berubah seiring interaksi pengguna atau perubahan kondisi.

Contoh:
```dart
class _CounterState extends State<Counter> {
  int _counter = 0; // Data dinamis

  void _increment() {
    setState(() {
      _counter++; // Mengubah nilai _counter dan memperbarui tampilan
    });
  }
}
```
setState() dipanggil untuk mengubah nilai _counter, 
dan metode build() akan berjalan ulang, sehingga widget akan menampilkan nilai yang terbaru.

#### 4. Jelaskan perbedaan antara const dengan final.
Jawab: const adalah tipe variabel yang mendapatkan nilainya saat compile time. Ini berarti saat mendeklarasikan const, kita harus langsung mengassign suatu nilai pada const. 
Nantinya, variabel const ini tidak dapat berubah lagi nilainya setelah diassign suatu nilai.

Berbeda halnya dengan final, yang juga mendeklarasikan variabel yang tidak dapat diubah, namun nilai dari final bisa ditentukan pada runtime. Artinya, variabel final hanya dapat diinisialisasi satu kali, 
tetapi nilainya dapat kita assign kapan pun itu namun hanya sekali saja. Setelah nilai ditetapkan, final tidak dapat diubah lagi.

Dengan demikian, perbedaan utama antara const dan final terletak pada waktu penetapan nilai dan fleksibilitas penggunaan. const bersifat lebih ketat karena nilainya harus ditentukan saat kompilasi, 
sedangkan final lebih fleksibel dan memungkinkan penetapan nilai pada runtime.

#### 5. Jelaskan bagaimana cara kamu mengimplementasikan checklist-checklist di atas.
Jawab:
- Pertama saya membuat project baru dengan flutter create.
- Kemudian saya mencoba mengimplementasikan card untuk menampilkan nama npm dan kelas sebagai informasi tambahan di aplikasi saya.
- Hal tersebut saya lakukan dengan mencoba membuat class infoCard

```dart
class InfoCard extends StatelessWidget {
  final String title;
  final String content;

  const InfoCard({super.key, required this.title, required this.content});

  @override
  Widget build(BuildContext context) {
    return Card(
      elevation: 4.0,
      child: Container( // Menggunakan container
        width: MediaQuery
            .of(context)
            .size
            .width / 3.5, // Mengatur ukuran
        padding: const EdgeInsets.all(16.0), // Menambahkan padding
        child: Column( // Didalam container terdapat Widget column
          children: [
            Text(
              title,
              style: const TextStyle(fontWeight: FontWeight.bold),
            ), // Text untuk judul
            const SizedBox(height: 8.0),
            Text(content), // Text untuk content
          ],
        ),
      ),
    );
  }
}
```

- Setelah itu saya juga mencoba mengimplementasi masing masing button dengan membuat widget khusus
atau class yang nantinya menerima input berupa nama, icon dan color.
- Untuk mempermudah implementasi, saya menyimpan nilai nama, icon dan color terpisah di class terpisah
dengan widget untuk button nya.

Menyimpan nama, icon dan warna yang akan dipakai
```dart 
class ItemHomepage {
  final String name;
  final IconData icon;
  final Color color;

  ItemHomepage(this.name, this.icon, this.color);
}
```

Button masing-masing yang akan memanggil snackbar ketika ditekan
```dart
class ItemCard extends StatelessWidget {
  final ItemHomepage item;

  const ItemCard(this.item, {super.key});

  @override
  Widget build(BuildContext context) {
    return Card(
      elevation: 3,
      shape: RoundedRectangleBorder(borderRadius: BorderRadius.circular(12)),
      color: item.color,
      child: InkWell(
        onTap: () {
          ScaffoldMessenger.of(context)
            ..hideCurrentSnackBar()
            ..showSnackBar(SnackBar(content: Text("Kamu telah menekan tombol ${item.name}!")));
        },
        child: Container(
          padding: const EdgeInsets.all(8),
          child: Center(
            child: Column(
              mainAxisAlignment: MainAxisAlignment.center,
              children: [
                Icon(
                  item.icon,
                  color: Colors.white,
                  size: 30.0,
                ),
                const SizedBox(height: 5),
                Text(
                  item.name,
                  textAlign: TextAlign.center,
                  style: const TextStyle(color: Colors.white),
                ),
              ],
            ),
          ),
        ),
      ),
    );
  }
}
```

Kemudian saya gabungkan semuanya dalam class utama saya. Beberapa widget layouting saya gunakan seperti
GridView.count, Row, dan Column.
```dart
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text(
          'E-Commerce App',
          style: TextStyle(
            color: Colors.white,
            fontWeight: FontWeight.bold,
          ),
        ),
        backgroundColor: Theme.of(context).colorScheme.primary,
      ),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.center,
          children: [
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                InfoCard(title: 'NPM', content: npm),
                InfoCard(title: 'Name', content: name),
                InfoCard(title: 'Class', content: className),
              ],
            ),
            const SizedBox(height: 16.0),
            Center(
              child: Column(
                children: [
                  const Padding(
                    padding: EdgeInsets.only(top: 16.0),
                    child: Text(
                      'Welcome to Your E-Commerce App',
                      style: TextStyle(
                        fontWeight: FontWeight.bold,
                        fontSize: 18.0,
                      ),
                    ),
                  ),
                  GridView.count(
                    primary: false,
                    padding: const EdgeInsets.all(20),
                    crossAxisSpacing: 10,
                    mainAxisSpacing: 10,
                    crossAxisCount: 3,
                    shrinkWrap: true,
                    children: items.map((ItemHomepage item) {
                      return ItemCard(item);
                    }).toList(),
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

</details>