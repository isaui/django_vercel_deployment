# Deployment Django ke Vercel

Panduan singkat untuk men-deploy proyek Django ke Vercel.

## Langkah-langkah Deployment

1. Buat file `vercel.json` di root proyek:

```json
{
  "builds": [
    {
      "src": "nama_proyek_django/wsgi.py",
      "use": "@vercel/python"
    }
  ],
  "routes": [
    {
      "src": "/(.*)",
      "dest": "nama_proyek_django/wsgi.py"
    }
  ]
}
```

Ganti `nama_proyek_django` dengan nama proyek Django Anda.

2. Ubah file `wsgi.py`:

Tambahkan baris berikut di akhir file `wsgi.py`:

```python
app = application
```

3. Daftarkan repositori GitHub ke Vercel:

   a. Buka [dashboard Vercel](https://vercel.com/dashboard)
   b. Klik tombol "Add New..." dan pilih "Project"
   c. Pilih "Continue with GitHub" dan autentikasi jika diminta
   d. Pilih repositori proyek Django Anda dari daftar
   e. Konfigurasi pengaturan proyek (Vercel biasanya akan mendeteksi otomatis bahwa ini adalah proyek Python/Django)
   f. Klik "Deploy"

Vercel akan men-deploy proyek Anda dan memberikan URL untuk mengaksesnya.

Catatan: Pastikan semua perubahan telah di-push ke repositori GitHub sebelum melakukan deployment.
