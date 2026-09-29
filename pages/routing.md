---
title: Navigation & Routing
permalink: /routing/
---

# Navigation and Routing

Suatu aplikasi mobile baik di Android ataupun iOS umumnya memiliki banyak halaman. Untuk berpindah dari satu halaman ke halaman lainnya, kita dapat menggunakan widget `Navigator`.

Widget `Navigator` bekerja dengan konsep stack atau tumpukkan, artinya jika kita ingin berpindah dari halaman 1 ke halaman 2, maka widget `Navigator` akan menumpuk halaman 2 diatas halaman 1. Jika ingin kembali ke halaman 1, maka halaman 2 akan dikeluarkan dari tumpukkan. Gambar dibawah ini menunjukkan konsep stack.

![Push Pop](/assets/img/push_pop.png)

## Simple Routing
Terdapat dua method dalam widget `Navigator` untuk memasukkan dan mengeluarkan halaman ke atau dari stack.

- `Navigator.push()` digunakan untuk memasukkan halaman kedalam stack.
- `Navigator.pop()` digunakan untuk mengeluarkan halaman dari stack.

![Simple Routing](/assets/img/simple.gif)

Kita akan membuat aplikasi seperti gambar diatas. Aplikasi terdiri dari 2 halaman dengan masing-masing halaman terdiri dari 1 buah tombol untuk navigasi.

Buat project baru dan sesuaikan kode `main.dart` seperti berikut ini

**`main.dart`**

```dart
import 'package:flutter/material.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MainPage(),
    );
  }
}
```

Berikutnya buat folder **pages** di dalam folder lib dan buat dua buah file `main_page.dart` dan `second_page.dart`.

**`main_page.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation/pages/second_page.dart';

class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Main Page"),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () { },
          child: Text("Ke halaman berikutnya"),
        ),
      ),
    );
  }
}
```

**`second_page.dart`**
```dart
import 'package:flutter/material.dart';

class SecondPage extends StatelessWidget {
  const SecondPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Second Page"),
      ),
      body: Center(
        child: ElevatedButton(
          onPressed: () { },
          child: Text("Kembali ke halaman sebelumnya"),
        ),
      ),
    );
  }
}
```

Untuk berpindah dari `MainPage` ke `SecondPage` kita tambahkan widget `Navigator` dengan method `push` kedalam method `onPressed()` `ElevatedButton`.

```dart
onPressed: () {
  Navigator.push(
    context,
    MaterialPageRoute(builder: (context) => const SecondPage()),
  );
},
```

Untuk kembali ke `MainPage` dari `SecondPage` kita tambahkan widget `Navigator` dengan method `pop` kedalam method `onPressed()` `ElevatedButton`.

```dart
onPressed: () {
  Navigator.pop(context);
},
```

## Named Routing

Saat aplikasi memiliki banyak halaman maka cara diatas menjadi tidak efektif, karena kita harus mengingat nama class masing-masing halaman untuk navigasi. Untuk navigasi yang lebih mudah, kita dapat memberi nama untuk masing-masing halaman sesuai keinginan kita agar lebih mudah mengingatnya.

Untuk menambahkan halaman ke dalam stack kita tidak lagi menggunakan method `push`, tetapi `Navigator.pushNamed()`. Untuk menghapus halaman dari stack kita tetap menggunakan method `pop`.

Untuk menggunakan named routing, ubah method main seperti berikut ini.

**`main.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation/pages/main_page.dart';
import 'package:navigation/pages/second_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: '/',
      routes: {
        '/': (context) => MainPage(),
        '/second': (context) => const SecondPage(),
      },
    );
  }
}
```

atau

**`main.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation/pages/main_page.dart';
import 'package:navigation/pages/second_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: MainPage(),
      routes: {
        '/second': (context) => const SecondPage(),
      },
    );
  }
}
```

Perbedaan kedua cara diatas adalah jika kita menggunakan properti `home` maka kita tidak perlu menyertakan routes ke halaman utama, namun jika tidak menggunakan properti `home`, kita harus menggunakan properti `initialRoute` dan mendaftarkan nama halaman utama di properti `route`.

Pada kode diatas, kita mendaftarkan dua buah halaman dengan namanya masing-masing. `'/'` untuk halaman utama, `'/second'` untuk halaman kedua.

Ubah kode `onPressed()` pada MainPage seperti berikut ini

```dart
onPressed: () {
  Navigator.pushNamed(context, '/second');
},
```

## Route Generator

Selain menggunakan cara diatas, kita bisa menggunakan widget `RouteGenerator` untuk mengatur navigasi pada aplikasi.

Buat file `route_generator.dart` di folder lib.

