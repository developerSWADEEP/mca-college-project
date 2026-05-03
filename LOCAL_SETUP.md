# Acon CMS Local Setup (Ubuntu/Linux)

This project is a CodeIgniter (PHP + MySQL) app.

## 1) Install required packages

```bash
sudo apt update
sudo apt install -y \
  php8.2 php8.2-cli php8.2-mysql php8.2-curl php8.2-gd php8.2-mbstring php8.2-xml \
  mysql-server
```

If `php8.2` is not available on your distro, install `php8.3` equivalents.

## 2) Start/enable MySQL

```bash
sudo systemctl enable --now mysql
sudo systemctl status mysql
```

## 3) Create DB + user

Run MySQL shell:

```bash
sudo mysql
```

Then execute:

```sql
CREATE DATABASE acon CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'acon_user'@'localhost' IDENTIFIED BY 'acon_pass_123';
GRANT ALL PRIVILEGES ON acon.* TO 'acon_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

## 4) Import project database

From this repository root:

```bash
mysql -u acon_user -p acon < database/acon.sql
```

## 5) Configure app DB credentials

Edit `cms/application/config/database.php` and set:

```php
'hostname' => 'localhost',
'username' => 'acon_user',
'password' => 'acon_pass_123',
'database' => 'acon',
```

## 6) Ensure writable permissions for uploads/logs/cache

```bash
chmod -R u+rwX cms/public/uploads cms/application/logs cms/application/cache
```

## 7) Run locally with PHP built-in server

```bash
php -S 127.0.0.1:8080 -t cms
```

Open:

- Frontend: `http://127.0.0.1:8080/`
- Admin: `http://127.0.0.1:8080/admin`

Default admin credentials (from project docs):

- Email: `admin@gmail.com`
- Password: `1234`

## 8) Quick checks if something fails

- PHP modules: `php -m | rg "mysqli|curl|gd|mbstring|pdo"`
- DB connection error: re-check `cms/application/config/database.php`
- Blank page: check logs in `cms/application/logs/`
