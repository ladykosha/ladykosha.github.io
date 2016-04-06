---
title: 'Dict, dictem и Emacs - словари на кончиках пальцев'
tags: [emacs, computer]
---

-   [Dictem](#dictem)
-   [Dictfmt](#dictfmt)
-   [Кое-что из мана dictfmt](#кое-что-из-мана-dictfmt)
-   [Некоторые начальные поля](#некоторые-начальные-поля)

Словари я люблю, уверена, что их должно быть много, и все “на кончиках пальцев”. Дабы легко уяснять, что означает конкретное слово, или как это будет на другом языке, или что мне там будет актуально - всё это может быть полезно, обо всём этом не хотелось бы задумываться.

Разумеется, я пробовала разные словари, в том числе как любителю и пользователю Emacs мне приглянулось сочетание dictd+dict+dictem. Это же так естественно - легко обращаться к словарям именно там, где они нужны, там, где я пишу тексты.

Про установку я почти ничего не могу сказать - в моём дебиане всё в репозитории, соответственно

    aptitude install dict dictd dictem dictzip dictfmt

Можно ещё `dictconv` добавить. Вдруг пригодится словари в dict-овский формат конвертировать.

Также можно сразу выбрать словари. Весьма интересный набор их можно скачать с <http://dict.mova.org/>. В репозиториях дебиан есть dict-de-en, dict-devil, dict-elements, dict-foldoc, dict-gcide, dict-jargon, dict-stardic, dict-vera, dict-wn, dict-xdict, mueller7-dict, mueller7accent-dict. У кого другие дистрибутивы, могут всё равно найти эти словари с описаниями на <http://www.debian.org/distrib/packages#search_packages>, и скачать. Есть заметное количество freedict-овских словарей, и в дебиановском репозитории, и на <http://www.freedict.org/ru/>, кому что удобнее. Чуточка есть тут: <ftp://ftp.dvo.ru/pub/dict/>.

Место для словарей по умолчанию - `/usr/share/dictd/`. Что стоит помнить при обновлении словарей: работающий dictd словари сам не подхватывает. Надо просто перезагрузить или последовать совету из мана: сначала отправьте dictd-у сигнал SIGUSR1, чтобы dictd выгрузил словари из памяти, потом обновите файлы и отправьте dictd-у сигнал SIGHUP, чтобы загрузить их снова.

Можно класть и в другую папку, если её указать в файле `/etc/dictd/dictd.order`.

Если словарь новый, а не обновление существовавшего, то надо ещё и `dictdconfig -w` запускать - чтоб “прописал”. Впрочем, в дебиане при установке из репозитория это делается автоматически.

Хорошо, теперь всё есть и всё сделано, что дальше? Можно поинтересоваться свойствами нашего сервера словарей. Можно сразу искать слова. Самый простой вариант поиска - просто `dict слово`.

Что касается свойств, интересно вот что:

`dict --strats` сообщит, какие стратегии поиска можно использовать. У меня это

-   exact - точное совпадение с заголовком словарной статьи.

-   prefix - совпадение префикса, то есть совпадение с началом заголовка.

-   nprefix - раньше не понимала и не использовала, но от написания статей есть польза пишущему. :) Запрос пишется так: skip\#count\#string, где skip и count - натуральные числа. Тогда результатом будут максимум count совпадений начала заголовка со строкой string, не считая первых skip из них, которые будут пропущены. Это из каждого словаря, который спросили.

-   substring - совпадение подстроки где угодно в заголовке.

-   suffix - совпадение суффикса, то есть совпадение с концом заголовка.

-   re - POSIX 1003.2 (“современные”) регекспы. См. <http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_04>.

-   regexp - “старые” регекспы. См. <http://pubs.opengroup.org/onlinepubs/9699919799/basedefs/V1_chap09.html#tag_09_03>

-   soundex - будет искать подобное по звучанию.

-   lev - найдёт слова, которые отличаются на одну “правку” (вставка, удаление или перестановка буквы).

-   word - найдёт совпадение с любым словом заголовка.

-   first - только совпадение с первым словом.

-   last - только совпадение с последним словом.

`dict –dbs` скажет, как наши словари зовутся на взгляд dictd. Это нам пригодится. Во-первых, можно прямо из консоли просить ответа лично у знакомого словаря. А во-вторых, мы собирались настроить dictem.

Что касается поиска слов непосредственно из консоли через dict, то советую обратить внимание на ключики

-   *-d dbname* или *–database dbname*\
    Cпособ обратиться к конкретному словарю, а не просить результаты сразу из всех.

-   *-m* или *–match*\
    Выдаст только сами совпадения, словарных статей к ним печатать не будет. Удобно, когда хочется узнать, что вы получите по своему запросу, когда ищете по подстрокам или при помощи регулярных выражений.

-   *-s strategy* или *–strategy strategy*\
    Будет искать заданным способом (из списка, который мы посмотрели выше)

-   *-C* или *–nocorrect*\
    Не пытаться угадать, что мы могли на самом деле искать, если результатов нет. Когда ключ не указан, предлагается список похожих слов.

### Dictem

Теперь про dictem. Что нужно добавить в ваш конфиг emacs, чтоб это счастье заработало.

Во-первых, затребовать сам dictem.

     (require 'dictem)

Во-вторых, по желанию, подключить горячие клавиши. Часто используются, имхо, две функции:

    ; SEARCH - спрашивает слово, в каком словаре искать и каким способом, показывает найденные словарные статьи.
    (global-set-key "\C-cs" 'dictem-run-search)

    ; DEFINE - спрашивает слово и словарь, показывает найденные словарные статьи.
    (global-set-key "\C-cd" 'dictem-run-define)

По умолчанию в обоих случаях ищет слово под курсором, что также уменьшает количество щёлканий по кнопкам :)

Теперь самое приятное - подборки словарей, как бы “виртуальные словари”. Имена самих словарей берём из списка, полученного по результатам `dict --dbs`, имена подборок - на ваш вкус. У меня там в основном направления перевода, в том числе типа `_lng-other`, когда хочу перевести с конкретного языка на любой знакомый, просто чтоб понять, что это. :)

Образец:

    (setq dictem-user-databases-alist
    '(("_en-ru"  . ("korolew_en-ru" "mueller24" "magus" "sinyagin_general_er" "engcom"))
    ("_en-en"  . ("foldoc" "gcide" "wn"))
    ("_ru-ru"  . ("beslov" "ses" "ushakov" "ozhegov" "ozhshv" "brok_and_efr" "rus_syn" "dalf" "bus"))
    ("_ru-en"  . ("sinyagin_general_re" "idioms" "sokrat_ru-en" "korolew_ru-en"))
    ("_other"   . ("smiley" "names" "religion" "teo" "ethnographic" "church"))
    ))

Теперь “словари” `_en-ru`, `_en-en`, `_ru-ru` и `_other` будут в списке словарей, который вам любезно подскажет emacs по клавише Tab, когда вы будете пытаться вспомнить, что у вас там вообще есть, и где же имеет смысл искать это конкретное слово :)

Если у вас уже уймища словарей, и их вы видеть в подсказках вообще не желаете, а хотите видеть исключительно вот такие “виртуальные”, то ваше счастье с вами:

    (setq dictem-use-user-databases-only t)

Такие подборки можно делать и с участием словарей из сети, если они указаны в вашем `.dictrc`. Но я предпочитаю локальные словари, поэтому не делала.

### Dictfmt

Наиболее родной для dictd формат словаря:

     _____ 

     заголовок
     определение в произвольное количество слов
     с произвольной разметкой и прочим выравниванием

     _____
     
     следующий заголовок
     следующее определение, которое может включать в себя и исходное слово
     и так далее до конца словаря.

     _____

В такой формат будет сконвертирован словарь командой `dictunformat`. Из такого формата собирается в словарь командой `dictfmt -t` или `dictfmt -c5 --utf8`.

Процесс правки словаря:

1.  `dictunzip dictionary.dict.dz` в dictionary.dict

2.  `dictunformat dictionary.index < dictionary.dict > dictionary.txt`

3.  найти в `dictionary.txt` нужную статью и подправить

4.  `dictfmt -t dictionary.txt`, возможно и с ещё дополнительными опциями, не знаю.

5.  `dictzip dictionary.dict`

Многовато мороки, честно говоря. Полная перепаковка файла, ни много, ни мало.

Мне пока удобно так. В специальной папке постоянно хранятся текстовые файлы словарей. При необходимости что-то в словаре изменить, можно внести правки в текстовый файл, после чего запустить “сборку”, которая сгенерит словарь, положит на место и заставит dictd перечитать словари.

### Кое-что из мана dictfmt

       --allchars
              Specifies that all characters should be used for the  search,
              by default only alphabetic, numeric characters and spaces are
              put to .index file and therefore are used in search.  Creates
              the special entry 00-database-allchars.

       --headword-separator sep
              sets the headword separator, which allows  several  words  to
              have  the same definition.  For example, if ´--headword-sepa-
              rator  %%%'  is  given,   and   the   input   file   contains
              ´autumn%%%fall',  both 'autumn' and 'fall' will be indexed as
              headwords, with the same definition.

       --allchars
              Specifies that all characters should be used for the  search,
              by default only alphabetic, numeric characters and spaces are
              put to .index file and therefore are used in search.  Creates
              the special entry 00-database-allchars.

       --index-keep-orig
              When --utf-8 is specified headwords are lowercased  and  non-
              alphanumeric  characters are removed from it before saving to
              .index  file  in  order  to  simplify   the   search.    When
              --index-keep-orig option is used fourth column is created (if
              necessary) in .index file, and contains an original  headword
              which  is returned by MATCH command.  This option may be use-
              ful to prevent converting " AT&T" to " ATT" or to keep proper
              nouns with uppercased first letter.

       --mime-header mime_header
              When client sends OPTION MIME command to the dictd ,  defini-
              tions  found  in this database are prepended by the specified
              MIME   header.   Creates   the   special    entry    00-data-
              base-mime-header.

### Некоторые начальные поля

    00-database-info Contains the informarion about database which is
    returned by SHOW INFO command, unless it is specified in the
    configuration file.

    00-database-short Contains the short name of the database which is
    returned by SHOW DB command, unless it is specified in the configuration
    file. See dictfmt -s.

    00-database-url URL where original dictionary sources were obtained
    from. See dictfmt -u. This headword is not used by dictd

    00-database-utf8 Presents if dictionary is encoded using UTF-8. See
    dictfmt --utf8

Пример:

     _____

     00databaseurl
     00-database-url
     http://mova.org
     _____

     00databaseshort
     00-database-short
     English-Belarusian Computer Dictionary
     _____
     
     00databaseinfo
     00-database-info
     This file was converted from the original database on:
          Mon Jan 20 19:15:35 2003


     The original data is available from:
     http://mova.org

     The original data was distributed with the notice shown below.  No
     additional restrictions are claimed.  Please redistribute this
     changed version under the same conditions and restriction that
     apply to the original version.

     English-Belarusian Computer Dictionary
     Version : (test)
     Editor  : Yauhen Radyna <project-b@tut.by>

     _____

Для раскрашивания статья пропускается через `colorit`. Сам `colorit` прогоняет через указанный препроцессор (по умолчанию m4) по правилам из конфига, и отдаёт в желаемом виде.

Приятного пользования! :)

Частично из моего же поста на welinux: <http://www.welinux.ru/post/7588/> и <https://ladycat.wordpress.com/2013/03/13/dict_n_dictem_n_emacs/>, но несколько дополненное.
