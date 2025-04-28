# Построение фоновой службы (taustapalvelu) и интерфейса (rajapinta)

В этом модуле вы научитесь создавать фоновую службу (taustapalvelu) на языке Python (*backend*). Это позволит вам строить веб-службу (web-palvelu) так, чтобы HTML/CSS/JavaScript-интерфейс (käyttöliittymä) веб-приложения (web-sovellus) передавал данные по HTTP-запросам главной программе на Python.

Пользователем фоновой службы (taustapalvelu) не обязательно должен быть только браузер. Применяя такой подход, любую внешнюю программу на любом языке программирования можно подключить к фоновой службе (taustapalvelu) благодаря протоколу HTTP.

## Установка библиотеки Flask (Flask-kirjasto)

Чтобы реализовать фоновую службу (taustapalvelu) на Python, используется библиотека Flask (Flask-kirjasto). Она даёт возможности для написания конечных точек (endpoint). Внешняя программа (например, браузер) может обращаться к этим конечным точкам (endpoint) и выполнять заложенную в фоновой службе (taustapalvelu) логику.

Рассмотрим пример: мы создаём фоновую службу (taustapalvelu), складывающую два числа. Хотя для простой операции сложения фоновая служба (taustapalvelu) не нужна, этот пример наглядно демонстрирует технические аспекты реализации.

Начнём с установки библиотеки Flask (Flask-kirjasto) напрямую в PyCharm:

1. Выберите View / Tool Windows / Python Packages.
2. В поле поиска введите Flask. В открывшемся списке выберите вариант Flask и нажмите Install.

После этого библиотека Flask (Flask-kirjasto) сразу доступна к использованию.

## Написание конечной точки (endpoint)

Когда Flask (Flask-kirjasto) установлен, можно создать первый вариант программы в файле summapalvelu.py:

```python
from flask import Flask, request

app = Flask(__name__)
@app.route('/summa')
def summa():
    args = request.args
    luku1 = float(args.get("luku1"))
    luku2 = float(args.get("luku2"))
    summa = luku1+luku2
    return str(summa)

if __name__ == '__main__':
    app.run(use_reloader=True, host='127.0.0.1', port=3000)

```

Последняя строка c методом app.run запускает фоновую службу (taustapalvelu). Служба открывается под IP-адресом 127.0.0.1 (специальный адрес для локальной машины), и доступна через порт (port) 3000. 

Строка `@app.route('/summa')` задаёт конечную точку (endpoint), говоря, что функция summa вызывается при запросе по адресу `http://127.0.0.1:3000/summa`. Технически браузер отправляет HTTP GET-запрос, на который отвечает фоновая служба (taustapalvelu).

Для передачи складываемых чисел требуется передать параметры в запросе GET, которые обрабатываются методом `request.args.get`. Например:
`http://127.0.0.1:3000/summa?luku1=13&luku2=28`
Первый параметр 13 превращается в число float и присваивается переменной luku1. Второй параметр 28 становится значением luku2. Затем вычисляется сумма и возвращается в виде строки.

При вызове из браузера результат отображается прямо в окне браузера:

![Taustapalvelun palauttama vastaus selainikkunassa](img/flaskvastaus.png)

Хотя технически это уже работает, результат ещё не в удобном формате для программной обработки.

## Формирование ответа в JSON

Чтобы ответ, отправляемый браузеру, удобно обрабатывать, часто используют формат JSON (JavaScript Object Notation). Его структура понятна и разработчикам на Python. Изменим функцию summa, чтобы вернуть данные в формате JSON. Cформировать JSON можно напрямую из словаря Python:

```python
from flask import Flask, request

app = Flask(__name__)
@app.route('/summa')
def summa():
    args = request.args
    luku1 = float(args.get("luku1"))
    luku2 = float(args.get("luku2"))
    summa = luku1 + luku2

    vastaus = {
        "luku1" : luku1,
        "luku2" : luku2,
        "summa" : summa
    }

    return vastaus

if __name__ == '__main__':
    app.run(use_reloader=True, host='127.0.0.1', port=3000)
```

