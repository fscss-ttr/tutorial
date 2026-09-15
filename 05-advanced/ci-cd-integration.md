# CI/CD Integration

Automate FSCSS compilation and deployment with CI/CD pipelines.

## GitHub Actions

### Basic Workflow

```yaml
# .github/workflows/build.yml
name: Build CSS

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install FSCSS
        run: npm install -g fscss
      
      - name: Compile FSCSS
        run: fscss src/styles.fscss dist/styles.css
      
      - name: Upload CSS
        uses: actions/upload-artifact@v3
        with:
          name: css
          path: dist/styles.css
```

### Deploy to GitHub Pages

```yaml
# .github/workflows/deploy.yml
name: Deploy

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      
      - uses: actions/setup-node@v3
        with:
          node-version: '18'
      
      - name: Install FSCSS
        run: npm install -g fscss
      
      - name: Compile FSCSS
        run: fscss src/styles.fscss dist/styles.css
      
      - name: Deploy to GitHub Pages
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./dist
```

## GitLab CI

### .gitlab-ci.yml

```yaml
stages:
  - build
  - test
  - deploy

build:
  stage: build
  image: node:18
  script:
    - npm install -g fscss
    - fscss src/styles.fscss dist/styles.css
  artifacts:
    paths:
      - dist/

test:
  stage: test
  image: node:18
  script:
    - npm install -g fscss
    - fscss src/styles.fscss dist/styles.css
    - npm test

deploy:
  stage: deploy
  image: alpine
  script:
    - echo "Deploying..."
  only:
    - main
```

## Netlify

### netlify.toml

```toml
[build]
  command = "npm install -g fscss && fscss src/styles.fscss dist/styles.css"
  publish = "dist"

[[headers]]
  for = "/*.css"
  [headers.values]
    Cache-Control = "public, max-age=31536000"
```

## Vercel

### vercel.json

```json
{
  "buildCommand": "npm install -g fscss && fscss src/styles.fscss dist/styles.css",
  "outputDirectory": "dist",
  "headers": [
    {
      "source": "/(.*)",
      "headers": [
        { "key": "Cache-Control", "value": "public, max-age=31536000" }
      ]
    }
  ]
}
```

## CircleCI

### .circleci/config.yml

```yaml
version: 2.1

jobs:
  build:
    docker:
      - image: cimg/node:18.0
    steps:
      - checkout
      - run:
          name: Install FSCSS
          command: npm install -g fscss
      - run:
          name: Compile FSCSS
          command: fscss src/styles.fscss dist/styles.css
      - store_artifacts:
          path: dist/
          destination: css

workflows:
  build-and-test:
    jobs:
      - build
```

## Best Practices

### 1. Cache Dependencies

```yaml
- uses: actions/cache@v3
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
```

### 2. Use Artifacts

```yaml
- uses: actions/upload-artifact@v3
  with:
    name: css
    path: dist/styles.css
```

### 3. Automate Testing

```yaml
- name: Test CSS
  run: npm test
```

## Exercises

1. Set up GitHub Actions for FSCSS
2. Configure auto-deployment
3. Add testing to your pipeline

---

**Congratulations! You've completed the Advanced section. Continue to [Expert](../06-expert/README.md).**
