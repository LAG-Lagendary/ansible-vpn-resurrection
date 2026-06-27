# Инфраструктура Marzban (Master + High Performance Nodes) на Ansible

Репозиторий настроен на автоматическое развертывание VPN-панели Marzban ("Головы") и сетевых нод ("Ног") с экстремальным тюнингом производительности ядра Linux.

## Структура репозитория
* `/inventory` - Описание хостов и доступов к серверам
* `/group_vars` - Логическое разделение переменных
* `/roles` - Инкапсулированные роли
  * `common` - Базовая безопасность, Docker, утилиты
  * `kernel_extreme` - Системный тюнинг ядра (BBR, FQ, IRQ affinity)
  * `marzban_master` - Деплой мастер-панели
  * `marzban_node` - Деплой сетевой ноды с пиннингом на CPU Core 0

## Быстрый запуск деплоя

1. Отредактируйте `inventory/production.ini`
2. Замените переменные в `group_vars/master.yml` (телеграм бота, админа)
3. Замените `marzban_master_public_key` в `group_vars/nodes.yml` на публичный ключ вашего мастера.
4. Запустите тесты и развертывание:
   ```bash
   # Проверить доступность хостов по SSH:
   ansible all -m ping -i inventory/production.ini

   # Запустить полный деплой всей инфраструктуры:
   ansible-playbook -i inventory/production.ini site.yml
   ```
