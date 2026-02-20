# Magento 2 Project

Magento 2.4.8-p3 (Community Edition) eCommerce platform.

## DevContainer Services

The devcontainer runs 6 services on a shared `magento` bridge network:

| Service        | Ports                   | Purpose                    |
| -------------- | ----------------------- | -------------------------- |
| **php**        | 9000 (internal)         | PHP-FPM application server |
| **nginx**      | 8000 (HTTP), 8443 (HTTPS) | Web server / reverse proxy |
| **db**         | 3306                    | MariaDB database           |
| **opensearch** | 9200 (API), 9600 (node) | Catalog search engine      |
| **rabbitmq**   | 5672 (AMQP), 15672 (UI) | Message broker             |
| **redis**      | 6379                    | Cache and session storage  |

### Database Credentials

All default: user `magento`, password `magento`, database `magento`, root password `magento`.

### Service Hostnames

Inside the devcontainer, services are reachable by their compose service name: `db`, `opensearch`, `rabbitmq`, `redis`, `nginx`.

## Environment

Claude runs inside the **php** container as user `dev` (home: `/home/dev`).

- **PHP**: 8.4 with Xdebug (mode: debug,profile)
- **Composer**: 2.x
- **Node**: 24.x / npm 11.x
- **Tools**: `gh` CLI, `git`, `claude`

Magento is **not installed by default** — `app/etc/env.php` and `app/etc/config.php` are absent until `bin/magento setup:install` is run.

## Project Structure

```
app/            Application code (modules, themes, config)
bin/magento     Magento CLI entrypoint
dev/            Tests and development tools
generated/      Auto-generated code (do not edit)
lib/            Internal Magento framework and web libraries
pub/            Web root (index.php, static, media)
setup/          Installer and performance toolkit
vendor/         Composer dependencies (do not edit)
```

## Commands

- **Magento CLI**: `bin/magento`
- **Run tests**: `vendor/bin/phpunit`
- **Code style fix**: `vendor/bin/php-cs-fixer fix`
- **Static analysis**: `vendor/bin/phpstan analyse`
- **Mess detector**: `vendor/bin/phpmd`
- **Code sniffer**: `vendor/bin/phpcs` / `vendor/bin/phpcbf`
- **Rector**: `vendor/bin/rector`
- ~~**MFTF**~~: **NEVER run MFTF tests** (`vendor/bin/mftf`)

## Coding Standards

PHP-CS-Fixer is configured in `.php-cs-fixer.dist.php`:

- PSR-2 compliant
- Short array syntax `[]`
- Ordered imports, no unused imports
- Proper spacing (concat, operators, semicolons)
- No trailing commas in single-line arrays
- Applies to `.phtml` files

Magento Coding Standard (phpcs) is also installed.

## Key Config Files

- `app/etc/di.xml` — Dependency injection
- `app/etc/db_schema.xml` — Database schema
- `app/etc/env.php` — Environment config (gitignored, contains secrets)
- `app/etc/config.php` — Module/scope config (gitignored)
- `composer.json` — PHP dependencies and autoloading

## Autoloading

- **PSR-0**: `app/code/`, `generated/code/`
- **PSR-4**: Magento namespaces mapped to specific framework directories
- Bootstrap: `app/etc/NonComposerComponentRegistration.php`
