# Flask Notes

- [Flask](https://flask.palletsprojects.com/)
- [Jinja](https://jinja.palletsprojects.com/)


## Tutorials

### General
- [Digital Ocean Tutorial Series: How To Build Web Applications with Flask](https://www.digitalocean.com/community/tutorials/how-to-create-your-first-web-application-using-flask-and-python-3)
- [Advanced Web Apps - Flask Tutorial](https://python-adv-web-apps.readthedocs.io/en/latest/flask.html)


## Libraries
- [Bootstrap Flask](https://github.com/helloflask/bootstrap-flask) - Flask helpers via Jinja macros for [Bootstrap](https://getbootstrap.com/) 4 & 5.
- [Flask-WTF](https://github.com/wtforms/flask-wtf/) - Simple integration of Flask and [WTForms](https://github.com/wtforms/wtforms/), including CSRF and more.
- [Flask-SQLAlchemy](https://github.com/pallets-eco/flask-sqlalchemy/) - [SQLAlchemy](https://www.sqlalchemy.org/) support for Flask.

### Flask-SQLAlchemy
To install Flask-SQLAlchemy, the [greenlet](https://github.com/python-greenlet/greenlet) concurrency library must be installed. This library (greenlet) must be compiled from source and it requires the **C++** compiler on the host system:
- CentOS/Rocky Linux:  `gcc-c++`
- Ubuntu/Debian Linux:  `g++`



## Nginx Configuration
The following Nginx configuration should work for a "standard" Flask application in the `/opt/flask_app` directory, which is served on TCP port 5000 (adjust the port as necessary in the `proxy_pass` directive) and has `index.html` template as the "home" page for the site/application.  This configuration can be included in the `server` section of an existing Nginx host configuration. The configuration provides for application to be accessed at http://hostname/flask_app/ (or https://...) and the associated Flask API *endpoint* directly (i.e., avoiding the use of the TCP port) at http://hostname/flask_app/api/.

```nginx
    ### FLASK NGINX CONFIG ###

    location /flask_app/ {
        autoindex on;
        autoindex_exact_size off;
        alias      /opt/flask_app/templates/;
        index     index.html;

        allow all;

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
    }

    location /flask_app/api/ {

        rewrite ^/flask_app/api/(.*)  /$1 break;

        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_redirect off;
        proxy_pass http://127.0.0.1:5000/;

        sendfile on;
        tcp_nopush on;
        tcp_nodelay on;
    }
```

[Reference](https://nginx.org/en/docs/)

## Flask Run
To use the `flask run` command, the following environment variables must be exported:
```bash
export FLASK_APP=app
export FLASK_ENV=development
```
In the above, `app` is the name of the Python file containing the Flask application, conventionally `app.py` and the typical values for `FLASK_ENV` are `development`, `testing`, and `production`. The `FLASK_ENV` values control things like the default logging level and whether or not debugging is or is not enabled.

As an alternative to exporting the environment variables, they can be passed on the command line at run time:
```bash
FLASK_APP=app FLASK_ENV=development flask run
```
Or, you may specify parameters via command-line switches:
```bash
flask --app app run --debug --port 5000
```

[Reference](https://flask.palletsprojects.com/en/2.3.x/config/)

### Running the Application Directly
A Flask application can be run directly as a Python application (i.e., **without** using `flask run`) by adding the following lines to the _end_ of your application file (typically, `app.py`):
```python
if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000, debug=True)
	# app.run(debug=False)
```
As with any Python script, this simply turns `app.py` into a runnable Python script, which allows us to launch it simply by running:
```bash
python app.py
```
Of course, we can still use `flask run`, if desired. So now we have multiple options for how to launch our application.

### Saving Environment Configuration in `.flaskenv`
Instead of having to export the environment settings at the shell or hard-coding values in your script, you can alternately save these settings in a configuration file named `.flaskenv` in the root directory of your project. To use this approach, you need to install the `python-dotenv` package.
```bash
(venv) $ python -m pip install python-dotenv
```
Now, you can create your `.flaskenv` file. Here's an example with various settings.
```bash
FLASK_APP=myapp.py
FLASK_DEBUG=1
FLASK_RUN_HOST=0.0.0.0
FLASK_RUN_PORT=5050
FLASK_CONFIG=development
```
All of these variables will be available to your Flask application environment as if they had been explicitly exported when you execute `flask run`.

[Reference](https://prettyprinted.com/tutorials/automatically_load_environment_variables_in_flask/)

## Routes

- **Static Routes**: Fixed URL routes, similar to standard web pages.
```python
@app.route("/")

@app.route("/about/")

@app.route("/about/copyright/")
```

- **Dynamic Routes**: Allow parameters to be passed to the associated method in `<` and `>` brackets.
```python
@app.route('/capitalize/<word>/')
def capitalize(word):
    return '<h1>{word}</h1>'.format(word=escape(word.capitalize()))
	
@app.route('/add/<float:n1>/<float:n2>/')
def add(n1, n2):
    return '<h1>{sum}</h1>'.format(sum=(n1 + n2))
```

- **`url_for` Directive**: Does a "reverse lookup" on the _Python_ **method name** to construct the corresponding _URL_ with appropriate dynamic parameters.
```python
@app.route("/president/<num>")
def detail(num):
   president_dict = presidents_list[int(num) - 1]
   return render_template("president.html", pres=president_dict)   
```

```html
  <ul>
  {% for president in presidents %}
    <li>
	  <a href="{{ url_for( 'detail', num=president.id }}">{{ president.name }}</a>
	</li>
  {% endfor %}
  </ul>
```
	
## Templates

- Flask automatically loads templates from files in the `\templates` directory under the directory containing the Flask application file (typically, `app.py`).
- Template files are conventionally named with `.html` extension and contain HTML and [Jinja](https://jinja.palletsprojects.com/) markup.
- Static files, such as images, JavaScript and CSS files should be stored in a directory named `static` also at the same level as the Flask application file with, optionally, sub-directories named, `img`, `js` and `css`, respectively.
- Templates and static files should be referenced in HTML/Jinja templates using the `url_for` directive:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>Flask HTML Templates</title>
    <link rel="stylesheet" href="{{ url_for('static', filename='css/main.css') }}">
	<script src="{{ url_for('static', filename='js/bootstrap.min.js') }}"></script>
</head>
<body>

</body>
```
- A typical/standard directory structure of a Flask application is:
```bash
flask_app
   ├── app.py
   ├── data.py   
   ├── config.py   
   ├── models.py
   ├── static/
   │   ├── img/
   │   │   ├── favicon.ico
   │   │   └── logo.png   
   │   ├── css/
   │   │   ├── main.css
   │   │   └── bootstrap.min.css
   │   └── js/
   │       ├── jquery.min.js
   │       └── bootstrap.min.js
   └── templates/
       ├── index.html
       └── base.html

```

## Flask Blueprints
Flask [blueprints](https://flask.palletsprojects.com/blueprints/) provide a mechanism for logically subdividing your application into its constituent components. For example, in an e-commerce application, you might want to divide the application into _authentication_, _cart_, _products_, _api_, and _general_ features that are somewhat independent. A blueprint behaves similarly to a Flask _application_, but it actually provides the structure for how to _extend_ an existing application.

Each Flask blueprint should be defined as a **module** and, therefore, must have an `__init__.py` file in the root directory of the blueprint. In all respects, the directory structure of the blueprint looks much like a Flask application, except typically the main blueprint file has the same name as the blueprint (e.g., `products.py`) instead of `app.py`. Also, within the _child_ `templates` directory, we create another directory layer with the blueprint name. This additional directory layer helps to avoid collisions in template names when referencing the template file names within the blueprint modules. Here's an example.
```bash
flask_ecommerce
│
├── products/                <------ blueprint
│   ├── static/
│   │   └── view.js
│   │
│   ├── templates/
│   │   └── products/        <------ Extra directory "layer" for template name clarity.
│   │       ├── base.html
│   │       ├── list.html
│   │       ├── product.html
│   │       └── index.html
│   │
│   ├── __init__.py          <------ __init__.py to identify this as a module.
│   └── products.py
│
├── static/
│   ├── logo.png
│   ├── main.css
│   └── generic.js
│
├── app.py
├── config.py
└── models.py
```

[Reference](https://realpython.com/flask-blueprint/)

## Flask Database Migrations
The [Flask-Migrate](https://flask-migrate.readthedocs.io/) extension provides database migration support to SQLAlchemy via [Alembic](https://alembic.sqlalchemy.org/). The basic pattern for running migrations on Flask project is:
- Install **Flask-Migrate** extension in virtual environment.
```bash
(venv) $ python -m pip install flask-migrate
```
- Add **Flask-Migrate** extension as a dependency to your application. Typically, this will be main Python file for small application projects or your `app/__init__.py` for a multi-file projects.
```python
from flask_migrate import Migration

...
db = SQLAlchemy(app)
migrate = Migrate(app, db)
```
- Initialize the database migrations. This is only required once for each project. It will create the `migrations` directory at the root of your project, along with the `alembic.ini` configuration file inside of it. The actual migration scripts themselves are contained in the `migrations/versions` directory. The entire `migrations` directory should be included your in source control archive.
```bash
(venv) $ flask db init
```
- Make any necessary changes to your database schema. This is done by editing/changing your Flask **model** classes and includes the initial creation of the classes.
- Create the necessary migration scripts.
```bash
(venv) $ flask db migrate
```
- Apply the database migrations to your database. Remember that the database migrations must be applied to the database in each environment, which may be different for development, test, and production.
```bash
(venv) $ flask db upgrade
```
You may also run `flask db downgrade` to _remove_ schema changes instead.
