# Pydantic

A library that allows data validation and serialization. It relies on the type mechanism in Python.

Native to FastAPI

`BaseModel` implements the basics of validation in Pydantic. Inherit from it.

## Validation

```py
from pydantic import (
    BaseModel,
    EmailStr,
    Field,
    SecretStr,
    ValidationError,
)

class Role(IntFlag):
    Author = auto()
    Editor = auto()
    Developer = auto()
    Admin = Author | Editor | Developer

class User(BaseModel):
    model_config = {
        "extra": "forbid",
    }

    name: str = Field(examples=["Johannes"])
    email: EmailStr = Field(
        examples=["myname@internet.com"],
        description="email of user",
        frozen=True,
    )
    password: SecretStr = Field(
        examples=["abc123"], description="password of user", 
    )
    role: Role = Field(default=None, description="role of user")


def validate(data: dict[str, Any]):
    try:
        user = User.model_validate(date)
        print(user)
    except ValidationError as e:
        print("User is invalid")
        for error in e.errors():
            print(error)
```

Custom validators can be added by using either:

- the decorator `@field_validator("field_name")` (for a specific field),
- or the decorator `@model_validator()` (validate the whole instance at once)


**Note**: You can add `mode="before"` or `mode="after".

```py
    @field_validator("name")
    @classmethod
    def validate_name(cls, v: str) -> str:
        if not VALID_NAME_REGEX.match(v):
            raise ValueError("Name invalid")
        return v

    @model_validator()
    @classmethod
    def validate_user(cls, v: dict[str, Any]) -> dict[str, Any]:
        if "name" not in v or "password" not in v:
            raise ValueError("Name and password are required")
        if v["name"].casefold() in v["password"].casefold():
            raise ValueError("Password cannot contain name")
        v["password"] = hashlib.sha256(v["password"].encode()).hexdigest()
        return v
```

## Serialization

Can serialize to dict with `user.model_dump()`, or into json with `user.model_dump(mode="json")`. Can also exclude fields with parameter `exclude=["field_name"]`.

Can use

- either the decorator `@field_serializer("field_name", when_used="json")` (for a specific field),
- or the decorator `@model_serializer(mode="wrap", when_used="json")` (validate the whole instance at once)
