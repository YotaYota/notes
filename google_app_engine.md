# Google App Engine

update
```
appcfg.py -A <project id> -V <version> update <path to app.yaml>
```

- It is not a good idea to have global variables since the server runs on
different threads. A global variable is not well defined in this context.

