# Nama : Felix Amon Sitinjak
# NIM : 312410063
# Kelas : TI.24.A1

**Index.php**

<img width="953" height="468" alt="image" src="https://github.com/user-attachments/assets/3c70a55e-cdb0-4cc0-92ae-6912b76f0c7d" />

**Code**
```
<?php 
include("koneksi.php"); 

// query untuk menampilkan data 
$sql = 'SELECT * FROM data_barang'; 
$result = mysqli_query($conn, $sql); 

?> 
<!DOCTYPE html> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8"> 
    <link href="style.css" rel="stylesheet" type="text/css" /> 
    <title>Data Barang</title> 
</head> 
<body> 
    <div class="container"> 
        <h1>Data Barang</h1> 
        <div class="main"> 
            <p><a href="tambah.php">Tambah Barang</a></p> 
            <table> 
            <tr> 
                <th>Gambar</th> 
                <th>Nama Barang</th> 
                <th>Katagori</th> 
                <th>Harga Jual</th> 
                <th>Harga Beli</th> 
                <th>Stok</th> 
                <th>Aksi</th> 
            </tr> 
            <?php if($result): ?> 
            <?php while($row = mysqli_fetch_array($result)): ?> 
            <tr> 
                <td><img src="<?= $row['gambar'];?>" alt="<?= $row['nama'];?>"></td> 
                <td><?= $row['nama'];?></td> 
                <td><?= $row['kategori'];?></td> 
                <td><?= $row['harga_jual'];?></td> 
                <td><?= $row['harga_beli'];?></td> 
                <td><?= $row['stok'];?></td> 
                <td>
                    <a href="ubah.php?id=<?= $row['id_barang'];?>">Ubah</a>
                    <a href="hapus.php?id=<?= $row['id_barang'];?>">Hapus</a>
                </td> 
            </tr> 
            <?php endwhile; else: ?> 
            <tr> 
                <td colspan="7">Belum ada data</td> 
            </tr> 
            <?php endif; ?> 
            </table> 
        </div> 
    </div> 
</body> 
</html>
```

**Penjelasan**
## Fitur Utama

* **Read (Baca Data):** Menampilkan semua data barang dari tabel `data_barang`.
* **Create (Tambah Data):** Menyediakan link ke halaman `tambah.php` untuk menambahkan barang baru.
* **Update (Ubah Data):** Menyediakan link `Ubah` untuk setiap barang yang mengarah ke `ubah.php` (dengan parameter `id_barang`).
* **Delete (Hapus Data):** Menyediakan link `Hapus` untuk setiap barang yang mengarah ke `hapus.php` (dengan parameter `id_barang`).
* Menampilkan gambar, nama, kategori, harga jual, harga beli, dan stok.

## Teknologi yang Digunakan

* **PHP** (dengan ekstensi `mysqli`)
* **MySQL / MariaDB**
* **HTML**
* **CSS** (file `style.css`)

## Cara Menjalankan Proyek

1.  **Web Server & Database:**
    Pastikan Anda memiliki server web dengan PHP dan server database MySQL (seperti XAMPP, WAMP, atau MAMP).

2.  **Clone atau Unduh Proyek:**
    Letakkan semua file proyek (`index.php`, `tambah.php`, `ubah.php`, `hapus.php`, `koneksi.php`, `style.css`) di dalam satu folder di direktori web server Anda (misalnya: `htdocs/data-barang`).

3.  **Buat Database:**
    * Buka `phpMyAdmin` (`http://localhost/phpmyadmin`).
    * Buat database baru, misalnya `db_toko`.

4.  **Buat Tabel `data_barang`:**
    Jalankan query SQL berikut di database `db_toko` Anda untuk membuat tabel yang diperlukan:

    ```sql
    CREATE TABLE data_barang (
      id_barang INT AUTO_INCREMENT PRIMARY KEY,
      nama VARCHAR(255) NOT NULL,
      kategori VARCHAR(100),
      harga_jual INT(11),
      harga_beli INT(11),
      stok INT(5),
      gambar VARCHAR(255)
    );
    ```

