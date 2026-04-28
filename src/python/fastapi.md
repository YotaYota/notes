# FastAPI

FastAPI is an ASGI framework.

## Minimal Start

```bash
uv add "fastapi[standard]"
```

```py
from fastapi import FastAPI

app = FastAPI()

@app.get("/")
async def root():
    return {"msg": "hello"}

@app.get("/items/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id}
```

## Request Variables

General pattern is: `Annotated[MyPydanticModel, Cookie()] = None`, where `Cookie()` can be any of `Path()`, `Body()`, `Header()`, `Cookie()`, `Query()`. Thses are functions that return special classes.

- path operation
- path parameter
    - Can be `Enum`
    - Validate with `Path()`
- query parameters: function params not part of path params
    - Can be optional, eg `: str | None = None`
    - Can be `bool` with automatic conversion
    - validation with `Query()`
- request bodies are defined with pydantic models
    - optional values has default values
    - can be validated with `Body()`
    - if there are multiple pydantic models, fastapi will treat them as **keys** in the request body.
- cookies use `Cookie()` (will otherwise be interpreted as query param)
    - cannot be set easily with JS, docs will not be able to set them
- cookies use `Header()` (will otherwise be interpreted as query param)
    - converts parameter names characters from underscore (_) to hyphen (-) to extract and document the headers.
    - use `list` eg `Annotated[list[str] | None, Header()]` to allow multiple headers with same name.
- response model use a pydantic model `-> MyModel` or `response_model`
    - fastapi will return server error instead of incorrect data
    - automatically serializes data
    - limits and filters output data to what is in the model
    - you can use `response_model` in path operations, eg `@app.get("/items/, response_model=list[Item])`
- response status eg `status_code=status.HTTP_201_CREATED`
- forms uses `Form()`, a class that inherits directly from Body.
    - requires `uv add python-multipart`
    - eg `username: Annotated[str, Form()], password: Annotated[str, Form()]` (or create pydantic model as type)
- files use `File`
    - `Annotated[bytes, File()]` will store the whole file in memory
    - `Annotated[UploadFile, File()]` is probably preferable

**Note**: You can declare multiple File and Form parameters in a path operation, but you can't also declare Body fields that you expect to receive as JSON, as the request will have the body encoded using multipart/form-data instead of application/json.

**Note**: functions params that are basic types becomes query params, while pydantic models become request body.

### Validation with `Annotated`

The recommended way.

Annotated is standard Python

```py
from typing import Annotated


def say_hello(name: Annotated[str, "this is just metadata"]) -> str:
    return f"Hello {name}"
```

FastAPI uses it for validation. with `Query`

```py
from typing import Annotated
from fastapi import FastAPI, Query

app = FastAPI()

@app.get("/items/")
def say_hello(q: Annotated[str | None, Query(max_length=50)] = None) -> str:
    results = {"items": [{"item_id": "Foo"}, {"item_id": "Bar"}]}
    if q:
        results.update({"q": q})
    return results
```

It also supports `Path()`, `Body()`, `Header()`, `Cookie()`, `Query()`. Subclasses of `Param` which is a subclass of Pydantic's `FieldInfo`.

**Note**: Old (deprecated) way was `q: str | None = Query(default=None, max_length=50)` without `Annotated`. Use Annotated!

**Note**: Use of list `async def read_items(q: Annotated[list[str] | None, Query()] = None):` for eg `?q=foo&q=bar`.

Custom validation: Using `AfterValidator` for custom validation

```py
from pydantic import AfterValidator

def check_valid_id(id: str):
    if not id.startswith(("isbn-", "imdb-")):
        raise ValueError('Invalid ID format, it must start with "isbn-" or "imdb-"')
    return id

@app.get("/items/")
async def read_items(
    id: Annotated[str | None, AfterValidator(check_valid_id)] = None,
):
```

Pydantic typed query params with: `filter_query: Annotated[FilterParams, Query()]`.

### Filtering Response `response_model`

- Validate the returned data
- Serialize the returned data to JSON using Pydantic
- will limit and filter the output data to what is defined in the return type

```py
class UserIn(BaseModel):
    username: str
    password: str
    email: EmailStr
    full_name: str | None = None


class UserOut(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None


@app.post("/user/", response_model=UserOut)
async def create_user(user: UserIn) -> Any:
    return user
```

endpoint will return user without `password` field.

**Note**: adding `-> UserOut` would give lint problems since user is defined as `UserIn` (although FastAPI filters out password field).

**Note**: this pattern works for returning _less_.

For returning _more_ you need a common base class and use the normal return syntax

```py
class BaseUser(BaseModel):
    username: str
    email: EmailStr
    full_name: str | None = None


class UserIn(BaseUser):
    password: str


@app.post("/user/")
async def create_user(user: UserIn) -> BaseUser:
    return user
```

### Responses

```py
from fastapi import FastAPI, Response
from fastapi.responses import JSONResponse, RedirectResponse

app = FastAPI()


@app.get("/portal")
async def get_portal(teleport: bool = False) -> Response:
    if teleport:
        return RedirectResponse(url="https://www.youtube.com/watch?v=dQw4w9WgXcQ")
    return JSONResponse(content={"message": "Here's your interdimensional portal."})
```

For weird cases turn off `response_model`,

```py
@app.get("/portal", response_model=None)
async def get_portal(teleport: bool = False) -> Response | dict:
```

Use `response_model_exclude_unset`, `response_model_exclude_unset`, `response_model_exclude_none` to modify how unset variables will appear in response.
Also `response_model_include` and `response_model_exclude`.

**Note**: It's recommended to use multiple classes over using these response_model parameters.

## Error Handling

- `HTTPException`, eg `raise HTTPException(status_code=404, detail="Item not found")`
- `detail` can contain anything value that can be converted to json, eg `dict` or `list`
- global listener to specific exceptions with `@app.exception_handler(X)` that will handle raised exception of type `X`
- can also ovveride built-in handlers, eg `@app.exception_handler(RequestValidationError)`
- FastAPI's `HTTPException` error class inherits from Starlette's `HTTPException`, where FastAPI subclass accepts any JSON-able data for the detail field
- If you register an exception handler, you should register it for Starlette's `HTTPException` (so you can catch Starlett internal errors).

```py
from fastapi import FastAPI, HTTPException
from fastapi.exception_handlers import (
    http_exception_handler,
    request_validation_exception_handler,
)
from fastapi.exceptions import RequestValidationError
from starlette.exceptions import HTTPException as StarletteHTTPException

app = FastAPI()


@app.exception_handler(StarletteHTTPException)
async def custom_http_exception_handler(request, exc):
    print(f"OMG! An HTTP error!: {repr(exc)}")
    return await http_exception_handler(request, exc)


@app.exception_handler(RequestValidationError)
async def validation_exception_handler(request, exc):
    print(f"OMG! The client sent invalid data!: {exc}")
    return await request_validation_exception_handler(request, exc)
```

## Path Operation Config

- `status_code`
- `tags` for OpenAPI docs
- `summary`
- `description` (or use docstring)
- `response_description`
- `deprecated`
