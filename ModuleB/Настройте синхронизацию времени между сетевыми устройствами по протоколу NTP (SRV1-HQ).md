### Задание:

a) В качестве сервера должен выступать SRV1-HQ

- 1. Используйте стратум 5
- 2. Используйте ntp2.vniiftri.ru в качестве внешнего сервера синхронизации времени

c) Используйте на всех устройствах московский часовой пояс.

### Вариант реализации:

### SRV1-HQ:

- Редактируем конфигурационный файл  **chrony**:

```
vim /etc/chrony.conf
```

- - Приводим его к следующему виду:

![](https://sysahelper.ru/pluginfile.php/830/mod_page/content/5/image%20%282%29.png)

- Перезагружаем службу **chronyd** для применения изменений:

```
systemctl restart chronyd
```

- Проверяем:

![](https://sysahelper.ru/pluginfile.php/830/mod_page/content/5/image%20%281%29.png)