**`route_generator.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation/pages/error_page.dart';
import 'package:navigation/pages/main_page.dart';
import 'package:navigation/pages/second_page.dart';

class RouteGenerator {
  static Route<dynamic> generateRoute(RouteSettings settings) {
    switch (settings.name) {
      case '/':
        return MaterialPageRoute(builder: (_) => const MainPage());
      case '/second':
        return MaterialPageRoute(builder: (_) => const SecondPage());
      default:
        return MaterialPageRoute(builder: (_) => const ErrorPage());
    }
  }
}
```

Kemudian buat file `error_page.dart` di folder pages.

**`error_page.dart`**
```dart
import 'package:flutter/material.dart';

class ErrorPage extends StatelessWidget {
  const ErrorPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Error"),
      ),
      body: const Center(
        child: Text("Page not found"),
      ),
    );
  }
}
```
Tambahkan sebuah tombol di halaman 1.

**`main_page.dart`**
```dart
import 'package:flutter/material.dart';

class MainPage extends StatelessWidget {
  const MainPage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Main Page"),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/second');
              },
              child: Text("Go to Second Page"),
            ),
            ElevatedButton(
              onPressed: () {
                Navigator.pushNamed(context, '/third');
              },
              child: Text("Go to other page"),
            ),
          ],
        ),
      ),
    );
  }
}
```

Perbarui file `main.dart`

**`main.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation/route_generator.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(onGenerateRoute: RouteGenerator.generateRoute);
  }
}
```

![Route Generator](/assets/img/routegenerator.gif)

> Mengapa saat tombol "Go to other page" ditekan diarahkan ke error page?

## Pass arguments

Kita dapat mengirimkan data saat berpindah ke halaman lainnya seperti contoh berikut ini.

![Pass Arguments](/assets/img/nav_data.gif)

### Simple Routing

Buat project baru dan sesuaikan file `main.dart` seperti berikut ini

**`main.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation_data/pages/home_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      home: HomePage(),
    );
  }
}
```

Buat folder pages didalam folder lib, dan buat 2 buah file `home_page.dart` dan `profile_page.dart`.

**`profile_page.dart`**
```dart
import 'package:flutter/material.dart';

class ProfilePage extends StatelessWidget {
  final String name;
  const ProfilePage(this.name, {super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Profile Page"),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              "Welcome",
              style: TextStyle(fontSize: 30.0),
            ),
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Text(
                name,
                style: const TextStyle(fontSize: 40.0, color: Colors.blue),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Pada kode `profile_page.dart` diatas, kita membuat sebuah properti `name` yang akan menampung data yang dikirimkan dari halaman `home_page.dart`.

```dart
final String name;
const ProfilePage(this.name, {super.key});
```

Pada halaman ini, kita buat 2 widget `Text`. Yang pertama berisikan Welcome dan yang kedua isinya sesuai dengan properti `name`.

```dart
const Text(
  "Welcome",
  style: TextStyle(fontSize: 30.0),
),

Text(
  name,
  style: const TextStyle(fontSize: 40.0, color: Colors.blue),
),
```

Selanjutnya kode halaman `home_page.dart` adalah sebagai berikut:

**`home_page.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation_data/pages/profile_page.dart';

class HomePage extends StatelessWidget {
  final myController = TextEditingController();
  HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Home Page"),
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Padding(
            padding: const EdgeInsets.all(12.0),
            child: TextField(
              onTap: () => myController.clear(),
              controller: myController,
              decoration: const InputDecoration(
                border: OutlineInputBorder(),
                hintText: 'Enter your name',
              ),
            ),
          ),
          ElevatedButton(
            onPressed: () {
              Navigator.push(
                  context,
                  MaterialPageRoute(
                      builder: (context) => ProfilePage(myController.text)));
            },
            child: const Text("Enter"),
          ),
        ],
      ),
    );
  }
}
```

Pada halaman `home_page.dart` kita membuat widget `TextField` dan `ElevatedButton`. Widget `TextField` digunakan untuk menerima input yang akan diteruskan ke halaman `profile_page.dart`.

Untuk menangani perubahan pada widget `TextField` kita membuat sebuah variabel `myController` yang merupakan sebuah controller `TextEditingController()`.

```dart
final myController = TextEditingController();
```

Kemudian kita hubungkan controller tersebut ke widget `TextWidget` menggunakan property `controller`.

```dart
controller: myController,
```

Karena kita sudah menghubungkan `myController` dengan widget `TextField` maka kita dapat mengakses data yang diinputkan menggunakan `myController.text` dan mengirimkannya ke halaman `profile_page.dart`.

```dart
MaterialPageRoute(
  builder: (context) => ProfilePage(myController.text),
),
```

### Named Routing

Jika kita menggunakan named routing, kita tidak dapat menggunakan teknik property dan constructor seperti pada simple routing.

Ubah halaman `profile_page.dart` menjadi seperti berikut ini:

**`profile_page.dart`**
```dart
import 'package:flutter/material.dart';

class ProfilePage extends StatelessWidget {
  const ProfilePage({super.key});

