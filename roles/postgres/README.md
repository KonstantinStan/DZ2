cat << 'EOF' > roles/postgres/README.md
PostgreSQL 18 (Custom Directory Role)
=====================================

Роль для развертывания PostgreSQL 18 на Ubuntu с размещением базы данных в каталоге `/data/postgres/data` через отдельный systemd-сервис.

Requirements
------------

- Ubuntu (WSL или виртуальная машина).
- Пакет `acl` на хосте (нужен для корректной работы `become_user: postgres`).

Role Variables
--------------

Дефолтные значения заданы в `defaults/main.yml`:

| Переменная | По умолчанию | Описание |
|---|---|---|
| `postgres__version` | `"18"` | Версия PostgreSQL |
| `postgres__bin_dir` | `"/usr/lib/postgresql/{{ postgres__version }}/bin"` | исполняемые файлы |
| `postgres__data_dir` | `"/data/postgres/data"` | Каталог данных базы |
| `postgres__service_name` | `"postgresql-custom"` | Название systemd-сервиса |
| `postgres__port` | `5432` | Порт подключения |
| `postgres__listen_addresses` | `"localhost"` | Интерфейсы прослушивания |
| `postgres__users` | `[]` | Список пользователей |
| `postgres__databases` | `[]` | Список баз данных |

Dependencies
------------

Сторонних зависимостей нет.

Example Playbook
----------------

Пример запуска роли:

```yaml
---
- hosts: vm1
  become: true
  roles:
    - role: postgres
      vars:
        postgres__port: 5432
        postgres__databases:
          - name: "app_db"
        postgres__users:
          - name: "app_user"
            password: "MySecretPassword123"
