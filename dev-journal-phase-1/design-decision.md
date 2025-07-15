# 🛠️ Dev Journal – Smart Photo Storage

Ini adalah jurnal pengembangan proyek Smart Photo Storage. Catatan ini digunakan untuk melacak keputusan teknis, pertimbangan desain, dan refleksi selama pengembangan berlangsung.

---

## 💡 Desain & Pertimbangan Awal

### Penyimpanan Gambar
- Saat ini disimpan ke disk lokal di folder `/uploads`
- Akan diganti dengan MinIO pada fase polishing
- Pertimbangan: agar backend dan storage bisa dipisahkan dengan clean interface

### Autentikasi & JWT
- JWT disimpan di frontend (sementara) via localStorage
- Akan di-upgrade ke HTTP-only cookie di fase akhir
- Middleware untuk verifikasi token sudah diterapkan di backend

### Keamanan Akses Gambar
- Saat ini folder `/uploads` di-serve secara public
- Akan diganti dengan endpoint proteksi berbasis user ID
- Akan dibarengi dengan migrasi ke MinIO

---