5.  **Konfigurasi Koneksi (`koneksi.php`):**
    Buat file `koneksi.php` dan isi dengan skrip berikut. Sesuaikan `host`, `user`, `password`, dan `database` jika perlu.

    ```php
    <?php
    // ganti "root" dengan username Anda, "" dengan password Anda, dan "db_toko" dengan nama database Anda
    $conn = mysqli_connect("localhost", "root", "", "db_toko");

    if (!$conn) {
        die("Koneksi gagal: " . mysqli_connect_error());
    }
    ?>
    ```

**tambah.php**

<img width="959" height="473" alt="image" src="https://github.com/user-attachments/assets/ce12374b-8ec1-4e71-a788-3b67c5a5b32e" />

**COde**
```
<?php 
error_reporting(E_ALL); 
include_once 'koneksi.php'; 
 
if (isset($_POST['submit'])) 
{ 
    $nama = $_POST['nama']; 
    $kategori = $_POST['kategori']; 
    $harga_jual = $_POST['harga_jual']; 
    $harga_beli = $_POST['harga_beli']; 
    $stok = $_POST['stok']; 
    $file_gambar = $_FILES['file_gambar']; 
    $gambar = null; 
    if ($file_gambar['error'] == 0) 
    { 
        $filename = str_replace(' ', '_',$file_gambar['name']); 
        $destination = dirname(__FILE__) .'/gambar/' . $filename; 
        if(move_uploaded_file($file_gambar['tmp_name'], $destination)) 
        { 
            $gambar = 'gambar/' . $filename;; 
        } 
    } 
    $sql = 'INSERT INTO data_barang (nama, kategori,harga_jual, harga_beli, 
stok, gambar) '; 
    $sql .= "VALUE ('{$nama}', '{$kategori}','{$harga_jual}', 
'{$harga_beli}', '{$stok}', '{$gambar}')"; 
    $result = mysqli_query($conn, $sql); 
    header('location: index.php'); 
}
?> 
<!DOCTYPE html> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8"> 
    <link href="style.css" rel="stylesheet" type="text/css" /> 
    <title>Tambah Barang</title> 
</head> 
<body> 
<div class="container"> 
    <h1>Tambah Barang</h1> 
    <div class="main"> 
        <form method="post" action="tambah.php" 
enctype="multipart/form-data"> 
            <div class="input"> 
                <label>Nama Barang</label> 
                <input type="text" name="nama" /> 
            </div> 
            <div class="input"> 
                <label>Kategori</label> 
                <select name="kategori"> 
                    <option value="Komputer">Komputer</option> 
                    <option value="Elektronik">Elektronik</option> 
                    <option value="Hand Phone">Hand Phone</option> 
                </select> 
            </div> 
            <div class="input"> 
                <label>Harga Jual</label> 
                <input type="text" name="harga_jual" /> 
            </div> 
            <div class="input"> 
                <label>Harga Beli</label> 
                <input type="text" name="harga_beli" /> 
            </div> 
            <div class="input"> 
                <label>Stok</label> 
                <input type="text" name="stok" /> 
            </div> 
            <div class="input"> 
                <label>File Gambar</label> 
                <input type="file" name="file_gambar" /> 
            </div> 
            <div class="submit"> 
                <input type="submit" name="submit" value="Simpan" /> 
            </div> 
        </form> 
    </div>
</div> 
</body> 
</html>
```

**Penjelasan**
## Fitur Utama

* **Read (Baca Data):** Menampilkan semua data barang dari tabel `data_barang`.
* **Create (Tambah Data):** Menyediakan formulir untuk menambahkan barang baru (termasuk nama, kategori, harga, stok, dan gambar).
* **Update (Ubah Data):** Menyediakan link `Ubah` untuk setiap barang yang mengarah ke `ubah.php`.
* **Delete (Hapus Data):** Menyediakan link `Hapus` untuk setiap barang yang mengarah ke `hapus.php`.
* **File Upload:** Mengizinkan pengguna mengunggah gambar produk, yang akan disimpan di folder `/gambar` di server.

## Teknologi yang Digunakan

* **PHP** (dengan ekstensi `mysqli`)
* **MySQL / MariaDB**
* **HTML**
* **CSS** (file `style.css`)