Теперь программа возвращает JSON, что удобно для дальнейшей работы, например, на JavaScript в браузере:

![JSON-vastaus selainikkunassa](img/flask_json.png)

На основе описанного набора идей можно построить более сложную фоновую службу (taustapalvelu) с нужным количеством конечных точек (endpoint).

## Разбор запроса

В предыдущих примерах параметры передавались в виде `?luku1=...`. Это классический способ передачи параметров в HTTP-запросах.

Альтернативой является указание ресурса как части URL. Ниже пример простого «эхо-сервиса» (kaikupalvelu), возвращающего в ответ переданную строку, продублированную дважды. Здесь строка не передаётся как параметр, а выступает частью адреса. Flask (Flask-kirjasto) позволяет легко обрабатывать такие конструкции:

```python
from flask import Flask

app = Flask(__name__)
@app.route('/kaiku/<teksti>')
def kaiku(teksti):
    vastaus = {
        "kaiku" : teksti + " " + teksti
    }
    return vastaus

if __name__ == '__main__':
    app.run(use_reloader=True, host='127.0.0.1', port=3000)
```

При обращении к этому адресу ответ выглядит так:

![Kaikupalvelu selainikkunassa](img/osoite.png)

Разработчик фоновой службы (taustapalvelu) сам решает, как интерпретировать часть URL после домена. В REST-архитектурном стиле (REST) принято представлять ресурс так, как во втором примере — в самом пути.

## Обработка ошибок

В примере с суммированием теперь параметры передаются через путь вида `http://127.0.0.1:3000/summa/42/117`, и предполагается, что пользователь не ошибётся при вводе.

Если ошибки не обрабатываются, возможны ситуации:
1. Запрошен неправильный путь (endpoint): `http://127.0.0.1:3000/sumka/42/117`
2. Путь правильный, но один из параметров не может быть преобразован к числу: `http://127.0.0.1:3000/summa/4t23/117`

В первом случае Flask (Flask-kirjasto) вернёт код 404 (Not found) по умолчанию, во втором — код 500 (Internal server error). Хотя вызывающая сторона может обрабатывать эти ответы, мы можем дать более понятную информацию о сбое.

Ниже программа, обрабатывающая такие ошибки более детально:
1. Если запрошен неверный путь, возвращается 404 и JSON {"status": 404, "teksti": "Virheellinen päätepiste"}.
2. Если параметр не преобразуется в число, возвращается 400 (Bad Request) и JSON {"status": 400, "teksti": "Virheellinen yhteenlaskettava"}.

В случае успешной операции код состояния (status) включается в тело ответа просто как дополнительная информация. Реальный HTTP-статус указывается при создании объекта Response, где задаётся статус и MIME-тип.

```python
from flask import Flask, Response
import json

app = Flask(__name__)
@app.route('/summa/<luku1>/<luku2>')
def summa(luku1, luku2):
    try:
        luku1 = float(luku1)
        luku2 = float(luku2)
        summa = luku1+luku2

        tilakoodi = 200
        vastaus = {
            "status": tilakoodi,
            "luku1": luku1,
            "luku2": luku2,
            "summa": summa
        }

    except ValueError:
        tilakoodi = 400
        vastaus = {
            "status": tilakoodi,
            "teksti": "Virheellinen yhteenlaskettava"
        }

    jsonvast = json.dumps(vastaus)
    return Response(response=jsonvast, status=tilakoodi, mimetype="application/json")

@app.errorhandler(404)
def page_not_found(virhekoodi):
    vastaus = {
        "status" : "404",
        "teksti" : "Virheellinen päätepiste"
    }
    jsonvast = json.dumps(vastaus)
    return Response(response=jsonvast, status=404, mimetype="application/json")

if __name__ == '__main__':
    app.run(use_reloader=True, host='127.0.0.1', port=3000)
```
