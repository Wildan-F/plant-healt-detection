<div align="center">

# Tomato leaf health detector

**A system that detects the health of tomato plants via their leaves.**

<p>
  <img alt="YOLO26" src="https://img.shields.io/badge/YOLO26-Computer%20Vision-111827?style=flat-square">
  <img alt="ONNX" src="https://img.shields.io/badge/Model-ONNX-00599C?style=flat-square">
  <img alt="JavaScript" src="https://img.shields.io/badge/JavaScript-ES%20Modules-F7DF1E?style=flat-square&logo=javascript&logoColor=111827">
  <img alt="WebGPU" src="https://img.shields.io/badge/Acceleration-WebGPU-7C3AED?style=flat-square">
  <img alt="WebAssembly" src="https://img.shields.io/badge/Fallback-WebAssembly-654FF0?style=flat-square&logo=webassembly&logoColor=white">
  <img alt="GitHub Pages" src="https://img.shields.io/badge/Hosting-GitHub%20Pages-222222?style=flat-square&logo=githubpages&logoColor=white">
  <img alt="License AGPL-3.0" src="https://img.shields.io/badge/License-AGPL--3.0-blue?style=flat-square">
</p>

<p><em>Static site · Client-side inference · Responsive UI · No application backend</em></p>

</div>
<p align="center">
  <a href="#english"><img alt="Read in English" src="https://img.shields.io/badge/READ-ENGLISH-2563EB?style=for-the-badge&logo=readthedocs&logoColor=white"></a>
  <a href="#bahasa-indonesia"><img alt="Baca dalam Bahasa Indonesia" src="https://img.shields.io/badge/BACA-BAHASA%20INDONESIA-DC2626?style=for-the-badge&logo=readthedocs&logoColor=white"></a>
</p>

---

<a id="english"></a>

## 🇬🇧 English

---

## What is this application for?

It helps in assessing the health of tomato plants by analyzing their leaves and provides recommendations. You can do this either by taking a photo directly or by uploading an image.

## Technology stack