## Cara Menjalankan Proyek

1.  **Web Server & Database:**
    Pastikan Anda memiliki server web dengan PHP dan server database MySQL (seperti XAMPP, WAMP, atau MAMP).

2.  **Clone atau Unduh Proyek:**
    Letakkan semua file proyek (`index.php`, `tambah.php`, `koneksi.php`, dll.) di dalam satu folder di direktori web server Anda (misalnya: `htdocs/data-barang`).

3.  **Buat Database:**
    * Buka `phpMyAdmin` (`http://localhost/phpmyadmin`).
    * Buat database baru, misalnya `db_toko`.

4.  **Buat Tabel `data_barang`:**
    Jalankan query SQL berikut di database `db_toko` Anda. Skema ini sesuai dengan yang digunakan di `index.php` dan `tambah.php`.

    ```sql
    CREATE TABLE data_barang (
      id_barang INT AUTO_INCREMENT PRIMARY KEY,
      nama VARCHAR(255) NOT NULL,
      kategori VARCHAR(100),
      harga_jual INT(11),
      harga_beli INT(11),
      stok INT(5),
      gambar VARCHAR(255)
    );
    ```

5.  **Konfigurasi Koneksi (`koneksi.php`):**
    Buat file `koneksi.php` dan isi dengan skrip berikut. Sesuaikan `host`, `user`, `password`, dan `database` jika perlu.

    ```php
    <?php
    // ganti "root" dengan username Anda, "" dengan password Anda, dan "db_toko" dengan nama database Anda
    $conn = mysqli_connect("localhost", "root", "", "db_toko");

    if (!$conn) {
        die("Koneksi gagal: " . mysqli_connect_error());
    }
    ?>
    ```
**ubah.php**

<img width="959" height="469" alt="image" src="https://github.com/user-attachments/assets/c1156b31-998f-4210-83ae-df675e14b620" />

**Code**
```
<?php 
error_reporting(E_ALL); 
include_once 'koneksi.php'; 
 
if (isset($_POST['submit'])) 
{ 
    $id = $_POST['id']; 
    $nama = $_POST['nama']; 
    $kategori = $_POST['kategori']; 
    $harga_jual = $_POST['harga_jual']; 
    $harga_beli = $_POST['harga_beli']; 
    $stok = $_POST['stok']; 
    $file_gambar = $_FILES['file_gambar']; 
    $gambar = null; 
     
    if ($file_gambar['error'] == 0) 
    { 
        $filename = str_replace(' ', '_', $file_gambar['name']); 
        $destination = dirname(__FILE__) . '/gambar/' . $filename; 
        if (move_uploaded_file($file_gambar['tmp_name'], $destination)) 
        { 
            $gambar = 'gambar/' . $filename;; 
        } 
            } 
 
    $sql = 'UPDATE data_barang SET '; 
    $sql .= "nama = '{$nama}', kategori = '{$kategori}', "; 
    $sql .= "harga_jual = '{$harga_jual}', harga_beli = '{$harga_beli}', stok 
= '{$stok}' "; 
    if (!empty($gambar)) 
        $sql .= ", gambar = '{$gambar}' "; 
    $sql .= "WHERE id_barang = '{$id}'"; 
    $result = mysqli_query($conn, $sql); 
 
    header('location: index.php'); 
} 
 
$id = $_GET['id']; 
$sql = "SELECT * FROM data_barang WHERE id_barang = '{$id}'"; 
$result = mysqli_query($conn, $sql); 
if (!$result) die('Error: Data tidak tersedia'); 
$data = mysqli_fetch_array($result); 
 
function is_select($var, $val) { 
    if ($var == $val) return 'selected="selected"'; 
    return false; 
} 
 
?> 
<!DOCTYPE html> 
<html lang="en"> 
<head> 
    <meta charset="UTF-8"> 
    <link href="style.css" rel="stylesheet" type="text/css" /> 
    <title>Ubah Barang</title> 
</head> 
<body> 
<div class="container"> 
    <h1>Ubah Barang</h1> 
    <div class="main"> 
        <form method="post" action="ubah.php" 
enctype="multipart/form-data"> 
            <div class="input"> 
                <label>Nama Barang</label> 
                <input type="text" name="nama" value="<?php echo 
$data['nama'];?>" /> 
            </div> 
            <div class="input"> 
                <label>Kategori</label> 
                <select name="kategori"> 
                <option <?php echo is_select 
('Komputer', $data['kategori']);?> value="Komputer">Komputer</option> 
                    <option <?php echo is_select 
('Komputer', $data['kategori']);?> value="Elektronik">Elektronik</option> 
                    <option <?php echo is_select 
('Komputer', $data['kategori']);?> value="Hand Phone">Hand Phone</option> 
                </select> 
            </div> 
            <div class="input"> 
                <label>Harga Jual</label> 
                <input type="text" name="harga_jual" value="<?php echo 
$data['harga_jual'];?>" /> 
            </div> 
            <div class="input"> 
                <label>Harga Beli</label> 
                <input type="text" name="harga_beli" value="<?php echo 
$data['harga_beli'];?>" /> 
            </div> 
            <div class="input"> 
                <label>Stok</label> 
                <input type="text" name="stok" value="<?php echo 
$data['stok'];?>" /> 
            </div> 
            <div class="input"> 
                <label>File Gambar</label> 
                <input type="file" name="file_gambar" /> 
            </div> 
            <div class="submit"> 
            <input type="hidden" name="id" value="<?php echo 
$data['id_barang'];?>" /> 
                <input type="submit" name="submit" value="Simpan" /> 
            </div> 
        </form> 
    </div> 
</div> 
</body> 
</html> 
```

