<p align="center">
  <img src="https://upload.wikimedia.org/wikipedia/commons/a/af/Tux.png" alt="LDAP" width="120">
</p>

## ![Lesson](https://img.shields.io/badge/Lesson-LDAP_FreeIPA-00758F?style=for-the-badge&logo=linux&logoColor=white&labelColor=111827)![Author](https://img.shields.io/badge/Author-Kamil%20Ibragimov-10B981?style=for-the-badge&logo=github&logoColor=white&labelColor=111827)![Date](https://img.shields.io/badge/Date-26.12.2025-F59E0B?style=for-the-badge&logo=calendar&logoColor=white&labelColor=111827)

### 📌 Задание
1. Развернуть стенд на 3 ВМ (CentOS 8).
2. Настроить сервер **FreeIPA** для централизованной аутентификации.
3. Написать **Ansible-playbook** для автоматического ввода клиентов в домен.
4. Проверить работу сквозной авторизации через Kerberos.

### ✅ Результат
- [x] Стенд поднимается через `vagrant up`.
- [x] Сервер и клиенты настроены. Результат см. на скриншотах:
  - 🖼️ [ipa_server_setup](otus_root_ipa_1.png)
  - 🖼️ [ansible_provision](otus_ldap.png)
  - 🖼️ [user_auth_test](otus_user.png)
    
---

## 🧰 Шаг 1 — Инфраструктура
| Хост | IP | Роль | ОС |
|------|-----------|------|----|
| **ipa.otus.lan** | 192.168.57.10 | Master FreeIPA | CentOS 8 |
| **client1.otus.lan** | 192.168.57.11 | Client | CentOS 8 |
| **client2.otus.lan** | 192.168.57.12 | Client | CentOS 8 |

---

## 🧰 Шаг 2 — Запуск
1. Поднимаем виртуалки:
```bash
vagrant up
```
2. Запускаем автоматизацию для настройки клиентов:
```bash
cd ansible
ansible-playbook -i hosts provision.yml
```

---

## 🧰 Шаг 3 — Проверка

### 1. Проверка Kerberos на сервере
```bash
vagrant ssh ipa.otus.lan
sudo -i
kinit admin
klist  # Должен быть билет krbtgt/OTUS.LAN@OTUS.LAN
```

### 2. Создание тестового пользователя
```bash
ipa user-add otus-user --first=Otus --last=User --password
```

### 3. Проверка авторизации на клиенте
Заходим на клиент под пользователем, которого нет в локальном `/etc/passwd`:
```bash
vagrant ssh client1.otus.lan
kinit otus-user
id otus-user  # Покажет UID из базы LDAP
```
