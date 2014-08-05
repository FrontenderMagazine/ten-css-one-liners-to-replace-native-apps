# Десять однострочников на CSS, которые помогут отказаться от нативных приложений

> Хокон Виум Ли (Håkon Wium Lie) является отцом CSS, главным инженером в Opera и 
одним из первых приверженцев веб-стандартов. Ранее в этом году мы опубликовали 
его заметку [«CSS Regions возможно вредны для веба»][1]. Когда Хокон Виум 
говорит - мы слушаем, независимо от того согласны с ним или нет. Сегодня Хокон 
представляет свою концепцию под названием CSS Фигуры (CSS Figures) и приводит 
доводы в её пользу.

С появлением планшетов и мобильных устройств нам пришлось пересмотреть свой 
подход к веб-дизайну. Полосы прокрутки, пролистываемые с помощью мыши, должны 
быть заменены жестами перелистывания, а компоненты должны свободно плавать в 
мультиколоночной разбивке страницы. Может ли это всё найти свое выражение в CSS?

Перелистываемые конструкции, плавающие компоненты и многоколоночная разметка 
страниц уже сегодня активно используются на мобильных устройствах. В качестве 
примеров можно привести [журнал Flipboard][2], [интерактивную книгу «Наш выбор»][3] 
или [приложение Paper от Facebook][4]. Они все являются нативными приложениями. 
Если мы хотим чтобы веб восторжествовал на этих устройствах (а мы этого хотим), 
жизненно важно предоставить веб-разработчикам возможность создавать презентации 
такого типа на основе веб-стандартов. Если веб-стандарты не могут предложить 
нужные инструменты, разработчикам ничего больше не остаётся, как создавать 
нативные приложения. 

Последние несколько лет я работал над редакцией двух спецификаций, которые в 
сочетании друг с другом дают такие функциональные возможности: [Многоколоночная 
разметка на CSS (CSS Multi-column Layout)][5] и [CSS Фигуры (CSS Figures)][6]. 
Думаю они существенно поспособствуют вебу в том чтобы оставаться привлекательным 
окружением для поставщиков интернет-контента. 

 В этой статье я продемонстрирую как просто писать CSS-код на основе этих двух 
 спецификаций. Для этого я ознакомлю вас с 10 однострочниками. Настоящие таблицы 
 стилей будут немного длиннее, но всё равно компактными, простыми для чтения и 
 пригодными для многократного использования. Вот несколько скриншотов чтобы дать 
 вам визуальное представление к чему мы стремимся:

![скриншот1][Три варианта представления веб-страницы с разным количеством колонок для экранов разных размеров]

## Создание страницы

Моим первым примером кода будет статья с заголовком, текстом и парочкой 
изображений. В обычных браузерах статья будет отображаться в одной колонке с 
полосой прокрутки по правую сторону. Используя Многоколоночную разметку на CSS, 
можно придать статье форму двух колонок вместо одной:

    article { columns: 2 }

Это могущественный однострочник, однако его можно улучшить; можно сделать так, 
чтобы количество колонок зависело от доступного пространства, чтобы на узком 
экране отображалась одна колонка, на более широком - две и т.д. Всё что для 
этого нужно - указать, что оптимальной длиной строки является 15em и количество 
колонок будет рассчитываться соответственно:

    article { columns: 15em }

В этом месте CSS-код для меня превращается в поэзию: одна короткая строчка кода 
подстраивает текст под узенький телефон и широкоформатный экран телевизора, под 
формат мельчайшей печати и формат для читателей с дефектами зрения. Никакого 
JavaScript, медиа-запросов или других затратных средств разработки. Всего лишь 
одна изумительно отзывчивая строчка кода. Эта строчка использовалась для 
подготовки скриншотов, представленных выше. К тому же, она работает в 
современных версиях браузеров (что пока не является таковым для последующих 
примеров).

На скриншотах выше вы видите пролистываемое представление, а не прокручиваемое. 
Это легко воплощается в реальность с помощью:

    article { overflow: paged-x }

Код, приведённый выше, указывает что статья должна быть разбита на страницы, 
расположенные в стеке по оси x (что на русском значит: по направлении вправо). 
Браузеры, поддерживающие это свойство, должны предоставлять интерфейс для 
навигации по этим страницам. Например, возможность перехода на следующую 
страницу, выполнив жест «свайп» или наклонив устройство. Также уместно 
визуальное указание того, какая страница открыта, вроде того, как в 
прокручиваемых окружениях это можно понять по положению ползунка на полосе 
прокрутки. На планшете или мобильном телефоне перемещаться на следующую страницу 
или документ с помощью свайпа намного проще чем с помощью прокручивания.

## Изображения

