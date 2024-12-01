# Setting Up Laravel Project with Herd and PHPStorm

## 1. Laravel Herd Installation

1. Download Laravel Herd:
   ```bash
   # Download from https://herd.laravel.com
   # Install for Apple Silicon Mac
   ```

2. First-time Herd setup:
   ```bash
   # Open Herd.app
   # Follow first-time setup wizard
   # Herd will configure PHP, MySQL, PostgreSQL, and Redis
   ```

## 2. Create New Laravel Project

1. Navigate to your projects directory:
   ```bash
   cd ~/Sites    # Default Herd directory
   ```

2. Create new Laravel project:
   ```bash
   composer create-project laravel/laravel shopping-cart
   cd shopping-cart
   ```

## 3. Configure PHPStorm

1. Open project in PHPStorm:
   ```bash
   # Option 1: Command line
   open -a "PHPStorm" .
   
   # Option 2: File > Open in PHPStorm
   # Navigate to ~/Sites/shopping-cart
   ```

2. Configure PHP interpreter:
   ```
   # In PHPStorm:
   1. Go to Preferences > PHP
   2. Click '...' next to CLI Interpreter
   3. Add > Local
   4. PHP Executable: /Users/your-username/Library/Application Support/Herd/bin/php
   ```

3. Configure Composer:
   ```
   # In PHPStorm:
   1. Go to Preferences > PHP > Composer
   2. Path to composer.json: select composer.json in project root
   3. Composer executable: /opt/homebrew/bin/composer
   ```

4. Configure PostgreSQL database:
   ```
   # In PHPStorm:
   1. Go to Database tool window (View > Tool Windows > Database)
   2. Click '+' > PostgreSQL
   3. Fill in credentials:
      Host: localhost
      Port: 5432
      Database: laravel_cart
      User: cart_user
      Password: your_password
   ```

## 4. Project Configuration

1. Configure .env file:
   ```env
   DB_CONNECTION=pgsql
   DB_HOST=127.0.0.1
   DB_PORT=5432
   DB_DATABASE=laravel_cart
   DB_USERNAME=cart_user
   DB_PASSWORD=your_password
   
   # Herd automatically configures SSL certificates
   APP_URL=https://shopping-cart.test
   ```

2. Add local domain to hosts:
   ```bash
   # Herd handles this automatically
   # Your site will be available at https://shopping-cart.test
   ```

## 5. PHPStorm Recommended Settings

1. Enable Laravel Plugin:
   ```
   1. Go to Preferences > Plugins
   2. Search for "Laravel"
   3. Install Laravel plugin
   4. Restart PHPStorm
   ```

2. Configure Laravel IDE Helper:
   ```bash
   # In terminal:
   composer require --dev barryvdh/laravel-ide-helper
   
   # Generate IDE helper files
   php artisan ide-helper:generate
   php artisan ide-helper:models
   php artisan ide-helper:meta
   ```

3. Enable Laravel Live Templates:
   ```
   1. Go to Preferences > Editor > Live Templates
   2. Enable Laravel templates
   ```

## 6. Version Control Setup

1. Initialize Git:
   ```bash
   git init
   git add .
   git commit -m "Initial commit"
   ```

2. Configure PHPStorm Git integration:
   ```
   1. Go to Preferences > Version Control > Git
   2. Confirm Git executable path
   3. Enable Git integration for project
   ```

## 7. Running the Project

1. Start development server:
   ```bash
   # Herd automatically handles this
   # Access your site at https://shopping-cart.test
   ```

2. Run migrations:
   ```bash
   php artisan migrate
   ```

## Troubleshooting

1. If PHP interpreter isn't found:
   ```
   # Verify Herd PHP path:
   ls -l ~/Library/Application\ Support/Herd/bin/php
   ```

2. If PostgreSQL connection fails:
   ```bash
   # Check Herd services status in menu bar
   # Restart PostgreSQL in Herd if needed
   ```

3. If PHPStorm indexing is slow:
   ```
   1. Go to Preferences > Directory-Based Settings
   2. Exclude vendor and node_modules directories
   ```
