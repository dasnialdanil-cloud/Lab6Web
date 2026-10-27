# Lab6Web — Praktikum 6: JavaScript DOM dan Event Handling

| | |
|---|---|
| **Nama** | Daniel Dasnial |
| **NIM** | 312510492 |
| **Mata Kuliah** | Pemrograman Web |
| **Pertemuan** | 7 — JavaScript DOM dan Event Handling |
| **Program Studi** | Teknik Informatika |

---

## 1. Tujuan Praktikum

1. Memahami DOM sebagai representasi dokumen HTML yang dapat diakses melalui JavaScript.
2. Menggunakan `id`, `class`, dan selector CSS untuk memilih elemen HTML.
3. Mengubah teks, atribut, kelas CSS, dan gaya elemen melalui JavaScript.
4. Menangani event pengguna menggunakan `addEventListener()`.
5. Membuat validasi form dan menampilkan pesan sesuai kondisi input.
6. Mengembangkan halaman interaktif dengan minimal tiga fungsi yang berjalan benar.

## 2. Struktur File

```
Lab6Web/
├── index.html            # Halaman Latihan 1–6 + Eksperimen
├── style.css             # Gaya halaman latihan
├── script.js             # JavaScript Latihan 1–6 + Eksperimen
├── mini-project/
│   ├── index.html        # Form Pendaftaran Workshop
│   ├── style.css
│   └── script.js
├── screenshots/          # Seluruh screenshot hasil pengujian
└── README.md
```

## 3. Cara Menjalankan

1. Ekstrak file ZIP, lalu buka folder `Lab6Web`.
2. Buka `index.html` di browser (klik dua kali, atau *Open with Live Server* di VS Code) untuk Latihan 1–6.
3. Buka `mini-project/index.html` untuk Mini Project Form Pendaftaran Workshop.
4. Tekan **F12** untuk membuka Developer Tools → tab **Console** guna memeriksa error.

> Tidak diperlukan instalasi apa pun. Cukup browser modern (Chrome, Edge, Firefox).

---

## 4. Langkah Kerja, Sintaks, dan Hasil

Halaman `index.html` memuat JavaScript dengan atribut `defer`:

```html
<script src="script.js" defer></script>
```

`defer` membuat JavaScript dijalankan **setelah HTML selesai diparsing**, sehingga elemen yang dipilih sudah tersedia di DOM.

### Latihan 1 — Mengubah Teks dengan DOM

```js
const judul = document.getElementById("judul");
const pesan = document.getElementById("pesan");
judul.textContent = "Belajar JavaScript DOM";
pesan.textContent = "Konten berhasil diubah oleh JavaScript.";
```

**Penjelasan:**
- `document.getElementById("judul")` memilih elemen berdasarkan atribut `id` dan menyimpannya ke variabel `judul`.
- `.textContent = "..."` mengganti isi teks elemen. Nilai awal HTML ("Latihan DOM" dan "Belum ada tindakan.") langsung diganti oleh JavaScript saat halaman dimuat.
- `textContent` memperlakukan nilai sebagai teks biasa (bukan markup HTML), sehingga aman.

**Hasil:** judul berubah menjadi *Belajar JavaScript DOM* dan paragraf berubah menjadi *Konten berhasil diubah oleh JavaScript.*

![Latihan 1](screenshots/01-latihan1-ubah-teks.png)

---

### Latihan 2 — Event Click

```js
const tombol = document.getElementById("tombolAksi");
tombol.addEventListener("click", function () {
  pesan.textContent = "Tombol diklik pada " + new Date().toLocaleTimeString();
});
```

**Penjelasan:**
- `addEventListener("click", fungsi)` menghubungkan event **click** pada tombol dengan sebuah fungsi.
- Setiap tombol diklik, fungsi dijalankan: `new Date().toLocaleTimeString()` mengambil jam saat itu, lalu ditampilkan di paragraf `#pesan`.
- Klik berulang akan memperbarui waktu pada pesan.

**Hasil:** setelah tombol diklik, pesan menampilkan waktu klik terakhir.

![Latihan 2](screenshots/02-latihan2-event-click.png)

