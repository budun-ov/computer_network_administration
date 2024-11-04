# Отчет по лабораторной работе №1

## Ход работы

## Шаг 1

Запускаем docker-compose и поднимаем контейнеры
![lab_1.1.png](images%2Flab_1.1.png)

## Шаг 2

Заходим в админку в Nextcloud
![lab_1.2.png](images%2Flab_1.2.png)

## Шаг 3

Чекаем логи в /var/www/html/data/nextcloud.log
![lab_1.3.png](images%2Flab_1.3.png)

## Шаг 4

Чекаем, что promtail собирает логи nextcloud
![lab_1.4.png](images%2Flab_1.4.png)

## Шаг 5

Подключили темплейт для мониторинга Nextcloud и сделали nextcloud доверенным доменом
![lab_1.5.png](images%2Flab_1.5.png)

## Шаг 6

1. Создали новый Host с добавленным шаблоном для пинга сервиса Nextcloud
![lab_1.6.png](images%2Flab_1.6.png)
2. Сервис Nextcloud имеет статус healthy
![lab_1.7.png](images%2Flab_1.7.png)

## Шаг 7

Подняли Grafana и установили плагин Zabbix
![lab_1.8.png](images%2Flab_1.8.png)

## Шаг 8

1. Добавили Data source - Loki
![lab_1.9.png](images%2Flab_1.9.png)

2. Добавили Data source - Zabbix
![lab_1.10.png](images%2Flab_1.10.png)

## Шаг 9

Создали дашборды отображающий логи и статус nextcloud
![lab_1.11.png](images%2Flab_1.11.png)

## Ответы на вопросы

1. **Чем SLO отличается от SLA?**

**Ответ:** SLA — это юридическое обязательство перед клиентом, а SLO — это внутренняя цель, которая помогает команде поддерживать высокий уровень обслуживания, чтобы выполнить SLA.

2. **Чем отличается инкрементальный бэкап от дифференциального?**

**Ответ:** Инкрементальный бэкап занимает меньше места и быстрее выполняется, но более сложен при восстановлении. Дифференциальный требует больше места, так как хранит все изменения с последнего полного бэкапа, но упрощает процесс восстановления.

3. **В чем разница между мониторингом и observability?**

**Ответ:** Мониторинг — это отслеживание известных метрик, а наблюдаемость позволяет глубже понять поведение системы и диагностировать неизвестные проблемы.