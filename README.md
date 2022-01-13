# bskb-python-clean-code

# Правила именования функций, переменных, ...
1. Для имен переменных и функций используйте подчеркивание, а не camelCase
-----------
Плохо:
```python
def testFoo():
    ...
```
Хорошо:
```python
def test_foo():
    ...
```

2. Для имен классов используйте InitialCaps
-----------
Плохо:
```python
class test_foo():
    ...
```
Хорошо:
```python
class TestFoo():
    ...
```

3. Имена полей моделей должны быть в нижнем регистре с использованием подчеркивания вместо camelStyle
-----------
Плохо:
```python
class Person(models.Model):
    FirstName = models.CharField(max_length=20)
```
Хорошо:
```python
class Person(models.Model):
    first_name = models.CharField(max_length=20)
```

# Избегайте использования плохих имен
Плохо:
```python
c = 0  # Такой пример не несет понятной логики, трудночитаем в проекте
```
Хорошо:
```python
columnCount = 0
```

# Избегайте вводящих в заблуждение имен
Плохо:
```python
day = list(day)
```
Хорошо:
```python
listOfDay = list(day)
```

# Избегайте магических констант
Плохо:
```python
if attribute_type == "system":
   ...
```
Хорошо:
```python
SYSTEM_ATTRIBUTE_TYPE = "system"
if attribute_type == SYSTEM_ATTRIBUTE_TYPE:
   ...
```

# Избегайте отрицательных условных выражений
По возможности избегать отрицательных условных выражений, особенно это касается функций и переменных означающих "не ..." (Смотрите пример).

Плохо:
```python
def notSystemTypeAttribute(attributeType):
   if attributeType != "system":
      True
   else:
      False

if notSystemTypeAttribute(attributeType) != False:  # Двойное отрицание: не системный и не равно
   ...
else:
   ...
```
Хорошо:
```python
def SystemTypeAttribute(attributeType):
   if attributeType == "system":
      True
   else:
      False

if SystemTypeAttribute(attributeType) == False:
   ...
else:
   ...
```

# Функции должны делать одно
Это, безусловно, самое важное правило в разработке программного обеспечения. Когда функции выполняют несколько задач, их сложнее составлять, тестировать и обдумывать. Когда вы можете выделить функцию только для одного действия, их можно легко реорганизовать, и ваш код будет читаться намного чище.

# Не добавляйте ненужный контекст
Плохо:
```python
class Attribute:
   self.attribute_type = None
   self.attribute_name = None
   self.attribute_value = None
```
Хорошо:
```python
class Attribute:
   self.type = None
   self.name = None
   self.value = None
```

# Инструкция assert
Инструкции призваны быть внутренними самопроверками (internal selfchecks) вашей программы. Они работают путем объявления неких условий, возникновение которых в вашем исходном коде невозможно. Если
одно из таких условий не сохраняется, то это означает, что в программе
есть ошибка.
Конструкция:
```
инструкция_assert ::= "assert" выражение1 ["," выражение2]
где выражение1 - условие, выражение2 - комментарий к ошибке
```
Пример:
```python
assert 0 <= price <= 100, "Error"
```
Ошибка:
```python
AssertionError: Error
```
Предостережение:
``` 
Не использовать assert для валидации данных!
```

# Разворачивание констант списка, словаря или множества
Плохо:
```
names = ['Элис', 'Боб', 'Дилберт']
```
Хорошо:
```python
 names = [
... 'Элис',
... 'Боб',
... 'Дилберт'
... ]
```

# Работа с файлами
Плохо:
```python
f = open('hello.txt', 'w')
f.write('привет, мир!')
f.close()
```
Вредно:
```python
f = open('hello.txt', 'w')
try:
  f.write('привет, мир!')
finally:
  f.close()
```
Хорошо:
```python
with open('hello.txt', 'w') as f:
  f.write('привет, мир!')
```

# Одинарные и двойные подчеркивания, дандеры

