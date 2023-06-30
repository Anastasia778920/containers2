# Задание 
1. запустить контейнер с ubuntu, используя механизм LXC
2. ограничить контейнер 256 Мб ОЗУ и проверить, что ограничение работает
3. добавить автозапуск контейнеру, перезагрузить ОС и убедиться, что контейнер действительно запустился самостоятельно
4. при создании указать файл, куда записывать логи
5. после перезагрузки проанализировать логи

# Решение 
- Процесс инициализации и списки, касающиеся образцов 

        lxd init
        lxc storage list
        lxc network list
        lxc remote list
        lxc image -c dasut list ubuntu: | head -n 11
        cat /etc/default/lxc-net
        cat /etc/lxc/default.conf
        ip a

- После мы инициализируем lxc контейнер: 
        
        lxc-create -n test1 -t ubuntu
    
![Alt text](<Снимок экрана 2023-06-30 в 12.29.55.png>)

- Запускаем контейнер командой: 
        
        lxc-start -n test1
        lxc-attach -n test1

- Переходим в конфиг по пути: 

        /var/lib/lxc/test1/config

- Открываем данный конфиг в текстовике и пишем в разделе #Container specific configuration следующее: lxc.cgroup2.memory.max = 256M

![Alt text](<Снимок экрана 2023-06-30 в 12.34.08.png>)

- После этого сохраняемся и выходим: 

        lxc-stop -n test1   # Остановка
        lxc-start -n test1  # Запуск
        lxc-attach -n test1 # Заходим
    
- Автозапуск: 

        lxc-ls -f

