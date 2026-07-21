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

<img width="756" height="411" alt="image" src="https://github.com/user-attachments/assets/adc338c8-875c-4e27-bae9-55d6660a835b" />

Создам еще 3 пула, итого:

<img width="759" height="96" alt="image" src="https://github.com/user-attachments/assets/94a0c905-b97b-4e42-9c71-948af8affa55" />

Проверим статус дисков:

<img width="753" height="644" alt="image" src="https://github.com/user-attachments/assets/8023c2bc-6395-4674-b28d-144c4e26fdca" />


Добавлю разные алгоритмы сжатия в каждую файловую систему:

<img width="757" height="123" alt="image" src="https://github.com/user-attachments/assets/72c97229-54f2-458c-8611-53bfd218900f" />

Проверю, что все файловые системы имеют разные методы сжатия:

<img width="756" height="126" alt="image" src="https://github.com/user-attachments/assets/06db6924-beaf-496d-92c1-f4323dc88c4a" />

Сжатие файлов будет работать только с файлами, которые были добавлены после включение настройки сжатия. 

Скачаю один и тот же текстовый файл во все пулы:

Процесс:

<img width="756" height="629" alt="image" src="https://github.com/user-attachments/assets/587b40ac-32d3-4331-8496-0a07b312c2bd" />

Проверим, что файл был скачан во все пулы:

<img width="759" height="311" alt="image" src="https://github.com/user-attachments/assets/153f9272-f386-4c1d-b142-866066af30bb" />

Уже на этом этапе видно, что самый оптимальный метод сжатия у нас используется в пуле zfstest3.
Проверим, сколько места занимает один и тот же файл в разных пулах и проверим степень сжатия файлов:









