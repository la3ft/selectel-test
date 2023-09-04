# Репозиторий тестового задания

## Использование

1. Скопировать репозиторий:
```shell
git clone https://github.com/la3ft/selectel-test
cd selectel-test
```
2. Ввести переменную хоста до которого мы будем проверять соединение:
```shell
export destination_host=<ip адрес>
```
3. Запустить плэйбуки: 
```shell
ansible-playbook -i inventory ansible/InfluxDB.yml
ansible-playbook -i inventory ansible/Grafana.yml
ansible-playbook -i inventory ansible/LLAMA.yml
```

## Описание ролей ansbile
- [Grafana install](/ansible/Grafana install/tasks/main.yml) - устанавливает docker и все необходимые компоненты, запускает графану в docker-контейнере;
- [InfluxDB install](/ansible/InfluxDB install/tasks/main.yml) - устанавливает influxdb, меняет стандартный порт на 5086(стандартный для LLAMA), создаёт БД 'llama';
- [llama install](/ansible/llama install/tasks/main.yml) - устанавливает golang-go для сборки компонентов утилиты llama, собирает исполняемые файлы, перемещает их в /bin и на основе [шаблонов](/ansible/llama install/templates) создаёт службы для работы в фоне.