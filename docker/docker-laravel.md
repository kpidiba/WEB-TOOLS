## LARAVEL DOCKER WITHOUT DATABASE WITH DEPENDENCIES TAILWIND ALREADY BUILD.

Since the compiled files are already in `public/build` and your Blade file links to them directly, the Node stage isn’t needed. The image just copies `public/` as part of the project.

### `Dockerfile`

```dockerfile
# ---------- Stage 1: PHP dependencies ----------
FROM composer:2 AS vendor
WORKDIR /app
COPY composer.json composer.lock ./
RUN composer install --no-dev --no-scripts --no-autoloader --prefer-dist --no-interaction
COPY . .
RUN composer dump-autoload --optimize --no-dev

# ---------- Stage 2: runtime ----------
FROM php:8.3-apache

RUN apt-get update && apt-get install -y --no-install-recommends \
        libzip-dev libicu-dev unzip \
    && docker-php-ext-install zip intl bcmath opcache \
    && a2enmod rewrite \
    && rm -rf /var/lib/apt/lists/*

ENV APACHE_DOCUMENT_ROOT=/var/www/html/public
RUN sed -ri -e "s!/var/www/html!${APACHE_DOCUMENT_ROOT}!g" /etc/apache2/sites-available/*.conf \
    && sed -ri -e "s!/var/www/!${APACHE_DOCUMENT_ROOT}!g" /etc/apache2/apache2.conf /etc/apache2/conf-available/*.conf

WORKDIR /var/www/html
COPY --from=vendor /app ./

RUN mkdir -p storage/framework/{sessions,views,cache} storage/logs bootstrap/cache \
    && chown -R www-data:www-data storage bootstrap/cache

EXPOSE 80

CMD ["sh", "-c", "php artisan config:cache && php artisan route:cache && php artisan view:cache && apache2-foreground"]
```

### `.dockerignore`

The important change is that `public/build` is **no longer ignored**, otherwise your CSS and JS wouldn’t make it into the image:

```git
.git
.env
node_modules
vendor
public/hot
storage/logs/*
storage/framework/sessions/*
storage/framework/cache/*
storage/framework/views/*
tests
```

Excluding `.env` keeps secrets out of the image, and excluding `vendor` and `node_modules` makes sure the container builds them fresh.

### 3. `docker-compose.yml`

```yaml
services:
  app:
    build: .
    ports:
      - "8080:80"
    env_file: .env.docker
    volumes:
      - sessions:/var/www/html/storage/framework/sessions
      - logs:/var/www/html/storage/logs

volumes:
  sessions:
  logs:
```

The `sessions` volume keeps file-based sessions across container restarts, so users aren’t logged out when you redeploy.

### 4. `.env.docker`

```dot
APP_NAME=MyApp
APP_ENV=production
APP_DEBUG=false
APP_URL=http://localhost:8080
APP_KEY=            # generate below

SESSION_DRIVER=file
CACHE_STORE=file
QUEUE_CONNECTION=sync
LOG_CHANNEL=stderr
```

Generate the key without a database or running container:

```bash
docker run --rm php:8.3-cli php -r "echo 'base64:'.base64_encode(random_bytes(32)).PHP_EOL;"
```

Paste the output into `APP_KEY=`.

### 5. Run it

```bash
docker compose up --build -d
```
