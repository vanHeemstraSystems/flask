# 200 - A Minimal Application

A minimal Flask application looks something like this:

```title="hello.py"
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```
hello.py

So what did that code do?

1. First we imported the **[Flask](https://flask.palletsprojects.com/en/3.0.x/api/#flask.Flask)** class. An instance of this class will be our WSGI application.

2. Next we create an instance of this class. The first argument is the name of the application’s module or package. ```__name__``` is a convenient shortcut for this that is appropriate for most cases. This is needed so that Flask knows where to look for resources such as templates and static files.

3. We then use the **[route()](https://flask.palletsprojects.com/en/3.0.x/api/#flask.Flask.route)** decorator to tell Flask what URL should trigger our function.

4. The function returns the message we want to display in the user’s browser. The default content type is HTML, so HTML in the string will be rendered by the browser.

Save it as ```hello.py``` or something similar. Make sure to not call your application ```flask.py``` because this would conflict with Flask itself.

To run the application, use the ```flask``` command or ```python -m flask```. You need to tell the Flask where your application is with the ```--app``` option.

```
$ flask --app hello run
```

You will be prompted as follows:

```
* Serving Flask app 'hello'
 * Debug mode: off
WARNING: This is a development server. Do not use it in a production deployment. Use a production WSGI server instead.
 * Running on http://127.0.0.1:5000
Press CTRL+C to quit
127.0.0.1 - - [16/Sep/2024 13:55:50] "GET / HTTP/1.1" 200 -
127.0.0.1 - - [16/Sep/2024 13:55:50] "GET /favicon.ico HTTP/1.1" 404 -
```

A browser window at http://127.0.0.1:5000 will show the following text:

```
Hello, World!
```

> Application Discovery Behavior
> As a shortcut, if the file is named ```app.py``` or ```wsgi.py```, you don’t have to use ```--app```. See [Command Line Interface](https://flask.palletsprojects.com/en/3.0.x/cli/) for more details.

Hence, our proper application we will name ```app.py```.


MORE