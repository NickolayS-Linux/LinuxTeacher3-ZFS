# LinuxTeacher3-ZFS
Выполнение ДЗ 4 (ZFS)

Создал аккаунт на GitHub - https://github.com/

Предварительно установленное и настроенное следующее ПО:

ПК на Linux c 16 ГБ ОЗУ или виртуальная машина с системой Ubuntu.

Oracle VirtualBox (https://www.virtualbox.org/wiki/Linux_Downloads).

Все дальнейшие действия были проверены при использовании VirtualBox 7.2.6 r172322, хостовая ОС: Ubuntu 24.04 Desktop. Гостевая система — Ubuntu 24.04.4 LTS.

Оформить отчет в README-файле в GitHub-репозитории.

В данном руководстве рассмотрено следующее:

* самостоятельно устанавливать ZFS;
 
* настраивать пулы;
 
* изучить основные возможности ZFS.

Помимо текущих дисков, которые уже присутствуют в системе, добавим 8 дополнительных дисков по 512 MB.

Запуск виртуальной машины с Ubuntu.

Заходим на сервер по SSH. Дальнейшие действия выполняются от пользователя root. Переходим в root пользователя: sudo -i

**1. Определим алгоритм с наилучшим сжатием**

Получаю список всех дисков, которые есть в виртуальной машине:

<img width="754" height="787" alt="image" src="https://github.com/user-attachments/assets/5c5d2a33-5227-40b7-b0c2-62db681172aa" />

Проверю, установлен ли пакет zfsutils-linux:

<img width="751" height="76" alt="image" src="https://github.com/user-attachments/assets/536b246f-72f2-4b4e-a9b3-3a66fc55153d" />

Как видим, пакет в системе не обнаружен. зачит устанавливаю:

<img width="756" height="255" alt="image" src="https://github.com/user-attachments/assets/5274617e-5a85-4c13-bd9f-98fcb9caee48" />

Соглашаемся...

<img width="757" height="480" alt="image" src="https://github.com/user-attachments/assets/c8e67994-a93d-44ee-ae98-1afb1d68bc9d" />

Так как при установке zfsutils-linux была ошибка: FATAL modprobe zfs, т.е у на снет модуля ядра zfs, устанаиваю модуль:

<img width="759" height="905" alt="image" src="https://github.com/user-attachments/assets/e77ccf42-786d-4a11-a43a-e2a67ac26428" />

Теперь у нас есть модуль ядра ZFS, можем приступать к выполнению домашнего задания дальше.

Создаю пул из двух дисков в режиме RAID 1:






