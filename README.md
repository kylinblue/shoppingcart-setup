# Setting Up a Laravel Shopping Cart Environment on Windows

## Prerequisites Installation

1. Install PHP 8.2+:
   ```powershell
   # Download PHP from windows.php.net
   # Extract to C:\php
   # Add C:\php to Windows PATH environment variable
   
   # Enable PHP extensions in php.ini (rename php.ini-development):
   # extension=pdo_pgsql
   # extension=pgsql
   # extension=curl
   # extension=fileinfo
   # extension=mbstring
   # extension=openssl
   ```

2. Install Composer:
   ```powershell
   # Download and run Composer-Setup.exe from https://getcomposer.org/download/
   ```

3. Install PostgreSQL:
   ```powershell
   # Download and install PostgreSQL from https://www.postgresql.org/download/windows/
   # Remember to note down your superuser password during installation
   ```

## PostgreSQL Setup

1. Create database and user (using pgAdmin or psql):
   ```sql
   -- Open pgAdmin or psql command prompt
   CREATE DATABASE laravel_cart;
   CREATE USER cart_user WITH ENCRYPTED PASSWORD 'your_password';
   GRANT ALL PRIVILEGES ON DATABASE laravel_cart TO cart_user;
   ```

## Laravel Project Setup

1. Create new Laravel project:
   ```powershell
   composer create-project laravel/laravel shopping-cart
   cd shopping-cart
   ```

2. Configure environment (.env file):
   ```env
   DB_CONNECTION=pgsql
   DB_HOST=127.0.0.1
   DB_PORT=5432
   DB_DATABASE=laravel_cart
   DB_USERNAME=cart_user
   DB_PASSWORD=your_password
   ```

3. Install additional packages:
   ```powershell
   composer require laravel/sanctum    # For API authentication
   composer require darryldecode/cart  # Shopping cart package
   ```

## Database Structure

1. Create migrations:
   ```powershell
   php artisan make:migration create_products_table
   php artisan make:migration create_cart_items_table
   php artisan make:migration create_orders_table
   ```

2. Basic migration structure:
   ```php
   // database/migrations/xxxx_create_products_table.php
   public function up()
   {
       Schema::create('products', function (Blueprint $table) {
           $table->id();
           $table->string('name');
           $table->text('description');
           $table->decimal('price', 10, 2);
           $table->integer('stock');
           $table->timestamps();
       });
   }

   // database/migrations/xxxx_create_cart_items_table.php
   public function up()
   {
       Schema::create('cart_items', function (Blueprint $table) {
           $table->id();
           $table->foreignId('user_id')->constrained();
           $table->foreignId('product_id')->constrained();
           $table->integer('quantity');
           $table->timestamps();
       });
   }
   ```

3. Run migrations:
   ```powershell
   php artisan migrate
   ```

## Basic Models

1. Create models:
   ```powershell
   php artisan make:model Product
   php artisan make:model CartItem
   php artisan make:model Order
   ```

## Project Structure

Organize your code with these directories:
```
app/
├── Http/
│   ├── Controllers/
│   │   ├── CartController.php
│   │   ├── ProductController.php
│   │   └── OrderController.php
│   └── Resources/
│       └── ProductResource.php
├── Models/
│   ├── Product.php
│   ├── CartItem.php
│   └── Order.php
└── Services/
    └── CartService.php
```

## Development Server

Start the development server:
```powershell
php artisan serve
```

## Windows-Specific Configuration

1. Configure PHP in Windows:
   ```powershell
   # Create/modify php.ini file in C:\php
   # Set post_max_size = 64M
   # Set upload_max_filesize = 64M
   # Set memory_limit = 512M
   ```

2. Configure Windows Firewall (if needed):
   ```powershell
   # Allow PHP and PostgreSQL through Windows Firewall
   # Default ports:
   # - Laravel development server: 8000
   # - PostgreSQL: 5432
   ```

3. Configure PostgreSQL for Windows:
   ```
   # Edit postgresql.conf:
   listen_addresses = '*'
   
   # Edit pg_hba.conf:
   host    all    all    127.0.0.1/32    md5
   ```

## Troubleshooting Windows-Specific Issues

1. If composer shows SSL errors:
   ```powershell
   composer config -g -- disable-tls true
   composer config -g -- secure-http false
   ```

2. If artisan commands fail:
   ```powershell
   # Check PHP path
   where php
   
   # Check PHP version
   php -v
   
   # Verify PHP extensions
   php -m
   ```

## Additional Security Steps

1. Set up CORS (config/cors.php):
   ```php
   return [
       'paths' => ['api/*'],
       'allowed_methods' => ['*'],
       'allowed_origins' => ['http://localhost:3000'],
       'allowed_headers' => ['*'],
       'exposed_headers' => [],
       'max_age' => 0,
       'supports_credentials' => true,
   ];
   ```

2. Configure session and authentication:
   ```powershell
   php artisan make:auth
   php artisan migrate
   ```
