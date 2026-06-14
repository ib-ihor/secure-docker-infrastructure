# secure-docker-infrastructure
Secure remote Docker infrastructure template featuring Portainer Agent over gRPC (grpcs) and Node Exporter behind an Nginx Reverse Proxy with Let's Encrypt SSL.3
# Secure DevOps Infrastructure Template

Цей репозиторій містить готові конфігураційні файли для безпечного підключення віддаленого Docker-сервера до локальних систем моніторингу (**Prometheus**) та керування (**Portainer Server**).

## 🔒 Особливості безпеки
* Усі сервісні порти контейнерів (`9100`, `9001`) закриті на `127.0.0.1` всередині сервера.
* Зв'язок із зовнішнім світом відбувається **виключно через Nginx Reverse Proxy (порт 443)** з шифруванням **Let's Encrypt SSL**.
* Метрики операційної системи захищені авторизацією **HTTP Basic Auth**.
* Керування контейнерами працює через нативний захищений протокол **gRPC (grpcs)**.

## 🚀 Швидке розгортання

1. Скопіюйте `docker-compose.yml` на віддалений сервер (IP: `1.2.3.4`) та запустіть сервіси:
   ```bash
   docker compose up -d
   ```
2. Скопіюйте вміст `nginx.conf` у конфігураційний файл вашого веб-сервера (наприклад, `/etc/nginx/sites-enabled/default`).
3. Налаштуйте DNS A-записи для ваших піддоменів:
   * `node.my.domen` -> `1.2.3.4`
   * `docker-portainer.my.domen` -> `1.2.3.4`
4. Згенеруйте SSL-сертифікати за допомогою Certbot:
   ```bash
   sudo certbot --nginx -d node.my.domen -d docker-portainer.my.domen
   ```
5. Перезапустіть Nginx:
   ```bash
   sudo systemctl restart nginx
   ```

## 🖥️ Параметри підключення клієнтів

* **Для Portainer Server (Локально):** 
  Додати нове оточення *Docker Agent* -> URL: `docker-portainer.my.domen:443`
* **Для Prometheus (Локально):** 
  Використовувати конфігурацію з файлу `prometheus.yml` (не забудьте згенерувати `.htpasswd` файл на сервері для Basic Auth).
