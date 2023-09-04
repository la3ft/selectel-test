# Репозиторий тестового задания

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
2. Ввести переменную хоста до которого мы будем проверять соединение:
```shell
export destination_host=<ip адрес>
```
3. Запустить плэйбуки: 
```shell
ansible-playbook -i ansible/inventory ansible/InfluxDB.yml
ansible-playbook -i ansible/inventory ansible/Grafana.yml
ansible-playbook -i ansible/inventory ansible/LLAMA.yml
ansible-playbook -i ansible/inventory ansible/LLAMA_dest.yml
```

## Описание ролей ansbile
- [Grafana install](/ansible/roles/Grafana%20install/tasks/main.yml) - устанавливает docker и все необходимые компоненты, запускает графану в docker-контейнере;
- [InfluxDB install](/ansible/roles/InfluxDB%20install/tasks/main.yml) - устанавливает influxdb, меняет стандартный порт на 5086(стандартный для LLAMA), создаёт БД 'llama';
- [llama install](/ansible/roles/llama%20install/tasks/main.yml) - устанавливает golang-go для сборки компонентов утилиты llama, собирает исполняемые файлы, перемещает их в /bin и на основе [шаблонов](/ansible/roles/llama%20install/templates) создаёт службы для работы в фоне.