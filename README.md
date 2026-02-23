# Source Watcher

PHP ETL (Extract–Transform–Load) with a REST API and web UI. Three projects:

| Project | Role |
|---------|------|
| **source-watcher-core** | ETL engine (extractors, transformers, loaders) |
| **source-watcher-api** | REST API (auth, DB, endpoints) |
| **source-watcher-board** | Web UI (login, transformations canvas) |

## Commands (from repo root)

Use `docker run` so you don't depend on Docker Compose networks (avoids iptables issues on some hosts).

### 1. Build the Docker dev environment (first time)

Do this once, and again after any change to `Dockerfile.dev`:

```bash
sudo docker compose -f docker-compose.dev.yml build
```

If Compose fails, build the image directly:

```bash
sudo docker build -f Dockerfile.dev -t source-watcher-dev:latest .
```

### 2. Installing dependencies

**Core:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-core composer:2 sh -c "composer install --no-interaction --ignore-platform-reqs"
```

**API:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-api composer:2 sh -c "composer install --no-interaction --ignore-platform-reqs"
```

### 3. Updating lock file

**Core:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-core composer:2 sh -c "composer update --no-interaction --ignore-platform-reqs"
```

**API:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-api composer:2 sh -c "composer update --no-interaction --ignore-platform-reqs"
```

### 4. Running tests

Run after installing dependencies (step 2).

**Core:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-core composer:2 sh -c "./vendor/bin/phpunit"
```

**API:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-api composer:2 sh -c "./vendor/bin/phpunit"
```

### 5. Running code coverage

Requires the dev image (step 1). Run after installing dependencies (step 2).

**Core:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-core source-watcher-dev:latest sh -c "./vendor/bin/phpunit --coverage-text"
```

- **Text:** summary is printed in the terminal.
- **HTML:** open `source-watcher-core/phpunit-report/coverage-html/index.html` in a browser for a line-by-line report.

**API:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-api source-watcher-dev:latest sh -c "./vendor/bin/phpunit --coverage-text"
```

**Optional:** After building `source-watcher-dev:latest`, you can use it instead of `composer:2` for the commands above and drop `--ignore-platform-reqs`.

## Samples (source-watcher-core)

Runnable ETL pipelines live under **source-watcher-core/samples/**. See **source-watcher-core/samples/README.md** for how to run each sample (Docker commands).
