# bunkerz-mirror

Зеркало сайта [bunkerz.pndz.ru](https://bunkerz.pndz.ru) на домене `bunkerzmirror.duckdns.org`.

## Стек

- **Traefik v3** — reverse proxy, автоматический SSL через Let's Encrypt
- **nginx** — проксирование на оригинальный сайт, замена домена в ответах

## Деплой

Требования: Docker + Docker Compose, открытые порты 80 и 443, DNS-запись `bunkerzmirror.duckdns.org` указывает на IP сервера.

```bash
git clone <repo-url>
cd bunkerz-mirror
docker compose up -d
```

SSL-сертификат получается автоматически при первом обращении к сайту.

## Как работает

```
Клиент → Traefik (SSL termination) → nginx → bunkerz.pndz.ru
```

- Traefik обрабатывает HTTPS и автообновление сертификата каждые 60 дней
- nginx заменяет все упоминания `bunkerz.pndz.ru` → `bunkerzmirror.duckdns.org` в HTML/CSS/JS ответах
- Сертификат хранится в Docker named volume `acme_data` и не теряется при перезапуске

## Команды

```bash
# Запуск
docker compose up -d

# Логи
docker compose logs -f

# Остановка
docker compose down

# Обновление образов
docker compose pull && docker compose up -d
```