---

### Latihan 3 — Mengubah Kelas dan Gaya CSS

CSS (`style.css`):

```css
.highlight {
  color: #173f75;
  background-color: #e6f1ff;
  border-left: 4px solid #3478c8;
  padding: 12px;
}
.teks-besar { font-size: 1.4rem; }
```

JavaScript:

```js
const tombolGaya = document.getElementById("tombolGaya");
const teksGaya = document.getElementById("teksGaya");
tombolGaya.addEventListener("click", function () {
  teksGaya.classList.toggle("highlight");
  teksGaya.classList.toggle("teks-besar");
});
```

**Penjelasan:**
- `classList.add("x")` → menambahkan class jika belum ada.
- `classList.remove("x")` → menghapus class.
- `classList.toggle("x")` → jika class ada maka dihapus, jika belum ada maka ditambahkan (bolak-balik).
- Dengan `toggle`, satu tombol dapat menyalakan dan mematikan tampilan *highlight* dan teks besar.

**Sebelum diklik:**

![Latihan 3 sebelum](screenshots/03a-latihan3-sebelum-toggle.png)

**Sesudah diklik (class `highlight` dan `teks-besar` aktif):**

![Latihan 3 sesudah](screenshots/03b-latihan3-sesudah-toggle.png)

---

### Latihan 4 — Event Input dan Penghitung Karakter

HTML:

```html
<label for="namaInput">Nama:</label>
<input type="text" id="namaInput" maxlength="40">
<p>Jumlah karakter: <span id="jumlahKarakter">0</span></p>
```

JavaScript:

```js
const namaInput = document.getElementById("namaInput");
const jumlahKarakter = document.getElementById("jumlahKarakter");
namaInput.addEventListener("input", function () {
  jumlahKarakter.textContent = namaInput.value.length;
});
```

**Penjelasan:**
- Event `input` terpicu **setiap kali isi kolom berubah** (mengetik, menghapus, atau menempel teks).
- `namaInput.value.length` memberi jumlah karakter saat ini, lalu ditulis ke `<span id="jumlahKarakter">`.
- Atribut `maxlength="40"` membatasi input maksimal 40 karakter.

**Hasil:** mengetik "Daniel Dasnial" menampilkan jumlah karakter 14.

![Latihan 4](screenshots/04-latihan4-penghitung-karakter.png)

---

### Latihan 5 — Validasi Form Pendaftaran

HTML:

```html
<form id="formPendaftaran" novalidate>
  <label for="nama">Nama lengkap</label>
  <input id="nama" name="nama" type="text">
  <label for="email">Email</label>
  <input id="email" name="email" type="email">
  <label for="usia">Usia</label>
  <input id="usia" name="usia" type="number" min="17" max="100">
  <button type="submit">Daftar</button>
</form>
<p id="pesanForm" aria-live="polite"></p>
```

JavaScript:

```js
form.addEventListener("submit", function (event) {
  event.preventDefault();
  const nama = document.getElementById("nama").value.trim();
  const email = document.getElementById("email").value.trim();
  const usiaInput = document.getElementById("usia").value;
  const usia = Number(usiaInput);
  const polaEmail = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;

  if (nama === "") {
    // "Nama wajib diisi."
  } else if (!polaEmail.test(email)) {
    // "Masukkan email yang valid."
  } else if (usiaInput === "" || !Number.isInteger(usia) || usia < 17 || usia > 100) {
    // "Usia harus berupa bilangan bulat 17–100."
  } else {
    // "Validasi berhasil. Form siap diproses."
    form.reset();
  }
});
```

**Penjelasan:**
- `novalidate` mematikan validasi bawaan browser agar validasi dilakukan oleh JavaScript.
- `event.preventDefault()` mencegah halaman dimuat ulang saat form dikirim, sehingga pemeriksaan bisa selesai lebih dulu.
- `.trim()` membuang spasi di awal/akhir agar input berisi spasi saja dianggap kosong.
- Email diperiksa dengan *regular expression* `/^[^\s@]+@[^\s@]+\.[^\s@]+$/` (ada karakter, `@`, domain, titik, dan akhiran).
- Usia diubah ke angka dengan `Number()`, lalu diperiksa harus bilangan bulat antara 17–100.
- Pesan error tampil berwarna merah, pesan sukses berwarna hijau (class `error` / `sukses`). Bila valid, `form.reset()` mengosongkan form.

