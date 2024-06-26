## dir(), globals(), locals()
Давайте рассмотрим, какие инструкции могут помочь вам понять области видимости и принцип замыкания.

1. `locals()` и `globals()` функции:  
- `locals()` возвращает словарь локальных символов в текущей области видимости.
- `globals()` возвращает словарь глобальных символов.  

2. dir() функция:  
- `dir()` возвращает отсортированный список имен в текущей локальной области видимости.


Пример:
```python
def say_name(name):
    def say_goodbye():
        print("Don't say me goodbye, " + name + "!")

    return say_goodbye

f = say_name("Ivan")

print("Global scope dict:", globals())
print("Local scope dict:", locals())
print("Local scope list:", dir())
```

Теперь, касательно замыкания (**closure**), важно понимать, что вложенная функция `say_goodbye` сохраняет доступ к переменной name, даже после завершения вызывающей функции `say_name`. Это происходит потому, что в момент создания функции `say_goodbye`, она "захватывает" (**captures**) переменную name из внешней функции `say_name`. Таким образом, say_goodbye является замыканием, так как она "замыкает" в себе значение `name` из внешней области видимости.

С использованием `dir(f)` вы сможете просмотреть область видимости для переменной `f`, и увидеть, какие имена связаны с этой переменной.
```python
# Используем dir() для просмотра области видимости переменной f
print("Scope list of variable 'f':", dir(f))
```

## Просмотр dir(замыкание)
Для инструкции `print("Scope list of variable 'f':", dir(f))` (`f`- результат замыкания) имеем вывод:
```cmd
Scope list of variable 'f': ['__annotations__', '__builtins__',  
'__call__', '__class__', '__closure__', '__code__', '__defaults__',  
'__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__',  
'__ge__', '__get__', '__getattribute__', '__globals__', '__gt__',  
'__hash__', '__init__', '__init_subclass__', '__kwdefaults__', '__le__',  
'__lt__', '__module__', '__name__', '__ne__', '__new__', '__qualname__',  
'__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__',  
'__str__', '__subclasshook__']
```

Из вывода `dir(f)` можно выделить несколько атрибутов, связанных с внутренней функцией `say_goodbye` и переменной `name`. Вот некоторые из них:

`__closure__`: Этот атрибут содержит **КОРТЕЖ**, представляющий замыкание (**closure**). В данном случае, вы можете просмотреть содержимое `__closure__`, чтобы увидеть, какие переменные замыкаются внутри функции `say_goodbye`. Ваш случай должен содержать значение (`'Ivan',`).  
`__code__`: Этот атрибут предоставляет объект кода для функции. Вы можете использовать `__code__.co_varnames` для просмотра локальных переменных функции. Ваш случай должен содержать (`'name',`).  
`__globals__`: Этот атрибут ссылается на глобальные переменные доступные внутри функции. Ваш случай должен содержать глобальные переменные.

Из полученных данных можно сделать следующие выводы:  
**Closure (`__closure__`):**  
Вы видите, что `__closure__` содержит **кортеж** с одним элементом. Этот элемент - объект типа `<cell at 0x...: str object at 0x...>`. Здесь `str object` - это тип объекта (строка), и адрес `0x...` представляет место в памяти, где хранится значение переменной name (в данном случае, `"Ivan"`).

**Local variables (`__code__.co_varnames`):**  
**Кортеж** имен переменных. В этом случае локальных переменных внутри функции `say_goodbye` нет, поэтому `co_varnames` пуст. Поместив в тело вложенной функции переменную, напрмер `nested_var = 'some_value_for_nested_var'`, в выводе мы увидим: `Local variables f.__code__.co_varnames: ('nested_var',)`

**Globals (`__globals__`):**  
**Словарь.** Глобальные переменные в данном случае содержат информацию о модуле, в котором определена функция. `__name__`, `__file__`, и другие глобальные переменные относятся к модулю, в котором выполняется скрипт.

