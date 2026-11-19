<div align="center">

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:050d1a,40:0a1f3d,80:003366,100:00d4ff&height=280&section=header&text=LED%20Panel%20%26%20Videotron&fontSize=56&fontColor=ffffff&animation=fadeIn&fontAlignY=38&desc=Security%20Research%20Notes&descAlignY=58&descSize=22&descColor=a8d8ff" width="100%"/>

<br/>

[![Typing SVG](https://readme-typing-svg.demolab.com?font=JetBrains+Mono&weight=700&size=16&duration=2800&pause=800&color=00D4FF&center=true&vCenter=true&repeat=true&width=850&height=52&lines=Dokumentasi+celah+keamanan+LED+panel+%26+videotron+komersial;Default+credentials+%E2%80%94+WiFi+AP+exploitation+%E2%80%94+Web+UI+access;NovaStar+%7C+Huidu+%7C+Xixun+%7C+Colorlight+%7C+PowerLED;Security+assessment+%26+hardening+untuk+sistem+LED)](https://git.io/typing-svg)

<br/>

<img src="https://img.shields.io/badge/Research-Security%20Assessment-00D4FF?style=for-the-badge&logo=hackaday&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Target-LED%20Panel%20%26%20Videotron-FF6B35?style=for-the-badge&logo=youtube&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Platform-Android%20%7C%20iOS%20%7C%20Windows-00C853?style=for-the-badge&logo=android&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Level-01%20%26%2002-7B2FBE?style=for-the-badge&logo=hackthebox&logoColor=white"/>

<br/><br/>

<img src="https://raw.githubusercontent.com/hirumisann/LED-Panel-Videotron-Security-Research-Notes/main/Catatan%20Keamanan%20LED%20dan%20Videotron.png" width="90%" alt="Catatan Keamanan LED dan Videotron"/>

<br/><br/>

</div>

---

<div align="center">

## <img src="https://api.iconify.design/mdi/format-list-bulleted.svg?color=%2300D4FF&height=28" alt="toc"/> &nbsp; Daftar Isi

</div>

<br/>

&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/cellphone.svg?color=%2300D4FF&height=16" alt="phone"/>&nbsp; **[Level 1 — Running Text Panel](#-level-1--running-text-panel)**
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/antenna.svg?color=%23a0a0a0&height=14" alt="antenna"/>&nbsp; [Skema Serangan](#skema-serangan)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/wifi.svg?color=%23a0a0a0&height=14" alt="wifi"/>&nbsp; [SSID Penanda](#ssid-penanda)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/key-variant.svg?color=%23a0a0a0&height=14" alt="key"/>&nbsp; [Tabel Kredensial Default](#tabel-kredensial-default)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/application.svg?color=%23a0a0a0&height=14" alt="app"/>&nbsp; [Aplikasi yang Digunakan](#aplikasi-yang-digunakan)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/format-list-numbered.svg?color=%23a0a0a0&height=14" alt="steps"/>&nbsp; [Langkah-Langkah](#langkah-langkah-contoh-huidu-ledart)

<br/>

&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/monitor.svg?color=%23FF6B35&height=16" alt="monitor"/>&nbsp; **[Level 2 — Videotron & Layar Besar](#-level-2--videotron--layar-besar)**
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/door-open.svg?color=%23a0a0a0&height=14" alt="door"/>&nbsp; [Jalur Masuk yang Realistis](#jalur-masuk-yang-realistis)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/magnify.svg?color=%23a0a0a0&height=14" alt="search"/>&nbsp; [Recon](#recon)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/database-search.svg?color=%23a0a0a0&height=14" alt="shodan"/>&nbsp; [Shodan Dorks](#shodan-dorks)
- &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<img src="https://api.iconify.design/mdi/lightning-bolt.svg?color=%23a0a0a0&height=14" alt="action"/>&nbsp; [Yang Bisa Dilakukan](#yang-bisa-dilakukan)

<br/>

---

<br/>

<div align="center">

## <img src="https://api.iconify.design/mdi/cellphone-wireless.svg?color=%2300D4FF&height=30" alt="phone"/> &nbsp; Level 1 — Running Text Panel

<img src="https://img.shields.io/badge/Estimasi-5%20Menit-00D4FF?style=flat-square&logo=clockify&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Perangkat-Cukup%20HP-00C853?style=flat-square&logo=android&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Skill-Pemula-7B2FBE?style=flat-square&logo=hackthebox&logoColor=white"/>

</div>

<br/>

Ini jalur yang paling sering dipakai — cocok buat panel kecil di depan toko, kantor, atau sekolah yang masih jalan dengan firmware bawaan pabrik tanpa ganti password.

<br/>

---

### <img src="https://api.iconify.design/mdi/map-marker-path.svg?color=%2300D4FF&height=22" alt="path"/> &nbsp; Skema Serangan

<br/>

```
  [ Panel Nyala ]
       |
       v
  [ Nyalain WiFi HP ]
       |
       v
  [ Scan Jaringan Sekitar ]
       |
       v
  [ Ketemu SSID Pabrikan? ]
       |
      YES
       |
       v
  [ Konek — Password Default ]
       |
       v
  [ Buka App Pabrikan ]
       |
       v
  [ Edit Teks / Konten ]
       |
       v
  [ UPLOAD ke Panel ]  <-- jangan lupa step ini
       |
       v
  [ DONE ]
```

<br/>

---

### <img src="https://api.iconify.design/mdi/wifi-marker.svg?color=%2300D4FF&height=22" alt="wifi"/> &nbsp; SSID Penanda

<br/>

Format SSID yang jadi penanda panel LED dengan firmware default:

<br/>

| <img src="https://api.iconify.design/mdi/wifi.svg?color=%2300D4FF&height=16" alt="wifi"/> Format SSID | <img src="https://api.iconify.design/mdi/tag.svg?color=%23FF6B35&height=16" alt="tag"/> Keterangan |
|:---|:---|
| `TF-WIFI_######` | PowerLED series |
| `HD-xxxx` | Huidu series |
| `WF-xxxx` | Generic WiFi controller |
| `LED-xxxx` | Generic |
| `XF-xxxx` | Xixun series |

<br/>

---

### <img src="https://api.iconify.design/mdi/key-chain-variant.svg?color=%2300D4FF&height=22" alt="key"/> &nbsp; Tabel Kredensial Default

<br/>

| <img src="https://api.iconify.design/mdi/factory.svg?color=%2300D4FF&height=16" alt="brand"/> Merek | <img src="https://api.iconify.design/mdi/wifi-lock.svg?color=%23FF6B35&height=16" alt="wifi-pass"/> Password WiFi | <img src="https://api.iconify.design/mdi/application-cog.svg?color=%2300C853&height=16" alt="app-common"/> App — Common | <img src="https://api.iconify.design/mdi/tune.svg?color=%237B2FBE&height=16" alt="app-hw"/> App — Hardware |
|:---|:---:|:---:|:---:|
| **Huidu / HD-WF** | `88888888` &nbsp;/&nbsp; `lc12345678` &nbsp;/&nbsp; `lc888888` | `168` | `888` |
| **Xixun / X series** | `12345678` | `168` | `888` *(LED Editor)* |
| **PowerLED / TF-WIFI** | `88888888` | `168` | — |
| **PSD series** | IP: `192.168.0.XXX` | user: `admin` | test mode: `666888` |

<br/>

---

### <img src="https://api.iconify.design/mdi/application-brackets.svg?color=%2300D4FF&height=22" alt="app"/> &nbsp; Aplikasi yang Digunakan

<br/>

| <img src="https://api.iconify.design/mdi/application.svg?color=%2300D4FF&height=16" alt="app"/> Aplikasi | <img src="https://api.iconify.design/mdi/devices.svg?color=%23FF6B35&height=16" alt="platform"/> Platform | <img src="https://api.iconify.design/mdi/factory.svg?color=%2300C853&height=16" alt="brand"/> Merek Target |
|:---|:---:|:---|
| **LedArt** | Android / iOS | Huidu |
| **XLGPowerLedPro** | Android / iOS | PowerLED |
| **LedshowTW / LED Editor** | Android / iOS | Xixun |
| **HDPlayer** | Windows (PC) | Huidu |

<br/>

---

### <img src="https://api.iconify.design/mdi/list-box.svg?color=%2300D4FF&height=22" alt="steps"/> &nbsp; Langkah-Langkah (Contoh Huidu LedArt)

<br/>

<details>
<summary><img src="https://api.iconify.design/mdi/chevron-down-circle.svg?color=%2300D4FF&height=16" alt="expand"/> &nbsp; <strong>Klik untuk expand langkah-langkah</strong></summary>

<br/>

**Step 1 — Posisi**

Masuk ke dalam radius sinyal panel. Biasanya 10–30 meter. Makin dekat makin stabil koneksinya.

---

**Step 2 — Scan Jaringan**

Aktifkan WiFi di HP. Scan jaringan sekitar, cari SSID dengan format `HD-xxxxxx` atau pabrikan lain yang tercantum di tabel di atas.

---

**Step 3 — Konek ke Panel**

Sambung ke SSID panel. Password: `88888888`

> Notifikasi **"No internet"** atau **"Connected without internet"** adalah hal normal. Ini jaringan lokal tertutup — abaikan dan lanjut.

---

**Step 4 — Buka LedArt**

Buka app **LedArt** → masuk ke menu **Device** → panel yang terhubung akan muncul otomatis dalam daftar.

---

**Step 5 — Akses Setting**

Pilih panel → masuk ke menu yang dibutuhkan:

| Tujuan | Password |
|:---|:---:|
| Hardware setting | `888` |
| Edit teks / konten | `168` |

---

**Step 6 — Edit & Upload**

Edit teks, atur warna dan efek animasi. Tekan **Send** atau **Upload** untuk kirim ke hardware panel.

</details>

<br/>

> **Catatan kritis:**
>
> <img src="https://api.iconify.design/mdi/alert-circle.svg?color=%23FF3D3D&height=14" alt="alert"/> Jangan hanya ganti teks di preview aplikasi. Harus tekan **Upload/Send** supaya perubahan dikirim ke hardware. Kalau tidak, teks balik ke semula saat panel reboot.
>
> <img src="https://api.iconify.design/mdi/camera.svg?color=%23FF6B35&height=14" alt="camera"/> Foto atau screenshot setting lama sebelum mengubah apapun — berguna untuk restore ke kondisi semula.

<br/>

---

<br/>

<div align="center">

## <img src="https://api.iconify.design/mdi/monitor-multiple.svg?color=%23FF6B35&height=30" alt="monitor"/> &nbsp; Level 2 — Videotron & Layar Besar

<img src="https://img.shields.io/badge/Target-Mall%20%7C%20Jalan%20Raya%20%7C%20Gedung-FF6B35?style=flat-square&logo=youtube&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Hardware-NovaStar%20%7C%20Colorlight%20%7C%20Linsn-00D4FF?style=flat-square&logo=raspberrypi&logoColor=white"/>
&nbsp;
<img src="https://img.shields.io/badge/Jalur-LAN%20%2F%20WiFi%20AP-7B2FBE?style=flat-square&logo=cisco&logoColor=white"/>

</div>

<br/>

Videotron komersial pakai arsitektur berbeda dari panel kecil — ada **sending card** dan **media player** terpisah. Merek dominan: NovaStar Taurus TB series, Colorlight, Linsn. Jalurnya LAN, bukan WiFi direct receh.

<br/>

---

### <img src="https://api.iconify.design/mdi/door-open.svg?color=%2300D4FF&height=22" alt="door"/> &nbsp; Jalur Masuk yang Realistis

<br/>

<details>
<summary><img src="https://api.iconify.design/mdi/wifi-cog.svg?color=%2300D4FF&height=16" alt="wifi-ap"/> &nbsp; <strong>Jalur 1 — WiFi AP Bawaan Player</strong></summary>

<br/>

Banyak NovaStar TB series broadcast access point dengan kredensial default:

```
Password AP : 12345678
```

Setelah terhubung, buka browser dan navigasi ke IP player untuk akses web UI.

</details>

<br/>

<details>
<summary><img src="https://api.iconify.design/mdi/application-cog.svg?color=%23FF6B35&height=16" alt="app"/> &nbsp; <strong>Jalur 2 — Web UI / Software Resmi</strong></summary>

<br/>

| <img src="https://api.iconify.design/mdi/application.svg?color=%2300D4FF&height=14" alt="app"/> Software | <img src="https://api.iconify.design/mdi/ip-network.svg?color=%23FF6B35&height=14" alt="ip"/> Akses / IP | <img src="https://api.iconify.design/mdi/key-variant.svg?color=%2300C853&height=14" alt="key"/> Kredensial Default |
|:---|:---|:---|
| **ViPlex Handy** *(NovaStar, mobile)* | `192.168.0.10` atau `192.168.41.1` | user: `admin` / pass: `123456` |
| **NovaLCT** *(NovaStar, PC)* — Sinkronisasi | — | `666` |
| **NovaLCT** *(NovaStar, PC)* — Upgrade | — | `666888` |
| **LEDVISION** *(Colorlight, PC)* | — | admin: `168` |

</details>

<br/>

<details>
<summary><img src="https://api.iconify.design/mdi/cloud-lock.svg?color=%237B2FBE&height=16" alt="cloud"/> &nbsp; <strong>Jalur 3 — Cloud Platform (dari internet)</strong></summary>

<br/>

| <img src="https://api.iconify.design/mdi/cloud.svg?color=%2300D4FF&height=14" alt="cloud"/> Platform | <img src="https://api.iconify.design/mdi/information.svg?color=%23FF6B35&height=14" alt="info"/> Catatan |
|:---|:---|
| **VNNOX** *(NovaStar)* | Login tenant kadang masih default |
| **Colorlight Cloud** | Perlu enum akun — sering masih default |

</details>

<br/>

---

### <img src="https://api.iconify.design/mdi/magnify-scan.svg?color=%2300D4FF&height=22" alt="scan"/> &nbsp; Recon

<br/>

```bash
# Cari semua host aktif di subnet player
nmap -sn 192.168.0.0/24

# Scan port spesifik yang dipakai player LED
nmap -p 80,443,5200,6000 <ip_target>
# 5200 = NovaStar VNNOX protocol
# 6000 = player control port
```

<br/>

---

### <img src="https://api.iconify.design/mdi/database-search.svg?color=%2300D4FF&height=22" alt="shodan"/> &nbsp; Shodan Dorks

<br/>

Untuk unit yang kebetulan expose ke internet publik:

```
http.title:"NovaStar"
http.title:"Viplex"
"LEDVISION"
port:5200
http.html:"Colorlight"
```

<br/>

> <img src="https://api.iconify.design/mdi/map-marker-alert.svg?color=%23FF6B35&height=14" alt="alert"/> **Catatan Indonesia:** Sebagian besar videotron di Indonesia berada di belakang **4G / CGNAT** — tidak visible dari internet publik. Jalur **LAN langsung di lokasi** jauh lebih reliabel dibanding scanning eksternal.

<br/>

---

### <img src="https://api.iconify.design/mdi/lightning-bolt-circle.svg?color=%2300D4FF&height=22" alt="action"/> &nbsp; Yang Bisa Dilakukan

<br/>

| <img src="https://api.iconify.design/mdi/gesture-tap.svg?color=%2300D4FF&height=14" alt="action"/> Aksi | <img src="https://api.iconify.design/mdi/eye.svg?color=%23FF6B35&height=14" alt="visible"/> Dampak Visible | <img src="https://api.iconify.design/mdi/gauge.svg?color=%2300C853&height=14" alt="impact"/> Impact |
|:---|:---|:---:|
| Ganti playlist / konten | Layar tampilkan konten baru | Medium |
| Upload video pendek | Langsung tayang real-time | High |
| Adjust brightness | Paling disadari — seisi area sadar | High |
| Freeze layar | Layar stuck di frame terakhir | Medium |
| Blackout / matiin layar | Layar mati total | High |

<br/>

---

<br/>

<div align="center">

<img src="https://img.shields.io/badge/Status-Research%20%26%20Documentation-00D4FF?style=for-the-badge&logo=readthedocs&logoColor=white"/>

<br/><br/>

<details>
<summary><img src="https://api.iconify.design/mdi/shield-alert.svg?color=%23FF3D3D&height=16" alt="disclaimer"/> &nbsp; <strong>Disclaimer</strong></summary>

<br/>

Dokumentasi ini dibuat untuk keperluan **security research** dan **awareness**.

Akses tanpa izin ke sistem orang lain melanggar hukum — termasuk UU ITE di Indonesia.

Gunakan pengetahuan ini untuk **hardening sistem sendiri** atau dalam konteks **authorized penetration testing**.

</details>

<br/>

<img src="https://capsule-render.vercel.app/api?type=waving&color=0:00d4ff,60:003366,100:050d1a&height=140&section=footer&fontSize=14&fontColor=a8d8ff&animation=fadeIn" width="100%"/>

</div>
