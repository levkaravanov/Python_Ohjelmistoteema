# Ассоциация (assosiaatio)

В этом модуле вы научитесь писать программы, в которых объекты (oliot) взаимодействуют между собой.

В объектно-ориентированном программировании программа состоит из классов (luokat). Из них во время выполнения создаются экземпляры (ilmentymät), то есть объекты (oliot). Объекты (oliot) могут взаимодействовать между собой: один объект (olio) может обрабатывать другие объекты (oliot) и вызывать их методы (metodit).

Эта связь между объектами (oliot) называется ассоциацией (assosiaatio). Программируя ассоциации (assosiaatiot), достигается сила объектно-ориентированного подхода: программа делится на маленькие, легко понимаемые части, а программист может писать код понемногу, фокусируясь на одном аспекте. Когда ассоциации (assosiaatiot) между объектами (oliot) хорошо спроектированы, даже обширная программа складывается из этих маленьких частей как бы незаметно.

## Проектирование структуры

В предыдущем модуле мы написали класс (luokka) `Koira` (Koira), где определяются свойства (ominaisuudet) собаки: имя (nimi), год рождения (syntymävuosi) и уникальная команда лая (yksilöllinen haukahdus). Кроме того, в классе (luokka) есть один метод (metodi): `hauku`. Класс (luokka) `Koira` выглядит так:

```python
class Koira:
    def __init__(self, nimi, syntymävuosi, haukahdus="Vuh-vuh"):
        self.nimi = nimi
        self.syntymävuosi = syntymävuosi
        self.haukahdus = haukahdus

    def hauku(self, kerrat):
        for i in range(kerrat):
            print(self.nimi + " haukkuu: " + self.haukahdus)
        return
```

Расширим пример, создав коирохойтолу (koirahoitola). Определим коирохойтолу (koirahoitola) так: собаку можно отвести в коирохойтолу (koirahoitola) и забрать оттуда. В определённый момент сотрудник коирохойтолы (koirahoitola) совершает обход; тогда он приветствует всех собак, и каждая собака лает.

Начнём с того, что потребуется для реализации коирохойтолы (koirahoitola).

Прежде всего коирохойтолу (koirahoitola) стоит реализовать как отдельный класс (luokka). Функциональность коирохойтолы (koirahoitola) никак не связана с отдельной собакой, и не должна быть прописана внутри `Koira`. Поэтому в программу пишем ещё один класс (luokka), названный `Hoitola` (Hoitola).

Далее подумаем, какие свойства (ominaisuudet) будут у коирохойтолы (koirahoitola). Замечаем, что коирохойтола (koirahoitola) должна знать, какие собаки в ней находятся в данный момент. Это можно реализовать с помощью списка: сделаем свойством (ominaisuus) коирохойтолы (koirahoitola) список, содержащий собак.

Подумаем, есть ли функции (toiminnot), которые следует записать в виде методов (metodit). Из недавнего определения коирохойтолы (koirahoitola) видно, что у этого класса (luokka) целесообразно создать три метода (metodit):
1. записать собаку в коирохойтолу (koirahoitola)
2. выписать собаку из коирохойтолы (koirahoitola)
3. совершить обход (kierroksen tekeminen) в коирохойтоле (koirahoitola).

Теперь программа определена и спроектирована, и мы можем приступить к её реализации.

## Программа из двух классов (luokat)

В нашем примере программа состоит из двух классов (luokat): `Koira` и `Hoitola` (Hoitola). В языке Python в одном файле исходного кода (lähdekooditiedosto) можно описать несколько классов (luokat), и это часто делается. Классы (luokat) могут быть и в разных файлах. Если на это решиться, то к другому файлу (модулю) можно обращаться лишь в том случае, если в начале программы стоит инструкция `import`, обозначающая импорт этого файла.

В небольших программах удобно писать все классы (luokat) в одном файле, что мы сейчас и сделаем. Создадим файл с именем koirahoitola.py и реализуем в нём нужную функциональность:

```python
class Koira:
    def __init__(self, nimi, syntymävuosi, haukahdus="Vuh-vuh"):
        self.nimi = nimi
        self.syntymävuosi = syntymävuosi
        self.haukahdus = haukahdus

    def hauku(self, kerrat):
        for i in range(kerrat):
            print(self.nimi + " haukkuu: " + self.haukahdus)
        return

class Hoitola:
    def __init__(self):
        self.koirat = []

    def koira_sisään(self, koira):
        self.koirat.append(koira)
        print(koira.nimi + " kirjattu sisään")
        return

    def koira_ulos(self, koira):
        self.koirat.remove(koira)
        print(koira.nimi + " kirjattu ulos")
        return

    def tervehdi_koiria(self):
        for koira in self.koirat:
            koira.hauku(1)

# Pääohjelma

koira1 = Koira("Muro", 2018)
koira2 = Koira("Rekku", 2022, "Viu viu viu")

hoitola = Hoitola()

hoitola.koira_sisään(koira1)
hoitola.koira_sisään(koira2)
hoitola.tervehdi_koiria()

hoitola.koira_ulos(koira1)
hoitola.tervehdi_koiria()
```

Примерная программа состоит из трёх частей:
1. класса (luokka) `Koira`
2. класса (luokka) `Hoitola` (Hoitola)
3. основной программы (pääohjelma).

Выполнение программы начинается с начала основной программы. Сначала создаются две собаки: Muro и Rekku. Затем создаётся новая коирохойтола (Hoitola):

