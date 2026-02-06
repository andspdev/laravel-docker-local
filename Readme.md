# Docker Installation (Local Development)

This document explains how to install and use **Docker** for **local development** purposes. The Docker configuration is designed to run several supporting services such as **Nginx, PHP 8.3, MySQL, phpMyAdmin, Redis, and Mailpit**, complete with CPU and memory limits to optimize system resource usage.

## Services & Resource Limits

* **Nginx** — CPU: 0.25 | Memory: 64 MB
* **Backend (PHP 8.3)** — CPU: 1 | Memory: 512 MB
* **MySQL8** — CPU: 1 | Memory: 768 MB
* **phpMyAdmin** — CPU: 0.25 | Memory: 128 MB
* **Redis** — CPU: 0.25 | Memory: 128 MB
* **Mailpit** — CPU: 0.25 | Memory: 128 MB

---

## Project Folder Structure

```text
- project-name
  ├─ docker
  ├─ src
  └─ docker-compose.yml
```

---

## Installation Steps

1. **Clone the Docker configuration repository**.

2. **Rename the folder** according to your project name (e.g. `project-name`).

3. **Remove the existing `src` directory** (if any):

   ```bash
   rm -rf src
   ```

4. **Clone your Laravel project** into this directory:

   ```bash
   git clone git@bitbucket.org:xxxxxxxx/project-laravel.git
   ```

5. **Rename the cloned project directory to `src`**:

   ```bash
   mv project-laravel src
   ```

6. **Build the Docker images** (make sure you are inside the `project-name` directory and `docker-compose.yml` exists):

   ```bash
   docker-compose build
   ```

   Wait until the build process is completed.

7. **Start the Docker containers**:

   ```bash
   docker-compose up -d
   ```

8. **Enter the backend container (shell mode)**:

   ```bash
   docker-compose exec app sh
   ```

9. **Install Laravel dependencies using Composer**:

   ```bash
   composer install
   ```

10. **Copy the environment file**:

    ```bash
    cp .env.example .env
    ```

11. **Configure the database settings in the `.env` file**:

    ```env
    DB_CONNECTION=mysql
    DB_HOST=db
    DB_PORT=3306
    DB_DATABASE=user_db
    DB_USERNAME=user
    DB_PASSWORD=user
    ```

    > Note: `DB_HOST=db` must remain unchanged as it follows the service name defined in `docker-compose.yml`.

12. **Generate the application key**:

    ```bash
    php artisan key:generate
    ```

13. **Create a symbolic link from storage to public**:

    ```bash
    php artisan storage:link
    ```

14. **Run database migrations**:

    ```bash
    php artisan migrate --force
    ```

15. **(Optional) Run database seeders**:

    ```bash
    php artisan db:seed
    ```

16. **Exit the container**:

    ```bash
    exit
    ```

---

## Check Docker Status

To verify that all containers are running properly:

```bash
docker stats
```

Press `CTRL + C` to exit.

Or use:

```bash
docker-compose ps
```

---

## Access Services in the Browser

### Laravel Project

* [http://localhost:8000](http://localhost:8000)

### phpMyAdmin

* URL: [http://localhost:8001](http://localhost:8001)
* DB User: `user`
* DB Password: `user`

### MySQL 8

* Port: `3306`
* Root password (default): `root`

### Redis

* Port: `6379`

### Mailpit

* Web UI: [http://localhost:8025](http://localhost:8025)
* SMTP Port: `1025`

---

## Docker User Permission Notes

This Docker setup uses **UID 1000**, which is the default user ID on most Linux systems.

If your operating system user **does not use UID 1000**, please adjust the user configuration in several files inside the `docker` directory accordingly.

To check the UID on your system:

```bash
id
```

Or for a specific user:

```bash
id username
```
