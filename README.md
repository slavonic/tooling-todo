# ЦСЯ инструментарий

Спецификация минимального, но полного инструментария для создания, правки и хранения документов
на церковнославянском языке.

1. Формат хранения - вариант Markdown
2. Кодировка - Unicode

Инструменты:

1. Экранный шрифт ЦСЯ. Шрифт фиксированной ширины, где все надстрочные и управляющие Unicode символы
   отображаются линейно. Такой шрифт решает проблему читаемости "трехэтажных" выносных символов часто
   встречающихся в ЦСЯ текстах. И снимает вопрос "в каком порядке это набрано?". Предполагается
   использование этого шрифта в текстовом редакторе для редактирования ЦСЯ Markdown.
2. Доработка имеющихся Unicode шрифтов с целью гармонизации с ЦСЯ блоками других часто используемых
   языков - русского, греческого, и латиницы (вес и размер букв, межбуквенное расстояние).
   Снимает необходимость переключения шрифта при публикации в HTML/PDF.
3. Конвертер из ЦСЯ Markdown в
   * HTML
   * LaTeX
   * XML
   * PDF?
4. Драйверы клавиатуры для ввода ЦСЯ текстов. Это может быть как драйвер уровня операционной системы,
   так и просто виртуальной клавиатурой (то есть реализованной внутри приложения). Желательно
   иметь возможность пользователю задать "горячие" клавиши для ввода частых комбинаций. Для
   тех кто имеет навыки работы с HIP, предоставить совместимый с HIP набор горячих клавиш.
   Например, ввод "\д" автоматически раскрывается в последовательность Unicode символов для
   представления надстрочного титла "д".

## Специальные разделы

### Спецификация ЦСЯ Markdown

1. Как и в любом варианте Markdown, блоки разделяются пустой строкой.
2. Разметка внутри блока - по возможности логическая, а не визуальная. Например, "красная
   буква" помечается префиксом "~", при этом окрашивается первая буква и все принадлежащие к ней
   выносные символы. Подобно этому, выделение `**xxx**` при преобразовании в публикуемый
   формат может быть стилизовано как разрядка, или киноварь, или что-нибудь ещё.
3. Документ может иметь (необязательный) YAML-заголовок. Заголовок не отображается при публикации,
   но позволяет указать мета-информацию. Например, отсылку к сканам исходного текста, редакторские
   пометы, стили отображения, место в литургической таксономии. Мета-информация представляется в
   виде `ключ: значение` (каждый ключ на своей строке YAML-заголовка).
4. Блок может иметь индивидуальную мета-информацию в виде кострукции `(:: мета ::)`. Где `мета` -
   это набор выражений `ключ: значение; ключ: значение`. Этот механизм позволяет задавать блоку
   текста произвольные ключи (подобно тому как YAML-заголовок задает ключи для файла в целом).
   Мета-информация блока может помещаться непосредственно до или после блока (не должно быть
   пустой строки между текстом блока и мета-описанием). Мета-информация блока перестает
   действовать с окончанием блока.
5. Markdown текст может иметь отдельно-стоящие мета-описатели (пустая строка до и после мета).
   В этом случае описанные в мета ключи будут действовать на все последующие блоки до конца файла.
6. По соглашению каждое новое предложение в блоке начинается с новой строки без ведущих пробелов.
   Если предложение очень длинное, его разбивают на несколько строк, но строки продолжения
   начинаются с одного или наскольких пробелов.

### Целевое назначение ЦСЯ текста

1. Библиографическое - цифровое отображение печатного издания. YAML будет иметь библиографические
   данные печатного издания, отсылку к базе данных
   постраничных сканов и, возможно, интервал страниц представленных в файле. Обязательно использование
   меток страниц в тексте (чтобы можно было автоматически запросить картинку с данным блоком).
   В некоторых случаях мета-информация блока может даже содержать геометрические координаты
   блока на картинке. Правки ошибок распознования и опечаток должны быть документированы.
2. Цифровой документ (компиляция, редакторская обработка, или создание нового документа). Это
   по сути оригинальное (цифровое) издание. Связь с источниками условная и (если есть) может быть описана
   в тексте или мета-данных. Метки страниц не используются. Редакторские изменения отслеживаются
   средствами git.

### Практические приложения

1. Оцифровка печатных изданий (в том числе редких и исторических)
2. Лингвистический, морфологический, синтактический, хронологический и т.д. анализ
3. Поиск и сравнение
4. Компиляция служб
5. Переводчики между разными ЦСЯ представлениями

