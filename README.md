# Репозиторий тестового задания

## Доска grafana
Доступ стандартный - admin, admin  
[Dashboard](http://45.92.176.106:3000/d/a911198f-d53f-473e-9cf2-488423547f12/llama-stats?orgId=1&from=now-3h&to=now)

## Перед использованием
Установить ansible:
```shell
apt install ansible
```

## Использование

1. Скопировать репозиторий:
```shell
git clone https://github.com/la3ft/selectel-test
cd selectel-test
```
2. Ввести адрес удаленного хоста в ansible/invetory в группу destination_host
```shell
vim ansible/inventory
```
3. Запустить плэйбуки: 
```shell
ansible-playbook -i ansible/inventory ansible/InfluxDB.yml
ansible-playbook -i ansible/inventory ansible/Grafana.yml
ansible-playbook -i ansible/inventory ansible/LLAMA_dest.yml
ansible-playbook -i ansible/inventory ansible/LLAMA.yml
```
4. Запустить утилиту scraper с указанием удаленного хоста:
```shell
scraper -llama.collector-hosts <ip удаленного хоста>
```
5. Добавить в grafana источник данных InfluxDB, настроить дашборд на своё усмотрение.

## Описание ролей ansbile
- [Grafana install](/ansible/roles/Grafana%20install/tasks/main.yml) - устанавливает docker и все необходимые компоненты, запускает графану в docker-контейнере;
- [InfluxDB install](/ansible/roles/InfluxDB%20install/tasks/main.yml) - устанавливает influxdb, меняет стандартный порт на 5086(стандартный для LLAMA), создаёт БД 'llama';
- [llama install](/ansible/roles/llama%20install/tasks/main.yml) - устанавливает golang-go для сборки компонента scraper утилиты llama, собирает исполняемые файлы, перемещает его в /bin;
- [llama install dest](/ansible/roles/llama%20install%20dest/tasks/main.yml) - устанавливает golang-go для сборки компонентов collector и reflector утилиты llama на удаленном хосте, собирает исполняемые файлы, перемещает их в /bin и на основе [шаблонов](/ansible/roles/llama%20install%20dest/templates) создаёт службы для работы в фоне.
