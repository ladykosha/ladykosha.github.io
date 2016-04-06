---
title: 'Projectile, perspective, nameframe'
---

-   <https://github.com/bbatsov/projectile> — основное, о чём я тут.

-   <https://github.com/nex3/perspective-el> — создаёт именованные перспективы, и как-то сужает тучку зрения. Так, автодополнение имени буфера начинает работать только внутри перспективы.

-   <https://github.com/john2x/nameframe> — в сочетании с двумя предыдущими, при переключении в другой проект, переключает в его фрейм с выделенной «перспективой».

Projectile — библиотека для работы с проектами. Проект — это каталог, в котором содержится специальный файл: либо .projectile, либо файлы, по которым можно узнать репозиторий какой-то vcs, либо ещё какие-то незнакомые мне варианты.

Кэширование
-----------

Since indexing a big project is not exactly quick (especially in Emacs Lisp), Projectile supports caching of the project’s files. To enable caching use this snippet of code:

    (setq projectile-enable-caching t)

Running `C-u C-c p f` will invalidate the cache prior to prompting you for a file to jump to.

Pressing `C-c p z` will add the currently visited file to the cache for current project. Generally files created outside Emacs will be added to the cache automatically the first time you open them.

Кэш сохраняется в папке emacs.

You can purge an individual file from the cache with M-x projectile-purge-file-from-cache or an entire directory with M-x projectile-purge-dir-from-cache.

Кнопочки
--------

If you ever forget any of Projectile’s keybindings just do a:

    C-c p C-h

You can change the default keymap prefix C-c p like this:

    (setq projectile-keymap-prefix (kbd “C-c C-p”))

It is also possible to add additional commands to projectile-command-map referenced by the prefix key in projectile-mode-map. You can even add an alternative prefix for all commands. Here’s an example that adds super-p as the extra prefix:

    (define-key projectile-mode-map (kbd “s-p”) ’projectile-command-map)

You can also bind the projectile-command-map to any other map you’d like (including the global keymap). Prelude does this for its prelude-mode-map.

Ignoring files
--------------

If you’d like to instruct Projectile to ignore certain files in a project, when indexing it you can do so in the .projectile file by adding each path to ignore, where the paths all are relative to the root directory and start with a slash. Everything ignored should be preceded with a - sign. Alternatively, not having any prefix at all also means to ignore the directory or file pattern that follows. Here’s an example for a typical Rails application:

    -/log
    -/tmp
    -/vendor
    -/public/uploads

This would ignore the folders only at the root of the project. Projectile also supports relative pathname ignores:

    -tmp
    -*.rb
    -*.yml
    -models

You can also ignore everything except certain subdirectories. This is useful when selecting the directories to keep is easier than selecting the directories to ignore, although you can do both. To select directories to keep, that means everything else will be ignored.

Example:

    +/src/foo
    +/tests/foo

Keep in mind that you can only include subdirectories, not file patterns.

If both directories to keep and ignore are specified, the directories to keep first apply, restricting what files are considered. The paths and patterns to ignore are then applied to that set.

Storing project settings
------------------------------

From project to project some things may differ even in same language - different coding styles, separate auto-completion sources, etc. If you need to set some variables according to selected project, you can use standard Emacs feature called Per-Directory Local Variables. To use it you must create file named .dir-locals.el inside project directory. This file must contain something like this:

    ((nil . ((secret-ftp-password . "secret")
             (compile-command . "make target-x")
             (eval . (progn
                       (defun my-project-specific-function ()
                         ;; ...
                         ))))
     (c-mode . (c-file-style . "BSD")))

The top-level alist member referenced with the key nil applies to the entire project. A key with the name eval will evaluate its arguments. In the example above, this is used to create a function. It could also be used to e.g. add such a function to a key map.

You can also quickly visit the the .dir-locals.el file with C-c p E (M-x projectile-edit-dir-locals RET).

Here are a few examples of how to use this feature with Projectile.

Configuring Projectile’s Behavior
---------------------------------

Projectile offers many customizable variables (via defcustom) that allows us to customize its behavior. Because of how dir-locals.el works, it can be used to set these customizations on a per-project basis.

You could enable caching for a project in this way:

    ((nil . ((projectile-enable-caching . t))))

If one of your projects had a file that you wanted Projectile to ignore, you would customize Projectile by:

    ((nil . ((projectile-globally-ignored-files . '("MyBinaryFile")))))

If you wanted to wrap the git command that Projectile uses to find list the files in you repository, you could do:

    ((nil . ((projectile-git-command . "/path/to/other/git ls-files -zco --exclude-standard"))))

If you want to use a different project name than how Projectile named your project, you could customize it with the following:

    ((nil . ((projectile-project-name . "your-project-name-here"))))

Configure a Project’s Compilation, Test and Run commands
--------------------------------------------------------

There are a few variables that are intended to be customized via .dir-locals.el.

-   for compilation - projectile-project-compilation-cmd

-   for testing - projectile-project-test-cmd

-   for running - projectile-project-run-cmd

They’re all set to nil by default, but by setting them you’ll override the default commands per each supported project type. These variables can be strings to run external commands or Emacs Lisp functions:

    (setq projectile-test-cmd #'custom-test-function)

Я пока тупо-механически вписала возможность запускать пока почти несуществующие задачи из doit. Всё заморочнее иметь дело с целым...

Idle Timer
----------

Projectile can be configured to run the hook projectile-idle-timer-hook every time Emacs is in a project and has been idle for projectile-idle-timer-seconds seconds (default is 30 seconds). To enable this feature, run:

    M-x customize-group RET projectile RET

and set projectile-enable-idle-timer to non-nil. By default, projectile-idle-timer-hook runs projectile-regenerate-tags. Add additional functions to the hook using add-hook:

    (add-hook 'projectile-idle-timer-hook 'my-projectile-idle-timer-function)

Мне пока незачем, но потенциально интересно.

Mode line indicator
-------------------

By default the minor mode indicator of Projectile appears in the form “ Projectile\[ProjectName\]”. This is configurable via the custom variable projectile-mode-line, which expects a sexp like

    '(:eval (format " Proj[%s]" (projectile-project-name))).

И это хорошо, потому что поумолчательный вариант длинный.

The project name will not appear by default when editing remote files (via TRAMP), as recalculating the project name (this is done on every keystroke) is a fairly slow operation there.

perspective
-----------

Создаёт именованные «перспективы», и из такой «перспективы» по умолчанию доступны лишь её буферы, а не все открытые. В сочетании с projectile понятна, без — вряд ли удобна, ибо вручную что ли каждый раз буферы в перспективу добавлять? Не сохраняет же.

Try the interactive command projectile-persp-switch-project, or you may also bind it to some handy keybinding.

    (define-key projectile-mode-map (kbd "s-s") 'projectile-persp-switch-project)

Проблемы
--------

using TRAMP with `/sudo::` fails, if projectile-global-mode is on. По такому поводу надо выключить projectile, открыть нужное, потом можно снова включить, и до перезапуска emacs проблемы не будет. <https://github.com/bbatsov/projectile/issues/835>

