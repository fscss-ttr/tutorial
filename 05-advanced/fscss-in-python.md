# FSCSS in Python

Use FSCSS in Python projects like Flask or Django.

## Setup

### 1. Install FSCSS

```bash
npm install -g fscss
```

### 2. Project Structure

```
my-python-app/
├── static/
│   ├── styles.fscss
│   └── css/
│       └── styles.css
├── templates/
│   └── index.html
└── app.py
```

## Flask Integration

### Basic Flask App

```python
# app.py
from flask import Flask, render_template
import subprocess

app = Flask(__name__)

def compile_fscss():
    """Compile FSCSS to CSS."""
    subprocess.run([
        'fscss',
        'static/styles.fscss',
        'static/css/styles.css'
    ])

@app.route('/')
def index():
    compile_fscss()
    return render_template('index.html')

if __name__ == '__main__':
    app.run(debug=True)
```

### Template

```html
<!-- templates/index.html -->
<!DOCTYPE html>
<html>
<head>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/styles.css') }}">
</head>
<body>
    <div class="container">
        <h1 class="title">Hello FSCSS</h1>
        <button class="button">Click Me</button>
    </div>
</body>
</html>
```

### FSCSS File

```fscss
/* static/styles.fscss */
$primary: #2563eb;
$spacing: 1rem;

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: $spacing;
}

.title {
    font-size: 2rem;
    font-weight: 700;
    color: $primary;
}

.button {
    background: $primary;
    color: white;
    padding: 12px 24px;
    border-radius: 8px;
    border: none;
    cursor: pointer;
}
```

## Django Integration

### Management Command

```python
# myapp/management/commands/compile_fscss.py
from django.core.management.base import BaseCommand
import subprocess
import os

class Command(BaseCommand):
    help = 'Compile FSCSS files to CSS'

    def handle(self, *args, **options):
        # Find all .fscss files
        for root, dirs, files in os.walk('static'):
            for file in files:
                if file.endswith('.fscss'):
                    input_file = os.path.join(root, file)
                    output_file = input_file.replace('.fscss', '.css')
                    
                    subprocess.run(['fscss', input_file, output_file])
                    self.stdout.write(
                        self.style.SUCCESS(f'Compiled {input_file}')
                    )
```

### Run Command

```bash
python manage.py compile_fscss
```

### Settings

```python
# settings.py
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, 'static'),
]

# Add to INSTALLED_APPS
INSTALLED_APPS = [
    # ...
    'myapp',
]
```

## Auto-Compilation

### Watch Mode

```python
# watch_fscss.py
import subprocess
import time

def watch():
    """Watch FSCSS files and recompile on changes."""
    subprocess.run([
        'fscss',
        '--watch',
        'static/styles.fscss',
        'static/css/styles.css'
    ])

if __name__ == '__main__':
    watch()
```

### Git Hook

```bash
# .git/hooks/pre-commit
#!/bin/bash
fscss static/styles.fscss static/css/styles.css
git add static/css/styles.css
```

## Best Practices

### 1. Compile Before Deploy

```bash
# In your deployment script
fscss static/styles.fscss static/css/styles.css
```

### 2. Use Virtual Environment

```bash
python -m venv venv
source venv/bin/activate
pip install flask
npm install fscss
```

### 3. Organize Styles

```
static/
├── styles.fscss
├── components/
│   ├── button.fscss
│   └── card.fscss
└── css/
    └── styles.css
```

## Exercises

1. Set up FSCSS in a Flask app
2. Create a Django management command
3. Implement auto-compilation

---

**Next: [FSCSS in PHP](./fscss-in-php.md)**