**Lima kondisi pengujian:**

| No | Kondisi | Hasil yang diharapkan |
|----|---------|-----------------------|
| 1 | Semua kosong | "Nama wajib diisi." |
| 2 | Nama kosong, lainnya valid | "Nama wajib diisi." |
| 3 | Email tidak valid (`daniel@mail`) | "Masukkan email yang valid." |
| 4 | Usia di bawah 17 (`15`) | "Usia harus berupa bilangan bulat 17–100." |
| 5 | Semua valid | "Validasi berhasil. Form siap diproses." |

**1) Semua kosong (tidak valid):**

![Latihan 5 semua kosong](screenshots/05a-latihan5-semua-kosong.png)

**2) Nama kosong (tidak valid):**

![Latihan 5 nama kosong](screenshots/05b-latihan5-nama-kosong.png)

**3) Email tidak valid:**

![Latihan 5 email tidak valid](screenshots/05c-latihan5-email-tidak-valid.png)

**4) Usia di bawah 17:**

![Latihan 5 usia di bawah 17](screenshots/05d-latihan5-usia-di-bawah-17.png)

**5) Semua input valid:**

![Latihan 5 valid](screenshots/05e-latihan5-semua-valid.png)

---

### Latihan 6 — Daftar Tugas (To-do List)

HTML:

```html
<input id="inputTugas" type="text">
<button id="tambahTugas" type="button">Tambah</button>
<p id="pesanTugas" aria-live="polite"></p>
<ul id="daftarTugas"></ul>
```

JavaScript (inti):

```js
tambahTugas.addEventListener("click", function () {
  const teks = inputTugas.value.trim();
  if (teks === "") {
    pesanTugas.textContent = "Tulis tugas terlebih dahulu.";
    return;
  }
  const item = document.createElement("li");
  const label = document.createElement("span");
  const selesai = document.createElement("button");
  label.textContent = teks + " ";
  selesai.type = "button";
  selesai.textContent = "Tandai selesai";
  selesai.addEventListener("click", function () {
    item.classList.toggle("selesai");
    selesai.textContent = item.classList.contains("selesai")
      ? "Batalkan selesai" : "Tandai selesai";
  });
  item.append(label, selesai);
  daftarTugas.appendChild(item);
  inputTugas.value = "";
  pesanTugas.textContent = "Tugas ditambahkan.";
  inputTugas.focus();
});
```

**Penjelasan:**
- `document.createElement("li")` membuat elemen baru lewat JavaScript; `append()` / `appendChild()` memasukkannya ke dalam DOM.
- Teks tugas dimasukkan dengan `textContent` (bukan `innerHTML`) sehingga teks pengguna tidak dijalankan sebagai markup HTML — lebih aman.
- Tombol *Tandai selesai* memakai `classList.toggle("selesai")`; CSS `.selesai span { text-decoration: line-through; }` mencoret teks.
- `inputTugas.focus()` mengembalikan kursor ke kolom input agar pengguna bisa langsung menambah tugas berikutnya.
- **Eksperimen 1** (tombol hapus): ditambahkan tombol *Hapus* pada tiap item dengan `item.remove()`.

**Input kosong → pesan peringatan:**

![Latihan 6 kosong](screenshots/06a-latihan6-input-kosong.png)

**Menambah tiga tugas:**

![Latihan 6 tambah](screenshots/06b-latihan6-tambah-tugas.png)

**Menandai tugas pertama selesai (dicoret):**

![Latihan 6 selesai](screenshots/06c-latihan6-tandai-selesai.png)

**Menghapus satu item (Eksperimen 1):**

![Latihan 6 hapus](screenshots/06d-latihan6-hapus-item.png)

---

## 5. Mini Project — Form Pendaftaran Workshop

