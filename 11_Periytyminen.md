# Наследование (Periytyminen)

Наследование (Periytyminen) — это механизм объектно-ориентированного программирования, при котором между классами создается иерархия: из одного класса, называемого надклассом (yliluokka), выводятся более конкретные подклассы (aliluokka).

В этом модуле вы научитесь использовать отношение наследования, создавая объектно-ориентированные Python-объекты.

## Надкласс (yliluokka) и подкласс (aliluokka)

Рассмотрим ситуацию, когда программа обрабатывает объекты Сотрудника (Työntekijä):

```python
class Työntekijä:

    työntekijöiden_lukumäärä = 0

    def __init__(self, etunimi, sukunimi):
        Työntekijä.työntekijöiden_lukumäärä = Työntekijä.työntekijöiden_lukumäärä + 1
        self.työntekijänumero = Työntekijä.työntekijöiden_lukumäärä
        self.etunimi = etunimi
        self.sukunimi = sukunimi

    def tulosta_tiedot(self):
        print(f"{self.työntekijänumero}: {self.etunimi} {self.sukunimi}")

työntekijät = []
työntekijät.append(Työntekijä("Viivi", "Virta"))
työntekijät.append(Työntekijä("Ahmed", "Habib"))

for t in työntekijät:
    t.tulosta_tiedot()
```

Программа создает двух сотрудников (Työntekijä): Вииви и Ахмеда, добавляет их в список и выводит его содержимое:

``` monospace
1: Viivi Virta
2: Ahmed Habib
```

У каждого сотрудника (Työntekijä) есть три свойства: номер сотрудника, имя и фамилия. Номер присваивается автоматически на основе количества сотрудников. Количество сотрудников — это переменная класса (class variable), значение которой определено для самого класса Сотрудника (Työntekijä). Обратите внимание, что переменная класса задается вне конструктора, а при обращении вместо `self` используется имя класса.

Предположим, возникла новая потребность: часть сотрудников получает почасовую оплату, а часть — месячную. Как добавить информацию об оплате к свойствам класса Сотрудника (Työntekijä)?

Одно из решений — добавить два отдельных свойства для почасовой и месячной оплаты. Но это неточно, и при использовании приложения пришлось бы всегда проверять эти поля, чтобы определить, к какому виду относится сотрудник. Кроме того, ничто технически не мешало бы настроить и часовую, и месячную оплату одновременно одному и тому же сотруднику.

Примем решение использовать механизм наследования (Periytyminen) в Python. Напишем для класса Сотрудника (Työntekijä) два уточняющих подкласса (aliluokka): Почасовая (Tuntipalkkainen) и Месячная (Kuukausipalkkainen). Когда мы создаем новый объект, мы можем сделать его, например, экземпляром класса Почасовая (Tuntipalkkainen). Тогда у него будут все свойства и методы от надкласса (yliluokka) Сотрудника (Työntekijä) (например, свойства имени или метод «работать»), но к ним добавятся свойства, определенные только для почасовых сотрудников — почасовая ставка.

Расширенная программа выглядит так:

```python
class Työntekijä:

    työntekijöiden_lukumäärä = 0

    def __init__(self, etunimi, sukunimi):
        Työntekijä.työntekijöiden_lukumäärä = Työntekijä.työntekijöiden_lukumäärä + 1
        self.työntekijänumero = Työntekijä.työntekijöiden_lukumäärä
        self.etunimi = etunimi
        self.sukunimi = sukunimi

    def tulosta_tiedot(self):
        print(f"{self.työntekijänumero}: {self.etunimi} {self.sukunimi}")

class Tuntipalkkainen(Työntekijä):

    def __init__(self, etunimi, sukunimi, tuntipalkka):
        self.tuntipalkka = tuntipalkka
        super().__init__(etunimi, sukunimi)

    def tulosta_tiedot(self):
        super().tulosta_tiedot()
        print(f" Tuntipalkka: {self.tuntipalkka}")

class Kuukausipalkkainen(Työntekijä):

    def __init__(self, etunimi, sukunimi, kuukausipalkka):
        self.kuukausipalkka = kuukausipalkka
        super().__init__(etunimi, sukunimi)

    def tulosta_tiedot(self):
        super().tulosta_tiedot()
        print(f" Kuukausipalkka: {self.kuukausipalkka}")


työntekijät = []
työntekijät.append(Tuntipalkkainen("Viivi", "Virta", 12.35))
työntekijät.append(Kuukausipalkkainen("Ahmed", "Habib", 2750))
työntekijät.append(Työntekijä("Pekka", "Puro"))
työntekijät.append(Tuntipalkkainen("Olga", "Glebova", 14.92))

for t in työntekijät:
    t.tulosta_tiedot()
```

Здесь создаются два сотрудника с почасовой оплатой (Tuntipalkkainen), один с месячной (Kuukausipalkkainen) и один сотрудник (Työntekijä) (Пекка), про которого не уточняется характер оплаты.

