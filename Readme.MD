# Deployment Django ke Vercel

Panduan singkat untuk men-deploy proyek Django ke Vercel dengan PostgreSQL.

## Langkah-langkah Deployment

1. Buat database PostgreSQL di Vercel:
   a. Buka [dashboard Vercel](https://vercel.com/dashboard)
   b. Klik tombol "Add New..." dan pilih "Project"
   c. Pilih repositori proyek Django Anda dari daftar
   d. Pada halaman konfigurasi, klik tab "Storage"
   e. Pilih "Connect Store" dan pilih "Postgres"
   f. Ikuti petunjuk untuk membuat database baru
   g. Setelah database dibuat, Anda akan mendapatkan URL database. Simpan URL ini untuk digunakan nanti.

2. Konfigurasi Django untuk menggunakan PostgreSQL:
   a. Jika belum ada, tambahkan `psycopg2-binary` dan `dj-database-url` ke `requirements.txt`:
   ```
   psycopg2-binary
   dj-database-url
   ```

   b. Install dependensi:
   ```
   pip install -r requirements.txt
   ```

   c. Buka file `settings.py` di proyek Django Anda.

   d. Di bagian atas file, tambahkan import berikut:
   ```python
   import dj_database_url
   ```

   e. Cari bagian `DATABASES` di `settings.py`. Ganti konfigurasi yang ada dengan kode berikut:
   ```python
   DATABASES = {
       'default': dj_database_url.config(
           default='postgres://username:password@host:port/database_name',
           conn_max_age=600,
       )
   }
   ```

   f. Ganti 'postgres://username:password@host:port/database_name' dengan URL database PostgreSQL yang Anda dapatkan dari Vercel. URL akan terlihat seperti ini:
   `postgres://default:abcdefghijklm@ep-cool-darkness-123456.us-east-1.postgres.vercel-storage.com:5432/verceldb` (hanya contoh)

3. Jalankan migrasi database:
   a. Buat migrasi jika ada perubahan model:
   ```
   python manage.py makemigrations
   ```
   b. Terapkan migrasi ke database:
   ```
   python manage.py migrate
   ```

4. Buat file `vercel.json` di root proyek:

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

5. Ubah file `wsgi.py`:

Tambahkan baris berikut di akhir file `wsgi.py`:

```python
app = application
```

6. Deploy proyek ke Vercel:
   a. Commit dan push semua perubahan ke repositori GitHub Anda
   b. Kembali ke halaman konfigurasi proyek di dashboard Vercel
   c. Klik "Deploy"

Vercel akan men-deploy proyek Anda dan memberikan URL untuk mengaksesnya.

Catatan:
- Pastikan untuk menambahkan domain Vercel Anda ke `ALLOWED_HOSTS` di `settings.py`.
- Untuk keamanan, sebaiknya gunakan variabel lingkungan di produksi untuk menyimpan URL database alih-alih menyimpannya langsung di `settings.py`.
- Jika Anda perlu menjalankan migrasi database setelah deployment, Anda dapat melakukannya melalui Vercel CLI atau menambahkan script build khusus di `vercel.json`.