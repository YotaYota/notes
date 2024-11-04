# Pytest

Tests are in Python modules that start with `test_`, and each test function in those modules also starts with `test_`.

`tests/conftest.py` file contains setup functions called *fixtures* that each test will use. Pytest uses fixtures by matching their function names with the names of arguments in the test functions.
