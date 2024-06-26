## Хэшируемы объекты в качестве ключей
В Python ключи словаря должны быть хэшируемыми объектами. Хэшируемые объекты - это объекты, у которых можно вычислить хэш-значение, и они неизменяемы (**immutable**). Такие объекты включают в себя строки, числа и кортежи.

Использование неизменяемых объектов в качестве ключей словаря обеспечивает эффективность поиска и доступа к элементам словаря, поскольку хэш-значение ключа используется для быстрого определения местоположения значения в памяти.

Если попытаться использовать изменяемый объект в качестве ключа, например, список, Python выдаст ошибку `TypeError`, поскольку списки изменяемы и не могут быть использованы в качестве ключей словаря.

## dir(some_obj) vars(some_obj)" 
`dir(some_obj)` возвращает список строк, который представляет атрибуты объекта. Точнее- это список строк имен атрибутов.

Атрибуты объекта, которые вы увидите в выводе `dir(obj)`, включают методы и переменные экземпляра (если `obj` - это экземпляр класса). Эти атрибуты включают в себя все доступные атрибуты объекта, включая методы, атрибуты экземпляра и атрибуты, унаследованные от его классов.

Получить словарь атрибутов объекта можно, используя `vars(obj)`. Ключи этого словаря - это имена атрибутов объекта, а значения - сами атрибуты.

## Отображение объекта - ключа.
`dict1 = {obj1:42}`.  
При использовании объекта в качестве ключа в словаре, Python использует его хэш-значение для определения его местоположения в хэш-таблице, которая является основной структурой данных для реализации словаря.

Когда вы вызываете `print(dict1)`, Python итерирует по парам ключ-значение в словаре `dict1` и выводит их. В этом случае, если объект obj1 является ключом, то Python использует его хэш-значение для быстрого поиска и доступа к соответствующему значению в словаре `dict1`.

При выводе словаря `dict1` Python обычно не выводит имена переменных в качестве ключей. Вместо этого, Python пытается преобразовать ключи в их строковое представление (если это возможно) и отображает это строковое представление.

Если объект `obj1` является экземпляром класса, то по умолчанию Python использует метод `__str__()` или `__repr__()` этого объекта для получения строки, которая будет использоваться в качестве ключа в выводе словаря. Если эти методы не определены явно, Python использует значения по умолчанию для встроенных типов.

Например, если объект `obj1` является экземпляром пользовательского класса `SomeClass` и в этом классе определен метод `__repr__()`, то Python использует результат вызова этого метода для представления ключа в выводе словаря `dict1`.

Если же ни `__str__()` ни `__repr__()` не определены, Python может использовать адрес памяти объекта в качестве идентификатора ключа.

## Сравнение объектов
При сравнении `obj1 == obj2` в Python используется метод `__eq__()`. Когда вы выполняете операцию сравнения двух объектов с помощью оператора `==`, Python вызывает метод `__eq__()` одного из объектов и передает в качестве аргумента другой объект.

По умолчанию, если метод `__eq__()` не определен в классе, он наследуется от базового класса object, который реализует сравнение объектов по их адресам в памяти, то есть сравнивает объекты по их идентификаторам.

Если вы хотите изменить поведение операции сравнения для ваших объектов, вы можете определить метод `__eq__()` в вашем классе и определить, какие критерии должны считаться равными для экземпляров вашего класса.

## Различие методов `__str__` и `__repr__`
Методы `__str__()` и `__repr__()` в Python предназначены для представления объектов в строковом формате, но имеют разные цели и использования:

**`__str__():`**

`__str__()` предназначен для создания "официального" представления объекта в виде строки, которое было бы человекочитаемым.
Обычно используется для вывода объектов в простом и понятном формате.
Этот метод вызывается функциями `str()` и `print()`.
Пример использования `__str__()`: Если у вас есть класс `Person` с атрибутами `name` и `age`, метод `__str__()` может быть реализован для возврата строки, которая содержит имя и возраст человека в удобочитаемом формате.

**`__repr__():`**

