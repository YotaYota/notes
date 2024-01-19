# Python

## `python -m module_name`

Does 2 things:

1.
It sets the name of `module_name` will be main, ie `__name__` will be `__main__`.

2.
Prevents `path/to` in `module_name` to be added to `PYTHONPATH`.
The difference between `python -m path.to.module_name` and `python path/to/module_name.py` is that the latter will
add `path/to` to the front of `PYTHONPATH`, potentially creating conflicting import names (where the local will be chosen).

## Structure, src-layout

```
├─ src
│  └─ packagename
│     ├─ __init__.py
│     └─ ...
├─ tests
│  └─ ...
├─ requirements.txt
└─ setup.py
```

[blog](https://blog.ionelmc.ro/2014/05/25/python-packaging/#the-structure%3E)
[setuptools src-layout](https://setuptools.pypa.io/en/latest/userguide/package_discovery.html#src-layout)

Use `pip` and `venv`.

cf. flat-layout

## Virtual Environment

```
python -m venv .venv
source .venv/bin/activate
...
deactivate
```

`activate` sets pip's path to the dir selected, this can be shown with `pip -V`.

Virtual environments does not litter the global pip installs.

## Development Mode (a.k.a. “Editable Installs”)

Setuptools instruct the Python interpreter and its import machinery to load the code under development
directly from the project folder without having to copy the files to a different location on disk.
Making changes to the source code take place in immediately, without requiring a new installation.

```
python -m venv .venv
source .venv/bin/activate
pip install --editable .
```

[setuptools site](https://setuptools.pypa.io/en/latest/userguide/development_mode.html)