## `__code__` описание составляющих.
**Code (`__code__`)**:  
**Объект** кода, который является внутренним представлением скомпилированного кода в Python.
```python
def get_variable_names(func):
    for i in dir(func.__code__):
        print(i,':',eval("func.__code__."+i))
    return func.__code__

def say_name_age(name, age):
    def say_info():
        nested_var = 'some_value_for_nested_var'
        print("nested_var:",nested_var)
        print("Name:", name)
        print("Age:", age)

    return say_info

f = say_name_age("Alice", 30)
variable_names = get_variable_names(f)
```
Вывод:
```cmd
__class__ : <class 'code'>
__delattr__ : <method-wrapper '__delattr__' of code object at 0x000001874E5CA550>
__dir__ : <built-in method __dir__ of code object at 0x000001874E5CA550>
__doc__ : Create a code object.  Not for the faint of heart.
__eq__ : <method-wrapper '__eq__' of code object at 0x000001874E5CA550>
__format__ : <built-in method __format__ of code object at 0x000001874E5CA550>
__ge__ : <method-wrapper '__ge__' of code object at 0x000001874E5CA550>
__getattribute__ : <method-wrapper '__getattribute__' of code object at 0x000001874E5CA550>
__gt__ : <method-wrapper '__gt__' of code object at 0x000001874E5CA550>
__hash__ : <method-wrapper '__hash__' of code object at 0x000001874E5CA550>
__init__ : <method-wrapper '__init__' of code object at 0x000001874E5CA550>
__init_subclass__ : <built-in method __init_subclass__ of type object at 0x00007FFC2D336BB0>
__le__ : <method-wrapper '__le__' of code object at 0x000001874E5CA550>
__lt__ : <method-wrapper '__lt__' of code object at 0x000001874E5CA550>
__ne__ : <method-wrapper '__ne__' of code object at 0x000001874E5CA550>
__new__ : <built-in method __new__ of type object at 0x00007FFC2D336BB0>
__reduce__ : <built-in method __reduce__ of code object at 0x000001874E5CA550>
__reduce_ex__ : <built-in method __reduce_ex__ of code object at 0x000001874E5CA550>
__repr__ : <method-wrapper '__repr__' of code object at 0x000001874E5CA550>
__setattr__ : <method-wrapper '__setattr__' of code object at 0x000001874E5CA550>
__sizeof__ : <built-in method __sizeof__ of code object at 0x000001874E5CA550>
__str__ : <method-wrapper '__str__' of code object at 0x000001874E5CA550>
__subclasshook__ : <built-in method __subclasshook__ of type object at 0x00007FFC2D336BB0>
co_argcount : 0
co_cellvars : ()
co_code : b'd\x01}\x00t\x00d\x02|\x00\x83\x02\x01\x00t\x00d\x03\x88\x01\x83\x02\x01\x00t\x00d\x04\x88\x00\x83\x02\x01\x00d\x00S\x00'
co_consts : (None, 'some_value_for_nested_var', 'nested_var:', 'Name:', 'Age:')
co_filename : C:\Users\user\documents\pro\chicago_spark\ChiSpark\sample.py
co_firstlineno : 23
co_flags : 19
co_freevars : ('age', 'name')
co_kwonlyargcount : 0
co_lines : <built-in method co_lines of code object at 0x000001874E5CA550>
co_linetable : b'\x04\x01\n\x01\n\x01\x0e\x01'
co_lnotab : b'\x00\x01\x04\x01\n\x01\n\x01'
co_name : say_info
co_names : ('print',)
co_nlocals : 1
co_posonlyargcount : 0
co_stacksize : 3
co_varnames : ('nested_var',)
replace : <built-in method replace of code object at 0x000001874E5CA550>
```
Вот пояснения для каждого элемента вывода:  
**`__class__`**: Класс объекта, в данном случае, это класс code.
**`__delattr__, __dir__, __doc__, __eq__, __format__, __ge__, __getattribute__, __gt__, __hash__, __init__, __init_subclass__, __le__, __lt__, __ne__, __new__, __reduce__, __reduce_ex__, __repr__, __setattr__, __sizeof__, __str__, __subclasshook__`**: Различные методы объекта `code`.  
**`co_argcount`**: Количество аргументов функции.  
**`co_cellvars`**: Переменные, которые являются ячейками замыкания вложенных функций.  
**`co_code`**: Байт-код функции.  
**`co_consts`**: Константы, используемые в функции.  
**`co_filename`**: Имя файла, содержащего код функции.  
**`co_firstlineno`**: Номер первой строки определения функции в исходном файле.  
**`co_flags`**: Флаги, указывающие наличие аргументов переменной длины, аргументов по ключевым словам и другие детали.  
**`co_freevars`**: Свободные переменные, используемые в замыканиях функции.  
**`co_kwonlyargcount`**: Количество ключевых аргументов в функции.  
**`co_lines`**: Линии кода функции.  
**`co_linetable`**: Таблица соответствия между байт-кодом и исходными строками.  
**`co_lnotab`**: Дополнительная информация о смещениях в байт-коде.  
**`co_name`**: Имя функции.  
**`co_names`**: Имена всех глобальных переменных и функций, использованных в функции.  
**`co_nlocals`**: Количество локальных переменных в функции.  
**`co_posonlyargcount`**: Количество позиционных только аргументов в функции.  
**`co_stacksize`**: Максимальный размер стека, используемый функцией.  
**`co_varnames`**: Имена локальных переменных функции.  

Это внутреннее представление скомпилированного кода в Python, которое содержит множество метаданных о функции.