Folder: `mini-project/` (dipisah menjadi `index.html`, `style.css`, `script.js`, dan responsif untuk desktop maupun mobile).

**Fitur interaktif (lebih dari tiga):**

| No | Fitur | Event yang dipakai |
|----|-------|--------------------|
| 1 | Penghitung karakter nama + ringkasan input langsung | `input` |
| 2 | Menampilkan sesi yang dipilih saat dropdown berubah (Eksperimen 3) | `change` |
| 3 | Validasi nama (min. 3 karakter, Eksperimen 2), email, dan sesi + pesan error/sukses | `submit` |
| 4 | Tombol Reset untuk menghapus seluruh input dan pesan | `click` |

Potongan kode utama:

```js
// Penghitung karakter + ringkasan langsung
inputNama.addEventListener("input", function () {
  hitungNama.textContent = inputNama.value.length;
  perbaruiRingkasan();
});

// Menampilkan sesi yang dipilih
pilihSesi.addEventListener("change", function () {
  infoSesi.textContent = pilihSesi.value ? "Sesi dipilih: " + pilihSesi.value : "";
  perbaruiRingkasan();
});

// Validasi nama minimal 3 karakter
if (nama === "") {
  setError(inputNama, errNama, "Nama wajib diisi.");
} else if (nama.length < 3) {
  setError(inputNama, errNama, "Nama minimal terdiri dari 3 karakter.");
}

// Tombol reset
tombolReset.addEventListener("click", function () {
  form.reset();
  // ...kosongkan pesan, hitungan, dan ringkasan
});
```

**Penjelasan:** `setError()` menambahkan class `invalid` (border merah) pada input yang salah dan menulis pesan di bawahnya. Panel *Ringkasan Input* diperbarui secara langsung oleh fungsi `perbaruiRingkasan()` setiap input berubah. Layout memakai CSS Grid dan `@media (max-width: 720px)` agar menjadi satu kolom di perangkat mobile.

**Tampilan awal:**

![Mini project awal](screenshots/08a-miniproject-awal.png)

**Submit dengan semua kolom kosong (tidak valid):**

![Mini project kosong](screenshots/08b-miniproject-semua-kosong.png)

**Nama kurang dari 3 karakter dan email salah (tidak valid):**

![Mini project nama pendek](screenshots/08c-miniproject-nama-kurang-3-email-salah.png)

**Event `change` dropdown + ringkasan langsung:**

![Mini project change](screenshots/08d-miniproject-change-dropdown-ringkasan.png)

**Pendaftaran berhasil (valid):**

![Mini project sukses](screenshots/08e-miniproject-sukses.png)

**Setelah tombol Reset:**

![Mini project reset](screenshots/08f-miniproject-reset.png)

**Tampilan mobile (lebar 390px):**

![Mini project mobile](screenshots/08g-miniproject-mobile.png)

---

## 6. Eksperimen Mandiri

| No | Eksperimen | Status |
|----|-----------|--------|
| 1 | Tombol hapus satu item pada To-do List | Selesai — lihat Latihan 6 |
| 2 | Validasi nama minimal 3 karakter | Selesai — lihat Mini Project |
| 3 | Event `change` pada dropdown sesi workshop | Selesai — lihat Mini Project |
| 4 | Tombol sembunyikan/tampilkan paragraf dengan class CSS | Selesai — lihat di bawah |
| 5 | Memindahkan script tanpa `defer` ke `<head>` | Dijelaskan di bawah |

### Eksperimen 4 — Sembunyikan / Tampilkan Paragraf

```css
.sembunyi { display: none; }
```

```js
tombolToggle.addEventListener("click", function () {
  paragrafToggle.classList.toggle("sembunyi");
  tombolToggle.textContent = paragrafToggle.classList.contains("sembunyi")
    ? "Tampilkan Paragraf" : "Sembunyikan Paragraf";
});
```

**Penjelasan:** class `.sembunyi` memakai `display: none` sehingga elemen hilang dari tampilan. `classList.toggle()` menambah/menghapus class itu setiap tombol diklik.

Paragraf tampil:

![Eksperimen 4 tampil](screenshots/07a-eksperimen4-tampil.png)

