# 100 - Installation

Based on "Flask Installation" at https://flask.palletsprojects.com/en/3.0.x/installation/

## Python Version

We recommend using the latest version of Python. Flask supports Python 3.8 and newer.

## Dependecies

These distributions will be installed automatically when installing Flask.

- Werkzeug implements WSGI, the standard Python interface between applications and servers.

- Jinja is a template language that renders the pages your application serves.

- MarkupSafe comes with Jinja. It escapes untrusted input when rendering templates to avoid injection attacks.

- ItsDangerous securely signs data to ensure its integrity. This is used to protect Flask’s session cookie.

- Click is a framework for writing command line applications. It provides the flask command and allows adding custom management commands.

- Blinker provides support for Signals.

### Optional dependencies

These distributions will not be installed automatically. Flask will detect and use them if you install them.

- python-dotenv enables support for Environment Variables From dotenv when running flask commands.

- Watchdog provides a faster, more efficient reloader for the development server.

### Greenlet

You may choose to use gevent or eventlet with your application. In this case, greenlet>=1.0 is required. When using PyPy, PyPy>=7.3.7 is required.

These are not minimum supported versions, they only indicate the first versions that added necessary features. You should use the latest versions of each

MORE