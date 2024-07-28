# Конспект-разбор на основе хендбука по Python от Яндекс Образования 

## раздел 5. Объектно-ориентированное программирование

### 5.1. Объектная модель Python. Классы, поля и методы

Мы описали все действия над объектом с помощью ***функций***. ***Такой подход в программировании называется процедурным*** и долгое время был популярным. Он позволяет эффективно решать простые задачи. Однако при усложнении задачи и появлении новых объектов процедурный подход приводит к дублированию и ухудшению читаемости кода.

**Объектно-ориентированное программирование (ООП)** позволяет устранить недостатки процедурного подхода. Язык программирования Python является объектно-ориентированным. Это означает, что **каждая сущность (переменная, функция и т. д.) в этом языке является объектом определённого класса**. 

Синтаксис создания класса в Python выглядит следующим образом:

    class <ИмяКласса>:
        <описание класса>

В классах описываются свойства объектов и действия объектов или совершаемые над ними действия.

- **Свойства объектов называются атрибутами**. По сути атрибуты — переменные, в значениях которых хранятся свойства объекта. Для создания или изменения значения атрибута необходимо использовать следующий синтаксис:

        <имя_объекта>.<имя_атрибута> = <значение>

- **Действия объектов называются методами**. Методы очень похожи на функции, в них можно передавать аргументы и возвращать значения с помощью оператора return, но вызываются методы после указания конкретного объекта. Для создания метода используется следующий синтаксис:

          def <имя_метода>(self, <аргументы>):
              <тело метода>

 Пример кода:

    class Car:

        def __init__(self, color, consumption, tank_volume, mileage=0):
            self.color = color
            self.consumption = consumption
            self.tank_volume = tank_volume
            self.reserve = tank_volume
            self.mileage = mileage
            self.engine_on = False

        def start_engine(self):
            if not self.engine_on and self.reserve > 0:
                self.engine_on = True
                return "Двигатель запущен."
            return "Двигатель уже был запущен."

        def stop_engine(self):
            if self.engine_on:
                self.engine_on = False
                return "Двигатель остановлен."
            return "Двигатель уже был остановлен."

        def drive(self, distance):
            if not self.engine_on:
                return "Двигатель не запущен."
            if self.reserve / self.consumption * 100 < distance:
                return "Малый запас топлива."
            self.mileage += distance
            self.reserve -= distance / 100 * self.consumption
            return f"Проехали {distance} км. Остаток топлива: {self.reserve} л."

        def refuel(self):
            self.reserve = self.tank_volume

        def get_mileage(self):
            return self.mileage

        def get_reserve(self):
            return self.reserve


        car_1 = Car(color="black", consumption=10, tank_volume=55)
        print(car_1.start_engine())
        print(car_1.drive(100))
        print(car_1.drive(100))
        print(car_1.drive(100))
        print(car_1.drive(300))
        print(f"Пробег {car_1.get_mileage()} км.")
        print(f"Запас топлива {car_1.get_reserve()} л.")
        print(car_1.stop_engine())
        print(car_1.drive(100)) 

Обратите внимание: взаимодействие с объектом класса вне описания класса осуществляется только с помощью методов, прямого доступа к атрибутам не происходит. Этот принцип ООП называется инкапсуляцией.

**Инкапсуляция** заключается в сокрытии внутреннего устройства класса за интерфейсом, состоящим из методов класса. Это необходимо, чтобы не нарушать логику работы методов внутри класса. Если не следовать принципу инкапсуляции и попытаться взаимодействовать с атрибутами напрямую, то могут происходить изменения, которые приведут к ошибкам.

Функции, которые могут работать с объектами разных классов, называются полиморфными. А сам принцип ООП называется **полиморфизмом**.

Говоря о полиморфизме в Python, стоит упомянуть принятую в этом языке так называемую «утиную типизацию» (Duck typing). Она получила своё название от шутливого выражения: «Если нечто выглядит как утка, плавает как утка и крякает как утка, это, вероятно, утка и есть» («If it looks like a duck, swims like a duck and quacks like a duck, then it probably is a duck»). В программах на Python это означает, что, если какой-то объект поддерживает все требуемые от него операции, с ним и будут работать с помощью этих операций, не заботясь о том, какого он на самом деле типа.


### 5.2. Волшебные методы, переопределение методов. Наследование

