# Что такое ООП

Конспект-разбор на основе [Хендбук по Python от Яндекс Образования](https://education.yandex.ru/handbook/python) 

раздел 5. Объектно-ориентированное программирование

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

