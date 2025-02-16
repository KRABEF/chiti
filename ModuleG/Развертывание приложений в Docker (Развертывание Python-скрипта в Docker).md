https://sysahelper.ru/mod/page/view.php?id=566
### Задание:

1) Развертывание приложений в Docker

- a) Общие требования:
    - Все действия выполняются на машине ControlVM. Выполнить развертывание Python-скрипта в Docker, настроить WordPress с использованием Docker Compose и развернуть базовый стек ELK для сбора и отображения логов.
- b) Развертывание Python-скрипта в Docker
    - 1. Напишите Python-скрипт 'py.py' в домашней директории пользователя ~~py.py~~, который выполняет следующие задачи:
        - i. Проверяет наличие файла input.txt в рабочей директории root.
        - ii. Выводит сообщение с содержимым.
        - iii. Если файла input.txt нет, выводит сообщение об ошибке.
    - 2. Создайте Dockerfile для Python-скрипта 'py.py' ~~file-copy-python~~ : 
        - i. Используйте базовый образ python:3.8-alpine.
        - ii. Python-скрипт py.py должен выполняться внутри контейнера.
        - iii. Реализуйте копирование файла input.txt в контейнер (этот файл может содержать произвольный текст).
        - iv. Контейнер при запуске должен выводит содержимое файла input.txt, после чего завершать свою работу.
    - 3. Сборка и запуск контейнера:
        - i. Соберите Docker-образ с именем file-copy-python~~.yml~~.
        - ii. Запустите контейнер и убедитесь, что содержимое файла выводится файл input.txt.

### Вариант реализации:

### ControlVM:

- В домашней директории пользователя **altlinux** из под пользователя **altlinux** создаём файл **py.py**:

```
vim ~/py.py
```

- - Помещаем в него следующее содержимое:
        - Реализуем функционал согласно требованиям задания:

```
import os

def main():
    working_directory = os.path.expanduser("/root")
    file_path = os.path.join(working_directory, "input.txt")

    if os.path.exists(file_path):
        with open(file_path, "r", encoding="utf-8") as file:
            content = file.read()
            print(content)
    else:
        print("Ошибка: файл input.txt не найден в директории /root.")

if __name__ == "__main__":
    main()
```

- Проверяем функционал скрипта:
    - Вывод об ошибке в случае отсутствия файла:

![](https://sysahelper.ru/pluginfile.php/931/mod_page/content/1/image.png)

- - Создаём файл и запускаем скрипт повторно:

![](https://sysahelper.ru/pluginfile.php/931/mod_page/content/1/image%20%281%29.png)

- В домашней директории пользователя **altlinux** из под пользователя **altlinux** создаём файл **Dockerfile**:

```
vim ~/Dockerfile
```

- - Помещаем в него следующее содержимое:
        - Реализуем функционал согласно требованиям задания:

```
FROM python:3.8-alpine
WORKDIR /root
COPY py.py .
COPY input.txt .
CMD ["python", "py.py"]
```

- Устанавливаем **docker**:

```
sudo apt-get install -y docker-engine
```

- Включаем и добавляем в автозагрузку службу **docker**:

```
sudo systemctl enable --now docker.service
```

- Запускаем сборку docker-образа из ранее описанного Dockerfile:
    - Имя собранного образа в данном случае будет **file-copy-python**
    - Запуск команды выполняется из домашнего каталога пользователя **altlinux**, в текущем каталоге находится сам **Dockerfile**, файл скрипта **py.py** и файл **input.txt**;

```
sudo docker build -t file-copy-python .
```

- - Результат:

![](https://sysahelper.ru/pluginfile.php/931/mod_page/content/1/image%20%282%29.png)

- Проверяем наличие собранного образа:

![](https://sysahelper.ru/pluginfile.php/931/mod_page/content/1/image%20%283%29.png)

- Запускаем docker-контейнер из собранного образа и проверяем что содержимое файла **input.txt** выводится на экран:

```
sudo docker run --rm file-copy-python
```

- - Результат:

![](https://sysahelper.ru/pluginfile.php/931/mod_page/content/1/image%20%284%29.png)