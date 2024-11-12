# Отчет по лабораторной работе №2

## Ход работы

<details>
  <summary style="font-size: 23px;">Часть 1. Установка и настройка Ansible</summary>
  <p style="font-size: 14px;">

1. Устанавливаем пакетный менеджер pip для нашего python: 

    ```curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py && python3 get-pip.py```

    ![lab_2.1.png](images%2Flab_2.1.png)

2. Устанавливаем, собственно, ansible: ```python3 -m pip install ansible```

    ![lab_2.2.png](images%2Flab_2.2.png)

3. Выбираем или создаем директорию, где будем работать. Создаем базовый конфиг файл, затем папку ```inventory``` и в ней файл с хостами (тренироваться будем на localhost)

4. Проверяем, что сервер с Ansible подключился к “клиенту” (в нашем случае это одна и та же машина, localhost): ```ansible my_servers -m ping -c local``` и ```ansible my_servers -m setup -c local```

    ![lab_2.3.png](images%2Flab_2.3.png)

5.   Пробуем выполнить команду посложнее на нашем клиенте
     - Создаем текстовый файл с производным содержимым, через модуль shell: ```ansible my_servers -c local -m shell -a 'echo test_file_content > $HOME/test.txt'```
     - Проверяем, что по нужному пути создался нужный файл с нужным именем и содержимым
     - Удаляем файл через модуль file: ```ansible my_servers -c local -m file -a 'path=$HOME/test.txt state=absent'```
   ![lab_2.4.png](images%2Flab_2.4.png)

</p>
</details>

<details>
  <summary style="font-size: 23px;">Часть 2. Установка Caddy</summary>
  <p style="font-size: 14px;">

1. В части 1 были рассмотрены базовые ad-hoc команды для Ansible, пора переходить к более сложным конструкциям - Ansible Playbooks. Устанавливать будем вебсервер Caddy. Для начала создадим в рабочей директории папку ```roles``` и в ней инициализируем исходное конфигурационное “дерево”: ```ansible-galaxy init caddy_deploy```

    ![lab_2.5.png](images%2Flab_2.5.png)

2. Наполняем файл ```roles/caddy_deploy/tasks/main.yml```. Здесь мы описываем непосредственно шаги, которые будут выполняться в нашем плейбуке (по сути, несколько команд ```ansible -m ******``` подряд)
   
    ![lab_2.6.png](images%2Flab_2.6.png)

3. Наконец, в рабочей директории создаем собственно файл конфигурации самого плейбука, где указываем нужные нам хосты и роли

    ![lab_2.7.png](images%2Flab_2.7.png)

4. Запускаем плейбук: ```ansible-playbook caddy_deploy.yml``` и проверяем, успешно ли все шаги выполнились

    ![lab_2.8.png](images%2Flab_2.8.png)

</p>
</details>

<details>
  <summary style="font-size: 23px;">Часть 3. Домен и настройка Caddyfile</summary>
  <p style="font-size: 14px;">

1. Регистрируем себе бесплатный домен на выданный ранее ip-адрес, например на сервисе duckdns.org

    ![lab_2.9.png](images%2Flab_2.9.png)

2. Попробуем использовать доп. возможности плейбуков - создадим шаблон (Jinja2) и переменные (в формате ```{{ var }}```)

    ![lab_2.10.png](images%2Flab_2.10.png)

3. Добавляем в наш плейбук (в tasks) новые шаги, отвечающие за создание конфигурационного файла из шаблона и последующую перезагрузку сервиса:
    ```YAML
       - ...
    
       - ...
    
       - name: Create config file
         template:
           src: templates/Caddyfile.j2  # Откуда берем
           dest: /etc/caddy/Caddyfile  # Куда кладем
    
       - name: Reload with new config
         service:
           name: caddy
           state: reloaded
      ```
    ![lab_2.11.png](images%2Flab_2.11.png)

4. Снова запускаем плейбук, после чего вводим в браузере имя своего домена и убеждаемся, что тестовая страничка Caddy автоматически поднялась на подписанном сертификате с https
    ![lab_2.12.png](images%2Flab_2.12.png)
    ![lab_2.13.png](images%2Flab_2.13.png)

</p>
</details>

<details>
  <summary style="font-size: 23px;">Задания</summary>
  <p style="font-size: 14px;">

1. Переписать пример с созданием и удалением файла из шага 5 Части 1 с ad-hoc команд на плейбук формат, а так же добавить четвертый шаг - перед удалением поменять содержимое файла на любое другое.

    ![lab_2.14.png](images%2Flab_2.14.png)

2. “Расширить” конфиг вебсервера Caddy любым функционалом по желанию: например, добавить проксирование, или какие-нибудь заголовки (header). Вместо дефолтной страницы Caddy подставить свою, хотя бы index.html с Hello world внутри. Добавить это в качестве дополнительного шага в tasks
    
    Подстановка index.html вместо дефолтной страницы (в tasks 'Copy custom index.html'):
    ![lab_2.15.png](images%2Flab_2.15.png)
    Результат:
    ![lab_2.16.png](images%2Flab_2.16.png)

</p>
</details>

### Примечание: все файлы со скриптами (которые мы непосредственно использовали) в папке ansible