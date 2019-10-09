---
title: 13 &mdash; Словари
---

# На прошлом занятии
* использование функций стандартной библиотеки (math, random)
* пример использования функций
* формат TSV


# План этого занятия
* создание и изменение словарей
* перебор словарей
* пример: частотный словарь


# Создание и изменение словарей

## Создание словарей

Словарь (dictionary) — **неупорядоченная** структура данных для хранения пар ключ—значение. Например:

```python
# Ключ: значение
>>> mendeleev_table = {
...     "кислород": "O",
...     "гелий": "He",
...     "дубний": "Db",
...     "золото": "Au"
... }
>>> gradebook = {
...     "Английский": 5,
...     "Физическая культура": 4,
...     "Алгебра": "отлично",
...     "Геометрия": "неуд"
... }
>>> empty_dict = {}  # пустой словарь
>>> empty_dict2 = dict()  # функция dict() используется для конструирования словарей, аналог list для списков
```
Заметим, что ключом словаря не может быть изменяемый элемент, например список (`list`).
Значениями могут быть только элементы неизменяемого типа: `str`, `int`, `tuple`.

## Доступ к элементам

Доступ к значениям словаря осуществляется так же как и доступ к элементам списка:

```python
>>> print(mendeleev_table["золото"])
Au
>>> mendeleev_table["висмут"] = "Bi"  # добавит в словарь новую пару "висмут": "Bi"
>>> print(mendeleev_table["висмут"])
Bi
```

Обратите внимание, что срезы для словарей не работают, так как словарь является неупорядоченной коллекцией и потому нет способа обратиться сразу к диапазону ключей.

## Обновление словаря

Добавить к словарю пары ключ—значение из другого словаря можно с помощью метода `.update()`:

```python
>>> rare_earth_metals = {"скандий": "Sc", "иттрий": "Y", "лантан": "La"}
>>> mendeleev_table.update(rare_earth_metals)
>>> print(mendeleev_table)
{'кислород': 'O',
 'гелий': 'He',
 'дубний': 'Db',
 'золото': 'Au',
 'скандий': 'Sc',  # <- добавленное значение
 'иттрий': 'Y',    # <- добавленное значение
 'лантан': 'La'}   # <- добавленное значение
```

Если в добавляемом словаре уже есть пара с тем же ключом, то значение будет изменено на новое. Например:

```python
>>> d1 = {0: "ноль", 1: "один"}
>>> d2 = {1: "единица", 2: "двойка"}
>>> d1.update(d2)
>>> print(d1)
{0: 'ноль', 1: 'единица', 2: 'двойка'}
```

# Перебор словарей

## Перебор ключей

При переборе словаря с помощью цикла `for` будут возвращаться его ключи.

```python
>>> for ru_name in mendeleev_table:
... 	print(ru_name)  # напечатает ключ, в нашем случае название элемента на русском
... 	print(mendeleev_table[ru_name])  # напечатает значение, в нашем случае символ элемента
кислород
O
гелий
He
дубний
Db
золото
Au
скандий
Sc
иттрий
Y
лантан
La
```

Если вам необходимо получить список, состоящий из ключей словаря, то используйте метод `.keys()` и функцию-конструктор списка `list()`

```python
>>> keys = list(mendeleev_table.keys())
>>> print(keys)  # напечатает список элементов на русском языке
['кислород', 'гелий', 'дубний', 'золото', 'скандий', 'иттрий', 'лантан']
```

Важно использовать `list()` так как функция `.keys()` возвращает не список, а специальную структуру данных - `dict_keys`, которая ведет себя иначе, чем обычный список (например, нельзя взять элемент по индексу):

```python
>>> print(mendeleev_table.keys())
dict_keys(['кислород', 'гелий', 'дубний', 'золото', 'скандий', 'иттрий', 'лантан'])
>>> mendeleev_table.keys()[0]
TypeError: 'dict_keys' object does not support indexing
```

Есть и второй способ получить все ключи словаря без использования функции `.keys()`:

```python
>>> keys = list(mendeleev_table) # просто построим список из словаря
>>> print(keys)
['кислород', 'гелий', 'дубний', 'золото', 'скандий', 'иттрий', 'лантан']
```

Работает это примерно по тому же принципу, что и проход циклом for: `for key in dictionary`.

## Перебор значений

Для перебора значений словаря используется метод `.values()`, который по своему поведению аналогичен `.keys()`:

```python
>>> for symbol in mendeleev_table.values():
... 	print(symbol)  # напечатает значение, в нашем случае символ элемента
O
He
Db
Au
Sc
Y
La
>>> values = list(mendeleev_table.values())  # список символов элементов
>>> print(values)
['O', 'He', 'Db', 'Au', 'Sc', 'Y', 'La']
```

## Перебор пар ключ—значение

Выше уже было показано, как можно перебрать все ключи и значения в одном цикле. Однако то же самое можно сделать более лаконичным образом с помощью метода `.items()`:

```python
>>> # Еще один пример, для нашего словаря с химическими элементами:
>>> for ru_name, symbol in mendeleev_table.items():
... 	print(symbol, "—", ru_name)
O — кислород
He — гелий
Db — дубний
Au — золото
Sc — скандий
Y — иттрий
La — лантан
```

## О порядке перебора словарей

Словарь является неупорядоченной структурой данных, и поэтому порядок, в котором будут выдаваться ключи или значения словаря, может изменяться от запуска к запуску программы, при использовании другой версии Python или при изменении погоды на Марсе.

> **Никогда не полагайтесь на то, что порядок значений словаря не изменится при следующем запуске программы.**

