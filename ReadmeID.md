# Instalasi Docker (Local Development)

Dokumen ini menjelaskan cara instalasi dan penggunaan **Docker** untuk kebutuhan **development lokal**. Konfigurasi Docker ini digunakan untuk menjalankan beberapa service pendukung seperti **Nginx, PHP 8.3, MySQL, phpMyAdmin, Redis, dan Mailpit**, lengkap dengan pembatasan resource (CPU & Memory) agar lebih hemat penggunaan sistem.

## Service & Resource Limit

* **Nginx** — CPU: 0.25 | Memory: 64 MB
* **Backend (PHP 8.3)** — CPU: 1 | Memory: 512 MB
* **MySQL8** — CPU: 1 | Memory: 768 MB
* **phpMyAdmin** — CPU: 0.25 | Memory: 128 MB
* **Redis** — CPU: 0.25 | Memory: 128 MB
* **Mailpit** — CPU: 0.25 | Memory: 128 MB

---

## Struktur Folder Project

```text
- project-saya
  ├─ docker
  ├─ src
  └─ docker-compose.yml
```

---

## Langkah Instalasi

1. **Clone repository konfigurasi Docker**.

2. **Ubah nama folder** sesuai dengan nama project yang diinginkan (contoh: `project-saya`).

3. **Hapus direktori `src`** (jika sudah ada):

   ```bash
   rm -rf src
   ```

4. **Clone project Laravel** ke dalam direktori tersebut:

   ```bash
   git clone git@bitbucket.org:xxxxxxxx/project-laravel.git
   ```

5. **Ubah nama folder hasil clone menjadi `src`**:

   ```bash
   mv project-laravel src
   ```

6. **Build Docker image** (pastikan berada di direktori `project-saya` dan terdapat `docker-compose.yml`):

   ```bash
   docker-compose build
   ```

   Tunggu hingga proses build selesai.

7. **Jalankan container Docker**:

   ```bash
   docker-compose up -d
   ```

8. **Masuk ke container backend (mode shell)**:

   ```bash
   docker-compose exec app sh
   ```

9. **Install dependency Laravel menggunakan Composer**:

   ```bash
   composer install
   ```

10. **Salin file environment**:

    ```bash
    cp .env.example .env
    ```

11. **Sesuaikan konfigurasi database pada file `.env`**:

    ```env
    DB_CONNECTION=mysql
    DB_HOST=db
    DB_PORT=3306
    DB_DATABASE=user_db
    DB_USERNAME=user
    DB_PASSWORD=user
    ```

    > Catatan: `DB_HOST=db` harus tetap sesuai dengan nama service di `docker-compose.yml`.

12. **Generate application key**:

    ```bash
    php artisan key:generate
    ```

13. **Buat symbolic link storage ke public**:

    ```bash
    php artisan storage:link
    ```

14. **Jalankan migrasi database**:

    ```bash
    php artisan migrate --force
    ```

15. **(Opsional) Jalankan seeder**:

    ```bash
    php artisan db:seed
    ```

16. **Keluar dari container**:

    ```bash
    exit
    ```

---

## Cek Status Docker

Untuk memastikan semua container berjalan dengan baik:

```bash
docker stats
```

Tekan `CTRL + C` untuk keluar.

Atau gunakan:

```bash
docker-compose ps
```

---

## Akses Service di Browser

### Laravel Project

* [http://localhost:8000](http://localhost:8000)

### phpMyAdmin

* URL: [http://localhost:8001](http://localhost:8001)
* DB User: `user`
* DB Password: `user`

### MySQL 8

* Port: `3306`
* Password root (default): `root`

### Redis

* Port: `6379`

### Mailpit

* Web UI: `http://localhost:8025`
* SMTP Port: `1025`

---

## Catatan Permission Docker User

Konfigurasi Docker menggunakan **UID 1000** (default user pada kebanyakan sistem Linux).

Jika user pada sistem operasi kamu **tidak menggunakan UID 1000**, silakan sesuaikan konfigurasi user di beberapa file dalam direktori `docker`.

Untuk mengecek UID user pada sistem operasi:

```bash
id
```

Atau untuk user tertentu:

```bash
id username
```
