Halo teman-teman, gw Rozak.

Kita masuk ke bagian yang paling seru dan paling praktis, yaitu **basic-basic OpenClaw**.

Di video ini, kita bakal bahas instalasi OpenClaw dari nol sampe berhasil running di browser lo.

Nggak ada integrasi apa-apa dulu ya.

Fokus kita cuma satu: bikin OpenClaw-nya nyala dan lo bisa liat antarmukanya di browser.

Dan yang penting, gw udah asumsikan lo udah punya Node.js terinstall di komputer lo.

Sekarang, langsung aja kita mulai.

---

## Persiapan: Cek Node.js

Pertama-tama, kita pastiin dulu Node.js lo sudah sesuai.

OpenClaw itu butuh **Node versi 24** yang direkomendasikan, atau minimal **Node 22.14 ke atas**.

Cara ngeceknya gampang.

Lo buka terminal atau command prompt, terus ketik perintah ini:

```bash
node -v
```

Kalau yang muncul angkanya 22.14 atau lebih tinggi, berarti aman.

Tapi kalau masih di bawah itu, lo harus upgrade dulu Node-nya.

Tenang, proses upgrade-nya nggak ribet kok, lo tinggal download versi terbaru dari website Node.

Gw saranin pake Node 24 biar paling stabil.

---

## Langkah 1: Install OpenClaw via npm

Nah, sekarang kita masuk ke instalasi.

Karena lo udah punya Node.js, cara paling simpel buat install OpenClaw adalah pake npm.

Jalankan perintah ini di terminal lo:

```bash
npm install -g openclaw@latest
```

Perintah ini akan menginstal OpenClaw secara global di sistem lo.

Tanda `-g` itu artinya global, jadi lo bisa pake perintah `openclaw` dari folder mana aja.

Proses instalasi ini mungkin agak lama karena banyak dependency yang didownload.

Santai aja, biarkan dia jalan sampai selesai.

---

## Langkah 2: Jalankan Onboarding Wizard

Setelah instalasi selesai, langkah berikutnya adalah menjalankan **onboarding wizard**.

Ini adalah proses setup interaktif pertama yang akan memandu lo step by step mengatur gateway, workspace, dan konfigurasi dasar OpenClaw.

Jalankan perintah ini:

```bash
openclaw onboard --install-daemon
```

Kenapa pake `--install-daemon`?

Karena ini akan menginstal OpenClaw sebagai layanan yang berjalan di background.

Jadi nanti OpenClaw bakal otomatis jalan setiap kali lo nyalain komputer.

Nggak perlu manual start setiap saat.

Wizard ini bakal nanya beberapa pertanyaan ke lo, kayak mau dimana workspace dibuat, dan konfigurasi dasar lainnya.

Lo tinggal ikuti aja petunjuknya.

Proses ini cuma makan waktu sekitar 2 menit kok.

---

## Langkah 3: Verifikasi Instalasi

Setelah onboarding selesai, lo perlu verifikasi bahwa OpenClaw udah terinstal dengan bener.

Coba jalankan perintah ini buat cek versi:

```bash
openclaw --version
```

Kalau muncul angka versi OpenClaw, berarti instalasi sukses.

Kalau error `command not found`, itu artinya path global npm belum terdaftar di environment variable PATH lo.

Cara fix-nya:

Pertama, cari tahu dimana global bin directory npm:

```bash
npm prefix -g
```

Terus tambahin folder bin-nya ke PATH di file `~/.zshrc` atau `~/.bashrc` lo.

Trus restart terminal.

Abis itu coba lagi perintah `openclaw --version`.

Udah jalan kan?

Bagus.

---

## Langkah 4: Cek Status Gateway

Sekarang kita cek apakah gateway OpenClaw udah berjalan.

Gateway ini adalah server lokal yang jadi otak dari seluruh sistem OpenClaw.

Jalankan perintah ini:

```bash
openclaw gateway status
```

Perintah ini buat ngecek apakah gateway-nya udah jalan atau belum.

Kalau statusnya `running`, berarti gateway lo udah aktif.

