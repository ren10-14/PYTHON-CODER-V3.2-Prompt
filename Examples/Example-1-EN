
markdown
# Example 1 — Simple Function with Full QA Pipeline

**Input:** Write a function that takes a list of numbers and returns the sum of squares of even numbers.

**Input data:**
```python
[1, 2, 3, 4, 5, 6]
Expected output:

python
56  # 2² + 4² + 6² = 4 + 16 + 36
Generated code:

python
def sum_of_squares_of_evens(numbers: list[int | float]) -> int | float:
    if not isinstance(numbers, list):
        raise TypeError(
            f"Expected list, got {type(numbers).__name__}"
        )
    for i, x in enumerate(numbers):
        if not isinstance(x, (int, float)):
            raise TypeError(
                f"Element [{i}]: expected int or float, "
                f"got {type(x).__name__}: {x!r}"
            )
    return sum(x ** 2 for x in numbers if x % 2 == 0)


if __name__ == "__main__":
    print(sum_of_squares_of_evens([1, 2, 3, 4, 5, 6]))
Test suite:

python
# filename: test_sum_squares.py
import pytest
from sum_squares import sum_of_squares_of_evens


def test_sum_of_squares_of_evens_typical() -> None:
    assert sum_of_squares_of_evens([1, 2, 3, 4, 5, 6]) == 56


def test_sum_of_squares_of_evens_all_even() -> None:
    assert sum_of_squares_of_evens([2, 4]) == 20


def test_sum_of_squares_of_evens_no_evens() -> None:
    assert sum_of_squares_of_evens([1, 3, 5]) == 0


def test_sum_of_squares_of_evens_empty_list() -> None:
    assert sum_of_squares_of_evens([]) == 0


def test_sum_of_squares_of_evens_single_even() -> None:
    assert sum_of_squares_of_evens([4]) == 16


def test_sum_of_squares_of_evens_negative_evens() -> None:
    assert sum_of_squares_of_evens([-2, -4]) == 20


def test_sum_of_squares_of_evens_floats() -> None:
    assert sum_of_squares_of_evens([2.0, 3.0, 4.0]) == 20.0


def test_sum_of_squares_of_evens_invalid_type_not_list() -> None:
    with pytest.raises(TypeError, match="Expected list"):
        sum_of_squares_of_evens("not a list")


def test_sum_of_squares_of_evens_invalid_type_none() -> None:
    with pytest.raises(TypeError, match="Expected list"):
        sum_of_squares_of_evens(None)


def test_sum_of_squares_of_evens_invalid_element_type() -> None:
    with pytest.raises(TypeError, match=r"Element \[1\]"):
        sum_of_squares_of_evens([1, "two", 3])
QA Analysis:

text
Confidence: MEDIUM

Potential issues:
- Float modulo edge case: x % 2 for floats like 2.0 returns 0.0 (treated as even), which is mathematically correct but may surprise users expecting integer-only semantics. Severity: LOW. Affected: sum_of_squares_of_evens.
- Boolean input: isinstance(True, int) is True in Python, so True/False pass the type check and are treated as 1/0. This is technically correct but may be unexpected. Severity: LOW. Affected: sum_of_squares_of_evens element validation.

Rule compliance:
All rules: OK.

Edge cases not tested:
- Very large lists (10M+ elements): performance untested, but the generator expression is memory-efficient. Risk: LOW.
- Boolean values in list ([True, False, 2]): True % 2 == 1 (odd), False % 2 == 0 (even, square = 0). Correct mathematically, but may surprise. Risk: LOW.
Correction Summary:

text
Version: sum_squares_v1
Status: PASS
Issues found: 0

Fixes to apply: (none required)

Patterns to remember:
- PATTERN-1: When a function accepts list[int | float], validate element types — not just the container type. Apply when: any public function takes a typed collection.
- PATTERN-2: Consider bool-as-int edge case when validating numeric types. Apply when: function accepts int/float and bool behavior matters.

Self-correction directive:
In future generations involving functions that accept typed collections (list[int], list[str], dict[str, int], etc.), always validate both the container type AND the element types in public functions. Raise TypeError with a message identifying the index and actual type of the invalid element. Additionally, consider whether bool inputs (which pass isinstance int check) need special handling for the specific use case.