При создании похожих классов появляется дублирование кода. В ООП для создания новых классов на основе других применяется **принцип наследования**.

Наследование позволяет при создании нового класса указать для него **базовый класс**. От базового класса наследуется вся его структура — атрибуты и методы. Созданный класс-наследник называется **производным классом**.

    class Pencil:

        def __init__(self, color="серый"):
            self.color = color

        def draw_picture(self):
            return f"Нарисован рисунок цветом '{self.color}'."


    class Pen(Pencil):

        def sign_document(self):
            if self.color not in ("синий", "чёрный", "фиолетовый"):
                return f"Ручкой цвета '{self.color}' нельзя подписать документ."
            return f"Подписан документ."

Для получения типа ручки нам нужно модифицировать метод __init__, добавив в него аргумент pen_type и сохранив его значение в атрибуте. Таким образом, нам нужно дополнить метод базового класса. Такая операция при наследовании называется расширением метода.

При расширении методов необходимо вначале вызвать метод базового класса с помощью функции super().


    class Pencil:

        def __init__(self, color="серый"):
            self.color = color

        def draw_picture(self):
            return f"Нарисован рисунок цветом '{self.color}'."


    class Pen(Pencil):

        def __init__(self, color, pen_type):
            super().__init__(color=color)
            self.pen_type = pen_type

        def sign_document(self):
            if self.color not in ("синий", "чёрный", "фиолетовый"):
                return f"Ручкой цвета '{self.color}' нельзя подписать документ."
            elif self.pen_type == "гелевая":
                return f"Ручкой типа '{self.pen_type}' нельзя подписать документ."
            return f"Подписан документ."

Если в производном классе метод базового класса переписывается заново, то говорят о **переопределении метода**. 

Наследование может производиться сразу от нескольких классов. В таком случае базовые классы перечисляются через запятую. Производный класс унаследует атрибуты и методы обоих базовых классов.

    class GreetingFormal:

        def __init__(self):
            self.formal_greeting = "Добрый день,"

        def greet_formal(self, name):
            return f"{self.formal_greeting} {name}!"


    class GreetingInformal:

        def __init__(self):
            self.informal_greeting = "Привет,"

        def greet_informal(self, name):
            return f"{self.informal_greeting} {name}!"


    class GreetingMix(GreetingFormal, GreetingInformal):

        def __init__(self):
            GreetingFormal.__init__(self)
            GreetingInformal.__init__(self)


    mixed_greeting = GreetingMix()
    print(mixed_greeting.greet_formal("Пользователь"))
    print(mixed_greeting.greet_informal("Пользователь"))

    Вывод программы:

    Добрый день, Пользователь!
    Привет, Пользователь!


**Специальных методов** в Python довольно много. Они нужны для описания взаимодействия с объектами при помощи стандартных операций и встроенных функций. Описание специальных методов называется **перегрузкой операторов** (operator overloading).

Имена специальных методов выделены слева и справа двумя символами подчёркивания. Как можно заметить, метод __init__ также является специальным.