  @override
  Widget build(BuildContext context) {
    String name = ModalRoute.of(context)?.settings.arguments as String;
    return Scaffold(
      appBar: AppBar(
        title: const Text("Profile Page"),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: [
            const Text(
              "Welcome",
              style: TextStyle(fontSize: 30.0),
            ),
            Padding(
              padding: const EdgeInsets.all(8.0),
              child: Text(
                name,
                style: const TextStyle(fontSize: 40.0, color: Colors.blue),
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Pada kode diatas, kita tidak lagi membuat property name pada class, namun diganti dengan sebuah variabel name yang berisi argument yang dikirimkan lewat named routing (baris 8).

```dart
String name = ModalRoute.of(context)?.settings.arguments as String;
```

Kemudian pada file `main.dart`kita ubah routingnya menjadi seperti berikut ini:

**`main.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation_data/pages/home_page.dart';
import 'package:navigation_data/pages/profile_page.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      initialRoute: '/',
      routes: {
        '/': (context) => HomePage(),
        '/profile': (context) => ProfilePage(),
      },
    );
  }
}
```

Berikut ini kode halaman `home_page.dart`.

**`home_page.dart`**
```dart
import 'package:flutter/material.dart';
import 'package:navigation_data/pages/profile_page.dart';

class HomePage extends StatelessWidget {
  final myController = TextEditingController();
  HomePage({super.key});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text("Home Page"),
      ),
      body: Column(
        mainAxisAlignment: MainAxisAlignment.center,
        children: [
          Padding(
            padding: const EdgeInsets.all(12.0),
            child: TextField(
              onTap: () => myController.clear(),
              controller: myController,
              decoration: const InputDecoration(
                border: OutlineInputBorder(),
                hintText: 'Enter your name',
              ),
            ),
          ),
          ElevatedButton(
            onPressed: () {
              Navigator.pushNamed(context, '/profile',
                  arguments: myController.text);
            },
            child: const Text("Enter"),
          ),
        ],
      ),
    );
  }
}
```

Pada halaman `home_page.dart` baris ke 30, pengiriman data lewat named route menggunakan property arguments.

```dart
Navigator.pushNamed(context, '/profile',
  arguments: myController.text);
```

# Latihan

## Techno Shop

Kita akan membuat aplikasi seperti berikut ini:

![Techno Shop](/assets/img/techno1.png)
![Techno Shop](/assets/img/techno2.png)

Buat folder **models** dan buat file `products.dart`

**`product.dart`**
```dart
class Product {
  final String name;
  final String description;
  final double price;

  Product({required this.name, required this.description, required this.price});
}
```

Buat folder **pages** dan buat file `product_list_page.dart` dan `product_detail_page.dart`

**`product_list_page.dart`**
```dart
class ProductListPage extends StatelessWidget {
  final List<Product> products = [
    Product(name: "Mouse Wireless", description: "Mouse tanpa kabel dengan sensor optik", price: 120000),
    Product(name: "Keyboard Mechanical", description: "Keyboard dengan switch biru dan lampu RGB", price: 450000),
    Product(name: "Headset Gaming", description: "Headset dengan mikrofon dan suara stereo", price: 320000),
  ];

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text("Daftar Produk TechnoShop")),
      body: ListView.builder(
        itemCount: products.length,
        itemBuilder: (context, index) {
          final product = products[index];
          return ListTile(
            title: Text(product.name),
            subtitle: Text("Rp ${product.price.toStringAsFixed(0)}"),
            trailing: Icon(Icons.arrow_forward),
            onTap: () {
              // Navigasi ke halaman detail
              Navigator.push(
                context,
                MaterialPageRoute(
                  builder: (context) => ProductDetailPage(product: product),
                ),
              );
            },
          );
        },
      ),
    );
  }
}
```

**`product_detail_page.dart`**
```dart
class ProductDetailPage extends StatelessWidget {
  final Product product;

  const ProductDetailPage({super.key, required this.product});

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: Text(product.name)),
      body: Padding(
        padding: const EdgeInsets.all(16.0),
        child: Column(
          crossAxisAlignment: CrossAxisAlignment.start,
          children: [
            Text(product.name, style: TextStyle(fontSize: 24, fontWeight: FontWeight.bold)),
            SizedBox(height: 10),
            Text(product.description),
            SizedBox(height: 20),
            Text("Harga: Rp ${product.price.toStringAsFixed(0)}", style: TextStyle(fontSize: 18)),
            Spacer(),
            Center(
              child: ElevatedButton.icon(
                icon: Icon(Icons.arrow_back),
                label: Text("Kembali ke Daftar"),
                onPressed: () {
                  Navigator.pop(context);
                },
              ),
            ),
          ],
        ),
      ),
    );
  }
}
```

Pada file `main.dart` ubah menjadi seperti berikut

**`main.dart`**
```dart
Widget build(BuildContext context) {
  return MaterialApp(
    title: 'TechnoShop',
    home: ProductListPage(),
  );
}
```



