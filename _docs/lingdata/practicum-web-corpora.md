# Web-корпуса. Aranea, SkELL, SketchEngine. Коллокации. Совместная встречаемость.

<http://aranea.juls.savba.sk/>

__Aranea Web Corpora__ - семейство сравнимых веб-корпусов. Есть для большого количества языков:
* Anglicum  - английский (Anglicum Africanum - африканский английский, Anglicum Asiaticum - азиатский английский)
* Bohemicum - чешский
* Bulgaricum - болгарский
* Finnicum - финский
* Francogallicum - французский
* Georgianum - грузинский
* Germanicum -немецкий
* Hispanicum -испанский
* Hungaricum -венгерский 
* Italicum - итальянский
* Nederlandicum - голландский
* Polonicum - польский
* Russicum - русский
* Sinicum - китайский
* Slovacum - словацкий и т.д.

**Что значит сравнимых?**

Очевидно, что мы не можем сравнивать данные художественных текстов на английском языке 19 века и устную речь на русском языке 21 века. Однако иногда нужно проводить кросс-лингвистические исследования, и тогда нужны корпуса на разных языках не с одинаковыми (как в параллельном корпусе), а с похожими текстами. С помощью корпусов семейства Aranea можно проводить такие исследования. Все эти корпуса содержат тексты примерно с примерно одинаковым распределением текстов по времени (в основном современные тексты), стилям и тематикам (например, мы можем предположить, что тематики новостей примерно одинаковы в разных странах).

**Что значит веб?**

Эти корпуса были созданы посредством обкачивания Интернет-ресурсов на соответствующих языках. Что хорошо - много данных! Что плохо - нужно уделить особое внимание предобработке текстов. Основные проблемы: дублирующиеся тексты и опечатки, включая те, что возникли во время скачивания текста с веба.

### Типы поиска

* леммы - искать все словоформы, имеющие данную лемму
* словоформы - искать конкретную словоформу
* фразы - искать несколько словоформ в цепочке
* CQL (Corpus Query Language) - мудреный язык, позволяющий делать мудреные запросы

### Как делать запросы на CQL

