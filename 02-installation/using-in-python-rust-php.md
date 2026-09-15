# Using FSCSS in Python, Rust, and PHP Projects

FSCSS is JavaScript-based, but you can use it in any project. Here's how.

## The Core Concept

FSCSS compiles `.fscss` files to `.css` files. Any language that can:

1. Run JavaScript (Node.js)
2. Execute shell commands
3. Include CSS files

...can use FSCSS.

## Python Projects

### Method 1: CLI via Subprocess

Use Python's `subprocess` module to run FSCSS:

```python
import subprocess

def compile_fscss(input_file, output_file):
    """Compile an FSCSS file to CSS."""
    result = subprocess.run(
        ['fscss', input_file, output_file],
        capture_output=True,
        text=True
    )
    if result.returncode != 0:
        raise Exception(f"FSCSS error: {result.stderr}")
    return result.stdout

# Usage
compile_fscss('src/styles.fscss', 'dist/styles.css')
```

### Method 2: Flask Integration

For Flask projects, compile FSCSS when the app starts:

```python
from flask import Flask
import subprocess

app = Flask(__name__)

def compile_assets():
    """Compile all FSCSS files."""
    styles = [
        ('src/styles.fscss', 'static/css/styles.css'),
        ('src/components.fscss', 'static/css/components.css'),
    ]
    for input_file, output_file in styles:
        subprocess.run(['fscss', input_file, output_file])

# Compile on startup
compile_assets()

@app.route('/')
def index():
    return render_template('index.html')
```

### Method 3: Django Management Command

Create a custom management command:

```python
# myapp/management/commands/compile_fscss.py
from django.core.management.base import BaseCommand
import subprocess

class Command(BaseCommand):
    help = 'Compile FSCSS files to CSS'

    def handle(self, *args, **options):
        subprocess.run(['fscss', 'src/styles.fscss', 'static/css/styles.css'])
        self.stdout.write(self.style.SUCCESS('FSCSS compiled successfully'))
```

Run with:

```bash
python manage.py compile_fscss
```

## Rust Projects

### Method 1: Build Script

Create a `build.rs` file in your project:

```rust
use std::process::Command;

fn main() {
    // Compile FSCSS files during build
    Command::new("fscss")
        .args(&["src/styles.fscss", "dist/styles.css"])
        .output()
        .expect("Failed to compile FSCSS");
}
```

### Method 2: CLI Wrapper

Create a Rust wrapper for FSCSS commands:

```rust
use std::process::Command;
use std::path::Path;

pub fn compile_fscss(input: &Path, output: &Path) -> Result<(), String> {
    let result = Command::new("fscss")
        .arg(input.to_str().unwrap())
        .arg(output.to_str().unwrap())
        .output()
        .map_err(|e| format!("Failed to execute FSCSS: {}", e))?;

    if !result.status.success() {
        let stderr = String::from_utf8_lossy(&result.stderr);
        return Err(format!("FSCSS compilation failed: {}", stderr));
    }

    Ok(())
}

fn main() {
    let input = Path::new("src/styles.fscss");
    let output = Path::new("dist/styles.css");

    match compile_fscss(input, output) {
        Ok(_) => println!("FSCSS compiled successfully"),
        Err(e) => eprintln!("Error: {}", e),
    }
}
```

### Method 3: Web Server Integration

For Actix-web or other Rust web frameworks:

```rust
use actix_web::{web, App, HttpServer, HttpResponse};
use std::process::Command;

async fn serve_css() -> HttpResponse {
    // Compile FSCSS on first request or cache
    Command::new("fscss")
        .args(&["src/styles.fscss", "dist/styles.css"])
        .output()
        .expect("Failed to compile FSCSS");

    let css = std::fs::read_to_string("dist/styles.css")
        .unwrap_or_else(|_| "/* CSS not found */".to_string());

    HttpResponse::Ok()
        .content_type("text/css")
        .body(css)
}
```

## PHP Projects

### Method 1: Composer Script

Add to your `composer.json`:

```json
{
  "scripts": {
    "compile:fscss": "fscss src/styles.fscss dist/styles.css",
    "compile:all": ["@compile:fscss"]
  }
}
```

Run with:

```bash
composer compile:fscss
```

### Method 2: Laravel Integration

Create an artisan command:

```php
// app/Console/Commands/CompileFscss.php
<?php

namespace App\Console\Commands;

use Illuminate\Console\Command;

class CompileFscss extends Command
{
    protected $signature = 'fscss:compile';
    protected $description = 'Compile FSCSS files to CSS';

    public function handle()
    {
        $input = resource_path('scss/styles.fscss');
        $output = public_path('css/styles.css');

        exec("fscss {$input} {$output}", $output, $returnCode);

        if ($returnCode === 0) {
            $this->info('FSCSS compiled successfully');
        } else {
            $this->error('FSCSS compilation failed');
        }
    }
}
```

### Method 3: WordPress Plugin

For WordPress themes, add to your `functions.php`:

```php
function compile_fscss() {
    $input = get_template_directory() . '/src/styles.fscss';
    $output = get_template_directory() . '/dist/styles.css';

    exec("fscss {$input} {$output}");
}
add_action('init', 'compile_fscss');
```

## General Pattern

Regardless of your language, the pattern is the same:

1. **Write** FSCSS files in your source directory
2. **Compile** using the FSCSS CLI
3. **Include** the compiled CSS in your application

## Automation

### GitHub Actions

```yaml
name: Build CSS
on: push

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      - run: npm install -g fscss
      - run: fscss src/styles.fscss dist/styles.css
      - uses: actions/upload-artifact@v3
        with:
          name: css
          path: dist/
```

### Makefile

```makefile
.PHONY: css

css:
	fscss src/styles.fscss dist/styles.css

watch:
	fscss --watch src/styles.fscss dist/styles.css
```

## Summary

FSCSS works with any language through its CLI. Use subprocess calls, build scripts, or package manager scripts to integrate FSCSS into Python, Rust, PHP, or any other project. The key is that FSCSS compiles files — any language can trigger that compilation.

---

**Next: [Back to Tutorial Home](../README.md) | Start [Beginner](../03-beginner/README.md)**