Paragraf tersembunyi:

![Eksperimen 4 tersembunyi](screenshots/07b-eksperimen4-tersembunyi.png)

### Eksperimen 5 — Script di `<head>` tanpa `defer`

Jika `<script src="script.js"></script>` diletakkan di `<head>` **tanpa** `defer`, browser berhenti memparsing HTML untuk mengunduh dan menjalankan script **sebelum** elemen di `<body>` dibuat. Akibatnya `document.getElementById("judul")` mengembalikan `null`, sehingga baris seperti `judul.textContent = ...` menghasilkan error di Console:

```
Uncaught TypeError: Cannot set properties of null (setting 'textContent')
```

Seluruh script berhenti dan fitur interaktif tidak berfungsi. **Solusinya:** gunakan atribut `defer` (script dijalankan setelah HTML selesai diparsing), letakkan `<script>` di akhir `<body>`, atau bungkus kode dengan event `DOMContentLoaded`.

---

## 7. Jawaban Pertanyaan Evaluasi

**1. Apa yang dimaksud dengan DOM dan apa manfaatnya?**
DOM (*Document Object Model*) adalah representasi dokumen HTML dalam bentuk struktur pohon objek yang dapat diakses dan diubah oleh JavaScript. Manfaatnya: memilih elemen, mengubah teks/atribut/gaya, menambah atau menghapus elemen, dan merespons aksi pengguna tanpa memuat ulang halaman.

**2. Perbedaan `getElementById()`, `querySelector()`, dan `querySelectorAll()`?**
- `getElementById()` memilih satu elemen berdasarkan `id`.
- `querySelector()` memilih **elemen pertama** yang cocok dengan selector CSS (mis. `.pesan`, `#tombol`).
- `querySelectorAll()` memilih **semua** elemen yang cocok dengan selector CSS dan mengembalikan `NodeList`.

**3. Fungsi `addEventListener()` dan tiga contoh event?**
`addEventListener()` menghubungkan suatu event dengan fungsi yang dijalankan saat event terjadi. Contoh event: `click`, `input`, `submit` (juga `change`, `keydown`, `mouseover`).

**4. Mengapa `event.preventDefault()` digunakan pada event submit form?**
Perilaku bawaan submit adalah mengirim form dan memuat ulang halaman. `preventDefault()` mencegahnya sehingga JavaScript dapat memvalidasi data dan menampilkan pesan tanpa halaman hilang/ter-refresh.

**5. Perbedaan `textContent` dan `innerHTML` dari sisi penggunaan dan keamanan?**
`textContent` menampilkan nilai sebagai teks biasa sehingga aman. `innerHTML` menafsirkan nilai sebagai markup HTML sehingga dapat membuat elemen, tetapi berisiko **XSS (Cross-Site Scripting)** jika memasukkan teks pengguna yang tidak dipercaya.

**6. Bagaimana `classList.toggle()` membantu mengubah tampilan elemen?**
`toggle()` menambahkan class bila belum ada dan menghapusnya bila sudah ada. Dengan satu tombol, tampilan elemen (mis. highlight, ukuran teks, sembunyi/tampil) dapat berganti bolak-balik tanpa kode `if` tambahan.

**7. Minimal tiga skenario pengujian validasi form?**
1. Semua kolom kosong → muncul pesan "wajib diisi".
2. Email tidak valid (mis. `daniel@mail`) → muncul "Masukkan email yang valid."
3. Usia di bawah 17 / di atas 100 / bukan bilangan bulat → muncul pesan rentang usia.
4. Semua input valid → muncul pesan sukses dan form dikosongkan.

---

## 8. Kesimpulan

Pada praktikum ini DOM digunakan untuk memilih dan memanipulasi elemen (`getElementById`, `textContent`, `classList`, `createElement`), event ditangani dengan `addEventListener()` (`click`, `input`, `change`, `submit`), dan validasi form dibuat dengan `preventDefault()`, `trim()`, serta *regular expression*. Seluruh latihan dan mini project berjalan sesuai harapan tanpa error di Console.

---
*Daniel Dasnial — NIM 312510492*
