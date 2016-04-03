---
layout: post
title: "Emacs - что записалось"
date: 2016-03-29 18:19:00 +0400
tags: ["emacs", "computer"]
---
Что записалось, в основном собранное по разным углам сети.

<!-- more -->

Пакеты и прочие расширения
--------------------------

### diminish

Оччень полезная штука. Позволяет убрать minor-моды из modeline, или заменить текст на свой. На прочую работу мода не влияет.

        (require 'diminish)
        (diminish 'anzu-mode)
        (diminish 'abbrev-mode)

И что там ещё неинтересно видеть.

Вариант с заменой текста:

        (diminish 'abbrev-mode "Abv")

### package-safe-delete

Прекрасная штука для меня - проверяет зависимости, прежде, чем удалять. Теперь стало возможно разгребать натащенное, не боясь всё попортить.

    M-x package-safe-delete<RET>package-name<RET>

### wtf

    ;;; wtf.el --- Look up conversational and computing acronyms

    ;; Copyright (C) 2005, 2006, 2007 Michael Olson

    ;; Author: Michael Olson <mwolson@gnu.org>
    ;; Date: Wed 16-May-2007
    ;; Version: 2.0
    ;; URL: http://mwolson.org/static/dist/elisp/wtf.el

Для смотрения популярных акронимов (аббревиатуры, которые произносятся слитно). Содержит длинный список таковых, умеет добавлять новые (в раздел customize файла init.el, видимо), умеет удалять (добавленные) или помечать удалёнными (те, что из исходного набора).

### Словарь синонимов

<https://www.emacswiki.org/emacs/synonyms.el>

Требует наличия словаря синонимов `mthesaur.txt` в папке. Словарь устроен очень просто: первое слово - название статьи, далее перечислены «синонимы». В общем-то без разницы, синонимы ли они, достаточно, что они в этой строке.

Может искать простые регулярные выражения.

‘C-u’ - Search for additional synonyms, in two senses. Return also synonyms that are matched partially by the input. Search the entire thesaurus for input matches, even if the input matches a thesaurus entry.

‘M–’ - Append the search results to any previous search results, in buffer \*Synonyms\*. (Normally, the new results replace any previous results.)

‘C-u C-u’ - ‘C-u’ plus ‘M–’: Search more and append results.

Способ быстро сгребать интересное из браузера. Emacs, org-mode, firefox.
------------------------------------------------------------------------

Фича - сохраняю сразу в файл ссылку, тайтл страницы, текст страницы и дату добавления.

Это в настроечном файл должно быть сразу. `(server-start)` чтобы можно было подключаться через emacsclient. У меня было и раньше, ибо удобно. `(add-to-list ’load-path ~/path/to/org-protocol/)` - поищите, `(require ’org-protocol)` - возможно, это заменяется на настройку переменной org-modules. Надо проверить наличие соответствующего файла и выше поставить `(add-to-list ’load-path /path/to/folder)`, не факт, что оно подхватывается автоматом. У меня лично из-за этого не грузилось.

Это в закладку. Да, джаваскрипт.

    javascript:location.href='org-protocol://capture://l/'\sout{encodeURIComponent(location.href)}'/'\sout{encodeURIComponent(document.title)}'/'+encodeURIComponent(window.getSelection())

l после `org-protocol://capture://` - это буква используемого шаблона. Шаблоны настраиваются через `M-x customize-apropos<RET>org-capture-templates`. Шаблон `l` у меня выглядит в итоге уже в конфиге так:

    ("l" "" entry (file+headline "\textasciitilde{}/Desktop/org/notes.org" "Captured links") "** \%:description 

-   Добавляется в файл notes.org, под заголовок \* Captured Links

-   :immediate-finish - чтоб не приходилось вручную подтверждать добавление, переключаясь в емакс. Если я что хочу подправить, я это в файле сделаю.

-   :prepend - расположение “новые вверху”.

Руководство на английском - тут. <http://orgmode.org/worg/org-contrib/org-protocol.html>

How to copy from one dired dir to the next dired dir shown in a split window?
-----------------------------------------------------------------------------

Call “customize-variable” then “dired-dwim-target”, then set the value to “On” by clicking the 〖Toggle〗 button. Then, click 〖Save for Future Sessions〗, then 〖Finish〗.

Or, put the following in your emacs init file:

(setq dired-dwim-target t)

Now, when you have dired of different dir in 2 panes, and when you press C to copy, the other dir in the split pane will be default destination.

How to make dired use the same buffer for viewing directory, instead of spawning many?
--------------------------------------------------------------------------------------

In dired, you can press a instead of Enter to open the dir. This way, the previous dir will be automatically closed.

If you want Enter and (parent dir) to use the same buffer, put the following in your emacs init file:

    (add-hook 'dired-mode-hook
     (lambda ()
      (define-key dired-mode-map (kbd "<return>")
        'dired-find-alternate-file) ; was dired-advertised-find-file
      (define-key dired-mode-map (kbd "^")
        (lambda () (interactive) (find-alternate-file "..")))
      ; was dired-up-directory
     ))

In a file, how to go to its directory?
--------------------------------------

Use the command “dired-jump” 【Ctrl+x Ctrl+j】.

Добавить команду в меню auctex?
-------------------------------

Add the following code to your init file:

(add-to-list ’TeX-command-list ’(“Make” “make” TeX-run-compile nil t)) Then you’ll be able to call the make program with C-c C-c Make RET.

Replace the second element of the list with “make -C build/digital” if you want “make -C build/digital” by default, and the fourth element to t instead of nil if you want to have the chance to modify the make command instead of sticking with the default (which you can change interactively with C-u C-c C-c anyway). \[2015-12-09 Wed 05:03\]

Ссылки
------

-   https://github.com/jwiegley/use-package - очень, очень полезная штука для работы с пакетами emacs.

-   punchagan/org2blog - GitHub <https://github.com/punchagan/org2blog> \[2011-03-31 Чтв 08:37\]

-   http://xtalk.msk.su/ ott/ru/emacs/ Статьи про Emacs и не только

-   http://orgmode.org/

-   init.el at master from rpdillon/emacs-config - GitHub - \[2011-03-09 Срд 16:14\] https://github.com/rpdillon/emacs-config/blob/master/init.el - Bootstrap the Emacs environment to load literate Emacs initialization files.

-   rigidus/.emacs.d - GitHub <https://github.com/rigidus/.emacs.d> \[2011-04-08 Птн 15:02\] Рабочее emacs-окружение. Подробное описание установки находится в нем самом в каталоге ./my-mans

-   Org-mode - как его использует конкретный человек - Bernt Hansen http://doc.norang.ca/org-mode.html

-   http://myemacs.ru/ 1260361799 emacs,tips Немного о GNU Emacs &lt;DD&gt;Блог с простенькими советами

-   Emacs Advanced dired Tips (File Management) <http://xahlee.org/emacs/emacs_dired_tips.html> \[2011-07-15 Птн 00:37\]

-   <http://tuhdo.github.io/index.html> - Emacs Mini Manual, helm, projectile.

-   deepin/deepin-emacs - GitCafe https://gitcafe.com/deepin/deepin-emacs/tree/master/site-lisp/extensions \[2016-02-20 Сб 01:14\] и Deepin Emacs: a emacs with Webkit browser - Emacs & Emacsist http://emacsist.com/10542 \[2016-02-20 Сб 01:13\]

-   Emacs & Emacsist http://emacsist.com/ \[2016-02-20 Сб 00:58\]

-   proselint http://proselint.com/ \[2016-02-20 Сб 00:58\]

-   package - Using cheatsheet.el and adding cheats to it - Emacs Stack Exchange http://emacs.stackexchange.com/questions/20308/using-cheatsheet-el-and-adding-cheats-to-it 2016-02-15 Пн 23:38

-   Emacs Stack Exchange http://emacs.stackexchange.com/ 2016-02-15 Пн 23:34

-   https://github.com/madsdk/yasnippets-latex/tree/master/snippets

-   Руководство по emacs &lt;http://ergoemacs.org/emacs/emacs.html&gt;

-   Практический emacs-lisp http://ergoemacs.org/emacs/elisp.html и прочее на http://ergoemacs.org/index.html тоже занятное есть.

-   Что-то про настройки орг-мода http://www.xiangji.me/2015/07/13/a-few-of-my-org-mode-customizations/ и http://emacs.readthedocs.org/en/latest/org\_mode.html

-   https://github.com/wuliang/MyEmacsConfig/blob/master/wl.el - что-то мне тут казалось занятным.

-   https://github.com/dimitri/el-get - el-get, более развесистый менеджер пакетов, использующий package.el как один из методов.
