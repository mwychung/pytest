.. _types:

How to use Types with Pytest
===========================================

You can add typing in pytest functions that helps to specify what type of data a function expects and returns.
This guide explains how to apply typing in pytest to improve code readability and prevent type-related errors.


1. Introduction to Typing
-----------------

Typing in Python helps developers specify the expected data types of variables, function parameters, and return values.
It improves code clarity and prevents type errors.

For example:


.. code-block:: python
    def add(x: int, y: int) -> int:
        return x + y

In this example:
`x: int` and `y: int` mean that `x` and `y` should be integers.
`-> int` shows that the function returns an integer.

It makes the intentions clear that both the input and output are expected to be integers.

2. Typing Test Functions
-----------------
Test functions in pytest check whether the code runs correctly.
While test functions do not return values, we can add `-> None` as the return type.

For example:

.. code-block:: python
    import pytest
    def test_add() -> None:
        result = add(2, 3)
        assert result == 5

In this function:
`test_add` is typed as `-> None` because it does not return anything.
The assertion `assert result == 5` checks if the result is correct.

Example result:
If the input data type is incorrect, like passing `add("2", 3)`, a `TypeError` will occur.

3. Typing Fixtures
-----------------
Fixtures in pytest helps set up data or provides resources needed for tests.
Adding type hints helps clarify what kind of data the fixtures returns, making code easier to read and easier to debug.

* Basic Fixture Typing:

If a fixture returns a number, you can specify it returns an `int`:

.. code-block:: python
    import pytest


    @pytest.fixture
    def sample_fixture() -> int:
        return 38


    def test_sample_fixture(sample_fixture: int) -> None:
        assert sample_fixture == 38

In this example:
`sample_fixture()` is typed to return an `int`.
In `test_sample_fixture`, using typing with fixtures, it ensures the return is an integer.
If you change the return from an integer to a string (``"42"``), the test will fail.


* Typing Fixtures with Lists and Dictionaries:
Here are the examples showing how to use List and Dict types in pytest.
When you want to use complex data structures like lists or dictionaries, import `List` and `Dict` from Python's `typing` module to specify the types.
Note: From Python 3.5 or later, typing module is built-in module in Python.


.. code-block:: python
    from typing import List
    import pytest


    # Fixture returning a list of integers
    @pytest.fixture
    def sample_list() -> List[int]:
        return [5, 10, 15]


    def test_sample_list(sample_list: List[int]) -> None:
        assert sum(sample_list) == 30
In this example, `List[int]` ensures that the list contains only integers, allowing functions like sum() to operate.


.. code-block:: python
    from typing import Dict
    import pytest


    # Fixture returning a dictionary
    @pytest.fixture
    def sample_dict() -> Dict[str, int]:
        return {"a": 50, "b": 100}


    def test_sample_dict(sample_dict: Dict[str, int]) -> None:
        assert sample_dict["a"] == 50
In this example, `Dict[str, int]` ensures that each key is a string and each value is an integer, ensuring clarity and consistency when accessing dictionary elements by key.


4. Typing Parameterized Tests
----------
`@pytest.mark.parametrize` allows developer to run the test multiple times with different sets of arguments.
Adding types helps to maintain consistency of values, especially for complex inputs.

For example, you are testing if adding 1 to `input_value` results in `expected_output` for each set of arguments.

.. code-block:: python
    import pytest


    @pytest.mark.parametrize("input_value, expected_output", [(1, 2), (5, 6), (100, 101)])
    def test_increment(input_value: int, expected_output: int) -> None:
        assert input_value + 1 == expected_output

In this example:
You are expecting the type of both `input_value` and `expected_output` are integers.


Conclusion
----------
Using typing in pytest helps improve readability of the tests and fixture returns, and prevent errors.