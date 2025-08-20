# Pytest

Tests are in Python modules that start with `test_`, and each test function in those modules also starts with `test_`.

`tests/conftest.py` file contains setup functions called *fixtures* that each test will use. Pytest uses fixtures by matching their function names with the names of arguments in the test functions.

## Fixtures

Two types: *Yield Fixture* and *Fixture Finalization* (`request.addfinalizer(finalizer_func)`). The former is preferred for simplicity.

Any code **before** `yield` is the setup.

Any code **after** `yield` is the teardown.

```py
@pytest.fixture
def fixture_name(scope='session'):
    # setup
    yield
    # teardown
```

### Scopes

| scope    | When setup and teardown run
|:--|:--|
| function |(default) Setup before each function, teardown after each function
| class    |Setup before first test of class, teardown after last test of class
| module   |Setup/Teardown bracketing tests of a module
| package  |Setup/Teardown bracketing tests of a package/directory
| session  |Setup/Teardown at the beginning and end of the entire test session / test run

