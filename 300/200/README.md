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

> Application Discovery Behavior<br/>
> As a shortcut, if the file is named ```app.py``` or ```wsgi.py```, you don’t have to use ```--app```. See [Command Line Interface](https://flask.palletsprojects.com/en/3.0.x/cli/) for more details.

Hence, our proper application we will name ```app.py```.

This launches a very simple builtin server, which is good enough for testing but probably not what you want to use in production. For deployment options see [Deploying to Production](https://flask.palletsprojects.com/en/3.0.x/deploying/).

Now head over to http://127.0.0.1:5000/, and you should see your hello world greeting.

If another program is already using port 5000, you’ll see ```OSError: [Errno 98]``` or ```OSError: [WinError 10013]``` when the server tries to start. See [Address already in use](https://flask.palletsprojects.com/en/3.0.x/server/#address-already-in-use) for how to handle that.

> Externally Visible Server:<br/>
> If you run the server you will notice that the server is only accessible from your own computer, not from any other in the network. This is the default because in debugging mode a user of the application can execute arbitrary Python code on your computer.
>
> If you have the debugger disabled or trust the users on your network, you can make the server publicly available simply by adding ```--host=0.0.0.0``` to the command line:
>
> $ flask run --host=0.0.0.0
>
> This tells your operating system to listen on all public IPs.

## Debug Mode

The ```flask run``` command can do more than just start the development server. By enabling debug mode, the server will automatically reload if code changes, and will show an interactive debugger in the browser if an error occurs during a request.

![debugger](https://github.com/user-attachments/assets/3ea0f718-231d-42b3-b2e0-61f1307cb7d7)

> Warning:
>
> The debugger allows executing arbitrary Python code from the browser. It is protected by a pin, but still represents a major security risk. Do not run the development server or debugger in a production environment.

To enable debug mode, use the ```--debug``` option.

```
$ flask --app hello run --debug
 * Serving Flask app 'hello'
 * Debug mode: on
 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)
 * Restarting with stat
 * Debugger is active!
 * Debugger PIN: nnn-nnn-nnn
```

See also:

- [Development Server](https://flask.palletsprojects.com/en/3.0.x/server/) and [Command Line Interface](https://flask.palletsprojects.com/en/3.0.x/cli/) for information about running in debug mode.

- [Debugging Application Errors](https://flask.palletsprojects.com/en/3.0.x/debugging/) for information about using the built-in debugger and other debuggers.

- [Logging](https://flask.palletsprojects.com/en/3.0.x/logging/) and [Handling Application Errors](https://flask.palletsprojects.com/en/3.0.x/errorhandling/) to log errors and display nice error pages.

## HTML Escaping

When returning HTML (the default response type in Flask), any user-provided values rendered in the output must be escaped to protect from injection attacks. HTML templates rendered with Jinja, introduced later, will do this automatically.

**escape()**, shown here, can be used manually. It is omitted in most examples for brevity, but you should always be aware of how you’re using untrusted data.

```
from flask import Flask
from markupsafe import escape

app = Flask(__name__)

@app.route("/<name>")
def hello(name):
    return f"Hello, {escape(name)}!"
```

If a user managed to submit the name ```<script>alert("bad")</script>```, escaping causes it to be rendered as text, rather than running the script in the user’s browser.

```<name>``` in the route captures a value from the URL and passes it to the view function. These variable rules are explained below.

Run above application, here named ```app.py```, thus not requiring the ```--app``` option in the call, as follows:

```
$ flask run
```

You will need to append a 'name' to the URL (e.g. ```john```), for the browser window to show:

```
Hello, john!
```

## Routing

MORE