# ansible-linux-bootstrap

Playbook для “приведения в порядок” чистого Ubuntu-сервера за одну команду: пользователь + SSH, UFW, Docker, Nginx, Fail2ban.

## Что это

Этот репозиторий поднимает базовую инфраструктуру на чистом Ubuntu (22.04/24.04):
- создаёт пользователя с sudo
- добавляет SSH ключ в `authorized_keys` (и опционально генерирует ключ на сервере)
- настраивает UFW (22/80/443)
- ставит Docker Engine + docker compose plugin
- ставит и включает Nginx (+ `/health`)
- включает Fail2ban (ssh)

## Технологии

- Ansible
- Роли: docker / nginx / security
- UFW, Fail2ban
- Docker, docker compose plugin
- Nginx

## Как запустить (3 команды)

> Запуск выполняй со своей машины (локально), где есть доступ по SSH к серверу.

1) Установить зависимости (ansible + коллекция для ufw):
```bash
sudo apt update && sudo apt install -y ansible
ansible-galaxy collection install community.general
