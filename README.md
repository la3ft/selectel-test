# Репозиторий тестового задания

## Доска grafana
Доступ стандартный - admin, admin  
[Dashboard](http://84.252.131.23:3000/d/f136ea1d-5dd8-4ae1-a64e-6999a624696c/llama-stats?orgId=1)

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
4. Изменить конфиг llama для указания удаленного хоста:
```shell
vim ansible/llama/configs/simple_example.yaml
```
5. Запустить утилиту collector с указанием измененного конфига:
```shell
collector -llama.config ansible/llama/configs/simple_example.yaml
```
6. Добавить в grafana источник данных InfluxDB, настроить дашборд на своё усмотрение.

## Описание ролей ansbile
- [Grafana install](/ansible/roles/Grafana%20install/tasks/main.yml) - устанавливает docker и все необходимые компоненты, запускает графану в docker-контейнере;
- [InfluxDB install](/ansible/roles/InfluxDB%20install/tasks/main.yml) - устанавливает influxdb, меняет стандартный порт на 5086(стандартный для LLAMA), создаёт БД 'llama';
- [llama install](/ansible/roles/llama%20install/tasks/main.yml) - устанавливает golang-go для сборки компонентов collector, scraper утилиты llama, собирает исполняемые файлы collector и scraper, перемещает их в /bin, на основе [шаблона](/ansible/roles/llama%20install/templates/scraper.service.j2) создаёт службу scraper для работы в фоне;
- [llama install dest](/ansible/roles/llama%20install%20dest/tasks/main.yml) - устанавливает golang-go для сборки компонента reflector утилиты llama на удаленном хосте, собирает исполняемый файл, перемещает его в /bin и на основе [шаблона](/ansible/roles/llama%20install%20dest/templates/reflector.service.j2) создаёт службу reflector для работы в фоне.

## Тестовое задание:
Постановка задачи: есть исходники https://github.com/dropbox/llama по измерению потерь в сети. Их необходимо собрать, запаковать в пакет/пакеты для ubuntu и на примере двух виртуальных машин показать работоспособность.

Должно получиться: две вм c серой сетью между ними(можно сделать у нас в облаке): одна вм, до которой измеряют потери по серой сети(только с серой сетью); на второй крутится весь необходимый софт(выступает jump хостом для доступа к первом вм) + графана для визуализации метрик.
Собирать исходники и паковать пакеты можно вручную, но кратко задокументировать действия, описание деплоя софта в ansible

Результат: документация по сборке пакетов и бинарников, плэйбуки ансибла, готовая инсталляция у нас в облаке, где в графане можно посмотреть графики