| Technology | What it does in this project |
|---|---|
| [Ultralytics YOLO26](https://docs.ultralytics.com/models/yolo26/) | Computer-vision model family used for inference |
| [`@ultralytics/yolo`](https://github.com/ultralytics/inference/tree/main/web) | Browser API for loading models, running predictions, and drawing annotations |
| [ONNX](https://onnx.ai/) | Model format loaded by the browser inference runtime |
| WebGPU | Hardware-accelerated inference when supported by the browser and device |
| WebAssembly (WASM) / CPU | Compatibility fallback when WebGPU is unavailable |
| HTML, CSS, JavaScript | Application structure, styling, and interaction |
| GitHub Pages + GitHub Actions | Static hosting and automatic deployment |

## Features

| Feature | Description |
|---|---|
| Camera selection | Choose the front or back camera on supported devices |
| Image testing | Select an image from your device and run one prediction |
| Confidence threshold | Adjust the minimum confidence used for predictions |
| Detection panel | Review detected classes and their confidence values |
| Performance indicators | Show inference time and approximate live FPS |
| Responsive interface | Adapt the workspace to desktop, tablet, and mobile screens |
| Static deployment | Publish the project with GitHub Pages and the included Actions workflow |

The current interface is primarily arranged around **object detection**. The browser library supports additional Ultralytics tasks, but task-specific result panels or visualizations may need extra UI code.

---

## Getting started

### 1. Create your own repository

There are two common ways to start. Choose the one that matches how you plan to work:

| Method | Use it when… | Steps |
|---|---|---|
| **Use this template** (recommended for assignments) | You want an independent project based on this starter | Open the repository on GitHub → select **Use this template** → **Create a new repository** → choose your repository name and owner |
| **Fork** | You want a copy linked to the original repository, or you may contribute improvements back | Open the repository on GitHub → select **Fork** → choose your account and create the fork |

After creating the repository, clone **your own repository** to your computer:

```bash
git clone https://github.com/<your-username>/<your-repository>.git
cd <your-repository>
```

Replace the placeholders with your GitHub username and repository name. You can also edit files directly in the GitHub web interface, but a local clone is more convenient for development.

### 2. Add the YOLO26 ONNX model

The application expects the default model at this path:

```text
models/yolo26n.onnx
```

Create the `models/` directory if it does not exist, then place the model inside it.

**Community-hosted download:** [YOLO26n ONNX — download model](https://huggingface.co/prithivMLmods/YOLO26-ONNX/resolve/main/yolo26n/yolo26n.onnx)

The link points to a community-hosted copy, not a download hosted on the official Ultralytics domain. Check its source and checksum before using or redistributing it. The SHA-256 value published for this mirror is:

```text
00e2d1062178fca312fee35cf5fc9b9c783b3e5c9675d48b84929f69ee391807
```

Verify the downloaded file if you use this mirror:

```bash
sha256sum models/yolo26n.onnx
```

On Windows PowerShell or Command Prompt:

```powershell
certutil -hashfile models/yolo26n.onnx SHA256
```

The expected filename is **`yolo26n.onnx`**. The `n` means *nano*, the lightweight model size. Ultralytics also provides `s`, `m`, `l`, and `x` model sizes. A different size or task variant may need more memory and can have different performance characteristics.

<details>
<summary><strong>Prefer to export the model yourself?</strong></summary>

This is the reproducible option: download/use the official YOLO26n checkpoint and export it with Ultralytics.

Install or upgrade the Python package:

```bash
python -m pip install --upgrade ultralytics
```

Export the checkpoint:

```python
from ultralytics import YOLO

model = YOLO("yolo26n.pt")
model.export(format="onnx")
```

Move the generated `yolo26n.onnx` file into the repository's `models/` directory. For the current browser inference package, use a compatible Ultralytics export; its documentation specifies **Ultralytics 8.4.142 or newer** for metadata expected by the browser runtime.

Official references:

- [YOLO26 model documentation](https://docs.ultralytics.com/models/yolo26/)
- [ONNX export documentation](https://docs.ultralytics.com/integrations/onnx)
- [Ultralytics browser inference documentation](https://github.com/ultralytics/inference/tree/main/web)

</details>

### 3. Run the project locally

Camera access requires a secure context: use **`localhost`** or **HTTPS**. Opening `index.html` directly with a `file://` URL may prevent the browser from granting camera access or loading modules correctly.

With Python installed, run this from the project directory:

```bash
python -m http.server 8000
```

Open the local address in your browser:

<http://localhost:8000>

You can also use a local development server extension in your code editor, such as Live Server in Visual Studio Code.

### 4. Publish with GitHub Pages

The repository already includes this workflow:

```text
.github/workflows/pages.yml
```

Follow these steps in **your own repository**:

1. Make sure `models/yolo26n.onnx` is present and the project files are committed.
2. Push your changes to the `main` branch.
3. Open **Settings → Pages**.
4. Under **Build and deployment**, set **Source** to **GitHub Actions**.
5. Open the **Actions** tab and check the **Deploy static site to GitHub Pages** workflow.
6. When the deployment succeeds, open the Pages URL shown in the workflow summary or in **Settings → Pages**.

Your site URL will usually look like this:

```text
https://<your-username>.github.io/<your-repository>/
```

This is a static website. You do not need to configure a Python, Node.js, or other application server for deployment. The GitHub Actions workflow uploads the repository files as a Pages artifact and deploys them.

---

## Project structure

```text
.
├── .github/
│   └── workflows/
│       └── pages.yml       # GitHub Pages deployment workflow
├── models/
│   ├── .gitkeep            # Keeps this folder in Git
│   └── yolo26n.onnx        # Add the model here
├── app.js                  # Model loading, inference, and UI logic
├── index.html              # Application markup and import map
├── style.css               # Responsive layout and visual styles
├── LICENSE                 # GNU Affero General Public License v3.0
└── README.md               # Bilingual documentation (English + Indonesian)
```

## Customize the starter

The main settings are near the top of `app.js`:

```js
const CONFIG = Object.freeze({
  defaultModelPath: "models/yolo26n.onnx",
  defaultConfidence: 0.25,
  annotation: {
    lineWidth: 3,
    font: "600 14px Manrope, sans-serif",
    labels: true,
    keypoints: true,
  },
});
```

| You want to change… | Start here |
|---|---|
| Default model filename or folder | `CONFIG.defaultModelPath` in `app.js` |
| Initial confidence threshold | `CONFIG.defaultConfidence` in `app.js` |
| Box and label appearance | `CONFIG.annotation` in `app.js` |
| Colors, spacing, and responsive layout | `style.css` |
| Page title, headings, and interface text | `index.html` |
| Detection list and displayed results | `updateDetections()` in `app.js` |

You can also load a different ONNX file from the interface. That file is used for the current browser session; it does not replace the repository's default model.

## Notes on browser support, privacy, and performance

- Use a modern browser with JavaScript modules enabled.
- Camera access requires permission from the user and a secure context (`https://` or `http://localhost`).
- WebGPU availability depends on browser support, device hardware, drivers, and operating-system configuration. The inference package uses automatic device selection and can fall back to CPU/WASM.
- Performance varies with the device, browser, model size, and input resolution. Start with YOLO26n when testing on student laptops or mobile devices.
- The app does not send camera frames or selected images to an application backend. Inference runs in the browser. The browser still downloads the model and third-party runtime assets from the configured repository/CDN sources.
- The `@ultralytics/yolo` package is imported from `esm.sh`. The ONNX Runtime Web assets are also fetched on first use and may be cached by the browser. An internet connection is therefore needed unless you configure and self-host all required assets.
- Host the ONNX file in this repository or another CORS-enabled location. GitHub release asset URLs do not provide the CORS headers needed for browser-side model loading.
- Keep the model path relative (for example, `models/yolo26n.onnx`). This works for GitHub Pages project sites that live below `/<repository>/`.

## Troubleshooting

| Problem | Things to check |
|---|---|
| Model cannot be loaded / `404` | Confirm the file is named `yolo26n.onnx`, is inside `models/`, and has been pushed to GitHub. Check capitalization: paths are case-sensitive on GitHub Pages. |
| The model loads locally but not on Pages | Check that `defaultModelPath` is relative and that the model file is included in the deployed repository. |
| Browser says camera access is unavailable | Open the site over HTTPS or `localhost`, allow camera permission, and check that another app is not using the camera. |
| WebGPU is unavailable | This can be normal on some browsers/devices. Automatic mode should use the CPU/WASM fallback; performance may be lower. |
| The page is blank or the module fails | Check the browser developer console and internet connection. The page loads the inference package from `esm.sh`. |
| Predictions are slow | Try YOLO26n, close other intensive browser tabs, and test a smaller image or a device with WebGPU support. |
| Custom ONNX file fails | Confirm it is a compatible Ultralytics ONNX export and includes the metadata expected by the browser inference package. |

## Suggested student exercises

1. Change the default confidence threshold and compare how the number of detections changes.
2. Test the same image with two different YOLO26 model sizes and compare latency and detection quality.
3. Train a custom model, export it to ONNX, and load it through the model picker.
4. Customize the interface and add a project name, description, and attribution.
5. Extend `updateDetections()` to display a task-specific output such as segmentation masks, pose keypoints, or oriented bounding boxes.

## License and third-party components

This repository is licensed under the **GNU Affero General Public License v3.0 (AGPL-3.0)**. See [`LICENSE`](./LICENSE) for the complete license text.

The project uses the Ultralytics YOLO ecosystem. Ultralytics states that its YOLO code and trained models are offered under AGPL-3.0 by default, with an Enterprise license for use cases that need different terms. When using Ultralytics code or models, review the upstream licensing requirements and make sure the complete project is licensed and distributed accordingly. Do not assume that the community-hosted ONNX mirror changes the model's original license.

Third-party components and services retain their own licenses and terms:

- [Ultralytics licensing](https://www.ultralytics.com/license)
- [Ultralytics browser inference package](https://github.com/ultralytics/inference/tree/main/web)
- [ONNX](https://onnx.ai/)
- [Google Fonts](https://fonts.google.com/) — Manrope and DM Mono

This documentation is provided for educational purposes and is not legal advice. If you plan to use the project in a commercial or closed-source product, review the relevant licenses and obtain professional advice where appropriate.
---

<div align="center">

**Made for learning, experimenting, and building with computer vision.**

[🇮🇩 Baca dalam Bahasa Indonesia](#bahasa-indonesia) · [Back to top](#yolo26-studio)

</div>

---

<a id="bahasa-indonesia"></a>

## 🇮🇩 Bahasa Indonesia

[🇬🇧 Read in English](#english) · [Kembali ke atas](#yolo26-studio)

---

## Apa itu YOLO26 Studio?

YOLO26 Studio adalah proyek awal (*starter project*) untuk mempelajari dan membangun aplikasi computer vision berbasis browser menggunakan **Ultralytics YOLO26**. Template ini menyediakan antarmuka yang dapat dikustomisasi, alur pemuatan model ONNX, inferensi webcam, dan pengujian gambar.

Inferensi dijalankan langsung di browser pengguna. Proyek ini tidak memerlukan server Python, endpoint API, maupun backend inferensi khusus. Browser mengunduh model dan aset runtime, kemudian memproses frame kamera atau gambar yang dipilih secara lokal.

> **Untuk siapa template ini?** Siswa, pengajar, dan developer yang membutuhkan fondasi praktis untuk bereksperimen dengan object detection dan mengembangkannya menjadi proyek computer vision sendiri.

## Teknologi yang digunakan

| Teknologi | Peran dalam proyek |
|---|---|
| [Ultralytics YOLO26](https://docs.ultralytics.com/models/yolo26/) | Keluarga model computer vision untuk inferensi |
| [`@ultralytics/yolo`](https://github.com/ultralytics/inference/tree/main/web) | API browser untuk memuat model, menjalankan prediksi, dan menggambar anotasi |
| [ONNX](https://onnx.ai/) | Format model yang dimuat oleh runtime inferensi browser |
| WebGPU | Akselerasi perangkat keras jika didukung browser dan perangkat |
| WebAssembly (WASM) / CPU | Jalur fallback jika WebGPU tidak tersedia |
| HTML, CSS, JavaScript | Struktur aplikasi, styling, dan interaksi |
| GitHub Pages + GitHub Actions | Hosting statis dan deployment otomatis |

## Fitur

| Fitur | Deskripsi |
|---|---|
| Inferensi webcam | Menjalankan prediksi dari aliran kamera secara langsung |
| Pemilihan kamera | Memilih kamera depan atau belakang pada perangkat yang mendukung |
| Pengujian gambar | Memilih gambar dari perangkat dan menjalankan satu prediksi |
| Pemilih model ONNX | Memuat file `.onnx` lain untuk sesi browser saat ini |
| Confidence threshold | Mengatur nilai minimum confidence untuk prediksi |
| Anotasi visual | Menggambar bounding box, nama kelas, dan nilai confidence |
| Panel deteksi | Melihat kelas yang terdeteksi beserta nilai confidence |
| Indikator performa | Menampilkan waktu inferensi dan perkiraan FPS secara langsung |
| Antarmuka responsif | Menyesuaikan tampilan untuk desktop, tablet, dan ponsel |
| Deployment statis | Mempublikasikan proyek menggunakan GitHub Pages dan workflow Actions |

Antarmuka saat ini terutama dirancang untuk **object detection**. Library browser mendukung tugas Ultralytics lainnya, tetapi panel hasil atau visualisasi khusus perlu ditambahkan pada UI.

---

## Memulai proyek

### 1. Buat repository milik Anda

Ada dua cara umum untuk memulai. Pilih sesuai kebutuhan pengerjaan:

| Metode | Gunakan ketika… | Langkah |
|---|---|---|
| **Use this template** (disarankan untuk tugas) | Anda ingin proyek mandiri yang menggunakan template ini sebagai dasar | Buka repository di GitHub → pilih **Use this template** → **Create a new repository** → tentukan nama repository dan pemiliknya |
| **Fork** | Anda ingin salinan yang terhubung ke repository asli, atau mungkin mengirimkan kontribusi kembali | Buka repository di GitHub → pilih **Fork** → pilih akun Anda dan buat fork |

Setelah repository dibuat, clone **repository milik Anda sendiri** ke komputer:

```bash
git clone https://github.com/<username-anda>/<nama-repository>.git
cd <nama-repository>
```

Ganti placeholder dengan username GitHub dan nama repository Anda. Anda juga dapat mengedit file langsung melalui antarmuka web GitHub, tetapi clone lokal biasanya lebih nyaman untuk pengembangan.

### 2. Tambahkan model YOLO26 ONNX

Aplikasi mengharapkan model default berada pada lokasi berikut:

```text
models/yolo26n.onnx
```

Buat folder `models/` jika belum tersedia, lalu letakkan file model di dalamnya.

**Unduh dari mirror komunitas:** [YOLO26n ONNX — unduh model](https://huggingface.co/prithivMLmods/YOLO26-ONNX/resolve/main/yolo26n/yolo26n.onnx)

Tautan tersebut mengarah ke salinan yang di-host oleh komunitas, bukan file yang di-host pada domain resmi Ultralytics. Periksa sumber dan checksum sebelum menggunakan atau mendistribusikan ulang file. Nilai SHA-256 yang dipublikasikan untuk mirror ini adalah:

```text
00e2d1062178fca312fee35cf5fc9b9c783b3e5c9675d48b84929f69ee391807
```

Verifikasi file yang diunduh jika Anda menggunakan mirror tersebut:

```bash
sha256sum models/yolo26n.onnx
```

Di Windows PowerShell atau Command Prompt:

```powershell
certutil -hashfile models/yolo26n.onnx SHA256
```

Nama file yang diharapkan adalah **`yolo26n.onnx`**. Huruf `n` berarti *nano*, yaitu ukuran model yang ringan. Ultralytics juga menyediakan ukuran `s`, `m`, `l`, dan `x`. Ukuran atau varian task lain mungkin membutuhkan memori lebih besar dan memiliki karakteristik performa yang berbeda.

<details>
<summary><strong>Ingin mengekspor model sendiri?</strong></summary>

Ini adalah cara yang lebih mudah direproduksi: gunakan checkpoint YOLO26n resmi dan ekspor dengan Ultralytics.

Instal atau perbarui package Python:

```bash
python -m pip install --upgrade ultralytics
```

Ekspor checkpoint:

```python
from ultralytics import YOLO

model = YOLO("yolo26n.pt")
model.export(format="onnx")
```

Pindahkan file `yolo26n.onnx` yang dihasilkan ke folder `models/` pada repository. Untuk package inferensi browser saat ini, gunakan hasil ekspor Ultralytics yang kompatibel; dokumentasinya menyebutkan **Ultralytics 8.4.142 atau lebih baru** untuk metadata yang dibutuhkan runtime browser.

Referensi resmi:

- [Dokumentasi model YOLO26](https://docs.ultralytics.com/models/yolo26/)
- [Dokumentasi ekspor ONNX](https://docs.ultralytics.com/integrations/onnx)
- [Dokumentasi inferensi browser Ultralytics](https://github.com/ultralytics/inference/tree/main/web)

</details>

### 3. Jalankan proyek di komputer lokal

Akses kamera membutuhkan konteks aman: gunakan **`localhost`** atau **HTTPS**. Membuka `index.html` secara langsung melalui URL `file://` dapat membuat browser menolak akses kamera atau gagal memuat module dengan benar.

Jika Python sudah terpasang, jalankan perintah berikut dari folder proyek:

```bash
python -m http.server 8000
```

Buka alamat lokal ini di browser:

<http://localhost:8000>

Anda juga dapat menggunakan ekstensi local development server pada editor kode, misalnya Live Server di Visual Studio Code.

### 4. Publikasikan menggunakan GitHub Pages

Repository ini sudah memiliki workflow berikut:

```text
.github/workflows/pages.yml
```

Ikuti langkah berikut pada **repository milik Anda**:

1. Pastikan `models/yolo26n.onnx` tersedia dan semua file proyek sudah di-commit.
2. Push perubahan ke branch `main`.
3. Buka **Settings → Pages**.
4. Pada bagian **Build and deployment**, atur **Source** menjadi **GitHub Actions**.
5. Buka tab **Actions** dan periksa workflow **Deploy static site to GitHub Pages**.
6. Setelah deployment berhasil, buka URL Pages yang ditampilkan pada ringkasan workflow atau **Settings → Pages**.

URL situs biasanya berbentuk seperti ini:

```text
https://<username-anda>.github.io/<nama-repository>/
```

Ini adalah website statis. Anda tidak perlu mengatur server aplikasi Python, Node.js, atau server lainnya untuk deployment. Workflow GitHub Actions akan mengunggah file repository sebagai artifact Pages lalu melakukan deployment.

---

## Struktur proyek

```text
.
├── .github/
│   └── workflows/
│       └── pages.yml       # Workflow deployment GitHub Pages
├── models/
│   ├── .gitkeep            # Menjaga folder ini tetap ada di Git
│   └── yolo26n.onnx        # Letakkan model di sini
├── app.js                  # Pemuatan model, inferensi, dan logika UI
├── index.html              # Markup aplikasi dan import map
├── style.css               # Layout responsif dan gaya visual
├── LICENSE                 # GNU Affero General Public License v3.0
└── README.md               # Dokumentasi bilingual (English + Bahasa Indonesia)
```

## Kustomisasi template

Pengaturan utama berada di bagian atas `app.js`:

```js
const CONFIG = Object.freeze({
  defaultModelPath: "models/yolo26n.onnx",
  defaultConfidence: 0.25,
  annotation: {
    lineWidth: 3,
    font: "600 14px Manrope, sans-serif",
    labels: true,
    keypoints: true,
  },
});
```

| Yang ingin diubah | Mulai dari sini |
|---|---|
| Nama atau folder model default | `CONFIG.defaultModelPath` di `app.js` |
| Confidence threshold awal | `CONFIG.defaultConfidence` di `app.js` |
| Tampilan box dan label | `CONFIG.annotation` di `app.js` |
| Warna, jarak, dan layout responsif | `style.css` |
| Judul halaman, heading, dan teks UI | `index.html` |
| Daftar deteksi dan hasil yang ditampilkan | `updateDetections()` di `app.js` |

Anda juga dapat memuat file ONNX lain melalui antarmuka. File tersebut hanya digunakan untuk sesi browser saat ini dan tidak menggantikan model default di repository.

## Catatan kompatibilitas browser, privasi, dan performa

- Gunakan browser modern yang mendukung JavaScript modules.
- Akses kamera memerlukan izin pengguna dan konteks aman (`https://` atau `http://localhost`).
- Ketersediaan WebGPU bergantung pada browser, perangkat keras, driver, dan sistem operasi. Package inferensi menggunakan pemilihan perangkat otomatis dan dapat beralih ke CPU/WASM.
- Performa dipengaruhi perangkat, browser, ukuran model, dan resolusi input. Mulailah dengan YOLO26n saat menguji pada laptop siswa atau perangkat mobile.
- Aplikasi tidak mengirim frame kamera atau gambar yang dipilih ke backend aplikasi. Inferensi berjalan di browser. Namun, browser tetap mengunduh model dan aset runtime pihak ketiga dari sumber repository/CDN yang dikonfigurasi.
- Package `@ultralytics/yolo` diimpor dari `esm.sh`. Aset ONNX Runtime Web juga diunduh saat pertama kali digunakan dan dapat disimpan dalam cache browser. Koneksi internet diperlukan kecuali semua aset runtime dikonfigurasi dan di-host sendiri.
- Host file ONNX di repository ini atau lokasi lain yang mendukung CORS. URL GitHub release asset tidak menyediakan header CORS yang diperlukan untuk pemuatan model dari browser.
- Pertahankan path model dalam bentuk relatif (contoh: `models/yolo26n.onnx`). Ini berfungsi untuk GitHub Pages project site yang berada di bawah `/<repository>/`.

## Pemecahan masalah

| Masalah | Hal yang perlu diperiksa |
|---|---|
| Model gagal dimuat / `404` | Pastikan nama file `yolo26n.onnx`, berada di dalam folder `models/`, dan sudah di-push ke GitHub. Perhatikan huruf besar/kecil karena path GitHub Pages bersifat case-sensitive. |
| Model berhasil dimuat lokal tetapi gagal di Pages | Pastikan `defaultModelPath` bersifat relatif dan file model ikut masuk ke repository yang di-deploy. |
| Browser menyatakan kamera tidak tersedia | Buka situs menggunakan HTTPS atau `localhost`, izinkan akses kamera, dan periksa apakah kamera sedang dipakai aplikasi lain. |
| WebGPU tidak tersedia | Ini normal pada beberapa browser/perangkat. Mode otomatis seharusnya menggunakan fallback CPU/WASM; performanya mungkin lebih rendah. |
| Halaman kosong atau module gagal dimuat | Periksa browser developer console dan koneksi internet. Halaman memuat package inferensi dari `esm.sh`. |
| Prediksi lambat | Coba YOLO26n, tutup tab browser yang berat, dan uji gambar yang lebih kecil atau perangkat dengan dukungan WebGPU. |
| File ONNX kustom gagal dimuat | Pastikan file tersebut merupakan hasil ekspor Ultralytics ONNX yang kompatibel dan memuat metadata yang dibutuhkan package inferensi browser. |

## Ide latihan untuk siswa

1. Ubah confidence threshold default dan bandingkan perubahan jumlah deteksi.
2. Uji gambar yang sama dengan dua ukuran model YOLO26 berbeda, lalu bandingkan latency dan kualitas deteksi.
3. Latih model kustom, ekspor ke ONNX, kemudian muat melalui model picker.
4. Kustomisasi antarmuka dan tambahkan nama proyek, deskripsi, serta atribusi.
5. Kembangkan `updateDetections()` untuk menampilkan output khusus task seperti segmentation mask, pose keypoints, atau oriented bounding boxes.

## Lisensi dan komponen pihak ketiga

Repository ini menggunakan lisensi **GNU Affero General Public License versi 3.0 (AGPL-3.0)**. Lihat file [`LICENSE`](./LICENSE) untuk teks lisensi lengkap.

Proyek ini menggunakan ekosistem Ultralytics YOLO. Ultralytics menyatakan bahwa kode YOLO dan model terlatihnya menggunakan AGPL-3.0 secara default, dengan lisensi Enterprise untuk kebutuhan yang memerlukan ketentuan berbeda. Saat menggunakan kode atau model Ultralytics, tinjau persyaratan lisensi upstream dan pastikan seluruh proyek dilisensikan serta didistribusikan sesuai ketentuan tersebut. Mirror ONNX yang di-host komunitas tidak otomatis mengubah lisensi asli model.

Komponen dan layanan pihak ketiga tetap mengikuti lisensi dan ketentuannya masing-masing:

- [Lisensi Ultralytics](https://www.ultralytics.com/license)
- [Package inferensi browser Ultralytics](https://github.com/ultralytics/inference/tree/main/web)
- [ONNX](https://onnx.ai/)
- [Google Fonts](https://fonts.google.com/) — Manrope dan DM Mono

Dokumentasi ini disediakan untuk tujuan pendidikan dan bukan nasihat hukum. Jika Anda berencana menggunakan proyek ini dalam produk komersial atau closed-source, periksa lisensi yang relevan dan minta saran profesional bila diperlukan.

---

<div align="center">

**Dibuat untuk belajar, bereksperimen, dan membangun dengan computer vision.**

[🇬🇧 Baca dalam Bahasa Inggris](#english) · [Kembali ke atas](#yolo26-studio)

</div>
