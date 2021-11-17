# Домашнее задание к занятию "3.2. Работа в терминале, лекция 2"

## 1. Какого типа команда `cd`? Попробуйте объяснить, почему она именно такого типа; опишите ход своих мыслей, если считаете что она могла бы быть другого типа.
Это внутренняя команда, возвращающая код завершения и меняющая текущую рабочую директорию для оболочки, которая её выполнила. В использование её как внешней не имеет смысла т.к. команда cd должна применить изменение к текущему запущенному процессу оболочки. Если запустить её как внешнюю, то она изменит каталог для запущенного процесса, но при его завершении, изменения каталога не сохранятся.

## 2. Какая альтернатива без pipe команде `grep <some_string> <some_file> | wc -l`? `man grep` поможет в ответе на этот вопрос. Ознакомьтесь с [документом](http://www.smallo.ruhr.de/award.html) о других подобных некорректных вариантах использования pipe.
`grep -c '123 error' test_grep.txt`

![test_grep](https://user-images.githubusercontent.com/92984527/142131869-80e97748-5f20-4dca-8074-286e9ae5d8ec.png)

![test_grep_2](https://user-images.githubusercontent.com/92984527/142131951-060936c8-c012-41bd-93e9-f76110da4167.png)
## 3. Какой процесс с PID `1` является родителем для всех процессов в вашей виртуальной машине Ubuntu 20.04?
`pstree -p`

![pstree](https://user-images.githubusercontent.com/92984527/142132354-ebf6beb7-984e-4e56-bf63-a525472f2d18.png)
## 4. Как будет выглядеть команда, которая перенаправит вывод stderr `ls` на другую сессию терминала?
`ls % 2> /dev/pts/1`

![ls_pts1](https://user-images.githubusercontent.com/92984527/142139900-1103bb6b-f8f1-4669-aa96-c044652bbe5e.png)

## 5. Получится ли одновременно передать команде файл на stdin и вывести ее stdout в другой файл? Приведите работающий пример.
	`vagrant@vagrant:~$ cat test_inout_in
	work in bash
	vagrant@vagrant:~$ cat test_inout_out
	cat: test_inout_out: No such file or directory
	vagrant@vagrant:~$ cat <test_inout_in >test_inout_out
	vagrant@vagrant:~$ cat test_inout_out
	work in bash
	vagrant@vagrant:~$`

![image](https://user-images.githubusercontent.com/92984527/142154697-96432929-7bf6-431a-87e0-6e174e0645bc.png)
## 6. Получится ли вывести находясь в графическом режиме данные из PTY в какой-либо из эмуляторов TTY? Сможете ли вы наблюдать выводимые данные?
Вывод в терминал:

	`vagrant@vagrant:~$ who
	vagrant  tty1         2021-11-17 08:05
	vagrant  pts/0        2021-11-17 08:04 (10.0.2.2)
	vagrant@vagrant:~$ tty
	/dev/pts/0
	vagrant@vagrant:~$ echo Helo world from pts0 to tty1 >/dev/tty1
	vagrant@vagrant:~$`

Перенаправление в PTY:

	`echo Not Helo but Hello world from tty1 to pts0 >/dev/pts/0`

![pts_tty](https://user-images.githubusercontent.com/92984527/142162564-a8ba5f6c-c0ba-41b1-a4eb-0f98a84586f8.png)

## 7. Выполните команду `bash 5>&1`. К чему она приведет? Что будет, если вы выполните `echo netology > /proc/$$/fd/5`? Почему так происходит?
![image](https://user-images.githubusercontent.com/92984527/142165993-bde346da-ca63-40e7-ae1c-9ebafb626ecc.png)
	Команда `bash 5>&1` создает дескриптор и перенаправляет его в `stdout`. При выполнении `echo netology > /proc/$$/fd/5` присходит "двойная" передача. `echo` передает вывод в дескриптор `5`, а тот, в свою очередь, перенаправляет в `stdout`.
## 8. Получится ли в качестве входного потока для pipe использовать только stderr команды, не потеряв при этом отображение stdout на pty? Напоминаем: по умолчанию через pipe передается только stdout команды слева от `|` на stdin команды справа.
Это можно сделать, поменяв стандартные потоки местами через промежуточный новый дескриптор, который вы научились создавать в предыдущем вопросе.
## 9. Что выведет команда `cat /proc/$$/environ`? Как еще можно получить аналогичный по содержанию вывод?
## 10. Используя `man`, опишите что доступно по адресам `/proc/<PID>/cmdline`, `/proc/<PID>/exe`.
## 11. Узнайте, какую наиболее старшую версию набора инструкций SSE поддерживает ваш процессор с помощью `/proc/cpuinfo`.
## 12. При открытии нового окна терминала и `vagrant ssh` создается новая сессия и выделяется pty. Это можно подтвердить командой `tty`, которая упоминалась в лекции 3.2. Однако:

    ```bash
	vagrant@netology1:~$ ssh localhost 'tty'
	not a tty
    ```

	Почитайте, почему так происходит, и как изменить поведение.
## 13. Бывает, что есть необходимость переместить запущенный процесс из одной сессии в другую. Попробуйте сделать это, воспользовавшись `reptyr`. Например, так можно перенести в `screen` процесс, который вы запустили по ошибке в обычной SSH-сессии.
## 14. `sudo echo string > /root/new_file` не даст выполнить перенаправление под обычным пользователем, так как перенаправлением занимается процесс shell'а, который запущен без `sudo` под вашим пользователем. Для решения данной проблемы можно использовать конструкцию `echo string | sudo tee /root/new_file`. Узнайте что делает команда `tee` и почему в отличие от `sudo echo` команда с `sudo tee` будет работать.
