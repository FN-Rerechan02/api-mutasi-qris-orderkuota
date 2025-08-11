# Mutasi Order Kuota API Documentation

## Overview
API untuk mengecek status transaksi QRIS pada platform OrderKuota PPOB. Dokumentasi ini mencakup autentikasi, verifikasi OTP, dan pengambilan riwayat transaksi.

## Daftar Isi
1. [Autentikasi](#autentikasi)
   - [Registrasi Akun](#registrasi-akun)
   - [Verifikasi OTP](#verifikasi-otp)
2. [Riwayat Transaksi](#riwayat-transaksi)
3. [Keamanan](#keamanan)
4. [Dukungan](#dukungan)

---

## Autentikasi

### Registrasi Akun
Mendaftarkan akun untuk mendapatkan kredensial akses.

**Endpoint**: `POST https://mutasi.rerechanstore.eu.org/api/auth`

#### Body Permintaan
```json
{
    "username": "jhondoe",
    "password": "secretpass"
}
```

#### Contoh Permintaan
```bash
curl --location 'https://mutasi.rerechanstore.eu.org/api/auth' \
--header 'Content-Type: application/json' \
--data '{
    "username": "jhondoe",
    "password": "secretpass"
}'
```

#### Respons
**Berhasil**: Mengembalikan status sukses dan OTP akan dikirim ke email username orderkuota yang terdaftar.

**Gagal** (401 Unauthorized):
```json
{
  "status": "Error",
  "data": {
    "error": "Username atau password salah"
  }
}
```

---

### Verifikasi OTP
Verifikasi OTP yang diterima setelah registrasi berhasil.

**Endpoint**: `POST https://mutasi.rerechanstore.eu.org/api/auth/otp`

#### Body Permintaan
```json
{
    "username": "jhondoe",
    "otp": "65574"
}
```

#### Contoh Permintaan
```bash
curl --location 'https://mutasi.rerechanstore.eu.org/api/auth/otp' \
--header 'Content-Type: application/json' \
--data '{
    "username": "jhondoe",
    "otp": "12345"
}'
```

#### Respons
**Berhasil**: Mengembalikan token akses (JWT) untuk permintaan terautentikasi.

**Gagal** (401 Unauthorized):
```json
{
  "status": "Error",
  "data": {
    "error": "OTP salah"
  }
}
```

---

## Riwayat Transaksi
Mengambil riwayat transaksi QRIS untuk pengguna terautentikasi.

**Endpoint**: `GET https://mutasi.rerechanstore.eu.org/api/history/{userid}`

#### Header
- `Authorization: Bearer <token-akses-anda>`

#### Contoh Permintaan
```bash
curl --location -g 'https://mutasi.rerechanstore.eu.org/api/history/jhondoe' \
--header 'Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...'
```

#### Respons
**Berhasil** (200 OK):
```json
{
  "success": true,
  "account": {
    "success": true,
    "results": {
      "id": 123,
      "username": "johndoe",
      "name": "johndoe",
      "email": "example@example.com",
      "phone": "087777777",
      "balance": 1234,
      "qrcode": "https://app.orderkuota.com/assets/qrcode/1234567890.png",
      "qris": "https://qris.orderkuota.com/qrnobu/123456789-QR.png",
      "balance_str": "Rp 99.999",
      "qris_balance": 20171,
      "qris_balance_str": "Rp 99.123",
      "qris_name": "TOKO JOHN DOE"
    }
  },
  "qris_history": {
    "success": true,
    "total": 12,
    "page": 1,
    "pages": 2,
    "results": [
      {
        "id": 100000,
        "debet": "123123",
        "kredit": "12324",
        "keterangan": "NOBU / JOHN DOE",
        "tanggal": "12/07/2025 19:02",
        "status": "IN",
        "fee": "",
        "brand": {
          "name": "OVO",
          "logo": "https://app.orderkuota.com/assets/qris/ovo.png"
        },
        "saldo_akhir": "20.123"
      }
    ]
  }
}
```

**Gagal** (403 Forbidden):
```json
{
  "status": "Error",
  "data": {
    "error": "Akses tidak diizinkan"
  }
}
```

---

## Keamanan
- Semua endpoint menggunakan HTTPS
- Autentikasi menggunakan token JWT dengan skema Bearer
- Data sensitif dienkripsi dalam transmisi (TLS 1.2+)
- Pembatasan permintaan (100 permintaan/menit per IP)
- Data akun orderkuota anda tidak dapat di akses oleh oranglain bahkan saya sekalipun.

---

## Donate For Badwidth Server
Donasi Untuk Biaya Badiwdth server api:
**Donasi USDT (Jaringan AVAXC_ERC20):**  
`0xbfa598ed73fee75c5f239d60bd46731e5b8fcce1`

**Kontak:**  
Email: support@rerechanstore.eu.org
---
