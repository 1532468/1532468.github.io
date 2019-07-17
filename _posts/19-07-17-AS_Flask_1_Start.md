---
title : "[Algorithm Site 만들기]-1 Flask Start"
categories:
  - bucket
---

Apache 이딴 거 하기 싫어서 Flask를 이용하여 웹을 만들어 보자.

1. Flask 설치
    ~~~
    conda install -y -c conda-forge flask
    ~~~

2. Hello World!

    ~~~Python
    main.py

    from flask import Flask
    app = Flask(__name__)

    @app.route("/")
    def main():
        return "Hello World!"

    if __name__ == '__main__' :
        app.run(host="0.0.0.0", port=5000)
    ~~~

    ~~~
    python main.py
    ~~~

3. HTML 파일 적용

    Flask는 templates에서 템플릿을 찾아 HTML페이지를 생성한다.\
    결국 html문법은 조금 알아야겠다.

    ~~~html
    templates/index.html

    <!DOCTYPE HTML>

    <html>
        <head>
            <title>B:CODE</title>
        </head>
        <body>
            <h1>Hello World!</h1>
        </body>
    </html>
    ~~~

4. Flask에서 호출

    ~~~Python
    main.py

    from flask import Flask, render_template

    app = Flask(__name__)

    @app.route("/")
    def main():
        return "Hello World!"

    @app.route("/index")
    def index() :
        return render_template('index.html')

    if __name__ == '__main__' :
        app.run(host="0.0.0.0", port=5000)
    ~~~

5. css 적용하기

    많은 블로그를 찾아보니 css파일을 static폴더에 저장을 한다.
    static/style.css
    ~~~css
    h1{
        color:blue;
        background-color:gray
    }
    ~~~

6. css파일 링크

    ~~~html
    <!DOCTYPE HTML>

    <html>
        <head>
            <title>B:CODE</title>
            <link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='css/style.css') }}" />
        </head>
        <body>
            <h1>Hello World!</h1>
        </body>
    </html>
    ~~~

한 번 만들고 싶은 알고리즘 사이트.. 언젠가는 만들겠지.. 화이팅!
