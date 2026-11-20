<div align="center">

<img src="https://api.iconify.design/mdi:led-on.svg?color=%2300e0a0&width=72" width="72" />

# Catatan Keamanan LED, Videotron & Gateway

**Dari Running Text Panel sampai Router Admin**

![Level](https://img.shields.io/badge/LEVEL-1_--_3-00e0a0?style=for-the-badge&labelColor=0d1117)
![Bidang](https://img.shields.io/badge/BIDANG-IoT_Display_%2B_LAN-38bdf8?style=for-the-badge&labelColor=0d1117)
![Region](https://img.shields.io/badge/KONTEKS-Indonesia-fbbf24?style=for-the-badge&labelColor=0d1117)
![Doks](https://img.shields.io/badge/DOKS-Security_Research-a78bfa?style=for-the-badge&labelColor=0d1117)

<sub>Rantai serangan berlapis: panel LED → videotron → gateway jaringan.</sub>

</div>

---

> [!IMPORTANT]
> **Baca urutannya dulu.** Dokumen ini tersusun sebagai eskalasi tiga level. Level 1 dan 2 mengandalkan kredensial default pabrikan pada perangkat display. Level 3 adalah lapisan jaringan yang dipakai kalau panel/videotron perlu didaftarkan permanen ke WiFi — dan di situ router ikut masuk sebagai target.
>
> Hampir semua jalur bergantung pada satu hal: **firmware bawaan pabrik yang tidak pernah diganti passwordnya.**

---

## <img src="https://api.iconify.design/mdi:format-list-bulleted.svg?color=00e0a0&width=20" width="20" /> Daftar Isi

| | Bagian |
|:--:|:--|
| <img src="https://api.iconify.design/mdi:sitemap.svg?color=38bdf8&width=18" width="18" /> | [Rantai Serangan Gabungan](#-rantai-serangan-gabungan) |
| <img src="https://api.iconify.design/mdi:cellphone.svg?color=00e0a0&width=18" width="18" /> | **Level 1 — Running Text Panel** |
| | <img src="https://api.iconify.design/mdi:map-marker-path.svg?color=38bdf8&width=16" width="16" /> [Skema Serangan](#-skema-serangan-1) |
| | <img src="https://api.iconify.design/mdi:wifi.svg?color=38bdf8&width=16" width="16" /> [SSID Penanda](#-ssid-penanda) |
| | <img src="https://api.iconify.design/mdi:key-variant.svg?color=fbbf24&width=16" width="16" /> [Tabel Kredensial Default](#-tabel-kredensial-default-1) |
| | <img src="https://api.iconify.design/mdi:apps.svg?color=fbbf24&width=16" width="16" /> [Aplikasi yang Digunakan](#-aplikasi-yang-digunakan) |
| | <img src="https://api.iconify.design/mdi:format-list-numbered.svg?color=a78bfa&width=16" width="16" /> [Langkah-Langkah](#-langkah-langkah-contoh-huidu-ledart) |
| <img src="https://api.iconify.design/mdi:monitor-multiple.svg?color=00e0a0&width=18" width="18" /> | **Level 2 — Videotron & Layar Besar** |
| | <img src="https://api.iconify.design/mdi:door-open.svg?color=38bdf8&width=16" width="16" /> [Jalur Masuk yang Realistis](#-jalur-masuk-yang-realistis) |
| | <img src="https://api.iconify.design/mdi:radar.svg?color=38bdf8&width=16" width="16" /> [Recon](#-recon) |
| | <img src="https://api.iconify.design/mdi:server-search.svg?color=fbbf24&width=16" width="16" /> [Shodan Dorks](#-shodan-dorks) |
| | <img src="https://api.iconify.design/mdi:cursor-default-click.svg?color=a78bfa&width=16" width="16" /> [Yang Bisa Dilakukan](#-yang-bisa-dilakukan) |
| <img src="https://api.iconify.design/mdi:router-wireless.svg?color=00e0a0&width=18" width="18" /> | **Level 3 — Router & Gateway** |
| | <img src="https://api.iconify.design/mdi:map-marker-path.svg?color=38bdf8&width=16" width="16" /> [Skema Serangan](#-skema-serangan-2) |
| | <img src="https://api.iconify.design/mdi:wifi-marker.svg?color=38bdf8&width=16" width="16" /> [Peta SSID Pabrikan](#-peta-ssid-pabrikan) |
| | <img src="https://api.iconify.design/mdi:key-variant.svg?color=fbbf24&width=16" width="16" /> [Kredensial Default Router](#-kredensial-default-router) |
| | <img src="https://api.iconify.design/mdi:terminal.svg?color=a78bfa&width=16" width="16" /> [Toolkit & Otomasi](#-toolkit) |
| | <img src="https://api.iconify.design/mdi:lan-connect.svg?color=00e0a0&width=16" width="16" /> [Daftarkan Panel](#-daftarkan-panel) |
| <img src="https://api.iconify.design/mdi:restore.svg?color=38bdf8&width=18" width="18" /> | [Restore & Cleanup](#-restore--cleanup) |
| <img src="https://api.iconify.design/mdi:shield-check.svg?color=00e0a0&width=18" width="18" /> | [Hardening](#-hardening) |
| <img src="https://api.iconify.design/mdi:alert.svg?color=f87171&width=18" width="18" /> | [Disclaimer](#-disclaimer) |

---

## <img src="https://api.iconify.design/mdi:sitemap.svg?color=00e0a0&width=24" width="24" /> Rantai Serangan Gabungan

```
   [ LEVEL 1 ]                        [ LEVEL 3 ]
   Panel LED nyala                    Router / gateway
        |                                   ^
        v                                   |
   SSID pabrikan terdeteksi                 |
        |                                   |
        v                                   |
   Konek pakai password default             |
        |                                   |
        v                                   |
   Buka app pabrikan --> edit teks          |
        |                                   |
        v                                   |
   Upload ke panel                          |
        |                                   |
        v                                   |
   Panel perlu WiFi permanen --------------+
        |
        v
   Masuk admin router (kredensial default)
        |
        v
   Daftarkan MAC panel / DHCP reservation
        |
        v
   [ DONE ]

   [ LEVEL 2 ]  jalur paralel untuk layar besar
   Videotron --> WiFi AP player / Web UI / Cloud --> upload konten
```

> [!NOTE]
> Level 1 dan 3 sering dipakai berpasangan: panel yang sudah bisa diubah teksnya tetap rapuh kalau IP-nya dinamis. Level 3 yang membuat perubahannya bertahan setelah reboot.

---

# <img src="https://api.iconify.design/mdi:cellphone.svg?color=00e0a0&width=28" width="28" /> Level 1 — Running Text Panel

Ini jalur yang paling sering dipakai — cocok buat panel kecil di depan toko, kantor, atau sekolah yang masih jalan dengan firmware bawaan pabrik tanpa ganti password.

## <img src="https://api.iconify.design/mdi:map-marker-path.svg?color=38bdf8&width=24" width="24" /> Skema Serangan

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

## <img src="https://api.iconify.design/mdi:wifi.svg?color=38bdf8&width=24" width="24" /> SSID Penanda

Format SSID yang jadi penanda panel LED dengan firmware default:

| Format SSID | Keterangan |
|:--|:--|
| `TF-WIFI_######` | PowerLED series |
| `HD-xxxx` | Huidu series |
| `WF-xxxx` | Generic WiFi controller |
| `LED-xxxx` | Generic |
| `XF-xxxx` | Xixun series |

## <img src="https://api.iconify.design/mdi:key-variant.svg?color=fbbf24&width=24" width="24" /> Tabel Kredensial Default

| Merek | Password WiFi | App — Common | App — Hardware |
|:--|:--|:--|:--|
| Huidu / HD-WF | `88888888` / `lc12345678` / `lc888888` | `168` | `888` |
| Xixun / X series | `12345678` | `168` | `888` (LED Editor) |
| PowerLED / TF-WIFI | `88888888` | `168` | — |
| PSD series | IP: `192.168.0.XXX` | user: `admin` | test mode: `666888` |

## <img src="https://api.iconify.design/mdi:apps.svg?color=fbbf24&width=24" width="24" /> Aplikasi yang Digunakan

| Aplikasi | Platform | Merek Target |
|:--|:--|:--|
| LedArt | Android / iOS | Huidu |
| XLGPowerLedPro | Android / iOS | PowerLED |
| LedshowTW / LED Editor | Android / iOS | Xixun |
| HDPlayer | Windows (PC) | Huidu |

## <img src="https://api.iconify.design/mdi:format-list-numbered.svg?color=a78bfa&width=24" width="24" /> Langkah-Langkah (Contoh Huidu LedArt)

1. Nyalakan panel, tunggu sampai booting penuh.
2. Di HP: buka **Setelan → WiFi**, scan.
3. Cari SSID berpola `HD-xxxx`, sambungkan dengan password default dari tabel di atas.
4. Abaikan notifikasi "WiFi tanpa internet".
5. Buka **LedArt** — panel biasanya terdeteksi otomatis dalam beberapa detik.
6. Kalau tidak terdeteksi: menu **Device → Add**, masukkan IP panel (`192.168.0.x`) secara manual.
7. Masuk ke **Program → Text**, tulis teks pengganti.
8. Atur warna, kecepatan, dan efek seperlunya.
9. Tekan **Upload / Send** — bukan sekadar simpan.
10. Putus WiFi panel, sambungkan HP kembali ke internet.
11. Cek panel secara fisik: teks harus berubah dan **bertahan setelah panel di-reboot**.

> [!WARNING]
> **Catatan kritis:** jangan hanya ganti teks di preview aplikasi. Harus tekan **Upload/Send** supaya perubahan dikirim ke hardware. Kalau tidak, teks balik ke semula saat panel reboot.

> [!TIP]
> **Catatan kritis:** foto atau screenshot setting lama sebelum mengubah apa pun — berguna untuk restore ke kondisi semula.

---

# <img src="https://api.iconify.design/mdi:monitor-multiple.svg?color=00e0a0&width=28" width="28" /> Level 2 — Videotron & Layar Besar

Videotron komersial pakai arsitektur berbeda dari panel kecil — ada sending card dan media player terpisah. Merek dominan: NovaStar Taurus TB series, Colorlight, Linsn. Jalurnya LAN, bukan WiFi direct receh.

## <img src="https://api.iconify.design/mdi:door-open.svg?color=38bdf8&width=24" width="24" /> Jalur Masuk yang Realistis

| # | Jalur | Catatan |
|:--:|:--|:--|
| 1 | **WiFi AP bawaan player** | Player menyiarkan SSID sendiri, password default pabrikan |
| 2 | **Web UI / software resmi** | Viplex, SmartLCT, LEDVISION — lewat LAN |
| 3 | **Cloud platform** | VNNOX / Colorlight Cloud, jalur dari internet |

## <img src="https://api.iconify.design/mdi:radar.svg?color=38bdf8&width=24" width="24" /> Recon

```bash
# Cari semua host aktif di subnet player
nmap -sn 192.168.0.0/24

# Scan port spesifik yang dipakai player LED
nmap -p 80,443,5200,6000 <ip_target>
# 5200 = NovaStar VNNOX protocol
# 6000 = player control port
```

## <img src="https://api.iconify.design/mdi:server-search.svg?color=fbbf24&width=24" width="24" /> Shodan Dorks

Untuk unit yang kebetulan expose ke internet publik:

```
http.title:"NovaStar"
http.title:"Viplex"
"LEDVISION"
port:5200
http.html:"Colorlight"
```

> [!IMPORTANT]
> **Catatan Indonesia:** sebagian besar videotron di Indonesia berada di belakang 4G / CGNAT — tidak visible dari internet publik. Jalur LAN langsung di lokasi jauh lebih reliabel dibanding scanning eksternal.

## <img src="https://api.iconify.design/mdi:cursor-default-click.svg?color=a78bfa&width=24" width="24" /> Yang Bisa Dilakukan

| Aksi | Dampak Visible | Impact |
|:--|:--|:--|
| Ganti playlist / konten | Layar tampilkan konten baru | Medium |
| Upload video pendek | Langsung tayang real-time | High |
| Adjust brightness | Paling disadari — seisi area sadar | High |
| Freeze layar | Layar stuck di frame terakhir | Medium |
| Blackout / matiin layar | Layar mati total | High |

---

# <img src="https://api.iconify.design/mdi:router-wireless.svg?color=00e0a0&width=28" width="28" /> Level 3 — Router & Gateway

> [!IMPORTANT]
> **Prasyarat mutlak:** perangkat sudah terhubung ke WiFi target. Ini jalur **internal**, bukan crack WPA2 dari luar. Tujuannya: panel butuh SSID + password + IP valid — semua itu diatur di router, bukan di panel.

## <img src="https://api.iconify.design/mdi:map-marker-path.svg?color=38bdf8&width=24" width="24" /> Skema Serangan

```
[ Sudah konek WiFi target ]
            |
            v
[ Cari gateway: ip route | arp-scan ]
            |
            v
[ Fingerprint merek dari SSID / MAC OUI ]
            |
            v
[ Buka http://<gateway> ]
            |
            v
[ Coba kredensial default pabrikan ]
            |
        BERHASIL?
        /        \
      YES         NO
       |           |
       |           v
       |    [ Cek stiker router ]
       |    [ Coba WPS PIN default ]
       |    [ Brute force kecil ]
       |           |
       v           v
[ MASUK PANEL ADMIN ]
            |
            v
[ Daftarkan MAC panel / DHCP reservation ]
            |
            v
[ Isi SSID + password ke app panel ]
            |
            v
[ Panel online permanen — selesai ]
```

### Recon gateway

```bash
ip route | grep default
ip neigh
sudo arp-scan --localnet
curl -sI http://192.168.1.1 | grep -Ei 'server|www-authenticate'
```

## <img src="https://api.iconify.design/mdi:wifi-marker.svg?color=38bdf8&width=24" width="24" /> Peta SSID Pabrikan

| Pola SSID | Merek | Kredensial |
|:--|:--|:--|
| `TP-Link_XXXX` / `TP-LINK_XXXX` | TP-Link | `admin/admin` |
| `Tenda_XXXXXX` | Tenda | `admin/admin` |
| `TOTOLINK_XXXX` | TOTOLINK | `admin/admin` |
| `NETGEARxx` | Netgear | `admin/password` |
| `dlink-XXXX` | D-Link | `admin` (kosong) |
| `LinksysXXXXX` | Linksys | `admin/admin` |
| `ASUS` / `ASUS_XX` | ASUS | `admin/admin` |
| `MikroTik-XXXXX` | MikroTik | `admin` (kosong) |
| `Huawei-XXXX` / `HG8245H-XXXX` | Huawei / IndiHome | `telecomadmin/admintelecom` |
| `ZTE-XXXX` / `ZTEG-XXXX` | ZTE / IndiHome | `admin/admin` |
| `FiberHome-XXXX` / `FH-XXXX` | FiberHome | `admin/admin` |
| `SAGEMCOM_XXXX` | Sagemcom | `admin/admin` |
| `Xiaomi_XXXX` | Xiaomi | diset saat boot pertama |
| `IndiHome-XXXX` | IndiHome | stiker + `telecomadmin` |

## <img src="https://api.iconify.design/mdi:key-variant.svg?color=fbbf24&width=24" width="24" /> Kredensial Default Router

Urutan percobaan: **1)** stiker bodi router, **2)** tabel paling umum, **3)** variasi pabrikan, **4)** brute force kecil.

**Paling umum:**

| User | Password |
|:--|:--|
| `admin` | `admin` |
| `admin` | `password` |
| `admin` | *(kosong)* |
| `root` | `root` |
| `admin` | `1234` |

**Per pabrikan:**

| Merek | User | Password |
|:--|:--|:--|
| TP-Link | `admin` | `admin` |
| D-Link | `admin` | *(kosong)* |
| Netgear | `admin` | `password` |
| Linksys | `admin` | `admin` |
| Tenda | `admin` | `admin` |
| TOTOLINK | `admin` | `admin` |
| MikroTik | `admin` | *(kosong)* |
| Ubiquiti | `ubnt` | `ubnt` |
| Cisco | `cisco` | `cisco` |
| ZyXEL | `admin` | `1234` |
| Huawei ONT | `telecomadmin` | `admintelecom` |
| ZTE ONT | `ZXDSL` | `ZXDSL` |
| FiberHome | `CUAdmin` | `CUAdmin` |

**URL panel admin:** `192.168.1.1` · `192.168.0.1` · `192.168.1.254` · `192.168.100.1` · `192.168.88.1` (MikroTik) · `10.0.0.138` · `tplinkwifi.net` · `routerlogin.net` · `tendawifi.com`

**PIN WPS default:** `12345670` · `00000000` · `12345678` · `11111111`

## <img src="https://api.iconify.design/mdi:terminal.svg?color=a78bfa&width=24" width="24" /> Toolkit

| Tool | Fungsi |
|:--|:--|
| `nmap` | discovery + `http-default-accounts` |
| `arp-scan` / `netdiscover` | enumerasi host LAN |
| `RouterSploit` | exploit + creds router |
| `hydra` | brute force HTTP basic/form |
| `macchanger` | bypass MAC filter |
| `reaver` / `wash` | WPS |
| `bettercap` | MITM / sniffing panel |
| Fing (mobile) | scan LAN dari HP |

```bash
sudo nmap -p 80,443,8080 --script http-default-accounts 192.168.1.0/24

hydra -l admin -P pass.txt -t 4 -f http-get://192.168.1.1

cd routersploit && python3 rsf.py
use creds/generic/http_basic_default
set target 192.168.1.1
run
```

> [!CAUTION]
> Beberapa firmware lockout setelah 5–10 percobaan gagal. Jeda antar percobaan, jangan tembak `rockyou.txt` penuh.

## <img src="https://api.iconify.design/mdi:lan-connect.svg?color=00e0a0&width=24" width="24" /> Daftarkan Panel

Dari panel admin router, siapkan ini **sebelum** menyentuh app panel:

| # | Aksi | Lokasi |
|:--:|:--|:--|
| 1 | Matikan AP / client isolation | Wireless → Advanced |
| 2 | Paksa 2.4 GHz | Wireless → Band |
| 3 | Channel 1–11, hindari 12–13 | Wireless → Channel |
| 4 | WPA2-PSK (AES), bukan WPA3 | Wireless → Security |
| 5 | SSID tanpa spasi & karakter khusus | Wireless → SSID |
| 6 | DHCP reservation untuk MAC panel | LAN → DHCP → Static Lease |
| 7 | Whitelist MAC panel kalau filter aktif | Wireless → MAC Filter |
| 8 | Matikan WPS setelah panel tersambung | Wireless → WPS |

Kalau panel tanpa keypad: set SSID open sementara → biarkan join → ambil MAC dari daftar client → pasang WPA2 lewat app → atau pakai WPS push button.

```bash
ping -c 3 <ip_panel>
```

> [!IMPORTANT]
> Jangan biarkan panel pakai IP dinamis. Reboot router = IP berubah = setup ulang dari nol.

---

## <img src="https://api.iconify.design/mdi:restore.svg?color=38bdf8&width=24" width="24" /> Restore & Cleanup

- [ ] Screenshot setiap halaman yang diubah, sebelum diubah
- [ ] Catat teks/konten asli panel sebelum diganti
- [ ] Catat SSID, password, channel, dan mode keamanan asli
- [ ] Catat isi DHCP static lease sebelum ditambah
- [ ] Ekspor config router kalau tersedia
- [ ] Kembalikan SSID/password ke nilai awal bila sudah tidak dipakai
- [ ] Hapus static lease yang dibuat
- [ ] Jangan ganti password admin kecuali diminta

---

## <img src="https://api.iconify.design/mdi:shield-check.svg?color=00e0a0&width=24" width="24" /> Hardening

| Temuan | Perbaikan |
|:--|:--|
| Password WiFi panel default | Ganti setelah instalasi |
| Password admin router default | Ganti ke 16+ karakter |
| WPS aktif | Matikan |
| Remote management nyala | Matikan kalau tidak dipakai |
| UPnP nyala | Matikan |
| Panel admin terbuka di LAN | Batasi ke IP tertentu |
| Firmware lawas | Update firmware |
| SSID menampilkan merek & model | Ganti SSID netral |
| Guest network tanpa isolasi | Aktifkan client isolation |

---

## <img src="https://api.iconify.design/mdi:alert.svg?color=f87171&width=24" width="24" /> Disclaimer

Dokumentasi ini dibuat untuk keperluan security research dan awareness.

Akses tanpa izin ke sistem orang lain melanggar hukum — termasuk UU ITE di Indonesia.

Gunakan pengetahuan ini untuk hardening sistem sendiri atau dalam konteks authorized penetration testing.

<div align="center">
<sub><b>Level 1</b> — Running Text Panel &nbsp;·&nbsp; <b>Level 2</b> — Videotron &nbsp;·&nbsp; <b>Level 3</b> — Router &amp; Gateway</sub>
</div>
