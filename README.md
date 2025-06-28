# Car Rental Portal on Apache, MySQL, and Ubuntu 20.04

A PHP-based car rental web application deployed in a real-world environment using Apache web server, MySQL database, and Ubuntu 20.04. This project allows users to browse and book vehicles, and includes an admin panel for managing bookings, vehicles, and users.

## Features
- User-friendly interface for browsing and booking cars.
- Admin panel for managing vehicles, brands, and bookings.
- Database-driven with MySQL for storing user data, bookings, and vehicle details.
- Supports multiple car brands and vehicle types.

## Technologies
- **Frontend**: HTML, CSS, JavaScript
- **Backend**: PHP
- **Database**: MySQL
- **Server**: Apache
- **Operating System**: Ubuntu 20.04

## Setup Instructions
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/car-rental-portal.git
   ```
2. **Move the Project to Apache’s Web Root**:
   Move the `carrental` folder to `/var/www/html`:
   ```bash
   sudo mv car-rental-portal/carrental /var/www/html/
   ```
3. **Set Permissions**:
   Ensure Apache can access the files:
   ```bash
   sudo chown -R www-data:www-data /var/www/html/carrental
   sudo chmod -R 755 /var/www/html/carrental
   ```
4. **Configure the Database**:
   - Create a MySQL database named `carrental`:
     ```bash
     sudo mysql -u root
     ```
     ```sql
     CREATE DATABASE carrental;
     EXIT;
     ```
   - Import the database schema (you’ll need to obtain `carrental.sql` separately, as it’s excluded via `.gitignore`):
     ```bash
     mysql -u root -p carrental < path/to/carrental.sql
     ```
5. **Configure Database Connection**:
   - Copy the sample configuration file:
     ```bash
     cp /var/www/html/carrental/includes/config.sample.php /var/www/html/carrental/includes/config.php
     ```
   - Edit `config.php` to add your MySQL credentials:
     ```php
     define('DB_USER', 'your_username');
     define('DB_PASS', 'your_password');
     ```
6. **Access the Application**:
   - User interface: `http://localhost/carrental`
     - Username: `test@gmail.com`
     - Password: `Test@123`
   - Admin panel: `http://localhost/carrental/admin`
     - Username: `admin`
     - Password: `Test@12345`

## Apache and MySQL Configuration
This section outlines the steps to configure Apache web server and MySQL database on Ubuntu 20.04 for running the Car Rental Portal in a real environment.

### Prerequisites
- Ubuntu 20.04
- Root or sudo privileges
- Internet connection for installing packages

### Apache Configuration
1. **Install Apache**:
   ```bash
   sudo apt update
   sudo apt install apache2
   ```
2. **Start and Enable Apache**:
   ```bash
   sudo systemctl start apache2
   sudo systemctl enable apache2
   ```
3. **Install PHP and Required Modules**:
   Install PHP 7.4 (default on Ubuntu 20.04) and necessary extensions:
   ```bash
   sudo apt install php libapache2-mod-php php-mysql
   ```
4. **Enable URL Rewriting** (for `.htaccess` support):
   ```bash
   sudo a2enmod rewrite
   ```
5. **Configure Apache Virtual Host**:
   Edit the default virtual host or create a new one:
   ```bash
   sudo nano /etc/apache2/sites-available/000-default.conf
   ```
   Ensure the following configuration:
   ```apache
   <VirtualHost *:80>
       ServerName localhost
       DocumentRoot /var/www/html/carrental
       <Directory /var/www/html/carrental>
           Options Indexes FollowSymLinks
           AllowOverride All
           Require all granted
       </Directory>
       ErrorLog ${APACHE_LOG_DIR}/carrental_error.log
       CustomLog ${APACHE_LOG_DIR}/carrental_access.log combined
   </VirtualHost>
   ```
   Save and enable the configuration:
   ```bash
   sudo systemctl restart apache2
   ```
6. **Test Apache**:
   Access `http://localhost` in your browser to verify Apache is running.

### MySQL Configuration
1. **Install MySQL**:
   ```bash
   sudo apt install mysql-server
   ```
2. **Secure MySQL Installation** (optional, recommended for production):
   ```bash
   sudo mysql_secure_installation
   ```
   Follow prompts to set a root password and secure the setup.
3. **Create a Dedicated MySQL User** (recommended over using `root`):
   Log into MySQL:
   ```bash
   sudo mysql -u root
   ```
   Create a user and grant privileges:
   ```sql
   CREATE USER 'carrental_user'@"localhost" IDENTIFIED BY 'secure_password';
   GRANT ALL PRIVILEGES ON carrental.* TO 'carrental_user'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```
   Replace `secure_password` with a strong password.
4. **Test Database Connection**:
   ```bash
   mysql -u carrental_user -p -e "SELECT 1 FROM carrental.admin"
   ```
5. **Import Database Schema**:
   Ensure the `carrental` database is created and populated:
   ```bash
   mysql -u carrental_user -p carrental < path/to/carrental.sql
   ```

### Troubleshooting
- **Database Connection Errors**:
  Check `/var/www/html/carrental/includes/config.php` credentials and ensure the MySQL user has access:
  ```sql
  SHOW GRANTS FOR 'carrental_user'@'localhost';
  ```
- **Apache Errors**:
  View logs for issues:
  ```bash
  sudo tail -f /var/log/apache2/error.log
  ```
- **PHP Issues**:
  Ensure PHP modules are enabled:
  ```bash
  sudo a2enmod php7.4
  sudo systemctl restart apache2
  ```

## Prerequisites
- Apache 2.4+
- PHP 7.4+
- MySQL/MariaDB
- phpMyAdmin (optional for database management)

## License
[MIT License](LICENSE)

## Contact
Feel free to reach out via [GitHub Issues](https://github.com/yourusername/car-rental-portal/issues).
