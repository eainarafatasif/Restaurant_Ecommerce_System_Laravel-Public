![Screenshot (262)](https://user-images.githubusercontent.com/48250220/191907727-6f3155f9-53c1-4f73-a639-15a3df620bb4.png)
![Screenshot (263)](https://user-images.githubusercontent.com/48250220/191907740-b59b4c10-6550-4b9e-bc35-483c633667f6.png)
![Screenshot (264)](https://user-images.githubusercontent.com/48250220/191907761-222d06bb-eca3-42cb-939a-c6da2cb6996e.png)
![Screenshot (265)](https://user-images.githubusercontent.com/48250220/191907811-3e036e5e-b895-4e58-83b6-795c59cad218.png)
![Screenshot (267)](https://user-images.githubusercontent.com/48250220/191907822-03fd61b9-a1fe-47fe-afd5-ac39249ffb1c.png)
![Screenshot (278)](https://user-images.githubusercontent.com/48250220/191907863-6a95633c-5b34-4e41-b6d7-d7aff6b09e55.png)
![Screenshot (269)](https://user-images.githubusercontent.com/48250220/191907922-66f4cf96-4535-4515-8b2b-1586fd8e76e0.png)
![Screenshot (270)](https://user-images.githubusercontent.com/48250220/191907930-483ef487-60d9-49c5-8b37-45c0a50900ce.png)
![Screenshot (271)](https://user-images.githubusercontent.com/48250220/191907947-9fdc1c0c-2d91-488c-bd4c-4d3cc90aea12.png)
![Screenshot (268)](https://user-images.githubusercontent.com/48250220/191907983-e143e2db-0ff1-446d-8b13-f9dff91e21b3.png)
![Screenshot (279)](https://user-images.githubusercontent.com/48250220/191908016-cfa165cb-36a7-464b-bd21-0b982ca9d39a.png)
![Screenshot (280)](https://user-images.githubusercontent.com/48250220/191908031-b358d407-a926-4463-987a-52aee91cb7dc.png)
![Screenshot (281)](https://user-images.githubusercontent.com/48250220/191908046-56d867fc-2553-457d-915c-09f773faec2e.png)
![Screenshot (272)](https://user-images.githubusercontent.com/48250220/191908062-7fd42d8a-780e-44a8-abff-bb0402d4192d.png)
![Screenshot (273)](https://user-images.githubusercontent.com/48250220/191908073-611afa48-98ef-4870-a8e3-631557bb3c84.png)
![Screenshot (274)](https://user-images.githubusercontent.com/48250220/191908089-c736bd30-a9a5-4346-8c2a-88017819cbb0.png)
![Screenshot (275)](https://user-images.githubusercontent.com/48250220/191908111-5aebdf44-88e3-4d28-bda0-3d573afbfe1e.png)
![Screenshot (276)](https://user-images.githubusercontent.com/48250220/191908135-0130d841-0d83-4d94-ae5f-a3f01a5b7257.png)
![Screenshot (277)](https://user-images.githubusercontent.com/48250220/191908145-31df506c-13e8-427f-8787-5162100229b1.png)


To deploy your Laravel project using Nginx, MySQL, and an SSL certificate on an Ubuntu server, and then upload a README file to your GitHub repository, follow these steps:

### 1. **Set Up Your Server**
- **Update your server:**
  ```bash
  sudo apt update && sudo apt upgrade -y
  ```

- **Install necessary packages:**
  ```bash
  sudo apt install nginx mysql-server php-fpm php-mysql git -y
  ```

- **Secure MySQL:**
  ```bash
  sudo mysql_secure_installation
  ```
  Follow the prompts to set a root password, remove anonymous users, disallow root login remotely, and remove the test database.

### 2. **Set Up MySQL Database**
- **Login to MySQL:**
  ```bash
  sudo mysql -u root -p
  ```
  
- **Create a database and user for your Laravel project:**
  ```sql
  CREATE DATABASE laravel_db;
  CREATE USER 'laravel_user'@'localhost' IDENTIFIED BY 'password';
  GRANT ALL PRIVILEGES ON laravel_db.* TO 'laravel_user'@'localhost';
  FLUSH PRIVILEGES;
  EXIT;
  ```

### 3. **Set Up Laravel**
- **Clone your Laravel project:**
  ```bash
  cd /var/www/html
  sudo git clone https://github.com/yourusername/your-laravel-project.git
  cd your-laravel-project
  ```

- **Install Composer dependencies:**
  ```bash
  sudo apt install composer -y
  composer install
  ```

- **Set up environment file:**
  ```bash
  cp .env.example .env
  nano .env
  ```
  Update the `.env` file with your database and other configuration details:
  ```env
  DB_DATABASE=laravel_db
  DB_USERNAME=laravel_user
  DB_PASSWORD=password
  ```

- **Generate an application key:**
  ```bash
  php artisan key:generate
  ```

### 4. **Set Up Nginx**
- **Create a new Nginx configuration file:**
  ```bash
  sudo nano /etc/nginx/sites-available/laravel
  ```

- **Add the following configuration:**
  ```nginx
  server {
      listen 80;
      server_name your_domain_or_IP;
      root /var/www/html/your-laravel-project/public;

      index index.php index.html index.htm index.nginx-debian.html;

      location / {
          try_files $uri $uri/ /index.php?$query_string;
      }

      location ~ \.php$ {
          include snippets/fastcgi-php.conf;
          fastcgi_pass unix:/var/run/php/php7.4-fpm.sock;
          fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
          include fastcgi_params;
      }

      location ~ /\.ht {
          deny all;
      }
  }
  ```

- **Enable the configuration and restart Nginx:**
  ```bash
  sudo ln -s /etc/nginx/sites-available/laravel /etc/nginx/sites-enabled/
  sudo nginx -t
  sudo systemctl restart nginx
  ```

### 5. **Set Up SSL with Certbot**
- **Install Certbot and the Nginx plugin:**
  ```bash
  sudo apt install certbot python3-certbot-nginx -y
  ```

- **Obtain and install the SSL certificate:**
  ```bash
  sudo certbot --nginx -d your_domain_or_IP
  ```

- **Follow the prompts to configure the SSL certificate.**

### 6. **Set Up Permissions**
- **Give Nginx ownership of the Laravel directory:**
  ```bash
  sudo chown -R www-data:www-data /var/www/html/your-laravel-project
  sudo chmod -R 755 /var/www/html/your-laravel-project
  ```

### 7. **Finalize the Laravel Setup**
- **Run migrations:**
  ```bash
  php artisan migrate
  ```

- **Optimize Laravel:**
  ```bash
  php artisan optimize
  ```