Таким образом, вы всегда должны считать, что все вышеуказанные примеры перебора словарей в цикле `for` или все списки, создаваемые с помощью методов `.keys()` и `.values()` имеют случайный порядок.

Если вам нужно запомнить порядок, в котором элементы были добавлены в словарь, то используйте [частный случай упорядоченного словаря `collections.OrderedDict`](https://docs.python.org/library/collections.html#collections.OrderedDict) (для использования необходимо сделать `import collections`).

С другой стороны, часто бывает полезно получать элементы словаря, упорядоченные по ключу или значению. Это можно сделать с помощью встроенной функции `sorted()`:

```python
>>> # Перебор по отсортированным ключам
>>> dictionary = {5: 'пять', 2: 'два', 1: 'один', 3: 'три', 4: 'четыре'}
>>> for key in sorted(dictionary):
... 	print(key, dictionary[key])  # печатает пару ключ-значение, ключи упорядочены
1 один
2 два
3 три
4 четыре
5 пять
```

```python
>>> # Перебор отсортированных значений
>>> for value in sorted(dictionary.values()):
... 	print(value)  # ключи мы не знаем, печатаем только значения (в алфавитном порядке)
два
один
пять
три
четыре
```

А вот так можно пройтись по всем ключам словаря, отсортированным по значениям этих ключей

```python
>>> for key in sorted(dictionary, key=dictionary.get):
... 	print(key, dictionary[key])
2 два
1 один
5 пять
3 три
4 четыре
```

> Нужно отметить, что начиная с версии питона 3.7, вышедшей в июне 2018, стандартом языка гарантировано, что словари упорядочены по порядку добавления в них элемента, то есть работают как `collections.OrderedDict`. Тем не менее, даже если вы используете на своём компьютере новую версию питона, это не гарантирует вам, что ваш код не будет запущен с использованием интерпретатора более старой версии, так что полагаться на порядок элементов при переборе всё равно не стоит.


# Пример: частотный словарь

Напишем простую функцию, которой передается список слов, а она возвращает словарь с парами слово—частота его встречаемости.

```python
def word_freq(words):
    """Возвращает словарь {слово: частота} на основе списка слов words"""
    freq = {}  # создаем пустой словарь
    for word in words:
       if word in freq:  # если слово уже входит в словарь, то увеличим соответствующую частоту
           freq[word] += 1 / len(words)
       else:  # если не входит, то создадим новую запись в словарь
           freq[word] = 1 / len(words)
    return freq
```

Этот пример является учебным, на практике гораздо удобнее использовать вместо `dict` специальный его случай [`collections.Counter`](https://docs.python.org/library/collections.html#collections.Counter). Он умеет автоматически из списка создавать словарь, в котором ключами являются элементы списка, а значениями — количество их повторов.

```python
import collections

words = ...
counter = collections.Counter(words)
```

# На чём потренироваться

1. Постройте частотный список (словарь {слово: частотность}) для текста из файла, вывести `n` самых часто употребляемых слов.
2. Порядок слов в языках мира. Скачайте таблицу [Feature 81A из WALS](http://wals.info/feature/81A) (кнопка для скачивания таблицы в формате tab-separated values — на странице слева). Какой порядок слов в скольких языках является доминантным? Нужно воспроизвести таблицу как на странице с картой (Values). Какие теоретически возможные порядки слов не встречаются ни разу?




# Домашнее задание

Программа с помощью отдельной функции принимает от пользователя название файла с английским текстом, читает этот файл и сообщает (по вариантам):

1. сколько в тексте форм на -ing (слова типа during и ring можно тоже считать) и сколько раз в тексте встречается форма на -ing, введенная пользователем.
2. сколько в тексте разных существительных с суффиксом -ness и какое существительное из них имеет максимальную частотность.
3. сколько в тексте существительных с суффиксом -hood, какое существительное имеет минимальную частотность, а также выводит слова, от которых эти существительные образованы (например, если нашлось childhood, то нужно напечатать child, и так для всех слов с этим суффиксом).
4. сколько в тексте форм на -ed, а также сколько из них образованы от глаголов на -y  или -e (например, studied, lied). Омонимией типа red можно не заморачиваться.
5. сколько в тексте прилагательных с суффиксом -ous, и какая средняя длина такого прилагательного.
6. какова частотность слов с приставкой omni- в тексте и частотность слов без приставки omni- (то есть, например, сообщает, сколько раз в тексте встречается слово omnibenevolent и сколько раз встречается слово benevolent, и так для всех слов с приставкой omni-)
7. сколько в тексте слов с приставкой un- (слова типа uncle тоже можно считать) и какой процент из них имеет длину большую, чем число, введенное пользователем (если пользователь ввел число 5, то какой процент слов с приставкой un- длиннее 5 символов).

Весь код должен быть в функциях. Например, чтение файла — одна функция, поиск нужных слов — другая функция, подсчет частотности слова, которое ввел пользователь — третья функция, и т.д. Тестировать можно, например, на [«Гордости и предубеждении»](https://raw.githubusercontent.com/pykili/pykili.github.io/master/content/Pride_and_Prejudice.txt).

**Выполните задание пройдя по ссылке в GitHub Classroom:**

- 1 группа: дедлайн 01 февраля 23:55 <https://classroom.github.com/a/WFHIkDaE>
- 2 группа: дедлайн 27 января 23:55 <https://classroom.github.com/a/fglFuAtl>
- 3 группа: дедлайн 27 января 23:55 <https://classroom.github.com/a/jk1hg7UM>
- 4 группа: дедлайн 29 января 23:55 <https://classroom.github.com/a/dxoCv1Zv>