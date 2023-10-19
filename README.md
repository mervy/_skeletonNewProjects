# Skeleton for new projects PHP

## Directories

```html
   .env
   README.md
+---public
       index.php
+---src    
    +---controllers
           .keep    
    +---library
           .keep    
    +---models
           .keep    
    +---views
           template.php       
        +---pages
                home.php       
        +---partials
                footer.php
                header.php
                navbar.php
```

## Examples .htaccess

- Example 01

Inside in root directory of project

```apache
# Enable friendly URLs
RewriteEngine On

# Redirect requests to the public folder
RewriteRule ^(.*)$ public/$1 [L] 

# Disable directory listing
Options -Indexes

# Block access to the .env file
<Files .env>
    Order allow,deny
    Deny from all
</Files>
```

- Example 02

Inside the file in public folder

```apache
# Redirect requests to the index.php file
RewriteEngine On    
# Check if the requested URL does not point to a directory.
RewriteCond %{REQUEST_FILENAME} !-d
# Check if the requested URL does not correspond to an existing file.
RewriteCond %{REQUEST_FILENAME} !-f
RewriteRule ^ index.php [L]
```

* - Exemplo de .env *

```env
# MySQL Database Connection Information
DB_HOST=localhost       # The database host (typically 'localhost' for the local server)
DB_NAME=database_name   # Database name
DB_USER=username        # Database user's name
DB_PASS=password        # Database user's password

# Other Configurations
DEBUG=true              # Set to 'true' to enable debugging messages, 'false' to disable
```

## PDO connection example

Install dep for .env files
```
composer require vlucas/phpdotenv
```

- Connection example
```php
<?php
    // Include the Composer autoloader to load the dotenv package
    require_once __DIR__ . '/vendor/autoload.php';

    use Mervy\Database\Connection; // Replace with the actual namespace of your database connection class
    use PDO;

    // Load environment variables from the .env file
    $dotenv = Dotenv\Dotenv::createImmutable(__DIR__);
    $dotenv->load();

    // Retrieve database connection information from environment variables
    $host = $_ENV['DB_HOST'];
    $dbname = $_ENV['DB_NAME'];
    $user = $_ENV['DB_USER'];
    $pass = $_ENV['DB_PASS'];

    // Create a PDO database connection
    try {
        $pdo = new PDO("mysql:host=$host;dbname=$dbname", $user, $pass);
        $pdo->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
        $pdo->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_ASSOC);
        
        // You can now use $pdo for database operations
        echo "Connected to the database successfully!";
    } catch (PDOException $e) {
        die("Error: " . $e->getMessage());
    }
?>
```
- Other example
```php
class Connection {
    private static $connection;

    private function __construct() {
        // Private constructor to prevent external instantiation
    }

    public static function getConnection() {
        if (!self::$connection) {
            $host = $_ENV['DB_HOST'];
            $dbname = $_ENV['DB_NAME'];
            $user = $_ENV['DB_USER'];
            $pass = $_ENV['DB_PASS'];

            try {
                self::$connection = new PDO("mysql:host=$host;dbname=$dbname", $user, $pass);
                self::$connection->setAttribute(PDO::ATTR_ERRMODE, PDO::ERRMODE_EXCEPTION);
                self::$connection->setAttribute(PDO::ATTR_DEFAULT_FETCH_MODE, PDO::FETCH_OBJ);
            } catch (PDOException $e) {
                die("Error: " . $e->getMessage());
            }
        }
        return self::$// Include the Composer autoloader to load the dotenv package
require_once __DIR__ . '/vendor/autoload.php';

use Dotenv\Dotenv;

// Load environment variables from the .env file
$dotenv = Dotenv::createImmutable(__DIR__);
$dotenv->load();;
    }
}

//Use:
// Include the Composer autoloader to load the dotenv package
require_once __DIR__ . '/vendor/autoload.php';

use Dotenv\Dotenv;

// Load environment variables from the .env file
$dotenv = Dotenv::createImmutable(__DIR__);
$dotenv->load();

// Get the PDO connection object
$pdo = Connection::getConnection();

// You can now use $pdo for database operations
echo "Connected to the database successfully!";

```