`__repr__()` предназначен для создания "неформального" или "представительного" представления объекта в виде строки, которое можно использовать для воссоздания объекта или его представления в коде Python.
Обычно используется для отладки и разработки, а также для предоставления информации о состоянии объекта.
Этот метод вызывается функцией `repr()`, а также используется интерактивным интерпретатором Python для отображения значений выражений.
Пример использования `__repr__()`: Метод `__repr__()` для класса Person может вернуть строку, содержащую информацию о классе и его атрибутах, которую можно использовать для создания нового объекта Person с теми же значениями атрибутов.

В целом, `__str__()` предназначен для представления объекта в удобном для чтения формате, в то время как `__repr__()` предназначен для создания строкового представления, которое может быть использовано для воссоздания объекта.

## Dummy repr / str / hash / eq
Имеем такой класс пустышку:
```python
class Dummy:
    def __init__(self, name):
        self.name = name

    def __str__(self):
        return f'{self.name}'

    def __repr__(self):
        return 'dummy_class'

    def __hash__(self):
        return hash(self.name)

    def __eq__(self, other):
        # Определяем логику сравнения
        if isinstance(other, Dummy):
            # Сравниваем объекты по длине имени
            if 1 <= len(self.name) <= 10 and 1 <= len(other.name) <= 10:
                return True
            elif 11 <= len(self.name) <= 20 and 11 <= len(other.name) <= 20:
                return True
            elif len(self.name) >= 21 and len(other.name) >= 21:
                return True
        return False
    
obj1 = Dummy('first')
obj2 = Dummy('second')

print("obj1:", obj1)  # Выведет результат работы __str__()
print("obj2:", obj2)  # Выведет результат работы __str__()

# Хэши объектов
print("Хэш obj1:", hash(obj1))
print("Хэш obj2:", hash(obj2))

dict1 = {obj1:1,obj2:2}

print(dict1)
```

Задается аттрибут имени. Хэширование - по имени объекта. Метод `__eq__` сравнивает объекты на основе длины их имени и возвращает `True`, если длина имени обоих объектов находится в одном десятке (1-10, 11-20, более 20), иначе возвращает `False`.

вывод:
```cmd
obj1: first
obj2: second
Хэш obj1: 8425721272306820815
Хэш obj2: 8673512251496676389
{dummy_class: 1, dummy_class: 2}
```

Класс `Dummy` реализован так, что он имеет методы `__str__()`, `__repr__()`, `__hash__()` и `__eq__()`. Давайте рассмотрим ваш вывод:

Вывод `print("obj1:", obj1)` и `print("obj2:", obj2)`:  
Поскольку определен метод `__str__()`, вывод `print()` использует именно его для строкового представления объектов `obj1` и `obj2`. Поэтому в вывод команды `print()` пападают их имена, переданные при создании объектов.

Вывод `print("Хэш obj1:", hash(obj1))` и `print("Хэш obj2:", hash(obj2))`:  
Метод `__hash__()` используется для вычисления хэш-значения объектов. Здесь отображаются их хэши, которые определяются по их именам.

Словарь `dict1` содержит две пары: `{obj1: 1, obj2: 2}`. Однако, при выводе Python используется метод `__repr__()` для ключей. В данном случае, поскольку ключи это объекты класса `Dummy` и для них определен метод `__repr__()`, Python выводит строковое представление, возвращаемое этим методом - `'dummy_class'`- вывод словаря на печать- `print(dict1)` -> `{dummy_class: 1, dummy_class: 2}`.

## Оператор IN
Пример с несколькими объектами пустышками:
```python
obj1 = Dummy('first')
obj2 = Dummy('second')
obj3 = Dummy('third')
obj4 = Dummy('fourth')

objects = [obj1,obj2,obj3]

print('obj4 is in [obj1,obj2,obj3]:',obj4 in objects)
```
вывод:
```cmd
obj4 is in [obj1,obj2,obj3]: True
```
При вызове инструкции `in` для объектов `obj1, obj2, и obj3`, Python будет использовать метод `__eq__()` для сравнения объекта `obj4` с каждым элементом списка `objects`.

Применительно к созданному ранее классу этот метод сравнивает объекты на основе их длины имени и возвращает `True`, если длина имени обоих объектов находится в одном десятке (1-10, 11-20, более 20), иначе возвращает `False`.

