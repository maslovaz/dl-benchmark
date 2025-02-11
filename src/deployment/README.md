# Развертывание инфраструктуры для запуска системы бенчмаркинга

## Использование скрипта развертывания инфраструктуры

Общий вид командной строки:

```bash
python3 deploy.py \
    -s <server_ip> -l <user_login> -p <user_psw> \
    -i <path_to_image> -d <folder_on_FTP> -n <container_name> \
    --machine_list <path_to_config> --project_folder <path_to_project_folder>
```

Аргументы командной строки:

- `-s / --server_ip <server_ip>` - IP-адрес FTP-сервера,
  на котором сохраняются образы контейнеров.
- `-l / --server_login <user_login>` - логин для подключения к FTP-серверу.
- `-p / --server_psw <user_psw>` - пароль для подключения к FTP-серверу.
- `-i / --image_path <path_to_image>` - путь до docker-образа.
- `-d / --upload_dir <folder_on_FTP>` - папка на FTP-сервере,
  куда будет загружен образ.
- `-n / --container_name <container_name>` - имя, с которым будет запущен
  docker-контейнер.
- `--machine_list <path_to_config>` - путь до конфигурационного файла
  со списком вычислительных узлов.
- `--project_folder <path_to_project_folder>` - путь до директории,
  содержащей исходные коды проекта.


Примечание: если агрументы не переданы или переданы
некорректно, скрипт аварийно завершит свою работу.

## Логика работы скрипта развертывания

1. Скрипт `deploy.py` копирует предоставленный docker-образ на FTP-сервер.
1. Осуществляется проход по списку вычислительных узлов, параметры которых приведены
   в конфигурационном файле `machine_list`, и запускается клиент `client.py`.
1. `client.py` скачивает docker-образ с FTP-сервера, после этого
   выполняет локальное развертывает docker-образа.

## Результаты работы скрипта развертывания

Результатом работы скрипта будет запущенный docker-контейнер
на каждой машине, указанной в конфигурационном файле `machine_list`.
