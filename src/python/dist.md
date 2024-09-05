# Distribution Package

## Requirements

- `build`
- `setuptools` build backend
- _pyproject.toml_ with `build-backend` specified

---

## `build`

- sdist: unbuilt
- wheel: build

Install `build`

```
pip install --upgrade build
```

which also install `setuptools` (build backend).

```
python -m build
```

## _pyproject.toml_

_pyproject.toml_ tells the build frontend which build backend to use.

Three possible tables:

- `[build-system]`
- `[project]`
- `[tool]`

Fields can be static or dynamic. When a field is dynamic, it is the build backend’s responsibility to fill it. Consult your build backend’s documentation to learn how it does it.

### `[build-system]`

- `build-backend`: specifies which build backend to use.
- `requires`: list of dependencies needed to build the project – typically just build backend package.

### `[project]`



example:
```
[build-system]
requires = ["setuptools >= 61.0"]
build-backend = "setuptools.build_meta"
```

## Source


