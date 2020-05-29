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

<img src="/imgs/img1.png" width="500px" height="200px"></img></br>


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


### 본문


### 결론