## co_consts в объекте code
Объект `co_consts` в объекте code содержит константы, используемые в функции. Эти константы могут включать строки, числа, кортежи, списки, другие константы и даже функции (если они определены внутри функции).  
`co_consts : (None, 'some_value_for_nested_var', 'nested_var:', 'Name:', 'Age:')`  
`None`: Это константа None, возвращаемая, например, из функций, которые не возвращают никакого значения явно.  
`'some_value_for_nested_var'`: Это строковая константа, которая используется в функции `say_info()` в качестве значения переменной `nested_var`.  
`'nested_var:'`: Это строковая константа, которая используется для форматирования вывода в функции `say_info()`.  
'`Name:'`: Это строковая константа, также используемая для форматирования вывода в функции `say_info()`.  
`'Age:'`: Это ещё одна строковая константа, также используемая для форматирования вывода в функции `say_info()`.

Эти константы представляют собой значения, которые используются внутри функции `say_info()`. Они определяются в коде функции и сохраняются в объекте `co_consts`, чтобы их можно было использовать во время выполнения функции.

## co_flags в объекте code
Объект co_flags в объекте code представляет собой набор флагов, которые указывают на различные свойства функции. Эти флаги могут указывать на наличие аргументов переменной длины, аргументов по ключевым словам и другие особенности функции.  
`co_flags : 19`  
Количество аргументов и типы:  
`co_argcount`: Этот флаг указывает количество аргументов функции (вашем случае 0), это количество включает имена всех аргументов функции, но не включает аргументы со звездочкой и двумя звездочками.  
`co_kwonlyargcount`: Этот флаг указывает количество аргументов по ключевым словам (вашем случае 0). Это количество включает только аргументы, которые могут быть переданы только по ключевым словам и не могут быть переданы позиционно.  
**Другие флаги:**  
`CO_OPTIMIZED`: Этот флаг указывает, что функция была оптимизирована компилятором.  
`CO_NEWLOCALS`: Этот флаг указывает, что функция создает новую таблицу локальных переменных для себя.  
`CO_NOFREE`: Этот флаг указывает, что в функции нет свободных переменных (переменных из объемлющей области видимости, используемых внутри функции).  
`CO_GENERATOR`: Этот флаг указывает, что функция является генератором.
В вашем случае, число `19` в двоичном виде будет `10011`, что означает, что функция не имеет аргументов, но может иметь аргументы по ключевым словам, и функция не является генератором и не оптимизирована компилятором.

## co_freevars в объекте code
Объект `co_freevars` в объекте code представляет собой кортеж, содержащий имена свободных переменных, используемых в замыкании функции. Свободные переменные - это переменные, определенные в объемлющей области видимости и используемые внутри функции.  
`co_freevars : ('age', 'name')`  
Этот вывод указывает на то, что в вашей функции есть свободные переменные `'age'` и `'name'`. Это означает, что функция `say_info()` использует переменные `name` и `age`, которые определены в объемлющей функции `say_name_age()`. Эти переменные замыкаются внутри функции `say_info()`, что позволяет ей использовать их значения из объемлющей области видимости.

В контексте замыкания, использование свободных переменных позволяет функции сохранять доступ к значениям, которые существовали в момент определения функции, даже если эти значения становятся недоступными в момент выполнения функции.

## Используем Closure (`__closure__`) чтобы понять значения замкнутых переменных
Используя атрибут `__closure__`, вы можете получить доступ к значениям переменных, которые замыкаются. В вашем случае `__closure__` содержит кортеж с одним элементом, который представляет замыкание. Этот элемент - объект типа `<cell at 0x...: str object at 0x...>`. Давайте извлечем значение переменной name из этого объекта:
```python
# Извлекаем значение переменной name из замыкания
closure_value = f.__closure__[0].cell_contents
print("Value of closed variable 'name':", closure_value)
```
вывод:
```cmd
Value of closed variable 'name': Ivan
```
Здесь `f.__closure__[0]` предоставляет доступ к ячейке замыкания, а `cell_contents` содержит фактическое значение переменной, которая была замкнута.  
Помните, что это специфично для вашего конкретного примера, где замыкается только одна переменная name. В более сложных случаях с несколькими переменными, вам придется итерироваться по элементам `__closure__` и извлекать значения соответствующих переменных.
```python
# Извлекаем значения переменных из замыкания
closure_values = [cell.cell_contents for cell in f.__closure__]
```
## Объекты cell кортежа `__closure__`
В контексте Python `cell` - это специальный объект, который представляет собой **ячейку памяти**, используемую для реализации замыканий. Замыкание- это функция, которая запоминает значения из внешнего области видимости, в которой она была определена, даже если эта область видимости больше не существует.

Когда вы создаете замыкание в Python, интерпретатор создает объекты типа `cell` для каждой переменной, которая захватывается функцией во внешней области видимости. Эти объекты `cell` хранят ссылки на значения переменных из внешней области видимости.

Таким образом, в примере `"cell at 0x00000216EB025030"` представляет собой строку, созданную интерпретатором Python для обозначения объекта типа `cell` по его адресу в памяти. Это не является частью операционной системы, а является **внутренней механикой Python** для поддержки замыканий.

Объекты `cell`, которые содержатся в атрибуте `func.__closure__`, не имеют множества методов, поскольку они представляют собой специальный тип данных, который используется интерпретатором Python для реализации замыканий.

Объекты `cell` предоставляют только один атрибут, который может быть полезен при работе с ними:

`cell_contents`: Это атрибут, который содержит фактическое значение, захваченное замыканием. Обращаясь к `cell_contents`, вы получаете доступ к значению переменной из внешней области видимости.


## Получение переменных замыкания. Динамика областей видимости.
Воспользуемся созданным классом `DictAnalizer` (модуль `dictan.py`) для удобного сравнения словарей с областями видимости для разных этапов программы. Также воспользуемся кортежами `.__code_.co_freevars`, `.__closure__` для получения информации об области замыкания.  
Используем скрипт (`closure.py`):
```python
import dictan
from mylog import MyLogger

