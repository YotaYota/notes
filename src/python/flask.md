# Flask

`instance_path`: the path Flask has chosen for the instance folder.

`@app.route()` creates a connection between URL and python function.

```
flask --app <app name> run --debug
```

```
from flask import current_app, g
```

- `current_app` points to the Flask application handling the request
- `g` is a special object that is unique for each request. It is used to store data that might be accessed by multiple functions during the request. 

A **Blueprint** is a way to organize a group of related views and other code.