### Таксономия
Под разметкой текста понимается следующее:

1. Стилистическая разметка: киноварь, разрядка, размер шрифта.
2. Структурная разметка: положение в богослужебном круге, день, глас, лик, и т.д.

В дальнейшем речь пойдет о структурной разметке. Единицей структурной разметки является блок (параграф).

В единое дерево идентификаторов всю структурную информацию очевидно не вместить,
поэтому предлагаю присваивать каждому блоку **набор** атрибутов `ключ: значение`. Это позволит отдельно
описывать отношение данного текста к разным структурам: положение в книге, роль в уставе, глас, лик, и пр.

Следует определить таксономию этих атрибутов. Собственно - какие бывают `ключи` и какие бывают `значения`.

Таксономия не зависит от формата хранения или кодировки. Она определяет общий "словарик" и необходима
для того чтобы адекватно ориентироваться в текстах размеченных разными командами.

Например, вот так:

Ключ `day` помечает блоки, которые относятся к определенному дню недели. Значения могут быть:
* `1` - понедельник
* `2` - вторник
* `3` - среда
* `4` - четверг
* `5` - пятница
* `6` - суббота
* `7` - воскресение

Ключ `glas` помечает блоки, которые поются определенным гласом. Значения могут быть:
* `1` - глас первый
* `2` - глас второй
* `3` - глас третий
* `4` - глас четвертый
* `5` - глас пятый
* `6` - глас шестой
* `7` - глас седьмой
* `8` - глас восьмой

### Привязка к печатному источнику

Оцифровка печатных изданий имеет следующие особенности:
1. возможность запросить факсимиле страницы с данным блоком текста. Это подразумевает разметку разбивки страниц.
2. разрешение ссылок на определенную страницу по метке страницы.
3. невозможность (или нежелательность) помещать иллюстрации точно в то место где они в печатном издании

#### Номер страницы и метка страницы
Отсканированные страницы книги логически нумеруются от 1 до конца. Это - номера страниц. Они зависят от процесса
сканирования: сканируем титульный лист или нет. Они никак не связаны с содержанием страниц.

Напечатанные на страницах - это метки страниц. Они являются частью печатного издания и подвержены тем же
ошибкам что и основной текст:

* страницы никак не помеченные - иногда позволительно экстаполировать из меток соседних страниц
* одна и та же метка на двух страницах (ошибка издателя)
* опечатка в метке страницы

#### Логические блоки текста и разрывы страниц
Разрыв страницы может произойти практически в любом месте связного текста. Нередко параграф начинается на одной странице и
продолжается на следующей. Также связность текста может прерываться иллюстрациями.

Для цифрового корпуса это приводит к тому что:
1. блок может принадлежать к нескольким печатным страницам
2. иллюстрации, которые разрывают параграф, переносятся при оцифровке вперёд и помещаются после окончания разорваного
   параграфа

#### Переносы слов
Переносы следует убирать. Если по каким-то причинам важно сохранить информацию о точном месте разбивки слова (как в
оригинальном печатном издании), рекомендуем вместо переноса вставлять "мягкий перенос" `U+00AD`.

Когда перенос слова происходит на разрыве страницы, то в цифровом представлении следует сместить разрыв страницы
вперед до конца перенесенного слова.

#### Исправления
Необходимо использовать git для хранения истории.

Ошибки в оцифровке (и разметке), помечаются словом `@typo` в тексте `git commit`

Исправления проблем оригинала, помечаются словом `@errata`.

#### Связка с факсимиле и разметка страниц
В мета-заголовке файла указывается `image-url` например так:

```
---
image-url: https://ponomar.net/corpora/Oktoih-1854/images?$pageno
---
```

То есть рекомендуем:
1. Выкладавать факсимиле страниц так, чтобы они были доступны по протоколу `HTTP GET`
2. Организовывать веб-адреса страниц таким образом, чтобы адрес можно было легко сгенерировать для произвольного
   номера страницы
3. В `image-url` можно использовать мета-переменную `$pageno`, которая является *номером страницы* (а не меткой
   страницы, см. определение выше).

Разметка страниц в ЦСЯ тексте производится в точке смены страниц **перед** текстом принадлежащем данной странице.
То есть, независимо от того где печатается метка страницы (наверху, внизу, сбоку), разметка помещается
перед текстом и обязана содержать номер страницы (также обычно указывает и метку страницы, если есть).