## Проверка на вхождение в список на основе `is` вместо `__eq__`
Вы можете создать функцию или декоратор для реализации функционала проверки на вхождение в список на основе `is` вместо `__eq__`. Вот пример функции, которая проверяет наличие объекта в списке с использованием `is` вместо `==`:
```python
def is_in_list(obj, obj_list):
    return any(obj is item for item in obj_list)
```

## Ограничения на создание операторов
Операторы в Python имеют фиксированное значение, и вы не можете создавать собственные операторы или менять их семантику без модификации ядра языка.

В Python нельзя напрямую переопределить синтаксис операторов, таких как например выдуманный `INS`, для использования в виде `obj2 INS objects_list` в контексте реализации функционала проверки на вхождение в список на основе `is` вместо `__eq__`.

## Сравнение словарей
Сравнение элементов словарей, как уже выяснено выше, производится с использованием интерфейсных методов `__eq__` и `__hash__`.  
Для сложноорганизованных пользовательских классов прямое сравнение может дать неоднозначный результат.

Ниже приведен класс для более подробного разбора состава словарей с выводом информации которую дают функции `id(), hash(), type(), repr()` для ключа и для значения. Далее приведен пример с использованием объектов кастомного класса в качестве ключей (при этом объекты также могут быть использованы и для значений, но этот кейс здесь не рассматривается).