```python
hoitola = Hoitola()
```

В этот момент управление переходит к инициализатору (alustaja) класса (luokka) `Hoitola` (Hoitola), где в свойство (ominaisuus) создаваемой коирохойтолы (Hoitola) добавляется пустой список koirat. В только что созданной коирохойтоле (Hoitola) пока нет ни одной собаки, но уже есть список, в который впоследствии можно добавить собак.

После этого в коирохойтолу (Hoitola) записывается первая собака (Muro):

```python
hoitola.koira_sisään(koira1)
```
Это метод (metodi), предоставляемый коирохойтолой (Hoitola): запись внутрь — явно функциональность коирохойтолы (Hoitola), и потому она реализована в классе (luokka) `Hoitola`. При записи необходимо указать, какую собаку мы записываем. Для этого объект (olio) `Koira` (по сути, ссылка на него) передаётся в вызов метода в качестве аргумента. В результате выполнения метод (metodi) `koira_sisään` добавляет полученную в параметре собаку в список коирохойтолы (Hoitola).

Аналогично в коирохойтолу (Hoitola) добавляется вторая собака, Rekku.

Затем сотрудник совершает обход и приветствует всех собак. Для этого вызывается метод (metodi) того же класса (luokka) `Hoitola`:

```python
hoitola.tervehdi_koiria()
```
Этот метод (metodi) реализован без параметров. Приветствие направлено всем собакам, находящимся в коирохойтоле (Hoitola), и сама коирохойтола (Hoitola) знает, какие собаки там сейчас есть. Метод (metodi) проходит по списку собак и заставляет каждую собаку лаять один раз.

Наконец, в примере одна собака (Muro) выписывается. Для этого вызывается соответствующий метод (metodi) класса (luokka) `Hoitola`, который удаляет переданный ему объект (olio) из списка. После этого собаки снова приветствуются, но теперь отвечает на приветствие только Rekku.

Работа программы становится понятна из её вывода:

```python
Muro kirjattu sisään
Rekku kirjattu sisään
Muro haukkuu: Vuh-vuh
Rekku haukkuu: Viu viu viu
Muro kirjattu ulos
Rekku haukkuu: Viu viu viu
```

Так мы написали программу, в которой есть экземпляры (ilmentymät) двух различных классов (luokat). Говорим, что между классами (luokat) `Hoitola` (Hoitola) и `Koira` существует постоянная ассоциация (pysyvä assosiaatiosuhde): у объекта (olio) `Hoitola` есть переменная экземпляра (instanssimuuttuja), содержащая ссылки на объекты (oliot) `Koira`.

Эта ассоциация (assosiaatio) в данном случае односторонняя: объект (olio) `Hoitola` знает, какие именно собаки у него на попечении, а объект (olio) `Koira` ничего не знает о коирохойтоле (Hoitola), в которой он находится. Ассоциация (assosiaatio) может быть одно- или двусторонней. Двустороннюю реализацию стоит использовать только при хороших основаниях. Тогда у программиста появляется дополнительная нагрузка – следить за тем, чтобы ссылки в обе стороны оставались согласованными.

## Временная ассоциация (tilapäinen assosiaatiosuhde)

Ранее мы отметили, что между классами (luokat) `Hoitola` (Hoitola) и `Koira` существует постоянная ассоциация (pysyvä assosiaatiosuhde): список собак хранится как свойство (ominaisuus) у коирохойтолы (Hoitola).

Между `Hoitola` (Hoitola) и `Koira` также есть другой вид связи: класс (luokka) `Hoitola` (Hoitola) предоставляет два метода (metodit), в параметрах которых передаётся ссылка на объект (olio) `Koira`. Ассоциация (assosiaatio) может существовать только во время вызова метода, если объект (olio) другой стороны передаётся в параметрах метода (metodi). Когда выполнение метода (metodi) заканчивается, связь, возникшая во время выполнения, исчезает, если информация об этой связи не сохранена как свойство (ominaisuus), как сделано в нашем примере.

Рассмотрим пример, где ассоциация (assosiaatio) чисто временная: между автомобилем (auto) и малярной мастерской (maalaamo). В примере создаётся синий автомобиль (auto) и передаётся в малярную мастерскую (maalaamo), чтобы его перекрасили в красный:

```python
class Auto:
    def __init__(self, rekisteritunnus, väri):
        self.rekisteritunnus = rekisteritunnus
        self.väri = väri

class Maalaamo:
    def maalaa(self, auto, väri):
        auto.väri = väri

maalaamo = Maalaamo()
auto = Auto("ABC-123", "sininen")
print("Auto on " + auto.väri)
maalaamo.maalaa(auto, "punainen")
print("Auto on nyt " + auto.väri)
```

Программа выводит цвет автомобиля до и после покраски:

```monospace
Auto on sininen
Auto on nyt punainen
```

В этом примере малярная мастерская (maalaamo) «знает» о перекрашиваемом автомобиле (auto) только во время выполнения метода (metodi) `maalaa`, поскольку ссылка на объект (olio) `Auto` получена в параметре метода. Когда выполнение метода (metodi) заканчивается, доступ к значению параметра уходит. Кроме того, автомобиль (auto) тоже не знает ничего о малярной мастерской (maalaamo). Ассоциация (assosiaatio) между малярной мастерской (maalaamo) и автомобилем (auto) в этом примере временная.