Добавление изображений в статью немного повышает сложность разметки. Текстовые 
строки могут быть легко распределены между несколькими колонками, однако если 
фигуры идут вперемешку с текстом, результат получится неравномерный; так как 
изображение не делится на части, если оно окажется в месте перехода в следующую 
колонку, мы получим неиспользованное пустое пространство. Чтобы этого избежать, 
в традиционной разметке газетного типа изображения помещаются в нижнюю или 
верхнюю часть колонок, тем самым позволяя тексту заполнить пустое пространство. 
Это можно выразить с помощью CSS добавив для свойства `float` значения `top` и 
`bottom`. Например:

    img { float: bottom }

Благодаря этому коду, синенькие изображения гавани на скриншотах выше плавают по 
нижнему краю страницы. CSS используется для того, что не может быть прописано с 
помощью HTML; невозможно предсказать сколько текстового контента поместится на 
экране до того, как он будет отформатирован. Следовательно, разработчик не может 
предугадать где нужно разместить изображение в исходном коде, чтобы в результате 
оно отображалось внизу колонки. Возможность задавать плавание фигур по верхнему 
и нижнему краю через `top` and `bottom` соответственно - это естественное 
расширение для свойства `float` (в дополнение к уже существующим `left` и 
`right`).

## Охват нескольких колонок

Ещё один трюк, одолженный у традиционной разметки, это когда фигура охватывает 
несколько колонок. Взгляните на эту вырезку из газеты:

![скриншот2][Вырезка из газеты, на которой можно увидеть текст, разделённый на четыре колонки, и изображения в нижнем левом, нижнем правом и верхнем правом углу]

*Использовано с разрешения Bristol Observer*

В статье из газеты изображение слева охватывает две колонки и размещается по их 
нижнему краю. Код, с помощью которого этого можно добиться в CSS прост: 

    figure { float: bottom; column-span: 2 }

Элемент HTML5 `figure` прекрасно подходит для помещения в него изображения и 
подписи под ним:

    <figure>
        <img src=cats.jpg>
        <figcaption>Isabel loves the fluffy felines</figcaption>
    </figure>

В статье из газеты также есть фигура, которая охватывает три колонки и 
располагается по верхнему правому углу. В предыдущей версии спецификации 
CSS-фигур это достигалось посредством `float: top-corner`. Однако, после 
обсуждения с разработчиками, стало ясно, что контент может быть расположен не 
только по линии углов. Поэтому в спецификацию были введены новые свойства для 
выражения того, что контент должен быть [смещён][7] до следующей колонки, 
страницы или строки.

## Смещённые фигуры

Для выражения того, что фото с котом из газетной вырезки должно быть расположено 
в верхней части последней колонки и охватить три колонки, можно использовать 
следующий код:

    figure { float: top; float-defer-column: last; column-span: 3 }

Этот код немного менее интуитивен (в сравнении с отвергнутым `top-corner`), 
однако он открывает ряд возможностей. Например, можно расположить элемент во 
второй колонке:

    .byline { float: top; float-defer-column: 1 }

Код выше смещает строку с указанием имени автора «By Collette Jackson» на одну 
колонку. Это значит, что если строка с именем автора естественным образом должна 
была бы располагаться в первой колонке, она вместо этого будет расположена во 
второй (так как вы видите в вырезке из газеты). Чтобы это работало для HTML-кода, 
необходимо чтобы строка с именем автора была прописана в начале статьи. Например 
вот так:

    <article>
      <h1>New rescue center pampers Persians</h1>
      <p class=byline>By Collette Jackson</p>
      ...
    </article>

## Смещённые рекламные объявления

Рекламные объявления - это ещё один тип контента, который лучше всего 
прописывать в начале исходного кода и смещать для последующего представления. 
Вот пример HTML-кода:

    <article>
      <aside id=ad1 src=ad1.png>
      <aside id=ad2 src=ad2.png>
      <h1>New rescue center pampers Persians</h1>
    </article>

А вот соответствующий CSS-код, с однострочником для каждого рекламного 
объявления:

    #ad1 { float-defer-page: 1 }
    #ad2 { float-defer-page: 3 }

В результате применения такого кода, объявления будут отображаться на второй и 
четвёртой странице. Опять таки, этого нельзя достичь, разместив рекламный 
объявления внутри остального текста, так как на разных устройствах переносы 
страниц будут в разных местах.

Думаю и читателям и рекламодателям понравится веб, ориентированный на 
постраничное представление. В бумажных изданиях реклама никому особо не мешает. 
Точно так же, по моему мнению, рекламные объявления на пролистываемых носителях 
информации будут менее назойливыми, чем на прокручиваемых.

## Смещённые врезки

Последний пример контента, который может быть смещён - это врезки. Врезка - это 
цитата взятая из статьи и представленная большим шрифтом в каком-то определённом 
месте страницы. В этом примере врезка отображена посередине второй колонки:

