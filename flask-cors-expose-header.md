## Note
This blog is written in both Korean and English. Korean is my native language and English is the language I want to learn.
`Feel free to make any questions or advices to me`

***

### Motivation
I was developing a web page that if we click download button, we can download file from server.
To meet this requirement, I made a REST api with flask and added "Content-Disposition" header.
But when I make a AJAX call from frontend with React dev server, I cannot parse this header.

### Content
Since I was running React dev server, when I want to use AJAX call, I have to think about CORS (Cross Origin Resource Sharing). By default, in CORS response, only next 7 headers are visible.
- Cache-Control
- Content-Language
- Content-Length
- Content-Type
- Expires
- Last-Modified
- Pragma

(You can check more info from https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Expose-Headers)

So if I want to parse "Content-Disposition" header to parse default filename, I have to
expose this header explicitly. In flask, we can easily do this with `expose_headers` when
creating CORS instance.
```
# main.py
import flask
from flask import Response
from flask_cors import CORS

app = flask.Flask(__name__)
CORS(app, expose_headers=["Content-Disposition"])

@app.route("/")
def hello_word():
    response = Response(
        response="hello word",
        mimetype="text/plain",
    )
    response.headers["Content-Disposition"] = "attachment; filename=file.txt"
    return response

if __name__ == "__main__":
    app.run(port=8080)


# ajax.html
# open this file with brower and go to console. You can see that headers are printed
<script>
    fetch("http://localhost:8080/").then((res) => {
        res.headers.forEach(console.log);
    })
</script>
```

<img src="/imgs/img1.png" width="900px" height="120px"></img></br>
<!-- [See result as image file](/imgs/img1.png) -->

### Summary
You can add `expose_headers` argument when creating CORS instance from flask to
expose hedaers other than
- Cache-Control
- Content-Language
- Content-Length
- Content-Type
- Expires
- Last-Modified
- Pragma


***

### 배경
React와 flask를 이용하여 file download 버튼을 만들던 중, react에서 AJAX를 이용하여 REST API를 call할 때 `Content-Disposition` header를 읽을 수 없는 경우가 있었다. 검색해보니 CORS가 적용된 response에서는 기본적으로 다음의 7가지 hedaer만 읽을 수 있다고 한다. (https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Expose-Headers)
- Cache-Control
- Content-Language
- Content-Length
- Content-Type
- Expires
- Last-Modified
- Pragma

### 본문
`Content-Disposition` header를 읽기 위해서는 CORS를 적용할 떄 어떤 header를 expose해줄지 정해주면 된다.
flask에서는 flask_cors라는 module을 이용해서 쉽게 cors를 설정해 줄 수 있는데 다음과 같이 expose할 header를 명시해줄 수 있다.

```
# main.py
import flask
from flask import Response
from flask_cors import CORS

app = flask.Flask(__name__)
CORS(app, expose_headers=["Content-Disposition"])

@app.route("/")
def hello_word():
    response = Response(
        response="hello word",
        mimetype="text/plain",
    )
    response.headers["Content-Disposition"] = "attachment; filename=file.txt"
    return response

if __name__ == "__main__":
    app.run(port=8080)


# ajax.html
# open this file with brower and go to console. You can see that headers are printed
<script>
    fetch("http://localhost:8080/").then((res) => {
        res.headers.forEach(console.log);
    })
</script>
```

<img src="/imgs/img1.png" width="900px" height="120px"></img></br>


### 결론
`flask_cors.CORS` instance를 생성할 때 `expose_headers` argument를 전달해주면 CORS가 적용된
response에서 visible한 header를 명시적으로 설정할 수 있다.