lg = MyLogger('scopylogger','INFO')
ll = 30

da = dictan.DictAnalyzer(lg,ll)

globals_dict_core, \
locals_dict_core = dict(globals()), dict(locals())
lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,"-Core (after import/creating serv objects)---------")
lg.mylev(ll,"-Global scope- dict(globals()):--------------------")
da.print_dict(globals_dict_core,0,False,False)
lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,"-Core (after import/creating serv objects)---------")
lg.mylev(ll,"-Local scope- dict(locals()):----------------------")
da.print_dict(locals_dict_core,0,False,False)
lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,"-Defining functions--------------------------------")

def outer_func(name,old,weight):
    lg.mylev(ll,"---------------------------------------------------")
    lg.mylev(ll,"--inside outer func--------------------------------")
    nested_var_in_outer_func = 'nested_var_in_OUTER_func_value'
    lg.mylev(ll,"outer_func for: " + str(name) + ", " +\
             str(old) + ", weight: "+ str(weight))
    globals_dict_before_closure, \
    locals_dict_before_closure = dict(globals()), dict(locals())
    lg.mylev(ll,"---------------------------------------------------")
    lg.mylev(ll,"------globals-in-outer-before-closure--------------")
    da.print_dict(locals_dict_before_closure)
    lg.mylev(ll,"---------------------------------------------------")
    lg.mylev(ll,"------locals-in-outer-before-closure---------------")
    da.print_dict(locals_dict_before_closure)
    lg.mylev(ll,"--comparing-globals-core-vs-globals-in-outer-func--")
    da.compare_dicts_info(
                        da.dict_info(globals_dict_core),
                        da.dict_info(globals_dict_before_closure),
                        False,'direct',True
                        )

    lg.mylev(ll,"---------------------------------------------------")
    lg.mylev(ll,"--comparing-locals-core-vs-locals-in-outer-func---")
    da.compare_dicts_info(
                        da.dict_info(locals_dict_core),
                        da.dict_info(locals_dict_before_closure),
                        False,'direct',True
                        )


    def nested_func():
        lg.mylev(ll,"---------------------------------------------------")
        lg.mylev(ll,"--inside nested func-------------------------------")
        lg.mylev(ll,"--using only part of outer_func params-------------")
        nested_var_in_nested_func = 'nested_var_in_NESTED_func_value'
        lg.mylev(ll,"name: " + name + ", old: " + old)
        nested_var_in_outer_func_modified_in_nested = \
            nested_var_in_outer_func + "_modified"
        nested_dict_in_nested_func = da.dict_info({'some_key':nested_var_in_outer_func_modified_in_nested})

        globals_dict_in_closure, \
        locals_dict_in_closure = dict(globals()), dict(locals())

        lg.mylev(ll,"---------------------------------------------------")
        lg.mylev(ll,"-cmprng-globals-in-outer-vs-globals-in-nested-func-")
        da.compare_dicts_info(da.dict_info(globals_dict_before_closure),
                              da.dict_info(globals_dict_in_closure),
                              False,'direct',True)
        
        lg.mylev(ll,"---------------------------------------------------")
        lg.mylev(ll,"-comparing-locals-outer-vs-locals-in-nested-func---")
        da.compare_dicts_info(da.dict_info(locals_dict_before_closure),
                              da.dict_info(locals_dict_in_closure),
                              False,'direct',True)


    return nested_func

def check_closured(func):
    if '__closure__' in dir(func):
        return True
    return False

def get_func_code_dict(func, asmbl_len=20, verbose = False):
    code_dict = {}
    if hasattr(func, '__code__'):
        for i in dir(func.__code__):
            value = getattr(func.__code__, i)
            if i in {'_co_code_adaptive', 'co_code',
                     'co_linetable', 'co_lnotab'}:
                if asmbl_len > 0:
                    value = str(value)[:asmbl_len]+"...."
            code_dict[i] = value
            if verbose:
                lg.mylev(ll,f"{i}"+': '+str(value))
    else: code_dict['empty_code_for'] = func
    return code_dict