![скриншот3][Изображение врезки в печатной газете]

Вот код для выражения этого посредством CSS:

    .pullquote#first { float-defer-line: 50% }

Для других типов контента также можно применять позиционирование с помощью 
отложенных строк. Например, фотографию можно расположить над сгибом газеты, 
указав на сколько строк её нужно сместить. Это будет работать и для сгибаемых 
экранов будущего.

Врезки - это довольно интересное явление, которое заслуживает дополнительного 
обсуждения. У врезок есть две функции. Во-первых, они представляют читателю 
некий интересный отрезок текста для привлечения его внимания. Во-вторых, 
визуальное представление статьи становится более разнообразным, когда тело 
текста прерывается за счёт большего шрифта. Обычно не следует использовать более 
одной врезки на страницу. На бумаге, когда известно сколько страниц займёт 
статья, легко предусмотреть нужное количество врезок. В веб контент 
форматируется по-разному под разные типы экранов; некоторые читатели увидят 
большое количество маленьких страниц, другие - меньшее количество больших. Чтобы 
убедиться, что на каждую страницу хватит врезок, разработчикам нужно 
предоставить их в щедром количестве. Лишние врезки не должны отображаться в 
конце статьи (что было бы естественным для веб-браузера), они должны быть 
пропущены; контент ведь в любом случае представлен в основном тексте статьи. 
Этого можно достичь с помощью однострочника:

    .pullquote { float-policy: drop-tail }

Если выражаться на языке прозы, в коде говорится: если врезка находится в 
хвостовой части статьи, её не следует отображать. Тот же однострочник можно 
использовать для лишних фотографий в конце статьи; в большинстве случаев 
разработчики стремятся разместить по изображению на страницу, но не больше 
одного.

## Упражнения для закрепления материала

Усердный читатель возможно сочтёт нужным ознакомиться со спецификациями [по 
Многоколоночной разметке на CSS][8] и [CSS-фигурам][9]. В них описано больше 
примеров использования и возможностей регулировки, помогающих разработчикам 
прописать идеальное представление фигур в веб.

Если вы хотите поиграть с CSS-фигурами, проще всего будет скачать 
[Opera 12.16][10] и открыть в нём [этот документ][11], который использовался для 
создания скриншотов для Рисунка 1. Исходя из моего опыта разработки, 
спецификации уже скорее всего были изменены и не все однострочники, 
представленные в этой статье, будут работать. Кроме того, [Prince][12] и 
[AntennaHouse][13], сервисы для массового форматирования [PDF документов][14], 
частично поддерживают CSS-фигуры.

Я с удовольствием выслушаю тех, кому понравился подход, описанный в этой статье 
и тех, кому он не понравился. Вы хотели бы чтобы он был добавлен в браузеры? 
Сообщите мне ниже или обратитесь с соответствующей просьбой к разработчикам 
своего любимого браузера ([Firefox][15], [Chrome][16], [Opera][17], [IE][18]). 
Если вам не нравятся представленные средства, какой код вы бы использовали в 
случаях, описанных в статье?

Страницы и колонки являются базовыми строительными блоками в типографии с тех 
пор, как римляне начали резать свитки на страницы. Но не поэтому браузеры должны 
их поддерживать. А потому, что они помогают нам добиться лучшего и более 
красивого пользовательского опыта на мобильных устройствах.

[1]: http://alistapart.com/blog/post/css-regions-considered-harmful
[2]: https://flipboard.com/
[3]: http://www.ted.com/talks/mike_matas
[4]: https://www.facebook.com/paper
[5]: http://www.w3.org/TR/css3-multicol/
[6]: http://figures.spec.whatwg.org/
[7]: http://figures.spec.whatwg.org/#deferring-page-floats
[8]: http://www.w3.org/TR/css3-multicol/
[9]: http://figures.spec.whatwg.org/
[10]: http://www.opera.com/download/guide/?ver=12.16
[11]: http://www.wiumlie.no/2014/css-figures/koster/prefix.html
[12]: http://www.princexml.com/
[13]: http://www.antennahouse.com/
[14]: http://www.wiumlie.no/2014/css-figures/koster/prefix-pr.pdf
[15]: https://input.mozilla.org/en-US/feedback
[16]: https://productforums.google.com/forum/?rd=1#!categories/chrome/give-feature-feedback-and-suggestions
[17]: http://forums.opera.com/categories/en-opera-next-and-opera-developer
[18]: https://connect.microsoft.com/IE/feedback/

[Три варианта представления веб-страницы с разным количеством колонок для экранов разных размеров]: img/koster.jpg
[Вырезка из газеты, на которой можно увидеть текст, разделённый на четыре колонки, и изображения в нижнем левом, нижнем правом и верхнем правом углу]: img/newspaper.jpg
[Изображение врезки в печатной газете]: img/pullquote.jpg