---
title: 'Fdupes и убиение дублей файлов'
---

Проблема поиска одинаковых файлов на диске знакома многим. Одним из очевидных решений является использование консольных программ fdupes и/или rdfind.

Если обшаривать не особенно обширные пространства, можно запустить fdupes с ключиком `-d`, и на каждую группу файлов она будет спрашивать, какие оставить. Заметим, варианта “удали всё нафиг, и чтоб глаза мои не видели!” не предусмотрено. Один экземпляр останется, придётся удалять ручками. Если пространства обширные и дублей много - разгребание в таком режиме просто неподъёмно.

Можно попросить fdupes сложить все обретённые сведения в файлик. Тогда с этим файликом надо будет как-то разбираться. И опять возникает потрясающая идея удалять всё ручками. Брр!

А ещё, когда разгребаешь от дублей большие объёмы, практически гарантированно возникают пустые папки, папки пустых папок, папки папок пустых папок, и опять же, разгребать всё это безобразие лучше бы не вручную.

Вопросы задали, переходим к ответам

-   Ответ номер один - это команда, которая может удалять файлы по выданному ей списку. Собирать из xargs и rm, опции по вкусу :) У меня это

        xargs --arg-file="2del.txt" --delimiter="\n" -I{} --max-args=1 --interactive --no-run-if-empty --verbose --max-procs=1 rm '{}'

    Берём инфу из файла с говорящим названием «удалить», разделитель элементов — новая строка, заменять на элементы из файла будем «», удаляем поштучно, спрашиваем разрешения, ничего не делаем с пустой или пробельной строкой, работаем в один процесс.

    Это самый осторожный вариант. Реально лучше убрать `--interactive`, и запускать `rm` с опцией `-f`.

-   Ответ номер два - это команда, которая может находить пустые папки: `find где_искать -type d -empty`. В принципе, find и удалять сразу может, но я предпочитаю сначала посмотреть, что я удаляю.

-   Ответ номер два-с-половиной - команда, которая может удалять пустые папки точно так же, по списку. Почти аналогично первому, но rmdir вместо rm.

Смысл xargs в том, чтобы передать аргументы команде. Xargs, в отличие от rm и rmdir, соглашается брать инфу и из файла, и из standart input, и умеет собрать команду с этой инфой. И это круто! :)

Как у меня это работает.

1.  Сначала, разумеется, fdupes -r интересующие\_папки &gt; dupes.txt. На выходе получаем файлик, в котором по группам собраны одинаковые файлы.

2.  Открыть dupes.txt, и “творить”. :) На выходе должен получиться список файлов на удаление и поуменьшившийся dupes.txt. Иногда копировала часть из dupes.txt и удаляла строки с оставляемым, иногда грепала по адресу каталога, иногда… в общем, как было удобно :)

3.  Скормить этот список своей любовно собранной команде по мотивам ответа 1.

4.  Пока не надоело или пока dupes.txt не опустел, повторять шаги 2-3.

5.  Запустить поиск пустых каталогов командой из ответа 2. Посмотреть, что получилось. Удалить ненужные командой по мотивам ответа 3. Повторять, пока ненужные пустые каталоги не кончатся.

6.  Забыть всё это до следующего раза :)