**Penjelasan**
## Fitur Utama

* **Read (Baca Data):** Menampilkan semua data barang dari tabel `data_barang`.
* **Create (Tambah Data):** Menyediakan formulir untuk menambahkan barang baru (termasuk nama, kategori, harga, stok, dan gambar).
* **Update (Ubah Data):** Menyediakan link `Ubah` untuk setiap barang yang mengarah ke `ubah.php`.
* **Delete (Hapus Data):** Menyediakan link `Hapus` untuk setiap barang yang mengarah ke `hapus.php`.
* **File Upload:** Mengizinkan pengguna mengunggah gambar produk, yang akan disimpan di folder `/gambar` di server.

## Teknologi yang Digunakan

* **PHP** (dengan ekstensi `mysqli`)
* **MySQL / MariaDB**
* **HTML**
* **CSS** (file `style.css`)

## Cara Menjalankan Proyek

1.  **Web Server & Database:**
    Pastikan Anda memiliki server web dengan PHP dan server database MySQL (seperti XAMPP, WAMP, atau MAMP).

2.  **Clone atau Unduh Proyek:**
    Letakkan semua file proyek (`index.php`, `tambah.php`, `koneksi.php`, dll.) di dalam satu folder di direktori web server Anda (misalnya: `htdocs/data-barang`).

3.  **Buat Database:**
    * Buka `phpMyAdmin` (`http://localhost/phpmyadmin`).
    * Buat database baru, misalnya `db_toko`.

4.  **Buat Tabel `data_barang`:**
    Jalankan query SQL berikut di database `db_toko` Anda. Skema ini sesuai dengan yang digunakan di `index.php` dan `tambah.php`.

    ```sql
    CREATE TABLE data_barang (
      id_barang INT AUTO_INCREMENT PRIMARY KEY,
      nama VARCHAR(255) NOT NULL,
      kategori VARCHAR(100),
      harga_jual INT(11),
      harga_beli INT(11),
      stok INT(5),
      gambar VARCHAR(255)
    );
    ```

5.  **Konfigurasi Koneksi (`koneksi.php`):**
    Buat file `koneksi.php` dan isi dengan skrip berikut. Sesuaikan `host`, `user`, `password`, dan `database` jika perlu.

    ```php
    <?php
    // ganti "root" dengan username Anda, "" dengan password Anda, dan "db_toko" dengan nama database Anda
    $conn = mysqli_connect("localhost", "root", "", "db_toko");

    if (!$conn) {
        die("Koneksi gagal: " . mysqli_connect_error());
    }
    ?>
    ```