Для вывода сообщений используется кастомный класс `MyLogger` на основе библиотеки `logging` (код класса находится в файле [mylog.py](https://github.com/kogriv/chicago_spark/blob/master/ChiSpark/enviserv/mylog.py) в другом моем репозитории `chicago_spark/ChiSpark/enviserv/mylog.py`)

```python
class DictAnalyzer:
    """
    A class for analyzing and comparing dictionaries.
    """

    def __init__(self, logger, llev=30):
        """
        Initialize a DictAnalyzer object.

        Args:
            logger: An instance of a logger object.
            llev (int, optional): Logging level. Defaults to 30.
        """
        self.logger = logger
        self.llev = llev

    def dict_info(self, dictionary, verbose=False):
        """
        Generate information about the keys and values of a dictionary.

        Args:
            dictionary (dict): The dictionary to analyze.
            verbose (bool, optional): If True, print verbose information. Defaults to True.

        Returns:
            dict: A dictionary containing information about keys and values.
        """
        info_dict = {}
        if verbose:
            self.logger.mylev(self.llev, "----------------------------")
        for key, value in dictionary.items():
            key_info = {
                'value': value,
                'key_repr': repr(key),
                'value_repr': repr(value),
                'key_id': id(key),
                'key_type': type(key).__name__,
                'key_hash': hash(key), #if isinstance(key, (int, str, tuple, frozenset)) else None,
                'value_id': id(value),
                'value_type': type(value).__name__
            }
            info_dict[key] = key_info

            if verbose:
                self.logger.mylev(self.llev, f"    '{key}': {key_info},")
        if verbose:
            self.logger.mylev(self.llev, "----------------------------")
        return info_dict

    def compare_dicts_info(self, dict1, dict2, compare_type='direct'):
        """
        Compare two dictionaries and identify matching,
        similar, and unique elements.

        Args:
            dict1 (dict): The first dictionary for comparison.
            dict2 (dict): The second dictionary for comparison.
            compare_type (str, optional): The method of comparison. Can be 'direct', 'by_name', or 'by_hash'.
                Defaults to 'direct'.

            structure for dicts defined in dict_info() function:
                info_dict[key] = key_info
                key_info = {
                    'value': value,
                    'key_repr': repr(key),
                    'value_repr': repr(value),
                    'key_id': id(key),
                    'key_type': type(key).__name__,
                    'key_hash': hash(key) if isinstance(key, (int, str, tuple, frozenset)) else None,
                    'value_id': id(value),
                    'value_type': type(value).__name__
                }

        Returns:
            tuple: A tuple containing four dictionaries:
                - Matching keys and values (kv).
                - Matching keys but differing values (k).
                - Unique elements from the first dictionary (d1).
                - Unique elements from the second dictionary (d2).
        """
        kv = {} # matching pairs by key and value
        k = {}  # matching keys, but not in kv keys
        d1 = {} # unique part of the 1st dictionary where keys are not in kv or k
        d2 = {} # unique part of the 2nd dictionary

        if compare_type == 'direct':
            """
            comparing keys directly from dictionaries,
            comparing values for 'value' elements
            """
            for key1, info1 in dict1.items():
                if key1 in dict2:
                    info2 = dict2[key1]
                    if info1['value'] == info2['value']:
                        kv[key1] = {'info1': info1, 'info2': info2}
                    else:
                        k[key1] = {'info1': info1, 'info2': info2}
                else:
                    d1[key1] = info1

            for key2, info2 in dict2.items():
                if key2 not in kv and key2 not in k:
                    d2[key2] = info2

        if compare_type == 'by_name':
            """
            comparing keys for 'key_repr' elements,
            comparing values for 'value_repr' elements
            """
            for key1, info1 in dict1.items():
                key2_matched = False
                for key2, info2 in dict2.items():
                    if info1['key_repr'] == info2['key_repr']:
                        key2_matched = True
                        if info1['value_repr'] == info2['value_repr']:
                            kv[key1] = {'info1': info1, 'info2': info2}
                        else:
                            k[key1] = {'info1':info1,'info2':info2}
                
                if not key2_matched:
                    d1[key] = info1
                    
            
            for key2, info2 in dict2.items():
                key1_matched = False
                for key1, info1 in dict1.items():
                    if info2['key_repr'] == info1['key_repr']:
                        key1_matched = True
                if not key1_matched:
                    d2[key2] = info2

        elif compare_type == 'by_hash':
            """
            comparing keys for 'key_hash' elements,
            comparing values for 'value_id' elements
            """
            for key1, info1 in dict1.items():
                key2_matched = False
                for key2, info2 in dict2.items():
                    if info1['key_hash'] == info2['key_hash']:
                        key2_matched = True
                        if info1['value_id'] == info2['value_id']:
                            kv[key1] = {'info1': info1, 'info2': info2}
                        else:
                            k[key1] = {'info1':info1,'info2':info2}
                
                if not key2_matched:
                    d1[key1] = info1
            
            for key2, info2 in dict2.items():
                key1_matched = False
                for key1, info1 in dict1.items():
                    if info2['key_hash'] == info1['key_hash']:
                        key1_matched = True
                if not key1_matched:
                    d2[key2] = info2

        return kv, k, d1, d2


if __name__ == '__main__':
    from mylog import MyLogger

    envilog = MyLogger('envilog', 30)

    # Создание экземпляра класса DictAnalyzer с передачей логгера и уровня логирования
    dictan = DictAnalyzer(envilog)

    print("sample sample of sample dicts")
    d01 = {'a': 1, 'b': 2, 'c': 3}
    d02 = {'b': 2, 'c': 4, 'd': 5}
    # Пример использования методов класса DictAnalyzer
    dict1 = dictan.dict_info(d01)
    dict2 = dictan.dict_info(d02)
    """
    envilog.mylev(30,"dict1:")
    envilog.mylev(30,dict1)

    envilog.mylev(30,"dict2:")
    envilog.mylev(30,dict2)
    """

    kv, k, d1, d2 = dictan.compare_dicts_info(dict1, dict2)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,d01)
    envilog.mylev(30,d02)
    envilog.mylev(30,"-----------direct comparison------------")
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"kv:")
    envilog.mylev(30,kv)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"k:")
    envilog.mylev(30,k)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"d1:")
    envilog.mylev(30,d1)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"d2:")
    envilog.mylev(30,d2)

    print("------------------------------------------")
    print("example of a dictionary with complex objects")
    class Dummy:
        def __init__(self, name):
            self.name = name

        def __str__(self):
            # return f'{self.name}'
            return 'dummy_class'

        def __repr__(self):
            #return 'dummy_class'
            return f'{self.name}'

        def __hash__(self):
            # return hash(self.name)
            # Сравниваем объекты по длине имени
            if 1 <= len(self.name) <= 10:
                return hash('ten_letters')
            elif 11 <= len(self.name):
                return hash('twenty_letters')
            elif len(self.name) >= 21:
                return hash('over_twenty_letters')
            return hash('hash')

        def __eq__(self, other):
            # Определяем логику сравнения
            if isinstance(other, Dummy):
                # Сравниваем объекты по длине имени
                if 1 <= len(self.name) <= 10 and 1 <= len(other.name) <= 10:
                    return True
                elif 11 <= len(self.name) <= 20 and 11 <= len(other.name) <= 20:
                    return True
                elif len(self.name) >= 21 and len(other.name) >= 21:
                    return True
            return False
    
    obj1 = Dummy('first')
    obj2 = Dummy('second')
    obj3 = Dummy('more_ten_letters')
    obj4 = Dummy('more_than_twenty_letters')
    obj4 = Dummy('another_one_long_letters')

    d01 = {obj1: 1, obj2: 2, obj3: 3}
    d02 = {obj2: 2, obj3: 4, obj4: 5}
    # Пример использования методов класса DictAnalyzer
    dict1 = dictan.dict_info(d01)
    dict2 = dictan.dict_info(d02)

    kv, k, d1, d2 = dictan.compare_dicts_info(dict1, dict2)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"--------represent using logging---------")
    envilog.mylev(30,d01)
    envilog.mylev(30,d02)
    print("--------represent using print---------")
    print(d01)
    print(d02)
    envilog.mylev(30,"-----------direct comparison------------")
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"kv:")
    envilog.mylev(30,kv)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"k:")
    envilog.mylev(30,k)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"d1:")
    envilog.mylev(30,d1)
    envilog.mylev(30,"----------------------------------------")
    envilog.mylev(30,"d2:")
    envilog.mylev(30,d2)
```
файл с классом - по ссылке [dictan.py](https://github.com/kogriv/chicago_spark/blob/master/ChiSpark/enviserv/dictan.py) в моем гит-репозитории `chicago_spark/ChiSpark/enviserv/dictan.py`

видим:
```cmd
sample sample of sample dicts
----------------------------------------
{'a': 1, 'b': 2, 'c': 3}
{'b': 2, 'c': 4, 'd': 5}
-----------direct comparison------------
----------------------------------------
kv:
{'b': {'info1': {'value': 2, 'key_repr': "'b'", 'value_repr': '2', 'key_id': 2022928602736, 'key_type': 'str', 'key_hash': -3182570623678059912, 'value_id': 2022923763984, 'value_type': 'int'}, 'info2': {'value': 2, 'key_repr': "'b'", 'value_repr': '2', 'key_id': 2022928602736, 'key_type': 'str', 'key_hash': -3182570623678059912, 'value_id': 2022923763984, 'value_type': 'int'}}}
----------------------------------------
k:
{'c': {'info1': {'value': 3, 'key_repr': "'c'", 'value_repr': '3', 'key_id': 2022928522096, 'key_type': 'str', 'key_hash': 8408648107562576778, 'value_id': 2022923764016, 'value_type': 'int'}, 'info2': {'value': 4, 'key_repr': "'c'", 'value_repr': '4', 'key_id': 2022928522096, 'key_type': 'str', 'key_hash': 8408648107562576778, 'value_id': 2022923764048, 'value_type': 'int'}}}
----------------------------------------
d1:
{'a': {'value': 1, 'key_repr': "'a'", 'value_repr': '1', 'key_id': 2022928837424, 'key_type': 'str', 'key_hash': -4646032095328237722, 'value_id': 2022923763952, 'value_type': 'int'}}
----------------------------------------
d2:
{'d': {'value': 5, 'key_repr': "'d'", 'value_repr': '5', 'key_id': 2022928521840, 'key_type': 'str', 'key_hash': -7120466352967853455, 'value_id': 2022923764080, 'value_type': 'int'}}
------------------------------------------
example of a dictionary with complex objects
----------------------------------------
--------represent using logging---------
{first: 2, more_ten_letters: 3}
{second: 2, more_ten_letters: 4, another_one_long_letters: 5}
--------represent using print---------
{first: 2, more_ten_letters: 3}
{second: 2, more_ten_letters: 4, another_one_long_letters: 5}
-----------direct comparison------------
----------------------------------------
kv:
{first: {'info1': {'value': 2, 'key_repr': 'first', 'value_repr': '2', 'key_id': 2020785873552, 'key_type': 'Dummy', 'key_hash': 8713895452076597488, 'value_id': 2022923763984, 'value_type': 'int'}, 'info2': {'value': 2, 'key_repr': 'second', 'value_repr': '2', 'key_id': 2020785872688, 'key_type': 'Dummy', 'key_hash': 8713895452076597488, 'value_id': 2022923763984, 'value_type': 'int'}}}
----------------------------------------
k:
{more_ten_letters: {'info1': {'value': 3, 'key_repr': 'more_ten_letters', 'value_repr': '3', 'key_id': 2020785868608, 'key_type': 'Dummy', 'key_hash': 2691375408090906058, 'value_id': 2022923764016, 'value_type': 'int'}, 'info2': {'value': 4, 'key_repr': 'more_ten_letters', 'value_repr': '4', 'key_id': 2020785868608, 'key_type': 'Dummy', 'key_hash': 2691375408090906058, 'value_id': 2022923764048, 'value_type': 'int'}}}      
----------------------------------------
d1:
{}
----------------------------------------
d2:
{another_one_long_letters: {'value': 5, 'key_repr': 'another_one_long_letters', 'value_repr': '5', 'key_id': 2020785873648, 'key_type': 'Dummy', 'key_hash': 2691375408090906058, 'value_id': 2022923764080, 'value_type': 'int'}}
```

для простых объектов в качестве ключей-значений получаем понятный результат.

Для объектов классов с неочевидной реализацией интерфейсных методов видим неоднозначный результат.

Выводы делайте сами, мой вывод мог бы быть таким, например: сравнивать можно (а иногда и нужно) по разным аттрибутам объектов.

## Обзор библиотек для сравнения словарей
Существует несколько популярных библиотек в Python, которые предоставляют функционал для анализа и сравнения словарей. Вот некоторые из них:

**DeepDiff:** Это библиотека Python, которая предоставляет мощные инструменты для сравнения объектов Python, включая словари. DeepDiff позволяет выявлять различия вложенных структур данных, таких как словари, списки и множества, и предоставляет детальные отчеты о различиях между ними.

**Dictdiffer:** Это еще одна библиотека Python, специализирующаяся на сравнении словарей. Dictdiffer позволяет находить различия между двумя словарями и предоставляет удобный интерфейс для итерации по этим различиям.
Это модуль в стандартной библиотеке Python, который позволяет сравнивать словари и находить различия между ними. Он предоставляет функции для создания объектов-сравнивателей, которые могут использоваться для сравнения словарей и выявления изменений между ними.

Вот простой пример использования модуля `dictdiffer` для сравнения двух словарей и выявления различий между ними:
```python
from dictdiffer import diff, patch, swap, revert

# Исходные словари для сравнения
dict1 = {'a': 1, 'b': 2, 'c': 3}
dict2 = {'b': 2, 'c': 4, 'd': 5}

# Находим различия между словарями
d = diff(dict1, dict2)

# Выводим найденные различия
print("Различия между словарями:")
for diff_type, key, value in d:
    print(diff_type, key, value)

# Применяем различия к одному из словарей (например, к dict1)
dict1_updated = patch(d, dict1)

# Выводим обновленный словарь
print("\nОбновленный словарь dict1 после применения различий:")
print(dict1_updated)

# Возвращаемся к изначальному состоянию
dict1_original = revert(d, dict1_updated)

# Выводим исходный словарь dict1
print("\nВозвращенный к изначальному состоянию словарь dict1:")
print(dict1_original)
```
Этот код сначала находит различия между `dict1` и `dict2`, затем применяет найденные различия к одному из словарей, а затем возвращает его к изначальному состоянию.

Обратите внимание, что `diff()` возвращает генератор различий между двумя словарями. `patch()` применяет найденные различия к словарю, а `revert()` отменяет изменения, возвращая словарь к изначальному состоянию.

## Принцип сравнения объектов в `dictdiffer`
Модуль `dictdiffer` ищет различия между словарями на основе их содержимого. По умолчанию `dictdiffer` сравнивает значения элементов словарей непосредственно между собой.

Вот как происходит процесс сравнения:

**Содержимое значений:** `dictdiffer` сравнивает значения, находящиеся по одним и тем же ключам в обоих словарях. Если значения не совпадают, `dictdiffer` определяет, что это различие.

**Ключи:** Если в одном из словарей есть ключ, отсутствующий в другом словаре, то это также считается различием.

**Структура:** `dictdiffer` учитывает структуру словарей, включая вложенные словари и списки. Он анализирует каждый элемент словарей и рекурсивно ищет различия.

**Типы данных:** `dictdiffer` учитывает типы данных значений, так что даже если значения одного и того же ключа имеют разные типы данных (например, строка и число), это будет считаться различием.

Поэтому, при использовании `dictdiffer`, важно помнить, что сравнение происходит на основе структуры и содержимого словарей, а не их хэшей, имен или идентификаторов.

В библиотеке `dictdiffer` для сравнения значений используется метод `__eq__` объектов.

Когда `dictdiffer` сравнивает значения по ключам в словарях, он вызывает метод `__eq__` для сравнения значений. Если метод `__eq__` не определен для конкретных объектов или возвращает `False`, `dictdiffer` может считать эти значения различными.

Это означает, что поведение сравнения зависит от того, как реализован метод `__eq__` для объектов, которые хранятся в словарях. Если объекты правильно реализуют метод `__eq__`, который определяет равенство на основе содержимого объектов, то dictdiffer будет использовать это для определения различий.

В случае, если для объектов не определен метод `__eq__`, будет использоваться стандартная реализация сравнения объектов, которая сравнивает их **идентификаторы** (`id`). Это может привести к **сравнению по ссылкам на объекты**, а не по их содержимому.

Таким образом, для корректной работы сравнения в `dictdiffer` важно убедиться, что объекты, хранимые в словарях, правильно реализуют метод `__eq__`, чтобы он корректно сравнивал их содержимое.

## "Заморозка" (хэширование) словаря
 Объекты типа `dict` в Python не могут быть использованы в качестве ключей в другом словаре или в множестве. Это происходит потому, что объекты типа `dict` не являются хешируемыми.

 Теоретически вы можете создать подкласс типа `dict` и переопределить метод `__hash__()` в этом подклассе. Однако, такой подход может привести к непредсказуемым результатам и нарушить ожидания других частей вашего кода, работающих со стандартными словарями.

Важно понимать, что хешируемость словарей в Python основана на содержимом их ключей, а не на содержимом значений. Если вы меняете способ, как хешируются словари, это может привести к несогласованному поведению в вашем коде.

При переопределении метода `__hash__()` в классе-подклассе `dict` следует быть осторожным и учитывать, что такое изменение может повлиять на операции хеширования и использования словарей в вашей программе.

Вместо этого, если вам нужно хешировать сложные структуры данных, включая словари, рекомендуется использовать альтернативные методы, такие как использование `frozenset` для хешируемых коллекций или создание хеш-функций специально для ваших пользовательских классов данных.

`frozenset` в Python не может содержать изменяемые типы данных, такие как словари, поэтому попытка создать `frozenset({1: 1, 2: 2})` вызовет ошибку типа. Однако, вы можете создать `frozenset` из неизменяемых типов данных, таких как список, кортеж или другой frozenset. Например:

```python
# dict = {1: 1, 2: 2} -> frozenset
frozen_dict = frozenset({(1, 1), (2, 2)})
```
Как только у вас есть `frozenset`, вы не сможете прочитать элементы, так как `frozenset` является неупорядоченным и не поддерживает доступ к элементам по индексу. Вам нужно будет использовать другие методы для работы с `frozenset`.

Вы можете создать неизменяемую копию словаря, используя `tuple` для хранения пар ключ-значение, но это потребует дополнительного кода.

Вот пример, как можно создать неизменяемый словарь с помощью `frozenset`:

```python
my_dict = {'a': 1, 'b': 2, 'c': 3}
frozen_dict = frozenset(my_dict.items())
```

В этом примере `frozenset(my_dict.items())` создает неизменяемое множество кортежей, представляющих пары ключ-значение из исходного словаря `my_dict`.

Однако, этот подход не является идеальным, так как он **не сохраняет порядок элементов словаря**, и вы не сможете легко восстановить исходную структуру словаря из `frozenset`.

Если вам действительно нужен неизменяемый словарь с возможностью восстановления его структуры, вам может потребоваться написать собственный класс, который будет реализовывать эту функциональность.

## Sl