def get_func_closure_val_dict(func, asmbl_len=20, verbose=False):
    closure_dict = {}
    if hasattr(func, '__closure__'):
        for closure_cell in func.__closure__:
            cell_address = hex(id(closure_cell))  # Получаем адрес ячейки и преобразуем его в строку
            value = closure_cell.cell_contents
            if asmbl_len > 0:
                str_value = str(value)[:asmbl_len]
                if len(str_value) == asmbl_len:
                    value = str_value + "...."
            closure_dict[cell_address] = value
            if verbose:
                print(f"{cell_address}: {value}")
    else:
        closure_dict['empty_closure_for'] = func
    return closure_dict


def get_closure_variables(func, verbose=False):
    closure_variables = {}
    if hasattr(func, '__closure__'):
        closure = func.__closure__
        if closure is not None:
            # Получение списка имен переменных из объекта функции
            var_names = func.__code__.co_freevars
            for name, cell in zip(var_names, closure):
                closure_variables[name] = cell.cell_contents
                if verbose:
                    lg.mylev(ll,f"{name}"+\
                             ': '+str(closure_variables[name]))
    return closure_variables

lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,'----f = outer_func("Ivan","30",80)-----------------')
f = outer_func("Ivan","30",80)

lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,'----f()--------------------------------------------')
f()

lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,'----f.__code__-------------------------------------')
lg.mylev(ll,f.__code__)
lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,'----get_func_code_dict(f)--------------------------')
f_code_dict = get_func_code_dict(f,20)
da.print_dict(f_code_dict)
lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,'----f.__closure__----------------------------------')
print(f.__closure__)
lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,'----get_func_closure_val_dict(f)-------------------')
f_closure_val_dict = get_func_closure_val_dict(f,50)
da.print_dict(f_closure_val_dict)
lg.mylev(ll,"---------------------------------------------------")
lg.mylev(ll,'----get_closure_variables(f)-----------------------')
f_closure_name_dict = get_closure_variables(f)
da.print_dict(f_closure_name_dict)
lg.mylev(ll,"---------------------------------------------------")