[Вот тут отлично написано](https://www.sketchengine.co.uk/documentation/corpus-querying/). Ключевые моменты:
* Каждое слово задаётся квадратными скобочками `[]`. 
* Внутри этих скобочек мы пишет атрибут, по которому хотим искать слово, и его значение. Например, `[word="mothers"]` ищет все случаи употребления словоформы `mothers`, а `[lemma="mother"]` ищет все слова с леммой `mother` (предыдущий запрос тоже сюда попадёт).
* Неоднословный запрос: `[word="every"] [word="so"] [word="often"]`.
* Атрибуты такие: lemma, word, lempos, tag (и несколько других)
* Теги перечислены тут -- <http://www.cis.uni-muenchen.de/~schmid/tools/TreeTagger/data/Penn-Treebank-Tagset.pdf>
* Атрибуты можно сочетать:
    * `[word="test" & tag!="V.*"]`
    * `[word="test" & !tag="V.*"]`
    * `[!(word="test" & tag="V.*")]`
    * `[word="test.*" & (tag="VVN" | tag="VP")]`
* Можно искать слова, идущие не подряд: `"confuse.*" []{0,10} [tag="IN" | tag="PP"]` - от 0 до 10 слов может стоять между словом, начинающимся с `confuse` и предлогом или личным местоимением.
* Ну и можно ещё много чего, об этом подробнее по ссылке.

### Что можно делать?

* сортировать - по правому контексту, по левому контексту, по найденной форме (сначала Shine, потом shine, потом shines)
* менять формат выдачи - KWIC (Key Word In a Context = конкорданс), по предложениям
* искать коллокации - про это ниже
* смотреть данные о частотности и распределении грамматических форм в корпусе - какой части речи больше, какой формы больше
* сохранять данные поиска

### Что такое коллокации?

Как вам нравятся такие предложения:

_I hope to succeed the goal._

_The tailor operated on my sleeves._

_I highly disagree with the opinion._

**Что-то не то, правда?**

Ещё парочка примеров:
<table>
<tr><th>Можно сказать</th><th>Нельзя сказать</th></tr>
<tr>
<td>
<ul>
<li>do homework</li>
<li>long lengths</li>
<li>cause problems</li>
<li>provide care</li>
</ul>
</td>
<td>
<ul>
<li>make homework</li>
<li>lengthy legs</li>
<li>cause solutions</li>
<li>provide harm</li>
</ul>
</td>
</tr>
</table>

Коллокацией называется словосочетание, имеющее признаки синтаксически и семантически целостной единицы, в которой выбор одного слова диктует выбор другого. Коллокации - не идиомы!

### Откуда берутся коллокации?

Не совсем понятно, почему именно эти два (или больше) слова объединяются в целостную единицу. Это может быть продиктовано даже простой традицией. Однако совершенно ясно, как можно искать коллокации! Нужно искать два слова, которые часто встречаются вместе. Но так ли всё просто?

Предположим, что слова в языке стоят в случайном порядке и их сочетаемость имеет случайный характер. Давайте ссыплем все слова из корпуса в большой мешок, перемешаем, распределим случайным образом по мешочкам-текстам, а далее выложим по канавкам-предложениям. Оценим вероятность того, что два слова окажутся рядом. Если гипотеза верна, то вероятность появления биграмма on in окажется весьма велика, да и сочетание also suggest будет вполне предсказуема, так как каждое из слов встречается с большой частотой. Вместе с тем, вероятность случайного появления рядом слов heartily и endorse (да еще именно в таком порядке) чрезвычайно мала - ведь каждое слово довольно редкое. Можно понять, что сочетаемость слов имеет разную "цену", или значимость.


<img src="https://4.bp.blogspot.com/-1mTZI2GT-9E/WgRQJv7apXI/AAAAAAAAlSQ/19XPvCDVoXoBFWCo7YgHB6DqIzAOFMQLgCLcBGAs/s1600/100.-collocation-with-go.jpg">

### Разные оценки связи  

Некоторые слова встречаются рядом чаще, чем другие, но не всякий N-gram является лингвистически интересным (ср. _at the_). Для того, чтобы показать, что связь между словами не случайна, придуманы разные формулы, они по-разному ранжируют коллокации, см. [подробнее](http://www.opencorpora.org/wiki/%D0%9A%D0%BE%D0%BB%D0%BB%D0%BE%D0%BA%D0%B0%D1%86%D0%B8%D0%B8):

* MI (коэффициент взаимной информации)
* T-score
* log-likelihood (коэффициент логарифмического правдоподобия)
* Dice (Дайс)

🤔 Можно ли по корпусной выдаче понять, в чём между ними разница?



<img src="https://raw.githubusercontent.com/pykili/pykili.github.io/master/img/data_webcorpora/skell_word.png"/>

## SkELL

<a href="https://ruskell.sketchengine.co.uk/run.cgi/skell">Russian SkELL</a>

SkELL -- открытые корпуса SketchEngine для изучающих иностранный язык. Основные функции:

* корпуса объемом около 1 млн. словоупотреблений, отобранные из большого интернет-корпуса (TenTen)
* слово в контексте
* сочетаемость слова (word sketches)
* похожие слова (облако слов) 


## Открытые корпуса Sketch Engine 

<a href="https://old.sketchengine.co.uk/open/">British Academic Written English Corpus (BAWE)</a>  

Полезные функции:
* word list -- например, списки 3-граммов
* word sketch -- сочетаемость слова, по синтаксическим конструкциям (например, для слова _matter_)
* thesaurus -- близкие по смыслу слова (например, для глагола _suggest_)
* sketch diff -- различия в сочетаемости двух слов (например, для _assume_ и _suggest_)

Большие корпуса SketchEngine -- enTenTen, ruTenTen, deTenTen, arTenTen -- в платном доступе (пробный бесплатный на 30 дней). Также есть опция создавать свои корпуса.

Задания до конца семинара: <https://docs.google.com/document/d/1DEzUg6ugAJB6lVk-CplARI6w3qBBqK-UPWRdRPPiURM/edit>