Цикл выводит следующий результат:
```monospace
1: Viivi Virta
 Tuntipalkka: 12.35
2: Ahmed Habib
 Kuukausipalkka: 2750
3: Pekka Puro
4: Olga Glebova
 Tuntipalkka: 14.92
```

Отношение надкласс-подкласс (yliluokka-aliluokka) в Python указывается тем, что в определении класса подкласса (aliluokka) в скобках пишется имя надкласса (yliluokka). Например, в начале `class Tuntipalkkainen(Työntekijä)` сообщается, что Почасовая (Tuntipalkkainen) является подклассом (aliluokka) Сотрудника (Työntekijä).

При необходимости подклассу (aliluokka) пишется свой собственный конструктор (алистайя, `__init__`). Когда создается экземпляр подкласса, вызывается только его собственный конструктор. На практике часто нужно сделать, как в примере: в коде вызывается конструктор подкласса, который, в свою очередь, вызывает конструктор надкласса (yliluokka). В таком случае подкласс задает значение почасовой ставки, а имя и фамилия определены в надклассе. Они передаются надклассу, вызывая его конструктор, то есть метод `__init__`. Доступ к надклассу (yliluokka) осуществляется через функцию `super()`: оператор `super().__init__(etunimi, sukunimi)` вызывает конструктор надкласса с параметрами имени и фамилии.

Свойства, определенные в надклассе (yliluokka), автоматически видны подклассу (aliluokka). Значит, мы можем создать экземпляр Почасовая (Tuntipalkkainen) `t` и в любой момент обратиться к его имени через `t.etunimi`.

## Переопределение (ylikirjoittaminen) методов

В рассмотренном примере метод `tulosta_tiedot`, написанный в надклассе Сотрудника (Työntekijä), выводит имя и фамилию. Метод работает нормально для объектов, создаваемых из класса Сотрудника (Työntekijä), когда неважно, почасовая или месячная у них оплата. Однако, например, для почасового сотрудника нужно вывести и почасовую ставку, а базовый метод этого не делает.

Решение — переопределить (ylikirjoittaminen) метод `tulosta_tiedot`. Переопределение означает, что в подклассе (aliluokka) создается своя реализация метода. Она имеет приоритет над методом, определенным в надклассе. Значит, при вызове `t.tulosta_tiedot()` для объекта, являющегося экземпляром Почасовая (Tuntipalkkainen), автоматически вызывается версия метода из подкласса. При том же вызове у обычного Сотрудника (Työntekijä) будет вызван метод из надкласса.

Благодаря переопределению мы получаем гибкость: сотрудники могут отличаться, у них могут быть разные структуры данных, но при этом в основной части программы можно выводить информацию по всем сотрудникам единообразным циклом:

```
for t in työntekijät:
    t.tulosta_tiedot()
```

Варианты и технические детали вызываемого метода скрыты там, где им место, — в классах. В основную программу они не «просачиваются».

## Множественное наследование (Moniperintä)

Иногда бывает так, что класс нужно определить сразу как подкласс (aliluokka) двух или более классов. Эта возможность называется множественным наследованием (Moniperintä). В Python множественное наследование разрешено, в отличие от некоторых других ООП-языков.

Пример ниже иллюстрирует множественное наследование. Определим два класса: Транспортное средство (Kulkuneuvo) и Спортивный инвентарь (Urheiluväline). Третий класс Велосипед (Polkupyörä) может быть подклассом для обоих упомянутых классов:

```python
class Kulkuneuvo:
    def __init__(self, nopeus):
        self.nopeus = nopeus

class Urheiluväline:
    def __init__(self, paino):
        self.paino = paino

class Polkupyörä(Kulkuneuvo, Urheiluväline):
    def __init__(self, nopeus, paino, vaihteet):
        Kulkuneuvo.__init__(self, nopeus)
        Urheiluväline.__init__(self, paino)
        
        self.vaihteet = vaihteet

pp = Polkupyörä(45, 18.7, 3)
print (pp.vaihteet)
print (pp.nopeus)
print (pp.paino)
```

Мы создаем объект Велосипед (Polkupyörä), из которого выводим количество передач, скорость и вес. Количество передач определено в классе Велосипед (Polkupyörä). Скорость наследуется из Транспортного средства (Kulkuneuvo), а вес — из Спортивного инвентаря (Urheiluväline). Программа выдает следующий результат:
```monospace
3
45
18.7
```

В этом случае мы не можем вызвать конструкторы надклассов (yliluokka) из конструктора Велосипед (Polkupyörä) через функцию `super` дважды следующим образом:

```python
# Неработающие вызовы конструкторов
super().__init__(nopeus)
super().__init__(paino)
```
Надкласс (yliluokka), к которому обращается `super`, определяется порядком поиска методов в Python. Здесь обе строки вызвали бы конструктор класса Транспортное средство (Kulkuneuvo), и программа работала бы неправильно.

Мы можем вызывать конструкторы надклассов напрямую по их имени:

```python
Kulkuneuvo.__init__(self, nopeus)
Urheiluväline.__init__(self, paino)
```