lg.mylev(ll,"---------------------------------------------------")
```
Рассмотрим вывод по частям:  
```cmd
---------------------------------------------------
-Core (after import/creating serv objects)---------
-Global scope- dict(globals()):--------------------
{
    __name__: __main__
    __doc__: None
    __package__: None
    __loader__: <_frozen_importlib_external.SourceFileLoader object at 0x00000217AC545DD0>
    __spec__: None
    __annotations__: { }
    __builtins__: <module 'builtins' (built-in)>
    __file__: C:\Users\user\documents\pro\chicago_spark\ChiSpark\closure.py
    __cached__: None
    dictan: <module 'dictan' from 'C:\\Users\\user\\documents\\pro\\chicago_spark\\ChiSpark\\dictan.py'>   
    MyLogger: <class 'mylog.MyLogger'>
    lg: <MyLogger scopylogger (INFO)>
    ll: 30
    da: <dictan.DictAnalyzer object at 0x00000217AC76BE90>
}
---------------------------------------------------
-Core (after import/creating serv objects)---------
-Local scope- dict(locals()):----------------------
{
    __name__: __main__
    __doc__: None
    __package__: None
    __loader__: <_frozen_importlib_external.SourceFileLoader object at 0x00000217AC545DD0>
    __spec__: None
    __annotations__: { }
    __builtins__: <module 'builtins' (built-in)>
    __file__: C:\Users\user\documents\pro\chicago_spark\ChiSpark\closure.py
    __cached__: None
    dictan: <module 'dictan' from 'C:\\Users\\user\\documents\\pro\\chicago_spark\\ChiSpark\\dictan.py'>   
    MyLogger: <class 'mylog.MyLogger'>
    lg: <MyLogger scopylogger (INFO)>
    ll: 30
    da: <dictan.DictAnalyzer object at 0x00000217AC76BE90>
}
```
Видим набор стандартных встроенных атрибутов (дандеры), а также  импортированные и созданные в начале программы модули/классы.

Определяем функции и запускаем их.

Далее попадаем в определение функции `outer_func`:
```cmd
---------------------------------------------------
-Defining functions--------------------------------
---------------------------------------------------
----f = outer_func("Ivan","30",80)-----------------
---------------------------------------------------
--inside outer func--------------------------------
outer_func for: Ivan, 30, weight: 80
---------------------------------------------------
------globals-in-outer-before-closure--------------
{
    name: Ivan
    old: 30
    weight: 80
    nested_var_in_outer_func: nested_var_in_OUTER_func_value
}
---------------------------------------------------
------locals-in-outer-before-closure---------------
{
    name: Ivan
    old: 30
    weight: 80
    nested_var_in_outer_func: nested_var_in_OUTER_func_value
}
```

Здесь для краткости убран вывод дандеров. Глобальная и локальная области содержат одинаковые элементы. Сравним как поменялись области по сравнению с началом.  
Динамика глобальной области:
```cmd
--comparing-globals-core-vs-globals-in-outer-func--
---kv-----------------------------------------
{
    dictan: <module 'dictan' from 'C:\\Users\\user\\documents\\pro\\chicago_spark\\ChiSpark\\dictan.py'>   
    MyLogger: <class 'mylog.MyLogger'>
    lg: <MyLogger scopylogger (INFO)>
    ll: 30
    da: <dictan.DictAnalyzer object at 0x00000217AC76BE90>
}
---k------------------------------------------
{ }
---d1-----------------------------------------
{ }
---d2-----------------------------------------
{
    globals_dict_core: scope_dict
    locals_dict_core: scope_dict
    outer_func: <function outer_func at 0x00000217AC76F060>
    check_closured: <function check_closured at 0x00000217ACC5C220>
    get_func_code_dict: <function get_func_code_dict at 0x00000217ACC5C2C0>
    get_func_closure_val_dict: <function get_func_closure_val_dict at 0x00000217ACC5C360>
    get_closure_variables: <function get_closure_variables at 0x00000217ACC5C400>
}
```
Видим, что отличие глобальных областей в новых определенных (созданных) функциях, а также добавились сами словари `globals_dict_core`, `locals_dict_core`.

Динамика локальной области:
```cmd
---------------------------------------------------
--comparing-locals-core-vs-locals-in-outer-func---
---kv-----------------------------------------
{ }
---k------------------------------------------
{ }
---d1-----------------------------------------
{
    dictan: <module 'dictan' from 'C:\\Users\\user\\documents\\pro\\chicago_spark\\ChiSpark\\dictan.py'>   
    MyLogger: <class 'mylog.MyLogger'>
    lg: <MyLogger scopylogger (INFO)>
    ll: 30
    da: <dictan.DictAnalyzer object at 0x00000217AC76BE90>
}
---d2-----------------------------------------
{
    name: Ivan
    old: 30
    weight: 80
    nested_var_in_outer_func: nested_var_in_OUTER_func_value
}
```

Общих элементов (не дандер) у локальных областей как видим- нет.  

Далее запускается на выполнение замкнутая функция и мы попадаем во вложенную (замкнутую область). Сравниваем текущие области с теми, которые получили на предыдущем этапе:
```cmd
---------------------------------------------------
----f()--------------------------------------------
---------------------------------------------------
--inside nested func-------------------------------
--using only part of outer_func params-------------
name: Ivan, old: 30
---------------------------------------------------
-cmprng-globals-in-outer-vs-globals-in-nested-func-
---kv-----------------------------------------
{
    dictan: <module 'dictan' from 'C:\\Users\\user\\documents\\pro\\chicago_spark\\ChiSpark\\dictan.py'>   
    MyLogger: <class 'mylog.MyLogger'>
    lg: <MyLogger scopylogger (INFO)>
    ll: 30
    da: <dictan.DictAnalyzer object at 0x00000217AC76BE90>
    globals_dict_core: scope_dict
    locals_dict_core: scope_dict
    outer_func: <function outer_func at 0x00000217AC76F060>
    check_closured: <function check_closured at 0x00000217ACC5C220>
    get_func_code_dict: <function get_func_code_dict at 0x00000217ACC5C2C0>
    get_func_closure_val_dict: <function get_func_closure_val_dict at 0x00000217ACC5C360>
    get_closure_variables: <function get_closure_variables at 0x00000217ACC5C400>
}
---k------------------------------------------
{ }
---d1-----------------------------------------
{ }
---d2-----------------------------------------
{
    f: <function outer_func.<locals>.nested_func at 0x00000217ACC5C4A0>
}
```
В глобальной области добавился один элемент `: <function outer_func.<locals>.nested_func at 0x00000217ACC5C4A0>` - это сама вложенная функция.

Смотрим динамику локальных областей:
```cmd
---------------------------------------------------
-comparing-locals-outer-vs-locals-in-nested-func---
---kv-----------------------------------------
{
    name: Ivan
    old: 30
    nested_var_in_outer_func: nested_var_in_OUTER_func_value
}
---k------------------------------------------
{ }
---d1-----------------------------------------
{
    weight: 80
}
---d2-----------------------------------------
{
    nested_var_in_nested_func: nested_var_in_NESTED_func_value
    nested_var_in_outer_func_modified_in_nested: nested_var_in_OUTER_func_value_modified
    nested_dict_in_nested_func:
    {
        some_key:
        {
            value: nested_var_in_OUTER_func_value_modified
            key_repr: 'some_key'
            value_repr: 'nested_var_in_OUTER_func_value_modified'
            key_id: 2300700295664
            key_type: str
            key_hash: 3322424967233704584
            value_id: 2300700986224
            value_type: str
        }
    }
    globals_dict_before_closure: scope_dict
    locals_dict_before_closure: scope_dict
}
```

Видим, что "за бортом" локальной области вложенной функции остался `weight: 80` - параметр не используется во вложенной функции. Также в локальную область вложенной функции попали разные объекты (переменные), которые были использованы внутри. Плюсом опять добавились словари областей, созданные на предыдущем этапе.

Далее смотрим состав `.__code`:
```cmd
---------------------------------------------------
----f.__code__-------------------------------------
<code object nested_func at 0x00000217AC7AE2E0, file "C:\Users\user\documents\pro\chicago_spark\ChiSpark\closure.py", line 52>
---------------------------------------------------
----get_func_code_dict(f)--------------------------
{
    _co_code_adaptive: b'\x95\x05\x97\x00t\....
    _varname_from_oparg: <built-in method _varname_from_oparg of code object at 0x00000217AC7AE2E0>        
    co_argcount: 0
    co_cellvars: ()
    co_code: b'\x95\x05\x97\x00t\....
    co_consts: (None, '---------------------------------------------------', '--inside nested func-------------------------------', '--using only part of outer_func params-------------', 'nested_var_in_NESTED_func_value', 'name: ', ', old: ', '_modified', 'some_key', '-cmprng-globals-in-outer-vs-globals-in-nested-func-', False, 'direct', True, '-comparing-locals-outer-vs-locals-in-nested-func---')
    co_exceptiontable: b''
    co_filename: C:\Users\user\documents\pro\chicago_spark\ChiSpark\closure.py
    co_firstlineno: 52
    co_flags: 19
    co_freevars: ('globals_dict_before_closure', 'locals_dict_before_closure', 'name', 'nested_var_in_outer_func', 'old')
    co_kwonlyargcount: 0
    co_lines: <built-in method co_lines of code object at 0x00000217AC7AE2E0>
    co_linetable: b'\xf8\x80\x00\xdd\x....
    co_lnotab: b'\x04\x01@\x01@\x01....
    co_name: nested_func
    co_names: ('lg', 'mylev', 'll', 'da', 'dict_info', 'dict', 'globals', 'locals', 'compare_dicts_info')  
    co_nlocals: 5
    co_positions: <built-in method co_positions of code object at 0x00000217AC7AE2E0>
    co_posonlyargcount: 0
    co_qualname: outer_func.<locals>.nested_func
    co_stacksize: 7
    co_varnames: ('nested_var_in_nested_func', 'nested_var_in_outer_func_modified_in_nested', 'nested_dict_in_nested_func', 'globals_dict_in_closure', 'locals_dict_in_closure')
    replace: <built-in method replace of code object at 0x00000217AC7AE2E0>
}
```

Описание элементов есть выше.

Последним блоком идет информация о замыкании:
```cmd
---------------------------------------------------
----f.__closure__----------------------------------
(<cell at 0x00000217AC6C0F40: dict object at 0x00000217AC77BB40>, <cell at 0x00000217AC6C0EB0: dict object 
at 0x00000217AC77BC00>, <cell at 0x00000217AC6C12D0: str object at 0x00000217AC6C8CF0>, <cell at 0x00000217AC6C09D0: str object at 0x00000217AC6B7370>, <cell at 0x00000217AC6C0F10: str object at 0x00000217AC6C8D30>)
---------------------------------------------------
----get_func_closure_val_dict(f)-------------------
{
    0x217ac6c0f40: {'__name__': '__main__', '__doc__': None, '__packa....
    0x217ac6c0eb0: {'name': 'Ivan', 'old': '30', 'weight': 80, 'neste....
    0x217ac6c12d0: Ivan
    0x217ac6c09d0: nested_var_in_OUTER_func_value
    0x217ac6c0f10: 30
}
---------------------------------------------------
----get_closure_variables(f)-----------------------
{
    globals_dict_before_closure: scope_dict
    locals_dict_before_closure: scope_dict
    name: Ivan
    nested_var_in_outer_func: nested_var_in_OUTER_func_value
    old: 30
}
---------------------------------------------------
---------------------------------------------------
```

Получаем два словаря: (адрес ячейки)-(значение ячейки) и (имя переменной)-(значение-переменной).  
Отметим, что вложенная локальная область шире, чем область замыкания. В локальной области содержатся эти (не замкнуте) элементы:
```cmd
nested_var_in_nested_func: nested_var_in_NESTED_func_value
nested_var_in_outer_func_modified_in_nested: nested_var_in_OUTER_func_value_modified
nested_dict_in_nested_func: {...}
```
При этом словари областей из внешней функции оказались в области замыкания. Это происходит поому, что данные словари содержат ссылки на объекты переменных `name`, `old`, которые перешли в область замыкания.


## Порядок следования в `func.__code__.co_freevars` и `func.__closure__`
В предыдущем разделе использовались инструкции:
```python
closure = func.__closure__
        if closure is not None:
            # Получение списка имен переменных из объекта функции
            var_names = func.__code__.co_freevars
            for name, cell in zip(var_names, closure):
                ...
```
При этом происходит совпадение порядков следования элементов в `var_names` и `closure` при итерировании по ним `zip(var_names, closure)`

Порядок следования элементов в `var_names` и `closure` обеспечивается тем фактом, что они относятся к одной и той же функции, и интерпретатор Python гарантирует согласованность порядка элементов при извлечении атрибутов `co_freevars` и `__closure__`.

Вот как это происходит:

`func.__code__.co_freevars`: Этот атрибут содержит список имен переменных из внешней области видимости, которые были захвачены замыканием внутри функции. Эти имена переменных ***хранятся в порядке, в котором они были обнаружены интерпретатором*** Python во время анализа кода функции.

`func.__closure__`: Этот атрибут содержит кортеж объектов `cell`, которые представляют собой захваченные переменные из внешней области видимости. Эти объекты `cell` также хранятся ***в порядке, в котором они были обнаружены и созданы в процессе интерпретации*** кода функции.

Поскольку оба атрибута связаны с одной и той же функцией и создаются интерпретатором Python на основе анализа одного и того же кода, порядок их элементов будет согласованным. Когда вы используете `zip(var_names, closure)`, вы фактически создаете пары из имен переменных и объектов `cell` в соответствии с их порядком, как он определен в атрибутах `co_freevars` и `__closure__`.

## Применение замыкания
Замыкания в Python обладают различными практическими применениями, и создание декораторов - одно из основных. Замыкания позволяют функциям сохранять состояние внешней функции, что полезно, например, при создании декораторов для модификации поведения других функций.

Они также часто используются для создания анонимных функций (lambda), а также в качестве замыканий для обработки данных в асинхронных операциях. Общее преимущество заключается в том, что замыкания позволяют передавать функции в качестве аргументов или возвращать их из других функций, что расширяет возможности языка.

## Еще одно описание замыкания. Про "контекст".
В контексте конструкции замыкания термин относится к сохранению окружающего (внешнего) контекста внутри вложенной функции. Это включает в себя переменные и параметры, определенные во внешней функции, и их значения, которые сохраняются и могут использоваться внутри вложенной функции даже после того, как внешняя функция завершила выполнение. Таким образом, вложенная функция "замыкает" доступ к окружающему контексту.

Обычная функция не сохраняет свой контекст после завершения выполнения. Когда функция завершает свою работу, все её локальные переменные и состояние уничтожаются, и к ним нельзя получить доступ извне.

В отличие от этого замыкания (или функции, вложенной в другую функцию) сохраняют свой контекст, то есть значения переменных и параметров внешней функции, которые были актуальны в момент создания замыкания. Таким образом, замыкания могут продолжать использовать этот контекст даже после того, как внешняя функция завершила выполнение.

## nonlocal
Следующий скрипт не будет выполняться
```python
def counter(start=0):
    def step():
        # nonlocal start
        start += 1
        return start
 
    return step
```
Проблема с этим кодом связана с тем, что переменная `start`, которая используется внутри функции `step()`, определена во внешней функции `counter()`. При каждом вызове `counter()` создается новый экземпляр переменной `start`, который привязан к этому конкретному вызову `counter()`. Однако, когда вы пытаетесь изменить значение `start` внутри функции `step()`, Python интерпретирует это как попытку создания новой локальной переменной start внутри `step()`, а не обращение к внешней переменной `start`.

Чтобы решить эту проблему, вы можете использовать ключевое слово `nonlocal`, чтобы указать, что переменная start не является локальной и должна быть найдена во внешней области видимости.

Изменение значения переменной внутри вложенной функции без использования ключевого слова nonlocal в Python интерпретируется как попытка создания новой локальной переменной с тем же именем. Это особенность языка, предназначенная для предотвращения неоднозначности и ошибок в программировании.

Когда вы используете имя переменной внутри вложенной функции, интерпретатор Python сначала ищет это имя в локальной области видимости этой функции. Если переменная с таким именем не найдена, Python будет искать её в объемлющих областях видимости. Однако, если вложенная функция пытается изменить значение переменной, которая уже существует в объемлющей функции, Python предполагает, что это новая локальная переменная с таким же именем, чтобы избежать побочных эффектов и неоднозначностей.

Ключевое слово nonlocal используется для явного указания интерпретатору Python на то, что переменная должна быть найдена в объемлющей области видимости и её значение должно быть изменено там, а не создана новая локальная переменная.

Это действительно особенность языка Python. Когда интерпретатор Python видит операцию присваивания (`start += 1`), он предполагает, что `start` - это локальная переменная в текущей функции. Если переменная не была определена внутри текущей функции (то есть в ней не использовалось ключевое слово `nonlocal` или `global`), Python будет считать эту переменную нелокальной и вызовет ошибку `UnboundLocalError`, говорящую о том, что переменная была использована перед присваиванием.

Такая строгость в интерпретации имени переменной помогает избежать неоднозначностей и ошибок в программировании. Без этого правила программисты могли бы не заметить опечатки в именах переменных и получить неожиданные результаты из-за создания новых переменных вместо использования существующих.