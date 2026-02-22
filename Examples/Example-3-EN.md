# Example 3 — Script with File I/O and Full QA Pipeline

**Input:** Write a script that reads a CSV file and prints the first 5 rows.

**Generated code:**
```python
import csv
import sys


def read_csv_head(filepath: str, n: int = 5) -> list[list[str]]:
    if not isinstance(filepath, str):
        raise TypeError(
            f"filepath must be a string, got {type(filepath).__name__}"
        )
    if not isinstance(n, int) or isinstance(n, bool):
        raise TypeError(
            f"n must be an integer, got {type(n).__name__}: {n!r}"
        )
    if n < 0:
        raise ValueError(f"n must be non-negative, got: {n}")
    try:
        with open(filepath, "r", encoding="utf-8") as f:
            reader = csv.reader(f)
            rows: list[list[str]] = []
            for i, row in enumerate(reader):
                if i >= n:
                    break
                rows.append(row)
    except FileNotFoundError:
        raise FileNotFoundError(
            f"Файл не найден: '{filepath}'"
        ) from None
    except PermissionError:
        raise PermissionError(
            f"Нет доступа к файлу: '{filepath}'"
        ) from None
    return rows


if __name__ == "__main__":
    if len(sys.argv) < 2:
        print("Использование: python csv_head.py <путь_к_csv> [количество_строк]")
        sys.exit(1)
    csv_path = sys.argv[1]
    count = 5
    if len(sys.argv) >= 3:
        try:
            count = int(sys.argv[2])
        except ValueError:
            print(f"Ошибка: '{sys.argv[2]}' не является целым числом.")
            sys.exit(1)
    try:
        for row in read_csv_head(csv_path, count):
            print(row)
    except (FileNotFoundError, PermissionError) as e:
        print(f"Ошибка: {e}")
        sys.exit(1)
    except ValueError as e:
        print(f"Ошибка: {e}")
        sys.exit(1)
Test suite:

python
# filename: test_csv_head.py
import pytest
from csv_head import read_csv_head


def test_read_csv_head_typical(tmp_path) -> None:
    csv_file = tmp_path / "data.csv"
    csv_file.write_text("a,b,c\n1,2,3\n4,5,6\n7,8,9\n", encoding="utf-8")
    result = read_csv_head(str(csv_file), 2)
    assert result == [["a", "b", "c"], ["1", "2", "3"]]


def test_read_csv_head_default_n(tmp_path) -> None:
    lines = "".join(f"row{i},val{i}\n" for i in range(10))
    csv_file = tmp_path / "big.csv"
    csv_file.write_text(lines, encoding="utf-8")
    result = read_csv_head(str(csv_file))
    assert len(result) == 5


def test_read_csv_head_fewer_rows_than_n(tmp_path) -> None:
    csv_file = tmp_path / "small.csv"
    csv_file.write_text("a,b\n1,2\n", encoding="utf-8")
    result = read_csv_head(str(csv_file), 10)
    assert len(result) == 2


def test_read_csv_head_empty_file(tmp_path) -> None:
    csv_file = tmp_path / "empty.csv"
    csv_file.write_text("", encoding="utf-8")
    result = read_csv_head(str(csv_file))
    assert result == []


def test_read_csv_head_n_zero(tmp_path) -> None:
    csv_file = tmp_path / "data.csv"
    csv_file.write_text("a,b\n1,2\n", encoding="utf-8")
    result = read_csv_head(str(csv_file), 0)
    assert result == []


def test_read_csv_head_file_not_found() -> None:
    with pytest.raises(FileNotFoundError, match="Файл не найден"):
        read_csv_head("/nonexistent/path.csv")


def test_read_csv_head_invalid_filepath_type() -> None:
    with pytest.raises(TypeError, match="filepath must be a string"):
        read_csv_head(123)


def test_read_csv_head_invalid_n_type() -> None:
    with pytest.raises(TypeError, match="n must be an integer"):
        read_csv_head("file.csv", "five")


def test_read_csv_head_negative_n() -> None:
    with pytest.raises(ValueError, match="n must be non-negative"):
        read_csv_head("file.csv", -1)


def test_read_csv_head_bool_n() -> None:
    with pytest.raises(TypeError, match="n must be an integer"):
        read_csv_head("file.csv", True)
QA Analysis:

text
Confidence: HIGH

Potential issues:
- CSV dialect: csv.reader uses default dialect (excel). Files with different delimiters (tab, semicolon) will be read as single-column rows. Severity: LOW. Affected: read_csv_head.
- Large files: the function reads up to n rows and stops — memory-efficient for any n. Risk: LOW.

Rule compliance:
All rules: OK.

Edge cases not tested:
- CSV files with different encodings (latin-1, cp1251): function enforces utf-8, will raise UnicodeDecodeError. Not tested but behavior is correct — the error propagates naturally. Risk: LOW.
- CSV files with quoted fields containing newlines: csv.reader handles this correctly per RFC 4180. Risk: LOW.
Correction Summary:

text
Version: csv_head_v1
Status: PASS
Issues found: 0

Fixes to apply: (none required)

Patterns to remember:
- PATTERN-1: For file I/O functions, always re-raise FileNotFoundError and PermissionError with descriptive messages including the filepath. Use from None to suppress chained traceback noise. Apply when: any function opens files.
- PATTERN-2: Use tmp_path pytest fixture for file I/O tests — never create real files. Apply when: testing code that reads/writes files.
- PATTERN-3: Check bool as subclass of int — isinstance(True, int) is True. Guard with isinstance(n, bool) check before int check. Apply when: function accepts int and bool should be rejected.

Self-correction directive:
In future generations involving file I/O: always re-raise file exceptions with descriptive messages including the path; always use tmp_path in tests; always guard int parameters against bool (isinstance check for bool before int). For CSV operations, note that the default dialect handles standard CSVs but not TSV/semicolon variants — mention this limitation in QA-ANALYSIS if the task does not specify a delimiter.
