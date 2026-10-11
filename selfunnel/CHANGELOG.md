# Changelog — Selfunnel Tunnel

## 1.1.0 — 2026-10-11

### Security
- **Checksum-гейт сборки включён:** build.yaml больше не перебивает `FRP_SHA256_*` пустотой; Dockerfile ОБЯЗАТЕЛЬНО сверяет sha256 tar-архива frp при сборке (до этого сборка шла без проверки — «WARNING: skipping checksum verification»).
- **TOML-инъекции закрыты:** `subdomain`, `local_host`, `local_port` из опций валидируются регэкспами до генерации `frpc.toml`. Поддомен — `^[a-z0-9][a-z0-9-]{0,38}[a-z0-9]$` (та же нормализация, что на бэке), `local_host` — только буквы/цифры/точка/дефис, порт — 1..65535.
- `log_level` whitelist наряду с schema (info|warn|error|debug).

### Изменено
- **frp 0.58.0 → 0.71.0** (актуальная версия из прод-скрипта `frp_manifest.json`).
- Все исходящие curl получили `User-Agent: selfunnel-addon/<ver>` — без UA часть CDN/прокси отдаёт 403 (Cloudflare error 1010).
- Добавлены `icon.png` (128×128) и `logo.png` (256×256) из фирменного логотипа selfunnel (исходник assets/logo-tile.svg).
- Документация полностью переписана: README, DOCS.md, CHANGELOG.
- Версия 1.0.0 → 1.1.0.

### Исправлено
- «Недопустимый поддомен» ловится до генерации конфига, а не на этапе запуска frpc.

## 1.0.0 — 2026-10-09
- Первый релиз: `install_token` → validate → frpc.toml → frpc (amd64/aarch64).
