# 📺 LED Panel & Videotron — Security Research Notes

> Dokumentasi celah keamanan pada running text panel dan videotron komersial.  
> Semua temuan bersifat **research** — dicatat sebagai referensi security assessment.

---

## 📋 Daftar Isi

- [Level 1 — Running Text Panel](#level-1--running-text-panel)
  - [Skema Serangan](#skema-serangan)
  - [SSID Penanda](#ssid-penanda)
  - [Tabel Kredensial Default](#tabel-kredensial-default)
  - [Aplikasi yang Digunakan](#aplikasi-yang-digunakan)
  - [Langkah-Langkah (Contoh Huidu LedArt)](#langkah-langkah-contoh-huidu-ledart)
  - [Catatan Penting](#catatan-penting)
- [Level 2 — Videotron & Layar Besar](#level-2--videotron--layar-besar)
  - [Jalur Masuk yang Realistis](#jalur-masuk-yang-realistis)
  - [Recon](#recon)
  - [Shodan Dorks](#shodan-dorks)
  - [Yang Bisa Dilakukan](#yang-bisa-dilakukan)

---

## Level 1 — Running Text Panel

> ⏱️ **Estimasi:** ~5 menit | 📱 **Perangkat:** Cukup HP

Ini jalur paling umum — cocok buat panel kecil di depan toko, kantor, atau sekolah yang masih pakai firmware bawaan pabrik tanpa ganti password.

---

### Skema Serangan

```
Panel nyala
  → Nyalain WiFi HP
  → Scan jaringan sekitar
  → Ketemu SSID pabrikan (format khas)
  → Konek pakai password default
  → Buka app pabrikan
  → Ganti teks / konten
  → Upload ke panel
```

---

### SSID Penanda

Format SSID yang jadi indikator panel LED dengan firmware default:

| Format SSID | Keterangan |
|---|---|
| `TF-WIFI_######` | PowerLED series |
| `HD-xxxx` | Huidu series |
| `WF-xxxx` | Generic WiFi controller |
| `LED-xxxx` | Generic |
| `XF-xxxx` | Xixun series |

---

### Tabel Kredensial Default

| Merek | Password WiFi Default | Password App — Common Setting | Password App — Hardware Setting |
|---|---|---|---|
| **Huidu / HD-WF** | `88888888` atau `lc12345678` / `lc888888` | `168` | `888` |
| **Xixun / X series** | `12345678` | `168` | `888` (LED Editor) |
| **PowerLED / TF-WIFI** | `88888888` | `168` | — |
| **PSD series** | IP: `192.168.0.XXX` | user: `admin` | test mode: `666888` |

---

### Aplikasi yang Digunakan

| Aplikasi | Platform | Merek Target |
|---|---|---|
| **LedArt** | Android / iOS | Huidu |
| **XLGPowerLedPro** | Android / iOS | PowerLED |
| **LedshowTW / LED Editor** | Android / iOS | Xixun |
| **HDPlayer** | Windows (PC) | Huidu |

---

### Langkah-Langkah (Contoh Huidu LedArt)

1. **Posisikan diri** dalam jangkauan sinyal panel (biasanya radius 10–30 meter).
2. **Nyalakan WiFi** di HP, scan jaringan sekitar.
3. **Sambung ke SSID panel** (contoh: `HD-A1B2C3`), password `88888888`.  
   > ⚠️ Muncul notifikasi "no internet" — **abaikan**, itu normal karena ini jaringan lokal tertutup.
4. **Buka LedArt** → masuk menu **Device** → pilih panel yang terdeteksi.
5. Akses sesuai kebutuhan:
   - **Hardware setting** → masukkan password `888`
   - **Menu teks / konten** → masukkan password `168`
6. **Edit teks**, atur warna dan efek animasi sesuai kebutuhan.
7. Tekan **Send / Upload** ke panel.

---

### Catatan Penting

```
❌ JANGAN hanya ganti teks di preview aplikasi.
   → Harus tekan Upload/Send agar perubahan dikirim ke hardware panel.
   → Kalau tidak, teks balik ke semula saat panel reboot.

📸 Foto/screenshot setting lama sebelum mengubah apapun.
   → Berguna untuk restore ke kondisi semula.
```

---

## Level 2 — Videotron & Layar Besar

> 🏢 **Target:** Videotron mall, jalan raya, gedung perkantoran  
> 🔧 **Hardware:** NovaStar Taurus TB series, Colorlight, Linsn  
> 🌐 **Jalur:** LAN / WiFi AP player (bukan panel WiFi biasa)

Videotron komersial pakai arsitektur berbeda — ada **sending card** dan **media player** terpisah. Bukan panel plug-and-play biasa.

---

### Jalur Masuk yang Realistis

#### 1. WiFi AP Bawaan Player
Banyak NovaStar TB series broadcast AP default:
```
SSID    : (nama default perangkat)
Password: 12345678
```
Setelah konek → buka browser → akses IP player.

#### 2. Web UI / Aplikasi Resmi

| Software | Akses | Kredensial Default |
|---|---|---|
| **ViPlex Handy** (NovaStar, mobile) | IP `192.168.0.10` atau `192.168.41.1` | user: `admin` / pass: `123456` |
| **NovaLCT** (NovaStar, PC) | Sinkronisasi | `666` |
| **NovaLCT** (NovaStar, PC) | Upgrade | `666888` |
| **LEDVISION** (Colorlight, PC) | Admin | `168` |

#### 3. Cloud Platform (Dapat diakses dari internet)

| Platform | Catatan |
|---|---|
| **VNNOX** (NovaStar) | Login tenant kadang masih default |
| **Colorlight Cloud** | Sama — perlu enum akun |

---

### Recon

```bash
# Cari host player di subnet lokal
nmap -sn 192.168.0.0/24

# Scan port spesifik player
nmap -p 80,443,5200,6000 <ip_target>
# Port 5200 = NovaStar VNNOX
```

---

### Shodan Dorks

Untuk unit yang kebetulan expose ke internet publik:

```
http.title:"NovaStar"
http.title:"Viplex"
"LEDVISION"
port:5200
http.html:"Colorlight"
```

> 📌 **Catatan Indonesia:** Sebagian besar videotron di Indonesia berada di belakang **4G / CGNAT**, sehingga tidak visible dari internet publik. Jalur **LAN langsung di lokasi** jauh lebih reliabel dibanding scanning eksternal.

---

### Yang Bisa Dilakukan

Setelah akses berhasil, kontrol yang tersedia bergantung pada hak akses:

| Aksi | Dampak Visible |
|---|---|
| Ganti playlist / konten | Layar tampilkan konten baru |
| Upload video pendek | Langsung tayang |
| Adjust brightness | Paling disadari — seisi area sadar |
| Freeze layar | Layar stuck di frame terakhir |
| Blackout (matiin layar) | Layar mati total |

---

## ⚠️ Disclaimer

> Dokumentasi ini dibuat untuk keperluan **security research dan awareness**.  
> Akses tanpa izin ke sistem orang lain melanggar hukum.  
> Gunakan pengetahuan ini untuk **hardening sistem sendiri** atau **authorized assessment**.

---

<div align="center">
  <sub>Research Notes — LED Panel & Videotron Attack Surface</sub>
</div>
