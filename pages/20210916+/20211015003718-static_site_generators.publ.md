
# Table of Contents

-   [Nikola](#org7bd08ec)
    -   [как создать сайт (не блог) на Николе](#org4c80f3a)
    -   [Ссылки](#org6fa1196)
-   [Jekyll](#orga057d75)
    -   [Ссылки](#orgce2b70d)

<div class="preview" id="org8bd6e5f">
<p>
Невеликий опыт про nikola и jekyll
</p>

</div>

-   <https://indieweb.org/static_site_generator> - что пишут на [indieweb](../computer/20210824165105-indieweb.publ.md).
-   [org-static-blog](../computer/emacs/20210731142039-org_static_blog.publ.md) - моё актуальное.
-   <https://staticsitegenerators.net/> - длинный список сайтогенераторов, описания во всплывающих подсказках.
-   <https://jamstack.org/generators/> - - ещё список. С видимыми некоторыми характеристиками.

Кроме nikola и jekyll сколько-нибудь долго я возилась только с [ikiwiki](https://ikiwiki.info/) на локалхосте, но так получилось, что ikiwiki невзлюбила кириллицу и за то время, которое я ещё продолжала пользоваться, это не исправилось (а я сама так и не поняла, в чём дело). А сейчас при всей симпатии возвращаться не хочется.

Заметка об окончании истории ikiwiki:

> October 16, 2016.  Завершила существование своей маленькой личной вики. Начала я её 05.02.2012, всего за историю там 817 изменений было. Полезная информация перенесена, эта маленькая история завершена, архивчик (вдруг сейчас ненужное и неважное сочту существенным позже? ))) останется. Но использовать эту вики, как раньше, я уже не собираюсь. Вот так.)

Себе на заметку: сайты без поиска по ним (можно внешнего, разумеется) люто бесят. Надо поиск! (Для здешнего садика прикрутила duckduckgo, правда покаа ещё всё проиндексируется, учитывая, что на старых страничках метатеги просят не индексировать :))

На сейчас очень радует, что «ничего не надо делать специально», работа с файликами «сама» отражается на саде в той мере, в какой я готова её отразить. А работаю я с текстовыми файлами, как мне удобно. И вот этого не было ни в одной предыдущей попытке. 


<a id="org7bd08ec"></a>

# Nikola

В большей степени сайтик с бложиком, чем jekyll. Использовала на gitlab (<http://agnessa.gitlab.io/>).

Дорабатывала тему, но, к сожалению, не записала, как. Тема нравилась, и я убедилась, что она нормально смотрится и работает с мобильного. На основе lanyon. Правда, не отображает вложенные меню, не поддерживает указание источников, не умеет галерей - хотя другие умеют. Вопрос, что лучше - разбираться или сменить тему. Или вообще забить. ) (UPD: Забила :)) 2021-10-15 00:42:16 +0300)

Дата и теги сейчас никак не сказываются на страницах (в отличие от постов блога), и это так устроен движок, а жаль.


<a id="org4c80f3a"></a>

## как создать сайт (не блог) на Николе

Разница между сайтом и блогом на Николе — два места в файле настроек conf.py.

1.  ****Главное****

    PAGES = (
        ("pages/*.rst", "", "story.tmpl"),
        ("pages/*.txt", "", "story.tmpl"),
        ("pages/*.html", "", "story.tmpl"),
    )

Здесь важно, что ****в середине в кавычках — пусто****. Поэтому страницы сайта (создаваемые из того, что лежит в папке pages при таком конфиге) пойдут не в специальную папку (которая могла быть в этих кавычках указана), а в корень сайта. Страница index.html, будущая главная страница сайта — тоже.

1.  Второстепенно, но если не сделать, будет ошибка.

    INDEX_PATH = "blog"

Важно, что не index. Какое другое имя поставить, blog, abracadabra или insert-your-word-here — пофиг. Лишь бы не было одноименной страницы. Если будет, никола будет честно выдавать ошибку, и отказываться решать за вас, какую именно страницу тут показывать — сгенерированный им индекс вашего блога или вашими руками сделанную.


<a id="org6fa1196"></a>

## Ссылки

-   <https://getnikola.com/creating-a-site-not-a-blog-with-nikola.html>
-   <https://getnikola.com> и <https://themes.getnikola.com>, <https://github.com/getnikola/nikola>

-   <https://getnikola.com/documentation.html> - документация.
-   <http://docs.makotemplates.org/en/latest/> - шаблоны на mako.


<a id="orga057d75"></a>

# Jekyll

Начато Mar 22, 2016.

Использую на github (<http://ladykosha.github.io/>).

Похоже, что excerpt работает только с постами, кусок странички добыть не получается. Возможно, стоит всё писать как посты. Или наоборот.

Список таймзон - <https://en.wikipedia.org/wiki/List_of_tz_database_time_zones>. Угадывать вредно.

Cтартовую тему обгрызла до состояния «теперь я знаю, что тут откуда». Ну, более-менее.

Одна из гениальных идей в основе Jekyll: что любой нормальный статический сайт - нормальный Jekyll project.

-   jekyll new myblog - создать новый бложик в папке myblog
    -   jekyll new myblog &#x2013;blank - c особо чистого листа.
    -   jekyll new &#x2013;help - а какие у меня есть варианты?
-   jekyll serve - предпросмотр существующего сайтика, вероятно на <http://127.0.0.1:4000/>, впрочем, об этом jekyll честно пишет в консольке.

Any file in /assets will be copied over to the user’s site upon build unless they have a file with the same relative path. You can ship any kind of asset here: SCSS, an image, a webfont, etc. These files behave like pages and static files in Jekyll:

If the file has YAML front matter at the top, it will be rendered.
If the file does not have YAML front matter, it will simply be copied over into the resulting site.
This allows theme creators to ship a default /assets/styles.scss file which their layouts can depend on as /assets/styles.css.

All files in /assets will be output into the compiled site in the /assets folder just as you’d expect from using Jekyll on your sites.

Your theme’s stylesheets should be placed in your theme’s \_sass folder, again, just as you would when authoring a Jekyll site.

Your theme’s styles can be included in the user’s stylesheet using the @import directive.


<a id="orgce2b70d"></a>

## Ссылки

-   <https://jekyllrb.com/docs/home/> - документация, в т.ч.
    -   <http://jekyllrb.com/docs/configuration/> - возможность устанавливать дефолтные настройки в конфиге
    -   <https://jekyllrb.com/docs/variables/> - переменные
-   <http://jekyllthemes.org> - темы для jekyll, может, присмотрю что. А скорее источник для того, чтоб смотреть «как это сделать».
-   <http://mrskat.com/posts/jekyll/> - симпатичный блог на jekyll, мб стоит взять за образец
-   <https://docs.github.com/categories/customizing-github-pages> - github-pages и jekyll.
-   <https://github.com/jekyll/jekyll/wiki/Liquid-Extensions>
-   <https://github.com/Shopify/liquid/wiki/Liquid-for-Designers> (Liquid - templating engine джекила)
-   <https://docs.github.com/articles/creating-a-custom-404-page-for-your-github-pages-site/>
-   <https://docs.github.com/articles/setting-up-your-github-pages-site-locally-with-jekyll/>
-   <https://docs.github.com/>

