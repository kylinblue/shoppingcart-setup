# Setting Up a Laravel Shopping Cart Environment on Apple Silicon Mac

## Prerequisites Installation

1. Install Homebrew:
   ```bash
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   
   # Add Homebrew to PATH for Apple Silicon
   echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zshrc
   source ~/.zshrc
   ```

2. Install PHP and extensions:
   ```bash
   brew install php@8.2
   
   # Install PHP extensions
   brew install postgresql
   
   # Link PHP
   brew link php@8.2
   ```

3. Install Composer:
   ```bash
   brew install composer
   ```

4. Install PostgreSQL:
   ```bash
   brew install postgresql@14
   
   # Start PostgreSQL service
   brew services start postgresql@14
   ```

## PostgreSQL Setup

1. Create database and user:
   ```bash
   psql postgres
   
   CREATE DATABASE laravel_cart;
   CREATE USER cart_user WITH ENCRYPTED PASSWORD 'your_password';
   GRANT ALL PRIVILEGES ON DATABASE laravel_cart TO cart_user;
   ```

## Laravel Project Setup

1. Create new Laravel project:
   ```bash
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
   ```bash
   composer require laravel/sanctum    # For API authentication
   composer require darryldecode/cart  # Shopping cart package
   ```

## Database Structure

1. Create migrations:
   ```bash
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
   ```bash
   php artisan migrate
   ```

## Basic Models

1. Create models:
   ```bash
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
```bash
php artisan serve
```

## Apple Silicon-Specific Configuration

1. PHP Configuration:
   ```bash
   # Edit PHP configuration
   php --ini  # Find your php.ini location
   
   # Recommended settings for M1/M2 Macs:
   memory_limit = 512M
   max_execution_time = 120
   upload_max_filesize = 64M
   post_max_size = 64M
   ```

2. PostgreSQL Configuration:
   ```bash
   # Edit postgresql.conf (location may vary based on installation):
   # /opt/homebrew/var/postgresql@14/postgresql.conf
   
   # Optimize for M1/M2 performance:
   shared_buffers = 512MB
   effective_cache_size = 1536MB
   maintenance_work_mem = 128MB
   ```

## Performance Optimization for Apple Silicon

1. Configure Laravel for M1/M2:
   ```php
   // config/cache.php
   'default' => env('CACHE_DRIVER', 'redis'),
   
   // Install Redis for better performance
   brew install redis
   brew services start redis
   composer require predis/predis
   ```

2. Configure Vite for faster asset compilation:
   ```javascript
   // vite.config.js
   export default defineConfig({
       plugins: [
           laravel({
               input: ['resources/css/app.css', 'resources/js/app.js'],
               refresh: true,
           }),
       ],
       server: {
           hmr: {
               host: 'localhost',
           },
       },
   });
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
   ```bash
   php artisan make:auth
   php artisan migrate
   ```

## Troubleshooting Apple Silicon Issues

1. If Composer shows memory errors:
   ```bash
   export COMPOSER_MEMORY_LIMIT=-1
   ```

2. If PostgreSQL fails to start:
   ```bash
   brew services restart postgresql@14
   ```

3. Check architecture-specific binaries:
   ```bash
   # Verify running native ARM64 versions
   arch           # Should output 'arm64'
   which php     # Should point to /opt/homebrew/bin/php
   ```
