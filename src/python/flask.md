# Flask

`instance_path`: the path Flask has chosen for the instance folder.

`@app.route()` creates a connection between URL and python function.

```
flask --app <app name> run --debug
```

```
from flask import current_app, g, session
```

- `current_app` points to the Flask application handling the request
- `g` is a special object that is unique for each request. It is used to store data that might be accessed by multiple functions during the request. 
- `session` is a dict that stores data across requests

A **View Function** is the code you write to respond to requests to the application. The view returns data that Flask turns into an outgoing response. View call `render_template()`.

A **Blueprint** is a way to organize a group of related views and other code. When using a blueprint, the name of the blueprint is prepended to the name of the function.

A **Template** is a file that contain static data as well as placeholders for dynamic data. Flask uses the Jinja. `g`, ` request` and `url_for()` is automatically available in templates.

