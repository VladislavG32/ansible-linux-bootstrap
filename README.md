# ansible-linux-bootstrap

Playbook для “приведения в порядок” чистого Ubuntu-сервера за одну команду: пользователь + SSH, UFW, Docker, Nginx, Fail2ban.

## Что это

Этот репозиторий поднимает базовую инфраструктуру на чистом Ubuntu (22.04/24.04):

- создаёт пользователя с sudo
- добавляет SSH ключ в `authorized_keys` (и опционально генерирует ключ на сервере)
- настраивает UFW (22/80/443)
- ставит Docker Engine + docker compose plugin
- ставит и включает Nginx (+ `/health`)
- включает Fail2ban (sshd)

## Технологии

- Ansible
- Роли: `security`, `docker`, `nginx`
- UFW, Fail2ban
- Docker Engine + docker compose plugin
- Nginx

## Как запустить (3 команды)

> Запуск выполняй с локальной машины (или из WSL Ubuntu), где есть доступ по SSH к серверу.

1) Установить зависимости:

sudo apt update && sudo apt install -y ansible
ansible-galaxy collection install community.general
2) Заполнить inventory.ini (IP сервера и пользователь для входа на первом прогоне).

3) Прогнать playbook:

ansible-playbook playbooks/site.yml

## Как прогнать на VPS/виртуалке
Создай VPS с Ubuntu 22.04/24.04 и получи доступ по SSH.

В inventory.ini укажи IP и пользователя (обычно root на первом прогоне).

Укажи путь к публичному ключу в playbooks/site.yml:

для WSL обычно: /mnt/c/Users/<USER>/.ssh/id_ed25519.pub

## Рекомендуемый порядок:

первый прогон: ssh_disable_root_login: false, ssh_disable_password_auth: false

второй прогон: включить true, когда вход под созданным пользователем по ключу уже работает

Как проверить
Nginx health:

curl http://IP_СЕРВЕРА/health
 ok
Docker:

docker --version
docker compose version
UFW:

sudo ufw status
Fail2ban:

sudo fail2ban-client status
sudo fail2ban-client status sshd


## Структура проекта
```bash
inventory.ini
ansible.cfg
playbooks/
  site.yml
roles/
  security/
  docker/
  nginx/
.env.example
README.md
