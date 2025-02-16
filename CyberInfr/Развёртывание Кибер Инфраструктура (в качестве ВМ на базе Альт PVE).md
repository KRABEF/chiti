### Задача:

- Получить установочный образ **cyber-infrastructure.iso** (пробная версия)
- Установить Кибер Инфраструктура на виртуальную машину на базе Альт PVE

### Вариант реализации:

- Переходим на сайт [киберпротекта](https://cyberprotect.ru/) ([https://cyberprotect.ru/](https://cyberprotect.ru/)), затем в разделе **Продукты** выбираем решение **Кибер Инфраструктура**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image.png)

- Выбираем **Пробная версия**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%282%29.png)

- **Заполняем** форму и нажимаем **Получить пробную версию**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%283%29.png)

- Нажимаем **Установочный файл (ISO)**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%284%29.png)

- Результат:
    - скаченный ISO образ "**cyber-infrastructure.iso**"

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%285%29.png)

- Устанавливаем Кибер Инфраструктуру на железо или же в качестве виртуальной машины (исходя из ресурсов вычислительных мощностпей):
    - В данном примере - установка будет выполнена на виртуальную машину используя Альт Виртуализация PVE
- Создаём виртуальную машину со следующими примерными характеристиками (в конце выполнения данного модуля будет информация о том, сколько необходимо ресурсов на одного участника)
    - **sata0** - для установки ОС;
    - **sata1** - для метаданных;
    - **sata2** - для хранилища данных;

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%286%29.png)

- Выполняем установку Кибер Инфраструктуры:
    - Выбираем **Установить Кибер Инфраструктура <№>** и нажимаем **Enter**

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%287%29.png)

- Ознакамливаемся с пользовательским соглашением для Кибер Инфраструктуры:
    - Выставляем **чек-бокс** и нажимаем **Далее**

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%288%29.png)

- При необходимости настраиваем **Сеть и имя хоста** и нажимаем **Далее**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%289%29.png)

- При необходимости настраиваем **Дата и время** и нажимаем **Далее**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2810%29.png)

- Выбираем **Да, нужно создать новый кластер** или **Пропустить настройку кластера** и нажимаем **Далее**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2811%29.png)

- Задаём **Пароль для панели администратора** и нажимаем **Далее**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2812%29.png)

- **Выбираем диск** на который будет установлена операционная система и нажимаем **Далее**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2813%29.png)

- **Задаём пароль** для корневой учётной записи и нажимаем **Начать установку**:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2814%29.png)

- Результат:
    - Процесс установки Кибер Инфраструктура:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2815%29.png)

- После установки виртуальная машина будет автоматически перезагружена:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2816%29.png)

- Результат:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2817%29.png)

- Проверяем доступ к веб-интерфейсу управления Кибер Инфраструктура:
    - Обращаемся по IP-адресу или доменному имени на порт **8888**;
    - Выполняем вход из под пользователя **admin** с паролем, который был задан на этапе установки Кибер Инфраструктуры для панели администратора;

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2818%29.png)

- Результат:
    - Доступ к веб-интерфейсу управления Кибер Инфраструктура:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2819%29.png)

- Ресурсы потребляемые виртуальной машиной после установки Кибер Инфраструктура:

![](https://sysahelper.ru/pluginfile.php/895/mod_page/content/4/image%20%2820%29.png)
