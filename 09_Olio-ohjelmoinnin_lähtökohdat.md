# Класс (luokka), объект (olio), конструктор (alustaja)

В этом модуле вы изучите основы объектно-ориентированного программирования (olio-ohjelmoinnin lähtökohdat). Вы научитесь писать классы (luokka), которые определяют общие свойства и операции для экземпляров (ilmentymät), то есть объектов (oliot). Вы узнаете принципы создания, инициализации и использования объектов (olioiden).

## Классы (luokat) и объекты (oliot)

В объектно-ориентированном программировании (olio-ohjelmointi) класс (luokka) означает общее понятие, которое задает общие черты для своих членов. 

Например, «собака (Koira)» — это такое общее понятие. У каждой собаки есть набор свойств (например, имя и год рождения), а также функции (в терминах Python (Python-termein) — методы (metodit)), такие как «лаять». 

Можно написать минимальный класс (luokka) «Собака (Koira)» так:

```python
class Koira:
    pass
```

Оператор `pass` ничего не делает и служит заполнителем, поскольку в теле класса (luokka) должна быть хотя бы одна инструкция. 

Данный класс (luokka) лишь указывает, что существует «Собака (Koira)». Он пока не определяет свойства или методы (metodit). 

Можно использовать «Собака (Koira)» так, что создаем из класса (luokka) объект (olio). Объект (olio) — это исполненная (во время работы программы) реализация класса. Ниже создается объект (olio) типа «Собака (Koira)» с именем «Rekku» и годом рождения 2022:

```python
class Koira:
    pass
   
koira = Koira()
koira.nimi = "Rekku"
koira.syntymävuosi = 2022

print (f"{koira.nimi:s} on syntynyt vuonna {koira.syntymävuosi:d}.")
```

Первая инструкция в главной программе создает объект (olio) типа «Собака (Koira)», на который ссылается переменная `koira`. Затем объекту (olio) назначаются имя и год рождения 2022. Эти свойства (ominaisuudet) относятся к конкретному объекту (olio). Можно создать много собак (koiria), каждая из которых имеет свои индивидуальные свойства. 

Свойству (ominaisuus) объекта (olio) ссылаются через имя объекта, точку и имя свойства, например `koira.nimi`. 

Последняя инструкция примера выводит имя и год рождения собаки (obъекта (olio)):

```monospace
Rekku on syntynyt vuonna 2022.
```

Обратите внимание на принятый в языке Python (Python-kieli) стиль написания названий классов (luokka): они пишутся с заглавной буквы. Если название состоит из нескольких слов, они пишутся слитно, начиная каждое слово с заглавной буквы (CamelCase). Например, «NäytönSuorakulmio».

## Конструктор (alustaja)

В предыдущем примере объект (olio) класса (luokka) «Собака (Koira)» создавался так, что сначала делался объект без свойств, а затем свойства назначались по одному. Это утомительно для программиста.

Чтобы упростить создание объектов (olioiden), часто пишут конструктор (alustaja) внутри класса (luokka), который автоматически устанавливает нужные значения свойств создаваемого объекта. В примере конструктор (alustaja) задает имя и год рождения, а главная программа использует его:

```python
class Koira:
    def __init__(self, nimi, syntymävuosi):
        self.nimi = nimi
        self.syntymävuosi = syntymävuosi

koira = Koira("Rekku", 2022)

print (f"{koira.nimi:s} on syntynyt vuonna {koira.syntymävuosi:d}.")
```

В языке Python (Python-kieli) конструктор (alustaja) определяется внутри класса (luokka) с помощью функции `__init__`. Первый параметр всегда `self`. Далее можно задать нужные параметры, которые передаются конструктору (alustaja). В примере это `nimi` и `syntymävuosi`. Эта функция выполняется при создании нового объекта (olio). В конце конструктора (alustaja) нет оператора `return`.

Внутри конструктора (alustaja) два оператора присваивания, которые назначают значения свойствам создаваемого объекта. Для ссылки на свойства (ominaisuudet) нового объекта (olio) используется `self`, за которым следует точка и имя свойства. 

Обратите внимание, что при создании объекта (olio) первый параметр `self` пропускается:

```python
# Неверно
koira = Koira(self, "Rekku", 2022)
```
нужно писать:
```python
koira = Koira("Rekku", 2022)
```

## Методы (metodit)

Ранее мы научились задавать свойства (ominaisuudet) для объекта (olio). Часто хочется определить и действия, которые объект (olio) может совершать, то есть методы (metodit). Ниже мы пишем метод (metodi) «hauku» в класс (luokka) «Собака (Koira)». Пример создает два объекта (oliot) «Собака (Koira)» и заставляет их лаять по-своему:

```python
class Koira:
    def __init__(self, nimi, syntymävuosi, haukahdus="Vuh-vuh"):
        self.nimi = nimi
        self.syntymävuosi = syntymävuosi
        self.haukahdus = haukahdus

    def hauku(self, kerrat):
        for i in range(kerrat):
            print(self.haukahdus)
        return


koira1 = Koira("Muro", 2018)
koira2 = Koira("Rekku", 2022, "Viu viu viu")

koira1.hauku(2)
koira2.hauku(5)
```

Теперь у конструктора (alustaja) три параметра, а для `haukahdus` задано значение по умолчанию. В примере собака (Muro) получает «штатный» вариант лая. 

Внутри класса (luokka) объявлен метод (metodi) `hauku`, который можно вызвать для любого объекта (olio) класса (luokka) «Собака (Koira)». Первый параметр метода всегда `self`, далее указываются нужные параметры. 

Вызов метода (metodi) делается так: имя объекта (olio), точка, название метода и параметры в круглых скобках. Например, `koira1.hauku(2)` вызывает метод (metodi) `hauku` для объекта (olio) `koira1`. Внутри метода (metodi) можно обратиться к свойствам (ominaisuudet) текущего объекта (olio) через `self.haukahdus`.

## Переменная класса (luokkamuuttuja) или статическая переменная

Ранее у собаки (коира) были свойства (ominaisuudet) имя, год рождения и лай («haukahdus»). Эти свойства относятся к конкретному объекту (olio). Однако иногда нужно хранить информацию, относящуюся ко всему классу (luokka), а не к какому-то отдельному объекту (olio). Например, для «Собака (Koira)» это может быть общее количество созданных собак. Такое значение можно хранить в переменной класса (luokkamuuttuja), то есть статической переменной.

Ниже в примере определена переменная класса (luokkamuuttuja) `tehty` для хранения количества созданных собак. Обратите внимание, что она задается вне конструктора (alustaja), и к ней не добавляется `self.` при объявлении. (Метод «hauku» из предыдущих примеров здесь опущен.)

```python
class Koira:

    tehty = 0

    def __init__(self, nimi, syntymävuosi, haukahdus="Vuh-vuh"):
        self.nimi = nimi
        self.syntymävuosi = syntymävuosi
        self.haukahdus = haukahdus
        Koira.tehty = Koira.tehty + 1

koira1 = Koira("Muro", 2018)
koira2 = Koira("Rekku", 2022, "Viu viu viu")
print (f"Koiria on nyt {Koira.tehty}.")
```

Чтобы обратиться к значению переменной класса (luokkamuuttuja), пишем название класса (luokka) и переменной. В примере это `Koira.tehty`.

Программа выводит:

```monospace
Koiria on nyt 2.
```
