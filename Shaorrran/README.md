<img src="cyberpunk.jpg" alt="sam" width="600" />

Вы один из разработчиков CD Projekt RED и занимаетесь исправлением багов, которые обнаружились после выхода игры Cyberpunk 2077. Среди самых разных багов, на которые начали жаловаться пользователи, обнаружился один довольно неприятный баг - невозможность пройти ряд побочных заданий из-за неверного порядка следования событий в этих заданиях, например, вас может дважды направить в одно и тоже место. Другими словами, в рамках некоторого задания игроку надо выполнить событие A, затем B, затем C и так далее, таким образом, получим цепочку событий `A->B->C->...` (например, найди ключи от машины, доедь до здания Арасаки, поднимись на 100-й этаж, осмотрись и т.п.). Для простоты будем полагать, что цепочка замкнутая, например, для четырех событий:

```
A -> B -> C -> D
^              |
+ <----------- +
```

Как уже было сказано, обнаружились ошибки, из-за которых выполнение заданий стало невозможным, например, следующие события являются невалидными:

Тут событие А недостижимо:
```
A -> B -> C
     ^    |
     +<---+
```

Тут событие А замкнулось на себя:
```
A -> +   B -> C
^    |        | 
+ <- +<-------+
```

Тут события A и B не связаны с событиями C,D,E:
```
A -> B    C -> D -> E
^    |    ^         |
+ <- +    + <------ +
```

Изменение порядка следования событий, оказывается, требует очень много времени, поэтому разработчики решили, что они сначала исправят те задания, в которых нужно изменить **всего одну** связь, чтобы как можно быстрее выкатить патч. Например, в следующей цепочке достаточно исправить связь между C и A:

```
A -> B -> C          A -> B -> C
     ^    |    =>    ^         |
     +<---+          + <------ +
```

В следующем примере нужно, чтобы A ссылалось на B:
```
A -> +    B -> C        A -> B -> C
^    |         |   =>   ^         |
+ <- + <------ +        + <------ +
```

Итак, напишите программу, которая определяет в каких заданиях достаточно сделать одно исправление, список заданий описан в файле `tasks.txt`. Формат входных данных следующий:

```
task1
A -> B
B -> C
C -> B

task2
A -> A
B -> C
C -> A

task3
A -> A
B -> B
```

Заданий во входном файле может быть сколько угодно, также как и событий в рамках задания. На выходе вы должны напечатать в стандартный поток вывода ответы для всех заданий в формате исправленных связей, например, для первой задачи из примера выше это будет `C -> A`. Если же исправление невозможно, то напечатайте `V, V, V...`. Для нашего примера вывод будет следующим:

```
C -> A
A -> B
V, V, V...
```

Чтобы случайно не создать новых багов вам обязтаельно надо проверить свое решение. Пример теста показан ниже:

```python
# add.py
def add(a, b):
    return a + b

# test_add.py
from add import add

def test_add():
    assert add(1, 2) == 3
```

Чтобы запустить тестирование вашего модуля установите pytest и coverage:

```python
$ python -m pip install pytest coverage
$ pytest test_add.py
===================== test session starts =====================
platform darwin -- Python 3.8.5, pytest-6.0.1, py-1.9.0, pluggy-0.13.1
rootdir: /Users/dementiy/Desktop/test_works/demo
plugins: pyfakefs-4.1.0
collected 1 item                                                                                              

test_add.py .                                           [100%]
===================== 1 passed in 0.09s =====================
$ coverage run -m pytest test_add.py && coverage report -m test_add.py 
Name          Stmts   Miss  Cover   Missing
-------------------------------------------
test_add.py       3      0   100%
```

Вам нужно создать в вашем существующем репозитори (который вы указали в теоретической части) новую ветку с именем `exam-winter-42` и на этой ветке создать папку с именем `exam-winter-42`. В этой папке должен быть файл `find_bugs.py` с вашим решением, которое можно запустить следующим образом:

```python
python find_bugs.py tasks.txt
```

Все ваши тесты должны быть в файле с именем `test_find_bugs.py`, так чтобы тесты можно было запустить следующим образом:

```python
python test_find_bugs.py
```

**Удачи!**
