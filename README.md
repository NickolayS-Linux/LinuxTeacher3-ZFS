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

Но сначала переименую пул zstest в zfstest0:

<img width="757" height="135" alt="image" src="https://github.com/user-attachments/assets/5ebffd75-4556-4541-bded-ff9aa7e44f97" />

Процесс:

<img width="757" height="620" alt="image" src="https://github.com/user-attachments/assets/3d4f3456-69a1-44b4-81b4-88b976c2faf3" />


Проверим, что файл был скачан во все пулы:

<img width="757" height="286" alt="image" src="https://github.com/user-attachments/assets/3de27ef5-9135-4484-9f6e-0b4369950769" />

Уже на этом этапе видно, что самый оптимальный метод сжатия у нас используется в пуле zfstest2.

Проверим, сколько места занимает один и тот же файл в разных пулах и проверим степень сжатия файлов:

<img width="757" height="112" alt="image" src="https://github.com/user-attachments/assets/95d00dc2-d637-403d-bcc1-bb46529011ff" />

Таким образом, получается, что алгоритм gzip-9 самый эффективный по сжатию. 

2. Определение настроек пула
 
Скачиваем архив в домашний каталог: 

<img width="757" height="244" alt="image" src="https://github.com/user-attachments/assets/cd71163e-8274-4195-9aae-1f978d86047d" />

Разархивируем его:

<img width="756" height="108" alt="image" src="https://github.com/user-attachments/assets/9783aaab-92f5-46af-a3ac-1bc12be2b548" />

Проверим, возможно ли импортировать данный каталог в пул:

<img width="757" height="215" alt="image" src="https://github.com/user-attachments/assets/87788294-22c8-4c64-94f4-cdd44214579f" />

Данный вывод показывает нам имя пула, тип raid и его состав. 

Сдела. импорт данного пула к нам в ОС:

Переименовывать импортированный пул не стану. Команда zpool status выдаст нам информацию о составе импортированного пула:

<img width="751" height="879" alt="image" src="https://github.com/user-attachments/assets/cff4c2d7-f2a9-466c-94c3-bc5552cfec5f" />

Далее нам нужно определить настройки. Выврд сразу всех параметром файловой системы:

<img width="753" height="954" alt="image" src="https://github.com/user-attachments/assets/163c5592-f6b8-45da-8120-65b07129a6cf" />

Получим некоторые значения натстроек пула, а именно: размер, тип (по типу FS мы можем понять, что позволяет выполнять чтение и запись), значение recordsize, тип сжатия (или параметр отключения), 
тип контрольной суммы:

<img width="754" height="339" alt="image" src="https://github.com/user-attachments/assets/8a304049-1bbd-44ac-a302-1b1bebdbef20" />

3. Работа со снапшотом, поиск сообщения от преподавателя
 
Скачаем файл, указанный в задании:

<img width="758" height="79" alt="image" src="https://github.com/user-attachments/assets/4bee94d4-66fa-4791-85c5-19883f917be2" />

Восстановлю файловую систему из снапшота:

<img width="755" height="73" alt="image" src="https://github.com/user-attachments/assets/c98f9f01-d55c-4b3b-a6c5-78fa7f7a538e" />

Далее, ищу в каталоге /otus/test файл с именем “secret_message”:

<img width="757" height="102" alt="image" src="https://github.com/user-attachments/assets/594b301b-55b3-4b20-bb72-8cd821facebd" />

Смотрю содержимое найденного файла:

<img width="755" height="102" alt="image" src="https://github.com/user-attachments/assets/135ae89e-7130-4ad7-9ec1-7535e525b31e" />

Тут мы видим ссылку на курс OTUS, задание выполнено.

Домашнее задание выполнено полностью.








