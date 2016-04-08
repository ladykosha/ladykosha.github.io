---
title: 'Разное компьютерное'
tags: computer
---

-   [Sass](#sass)
-   [qrencode - создать qr-код из командной строки](#qrencode---ux441ux43eux437ux434ux430ux442ux44c-qr-ux43aux43eux434-ux438ux437-ux43aux43eux43cux430ux43dux434ux43dux43eux439-ux441ux442ux440ux43eux43aux438)
-   [Про fstab](#ux43fux440ux43e-fstab)
-   [Подключение второго монитора](#ux43fux43eux434ux43aux43bux44eux447ux435ux43dux438ux435-ux432ux442ux43eux440ux43eux433ux43e-ux43cux43eux43dux438ux442ux43eux440ux430)
-   [easytag](#easytag)
-   [Release просрочен](#release-ux43fux440ux43eux441ux440ux43eux447ux435ux43d)
-   [Корзина](#ux43aux43eux440ux437ux438ux43dux430)
-   [apache](#apache)
-   [Gitweb](#gitweb)
-   [Настроить тачпад, чтоб реагировал на нажатие](#ux43dux430ux441ux442ux440ux43eux438ux442ux44c-ux442ux430ux447ux43fux430ux434-ux447ux442ux43eux431-ux440ux435ux430ux433ux438ux440ux43eux432ux430ux43b-ux43dux430-ux43dux430ux436ux430ux442ux438ux435)
-   [клавиатура](#ux43aux43bux430ux432ux438ux430ux442ux443ux440ux430)
-   [Заблокировать пользователя, не удаляя из системы](#ux437ux430ux431ux43bux43eux43aux438ux440ux43eux432ux430ux442ux44c-ux43fux43eux43bux44cux437ux43eux432ux430ux442ux435ux43bux44f-ux43dux435-ux443ux434ux430ux43bux44fux44f-ux438ux437-ux441ux438ux441ux442ux435ux43cux44b)
-   [Djvu](#djvu)
-   [Шифруем-расшифровываем](#crypt-decrypt)

Sass
----

<http://sass-lang.com/documentation/file.SASS_REFERENCE.html>

Sass: Syntactically Awesome Style Sheets http://sass-lang.com/ 2016-03-21 Пн 06:52

Sass: Документация на русском языке http://sass-scss.ru/ 2016-03-21 Пн 06:52


qrencode - создать qr-код из командной строки {#qrencode---ux441ux43eux437ux434ux430ux442ux44c-qr-ux43aux43eux434-ux438ux437-ux43aux43eux43cux430ux43dux434ux43dux43eux439-ux441ux442ux440ux43eux43aux438}
---------------------------------------------

Команда имеет следующий синтаксис:

    qrencode -o [filename.png] '[text/url/information to encode]'

Например, для того, чтобы сделать код со ссылкой на домашнюю страницу Google и сохранить его в файле google.png, необходимо ввести следующую команду:

    qrencode -o google.png 'http://google.com'

Про fstab {#ux43fux440ux43e-fstab}
---------

Памятка себе некоторой давности.

Файл /etc/fstab содержит информацию о файловых системах для команды mount(1). Строки файла /etc/fstab содержат следующие поля:

1.  Устройство, которое должно быть подмонтировано (UUID или адрес вроде sdb1, hda1).

2.  Каталог, в который монтируется файловая система.

3.  Тип файловой системы (например: vfat - FAT32, ext2 - правильно, ext2. Вообще типы смотреть в мане mount).

4.  Опции, показывающие как эта файловая система будет обрабатываться. Например:

    -   “default” - означает, что они монтируются автоматически, доступны для чтения и записи с асинхронным I/O (вводом/выводом);

    -   r - монтировать с доступом только на чтение;

5.  Флаги, относящиеся к файловой системе. Первая цифра, 0 или 1, показывает, должна ли система копироваться при помощи команды dump (это нужно для системных резервных копий). Вторая цифра может быть 0, 1 или 2, она показывает порядок, в котором файловая система должна быть проверена при загрузке.

    -   0 – не должна проверяться вовсе.

    -   1 – должна проверяться первой и использоваться как корневая (/).

    -   Для всех остальных систем ставится 2

Поля отделяются друг от друга пробелами. Строки, начинающиеся с символа \#, являются комментариями. Пустые строки игнорируются. Пример строки:

      # /dev/sda1
      UUID=9877-489A    /media/sda1     vfat     defaults,utf8,umask=007,gid=46     0 0

Чтобы узнать UUID - `ls -l /dev/disk/by-uuid`

Подключение второго монитора {#ux43fux43eux434ux43aux43bux44eux447ux435ux43dux438ux435-ux432ux442ux43eux440ux43eux433ux43e-ux43cux43eux43dux438ux442ux43eux440ux430}
----------------------------

Ещё памятка себе. –mode скорее всего уже не подходит.

    $ xrandr --output VGA1 --mode 1280x1024 --right-of VGA0
    $ xrandr --output VGA1 --off

Есть гуи, они содержат в названии randr.

Наиболее удобный на момент - `arandr`. После того, как подключен, активирован и размещён второй монитор, надо рестартить `fvwm` (мой windows manager). После этого он знает, как разворачивать на полный экран приложение, и всё такое.

easytag
-------

EasyTAG — это графический редактор тегов с открытым исходным кодом, распространяющийся под GNU General Public License. Редактор тегов разработан Джеромом Кудерком на языке Си с использованием GTK+ и id3lib для поддержки графики и обработки ID3. Хорошая, в общем, штука.

Так вот, обнаружена особенность: чтобы easytag мог работать с файлами на samba-шаре, шару очень желательно монтировать с опцией noserverino.

Release просрочен {#release-ux43fux440ux43eux441ux440ux43eux447ux435ux43d}
-----------------

Если вдруг при попытке обновить сведения о пакетах в репозиториях дебиана вылезло сообщение вроде “E: Файл Release просрочен, игнорируется”, тогда…

Чтобы это дело проигнорировать и всё же обновиться, надо выполнить команду:

        aptitude -o 'Acquire::Check-Valid-Until=false' update

Но лучше сначала проверить дату на своём компе. )

Корзина {#ux43aux43eux440ux437ux438ux43dux430}
-------

Корзина - просто папка. И лежит она в \~/.local/share/Trash/. Точнее, это 2 папки: \~/.local/share/Trash/files и \~/.local/share/Trash/info – с данными и информацией о них, необходимой для восстановления.

Можно очистить обе папки, чтобы очистить корзину, или зайти в \~/.local/share/Trash/files, чтобы восстановить удалённые файлы и папки.

apache
------

-   Чтобы добавить модуль из mods-avaliable, нужно сказать a2enmod. Чтобы сделать неактивным - a2dismod.

-   Чтобы добавить сайт из sites-avaliable, нужно сказать a2ensite. Чтобы сделать неактивным - a2dissite.

-   Сайтики отдельных пользователей удобно добавлять путём создания симлинков в /var/www/ на их домашние папки - ребёнкова вики была такова.

-   То, что мне нужно - обычно Про CGI http://localhost/doc/apache2-doc/manual/en/howto/cgi.html или Про виртуальные хосты.http://localhost/doc/apache2-doc/manual/en/vhosts/index.html

-   Идея делать подпапку существующего сайта отдельным сайтом - глупая идея.

Gitweb
------

мягкие ссылки на проекты должны лежать в `/var/lib/git`.

Настроить тачпад, чтоб реагировал на нажатие {#ux43dux430ux441ux442ux440ux43eux438ux442ux44c-ux442ux430ux447ux43fux430ux434-ux447ux442ux43eux431-ux440ux435ux430ux433ux438ux440ux43eux432ux430ux43b-ux43dux430-ux43dux430ux436ux430ux442ux438ux435}
--------------------------------------------

Поначалу стрёмно было. Мыша шевелится, а нажиматься - не нажимается. /usr/share/X11/xorg.conf.d/50-synaptics.conf - настроечный файл.

Добавила

    Option  "TapButton1"    "1"
    Option  "TapButton2"    "2"
    Option  "TapButton3"    "3"
    Option  "ClickFinger1"  "1"
    Option  "ClickFinger2"  "2"
    Option  "ClickFinger3"  "3"

после чего тачпад исправно начал работать за трёхкнопочную мышь.

Остальные возможные настройки берутся из вывода команды synclient -l. Теоретически можно настраивать хитрые скроллинги, реакцию на касание в углах…

        Option          "VertTwoFingerScroll"   "1"     # multitouch
        Option          "HorizTwoFingerScroll"  "1"     # multitouch
        Option          "LBCornerButton"        "8"     # browser "back" btn
        Option          "RBCornerButton"        "9"     # browser "forward" btn

И т.п. (примеры из дебианской вики http://wiki.debian.org/SynapticsTouchpad)

И положить это в /etc/X11/xorg.conf.d/что-нибудь.conf Вообще, если зачем-то в xorg.conf.d понадобилось лезть - копировать нужный файл в соотв. подпапку /etc и править уже там.

клавиатура {#ux43aux43bux430ux432ux438ux430ux442ux443ux440ux430}
----------

настройки в /etc/default/keyboard

    XKBMODEL="pc105"
    XKBLAYOUT="us,ru"
    XKBVARIANT=","
    XKBOPTIONS="grp:caps_toggle,compose:lwin,grp_led:scroll"
    BACKSPACE="guess"

Заблокировать пользователя, не удаляя из системы {#ux437ux430ux431ux43bux43eux43aux438ux440ux43eux432ux430ux442ux44c-ux43fux43eux43bux44cux437ux43eux432ux430ux442ux435ux43bux44f-ux43dux435-ux443ux434ux430ux43bux44fux44f-ux438ux437-ux441ux438ux441ux442ux435ux43cux44b}
------------------------------------------------

    passwd -l username

или

    passwd -u username 

См. ман по поводу подробностей.

Djvu
----

Для работы с djvu есть прекрасная штука - DjVuLibre. В дебиане - пакет djvulibre-bin. В его состав входит djvmcvt, которая как раз и нужна, чтоб делать из indirect djvu - bundled djvu, и наоборот.

**Сделать многостраничный “связанный” из “непрямого”**

djvmcvt -b docin.djvu docout.djvu

- docin.djvu здесь - тот самый индексный файл, который позволяет смотреть конвертируемый djvu как единую книгу.

- docout.djvu - результат.

**Сделать “непрямой” из обычного “связанного” многостраничного**

djvmcvt -i docin.djvu dir index.djvu

- docin.djvu здесь - многостраничный bundled djvu, который мы хотим сконвертировать

- index.djvu - имя будущего индексного файла, который позволит смотреть всё как книгу.

- dir - каталог, куда класть страницы indirect-ного djvu.

А этого на вилинухе нет. Это я поворачивала страницы.

    for i in *.tiff; do
    convert $i -rotate 90 "./rotated/$i"
    didjvu encode  -o "./rotated/$i.djvu"  "./rotated/$i"
    done

Потом из готовых отдельных:

    djvm -c doc.djvu *.tiff.djvu

Вставить в готовый файл пропущенную страницу 143:

    djvm -i doc.djvu 00167.djvu 143

## Шифруем-расшифровываем {#crypt-decrypt}

    echo TEXT| openssl enc -e -aes256 -out fileCrypted

Расшифровываем:

    openssl enc -d -aes256 -in fileCrypted

Источник: [www.commandlinefu.com/commands/view/11584/encrypted-archive-with-openssl-and-tar](www.commandlinefu.com/commands/view/11584/encrypted-archive-with-openssl-and-tar)