### 5.3. Модель исключений Python. Try, except, else, finally. Модули

    start = input()
    end = input()
    # Метод lstrip("-"), удаляющий символы "-" в начале строки, нужен для учёта
    # отрицательных чисел, иначе isdigit() вернёт для них False
    if not (start.lstrip("-").isdigit() and end.lstrip("-").isdigit()):
        print("
        ввести два числа.")
    else:
        interval = range(int(start), int(end) + 1)
        if 0 in interval:
            print("Диапазон чисел содержит 0.")
        else:
            print(";".join(str(1 / x) for x in interval))
    
Подход, который был нами применён для предотвращения ошибок, называется **Look Before You Leap (LBYL), или «Посмотри перед прыжком»**. В программе, реализующей такой подход, проверяются возможные условия возникновения ошибок до исполнения основного кода.

Существует другой подход для работы с ошибками: **Easier to Ask Forgiveness than Permission (EAFP), или «Проще попросить прощения, чем разрешения»**. В этом подходе сначала исполняется код, а в случае возникновения ошибок происходит их обработка. Подход EAFP реализован в Python в виде обработки исключений.
    
Исключения в Python являются классами ошибок. В Python есть много стандартных исключений. Они имеют определённую иерархию за счёт механизма наследования классов. В документации Python версии 3.10.8 приводится следующее дерево иерархии стандартных исключений:


    BaseException
     +-- SystemExit
     +-- KeyboardInterrupt
     +-- GeneratorExit
     +-- Exception
          +-- StopIteration
          +-- StopAsyncIteration
          +-- ArithmeticError
          |    +-- FloatingPointError
          |    +-- OverflowError
          |    +-- ZeroDivisionError
          +-- AssertionError
          +-- AttributeError
          +-- BufferError
          +-- EOFError
          +-- ImportError
          |    +-- ModuleNotFoundError
          +-- LookupError
          |    +-- IndexError
          |    +-- KeyError
          +-- MemoryError
          +-- NameError
          |    +-- UnboundLocalError
          +-- OSError
          |    +-- BlockingIOError
          |    +-- ChildProcessError
          |    +-- ConnectionError
          |    |    +-- BrokenPipeError
          |    |    +-- ConnectionAbortedError
          |    |    +-- ConnectionRefusedError
          |    |    +-- ConnectionResetError
          |    +-- FileExistsError
          |    +-- FileNotFoundError
          |    +-- InterruptedError
          |    +-- IsADirectoryError
          |    +-- NotADirectoryError
          |    +-- PermissionError
          |    +-- ProcessLookupError
          |    +-- TimeoutError
          +-- ReferenceError
          +-- RuntimeError
          |    +-- NotImplementedError
          |    +-- RecursionError
          +-- SyntaxError
          |    +-- IndentationError
          |         +-- TabError
          +-- SystemError
          +-- TypeError
          +-- ValueError
          |    +-- UnicodeError
          |         +-- UnicodeDecodeError
          |         +-- UnicodeEncodeError
          |         +-- UnicodeTranslateError
          +-- Warning
               +-- DeprecationWarning
               +-- PendingDeprecationWarning
               +-- RuntimeWarning
               +-- SyntaxWarning
               +-- UserWarning
               +-- FutureWarning
               +-- ImportWarning
               +-- UnicodeWarning
               +-- BytesWarning
               +-- EncodingWarning
               +-- ResourceWarning
    
Для обработки исключения в Python используется следующий синтаксис:

    try:
        <код , который может вызвать исключения при выполнении>
    except <классисключения_1>:
        <код обработки исключения>
    except <классисключения_2>:
        <код обработки исключения>
    ...
    else:
        <код выполняется, если не вызвано исключение в блоке try>
    finally:
        <код , который выполняется всегда>
        
Необходимо учитывать иерархию исключений для определения порядка их обработки в блоках except. Начинать обработку исключений следует с более узких классов исключений.    
    
Исключения можно принудительно вызывать с помощью оператора raise. Этот оператор имеет следующий синтаксис:

    raise <класс исключения>(параметры)

В качестве параметра можно, например, передать строку с сообщением об ошибке.    
    
В Python можно создавать свои собственные исключения. Синтаксис создания исключения такой же, как и у создания класса. При создании исключения его необходимо наследовать от какого-либо стандартного класса-исключения.    
    
                                                        
    class NumbersError(Exception):
        pass
    
    
    class EvenError(NumbersError):
        pass
    
    
    class NegativeError(NumbersError):
        pass
    
    
    def no_even(numbers):
        if all(x % 2 != 0 for x in numbers):
            return True
        raise EvenError("В списке не должно быть чётных чисел")
    
    
    def no_negative(numbers):
        if all(x >= 0 for x in numbers):
            return True
        raise NegativeError("В списке не должно быть отрицательных чисел")
    
    
    def main():
        print("Введите числа в одну строку через пробел:")
        try:
            numbers = [int(x) for x in input().split()]
            if no_negative(numbers) and no_even(numbers):
                print(f"Сумма чисел равна: {sum(numbers)}.")
        except NumbersError as e:  # обращение к исключению как к объекту
            print(f"Произошла ошибка: {e}.")
        except Exception as e:
            print(f"Произошла непредвиденная ошибка: {e}.")
    
            
    if __name__ == "__main__":
        main()                                

В программе основной код выделен в функцию main. А код вне функций содержит только условный оператор и вызов функции main при выполнении условия __name__ == "__main__". Это условие проверяет, запущен ли файл как самостоятельная программа или импортирован как модуль.