1. Одинарный начальный символ подчеркивания: *_var*
-----------
Префикс, состоящий из символа подчеркивания, подразумевается как
подсказка, которая должна сообщить другому программисту, что переменная или метод, начинающиеся с одинарного символа подчеркивания,
предназначаются для внутреннего пользования. Эта договоренность
определена в PEP 8.

Не будет работать подстановочный импорт (from my_module import *)

2. Одинарный замыкающий символ подчеркивания: *var_*
-----------
Иногда самое подходящее имя переменной уже занято ключевым словом
языка Python. По этой причине такие имена, как *class* или *def*, в Python
нельзя использовать в качестве имен переменных. В этом случае можно
в конец имени добавить символ одинарного подчеркивания, чтобы избежать конфликта из-за совпадения имен.  Эта договоренность
определена и объяснена в PEP 8.

3. Двойной начальный символ подчеркивания: *__var*
-----------
При использовании двойного подчеркивания интерпретатор python использует *искажение имени*, поэтому данные переменные и методы недоступны для использования извне. Таким образом переменные с двойным подчеркиванием можно использовать как приватные.

дандер — термин двойного подчеркивания(Пример: __baz будет звучать как "дандер baz")

4. Двойной начальный и замыкающий символ подчеркивания: *__var __*
-----------
*Искажение имен* не применяется, если имя начинается и заканчивается двойными символами подчеркивания. Имена, у которых есть начальный и замыкающий двойной символ подчеркивания, указывает на специальные методы в языке и зарезервированы для специального применения(__init__, __call__). Эти дандер-методы часто упоминаются как *магические методы*. В Python они представляют собой ключевое функциональное средство и должны применяться по мере необходимости. Тем не менее в контексте согласованных правил именования лучше воздержаться от использования имен, которые начинаются и заканчиваются двойными символами подчеркивания. 

5. Одинарный символ подчеркивания: *_*
-----------
По договоренности одинарный автономный символ подчеркивания иногда используется в качестве имени, чтобы подчеркнуть, что эта переменная временная или незначительная.
Пример:
```python
for _ in range(32):
 print('Привет, Мир.')
```

# Использование декоратора deprecated
Если у функции есть несколько версий, необходимо каждую версию переделать в отдельную функцию. Если создается новая версия, то старую необходимо удалить или добавить декоратор deprecated, это же касается классов, неиспользуемые классы необходимо удалить или добавить декоратор.

Использование кода:
```python
from deprecated import deprecated
@deprecated(reason="use another function")
def foo():
    print('deprecated func')
```
Консоль:
```python
DeprecationWarning: Call to deprecated function (or staticmethod) foo. (use another function)
  foo()
deprecated func
```
https://github.com/tantale/deprecated

# Версионность методов
Если у метода есть несколько версий, то необходимо каждую версию выносить в отдельную функцию.

Методы для каждой версии необходимо именовать по правилам: Имя головной функции + _ v + номер версии

Пример:
```python
def get_probes_card_by_journal(self, params):
    if self.version == 1:
        self.get_probes_card_by_journal_v1(params)
    elif self.version == 2:
        self.get_probes_card_by_journal_v2(params)
```

# Стиль моделей
Имена полей должны быть в нижнем регистре с использованием подчеркивания вместо camelStyle.

Порядок внутренних классов моделей и стандартных методов должен быть следующим:
* Все поля в базе
* Настраиваемые атрибуты менеджера
* class Meta
* def __str __()
* def save()
* def get_absolute_url()

# Правила написания JSON
Параметры JSON пишутся в стиле camelCase(должен начинаться со строчной буквы, а первая буква каждого последующего слова должна быть заглавной, все слова при этом пишутся слитно между собой.)

Плохо:
```json
{
    "method": "a",
    "params": {
      "filter_date-time": "true"
    }
}
```
Хорошо:
```json
{
    "method": "a",
    "params": {
      "filterDateTime": "true"
    }
}
```

# Singleton
Singleton - в однопоточном приложении будет единственный экземпляр некоторого класса.

Для Singleton использовать библиотеку ```pip install singleton-decorator```

Пример:
```python
@singleton
class YourClass:
    def method(self):
        ...
```
Документация: https://pypi.org/project/singleton-decorator/

