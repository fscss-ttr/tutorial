# FSCSS in PHP

Use FSCSS in PHP projects like Laravel, WordPress, or custom PHP apps.

## Setup

### 1. Install FSCSS

```bash
npm install -g fscss
```

### 2. Project Structure

```
my-php-app/
├── public/
│   └── css/
│       └── styles.css
├── src/
│   └── styles.fscss
└── index.php
```

## Basic PHP Integration

### Compile on Page Load

```php
<?php
// compile.php
function compile_fscss($input, $output) {
    $command = "fscss " . escapeshellarg($input) . " " . escapeshellarg($output);
    exec($command, $output, $returnCode);
    return $returnCode === 0;
}

// Compile if CSS is older than FSCSS
$input = 'src/styles.fscss';
$output = 'public/css/styles.css';

if (!file_exists($output) || filemtime($input) > filemtime($output)) {
    compile_fscss($input, $output);
}
?>
```

### Use in HTML

```php
<?php require_once 'compile.php'; ?>
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="css/styles.css">
</head>
<body>
    <div class="container">
        <h1 class="title">Hello FSCSS</h1>
    </div>
</body>
</html>
```

## Laravel Integration

### Artisan Command

```php
// app/Console/Commands/CompileFscss.php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;
use Symfony\Component\Process\Process;

class CompileFscss extends Command
{
    protected $signature = 'fscss:compile';
    protected $description = 'Compile FSCSS files to CSS';

    public function handle()
    {
        $files = [
            'resources/styles/app.fscss' => 'public/css/app.css',
            'resources/styles/admin.fscss' => 'public/css/admin.css',
        ];

        foreach ($files as $input => $output) {
            if (file_exists($input)) {
                $process = new Process(['fscss', $input, $output]);
                $process->run();

                if ($process->isSuccessful()) {
                    $this->info("Compiled: {$input} → {$output}");
                } else {
                    $this->error("Failed: {$input}");
                }
            }
        }

        $this->info('FSCSS compilation complete');
    }
}
```

### Register Command

```php
// app/Console/Kernel.php
protected $commands = [
    \App\Console\Commands\CompileFscss::class,
];
```

### Run Command

```bash
php artisan fscss:compile
```

### Auto-Compile on Save

```php
// app/Providers/AppServiceProvider.php
public function boot()
{
    if ($this->app->environment('local')) {
        // Watch for FSCSS changes in development
        exec('fscss --watch resources/styles/app.fscss public/css/app.css &');
    }
}
```

## WordPress Integration

### Functions.php

```php
// functions.php
function compile_fscss() {
    $input = get_template_directory() . '/src/styles.fscss';
    $output = get_template_directory() . '/dist/css/styles.css';
    
    // Ensure output directory exists
    $dir = dirname($output);
    if (!file_exists($dir)) {
        mkdir($dir, 0755, true);
    }
    
    // Compile if needed
    if (!file_exists($output) || filemtime($input) > filemtime($output)) {
        exec("fscss " . escapeshellarg($input) . " " . escapeshellarg($output));
    }
}

// Compile on theme activation
add_action('after_switch_theme', 'compile_fscss');

// Enqueue styles
function theme_styles() {
    wp_enqueue_style(
        'theme-style',
        get_template_directory_uri() . '/dist/css/styles.css'
    );
}
add_action('wp_enqueue_scripts', 'theme_styles');
```

## Composer Script

### Add to composer.json

```json
{
    "scripts": {
        "fscss:compile": "fscss src/styles.fscss public/css/styles.css",
        "fscss:watch": "fscss --watch src/styles.fscss public/css/styles.css",
        "post-install-cmd": ["@fscss:compile"]
    }
}
```

### Run with Composer

```bash
composer fscss:compile
```

## Best Practices

### 1. Compile in Development

```php
// Only compile in local environment
if (env('APP_ENV') === 'local') {
    exec('fscss --watch src/styles.fscss public/css/styles.css');
}
```

### 2. Cache in Production

Pre-compile all CSS before deploying.

### 3. Organize Files

```
src/
├── base/
│   ├── variables.fscss
│   └── reset.fscss
├── components/
│   ├── button.fscss
│   └── card.fscss
└── app.fscss
```

## Exercises

1. Set up FSCSS in a PHP project
2. Create a Laravel artisan command
3. Implement WordPress theme integration

---

**Next: [Theming Systems](./theming-systems.md)**