Kalau belum, lo bisa jalanin secara manual dengan perintah:

```bash
openclaw gateway --verbose
```

Atau kalau lo mau daemon-nya (background service) jalan otomatis, lo udah pake `--install-daemon` tadi, jadi seharusnya udah jalan.

---

## Langkah 5: Buka Dashboard di Browser

Nah, ini bagian yang paling ditunggu-tunggu.

Buka dashboard OpenClaw di browser lo.

Caranya gampang banget.

Jalankan perintah ini:

```bash
openclaw dashboard
```

Perintah ini akan otomatis membuka browser lo dan menampilkan Control UI OpenClaw.

Defaultnya, gateway OpenClaw berjalan di **port 18789**.

Lo juga bisa akses manual lewat URL ini:

```
http://127.0.0.1:18789/
```

Kalau halaman dashboard muncul, berarti semuanya berjalan dengan sempurna.

---

## Struktur File OpenClaw

Nah, sebelum kita tutup video ini, gw mau jelasin dikit tentang struktur file OpenClaw.

Ini penting biar lo paham dimana file-file konfigurasi dan memori disimpan.

### Lokasi Utama: `~/.openclaw/`

Semua file OpenClaw disimpan di folder **`~/.openclaw/`** di home directory lo.

Folder ini berisi konfigurasi, kredensial, dan data sesi.

### Struktur Workspace (`~/.openclaw/workspace/`)

Di dalam folder workspace inilah agent lo "tinggal".

Ini adalah file-file standar yang ada di workspace OpenClaw:

- **`SOUL.md`** → Ini adalah file yang ngatur kepribadian agent. Tone, gaya bicara, batasan-batasan, semua ditulis di sini.
- **`MEMORY.md`** → Ini adalah memori jangka panjang agent. Fakta, preferensi, keputusan, semuanya disimpan di sini.
- **`AGENTS.md`** → Instruksi operasional buat agent. Aturan dan prioritas.
- **`USER.md`** → Informasi tentang user: siapa user-nya dan gimana cara menyapanya.
- **`IDENTITY.md`** → Nama agent dan vibe-nya.
- **`TOOLS.md`** → Catatan tentang tools yang tersedia.
- **`BOOT.md`** → Startup checklist yang dijalankan otomatis saat gateway restart.
- **`memory/`** → Folder berisi daily memory log. Satu file per hari.

### Konsep Penting: Workspace vs Config

Gw mau tekankan satu hal.

**Workspace (`~/.openclaw/workspace/`)** itu adalah "memori" agent.

Semua file di sini adalah data yang dibaca dan ditulis oleh agent.

Ini yang harus lo backup kalau mau pindahin agent ke mesin lain.

Sedangkan **Config (`~/.openclaw/`)** adalah folder konfigurasi dan kredensial.

Jangan di-commit ke repository karena berisi data sensitif kayak API keys dan token.

---

## Ringkasan Command

Gw rangkum semua perintah yang udah kita bahas tadi biar lo gak lupa:

| Command | Fungsi |
|---------|--------|
| `npm install -g openclaw@latest` | Install OpenClaw secara global |
| `openclaw onboard --install-daemon` | Jalankan onboarding wizard + install daemon |
| `openclaw --version` | Cek versi OpenClaw |
| `openclaw gateway status` | Cek status gateway |
| `openclaw gateway --verbose` | Jalankan gateway manual dengan log detail |
| `openclaw dashboard` | Buka dashboard di browser |

---

Nah bro, itu dia basic-basic OpenClaw.

Mulai dari instalasi pake npm, onboarding, sampe dashboard muncul di browser lo.

Sekarang lo udah punya OpenClaw yang running dan siap dipake.

**Tapi inget**, ini baru instalasi dasar aja.

Belum ada integrasi ke Telegram, belum ada koneksi ke Google Calendar, belum ada apa-apa.

Itu semua bakal kita bahas di video-video selanjutnya.

Yang penting sekarang, OpenClaw-nya udah nyala dan lo bisa liat sendiri dashboard-nya di browser.

Gw tunggu di video berikutnya, bro.