# Создание миграций
Не изменять миграции которые попали в мастер

# Правила написания запросов к базе данных
Использовать select_related, когда объект, который вы собираетесь выбрать, является одним объектом, что означает пересылку ForeignKey, OneToOne и обратный OneToOne.

select_related работает путем создания соединения SQL и включения полей связанного объекта в оператор SELECT. По этой причине select_related получает связанные объекты в том же запросе к базе данных.

База данных для примера:
```python
class Publisher(models.Model):
    name = models.CharField(max_length=300)

    def __str__(self):
        return self.name

class Book(models.Model):
    name = models.CharField(max_length=300)
    price = models.IntegerField(default=0)
    publisher = models.ForeignKey(Publisher, on_delete=models.CASCADE)

    class Meta:
        default_related_name = 'books'

    def __str__(self):
        return self.name
        
class Store(models.Model):
    name = models.CharField(max_length=300)
    books = models.ManyToManyField(Book)

    class Meta:
        default_related_name = 'stores'

    def __str__(self):
        return self.name
```
Плохо:
```python
queryset = Book.objects.all()
for book in queryset:
    books.append({'id': book.id, 'name': book.name, 'publisher': book.publisher.name})
# Number of Queries : 101
# При каждом обращении делает новый запрос к базе
```
Хорошо:
```python
queryset = Book.objects.select_related('publisher').all()
for book in queryset:
    books.append({'id': book.id, 'name': book.name, 'publisher': book.publisher.name})
# Number of Queries : 1
# Не делает новый запрос к базе при обращение
```

Использовать prefetch_related, когда собираемся получить набор вещей. Это означает обработку ManyToMany и обратных ManyToMany, ForeignKey. 

prefetch_related выполняет отдельный поиск для каждой связи и выполняет «объединение» в Python. Он отличается от select_related. prefetch_related выполнял JOIN с использованием Python, а не в базе данных.

Плохо:
```python
queryset = Store.objects.all()
stores = []
for store in queryset:
    books = [book.name for book in store.books.all()]
    stores.append({'id': store.id, 'name': store.name, 'books': books})
# Number of Queries : 11
# В Store 10 элементов. Здесь происходит один запрос для выборки всех хранилищ, и во время итерации по каждому хранилищу выполняется другой запрос
```
Хорошо:
```python
queryset = Store.objects.prefetch_related('books')
stores = []
for store in queryset:
    books = [book.name for book in store.books.all()]
    stores.append({'id': store.id, 'name': store.name, 'books': books})
# Number of Queries : 2
```

# Правила для написания тестов eval()
eval() выполняет следующую последовательность действий:

Парсинг выражения. -> Компилирование в байт-код. -> Выполнение кода выражения Python. -> Возвращение результата.

1. Использовать конструкцию try:... except:... запрещено
2. Запрещено внутри теста создавать переменные или присваивать значения существующим переменным
3. Результатом выполнения теста должно быть булевое выражение

Пример:
```python
type(data) dict and "params" in data.keys() and type(data["params"]) dict and "username" in data["params"].keys()
```
4. Нельзя передавать конструкции с *if*, *import*, *def*, *class*, *for*, *while*, однако можно использовать *for* для например list comprehension
5. Аргументы в eval() можно не перадавать, тогда он будет использовать глобальные переменные

# Правила оформления статусов ошибок
1. Добавлять новый статус в общий список статусов ошибок в параметр ERROR_CODE 
2. Пример кода:  {"code": 400001, "http_code": 400, "message": "Отсутствует обязательный параметр"})

Первые 3 цифры *code* - это статус ошибки, следующие 3 цифры порядковый номер именно данного типа ошибок. *http_code* - Код состояния HTTP. *message* - информация о ошибке. 

Новый статус необходимо добавить в правильный блок отсортированный по коду состояний.

При передаче ошибки на стороне сервера, передавать front дополнительно параметр *details* - более подробную информацию о ошибке.

3. Статусы ошибок изменять запрещено, не актуальные статусы должны дополняться комментарием deprecated в коде 

#

Основная часть материала взята из книги "Чистый Python Дэн Бейбер"
