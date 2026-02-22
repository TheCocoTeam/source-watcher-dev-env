# Source Watcher

PHP ETL (Extract–Transform–Load) with a REST API and web UI. Three projects:

| Project | Role |
|---------|------|
| **source-watcher-core** | ETL engine (extractors, transformers, loaders) |
| **source-watcher-api** | REST API (auth, DB, endpoints) |
| **source-watcher-board** | Web UI (login, transformations canvas) |

## Commands (from repo root)

Use `docker run` so you don't depend on Docker Compose networks (avoids iptables issues on some hosts).

**Core – install deps and run tests:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-core composer:2 sh -c "composer install --no-interaction --ignore-platform-reqs && ./vendor/bin/phpunit"
```

**Core – update lock file:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-core composer:2 sh -c "composer update --no-interaction --ignore-platform-reqs"
```

**API – install deps:**
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-api composer:2 sh -c "composer install --no-interaction --ignore-platform-reqs"
```

**Core – code coverage (requires dev image with PCOV):**  
Build the dev image first (rebuild after any Dockerfile.dev change):  
`sudo docker compose -f docker-compose.dev.yml build`  
or if Compose fails: `sudo docker build -f Dockerfile.dev -t source-watcher-dev:latest .`  
Then run:
```bash
sudo docker run --rm -v "$(pwd)":/app -w /app/source-watcher-core source-watcher-dev:latest sh -c "composer install --no-interaction --ignore-platform-reqs && ./vendor/bin/phpunit --coverage-text"
```
- **Text:** summary is printed in the terminal.  
- **HTML:** open `source-watcher-core/phpunit-report/coverage-html/index.html` in a browser for a line-by-line report.

**Optional – use dev image for other commands:**  
After building `source-watcher-dev:latest`, you can use it instead of `composer:2` in the commands above and drop `--ignore-platform-reqs`.
