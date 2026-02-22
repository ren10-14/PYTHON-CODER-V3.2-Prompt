# Пример 3 — Скрипт с вводом-выводом и полным пайплайном QA

**Запрос:** Напиши скрипт, который читает CSV-файл и выводит первые 5 строк.

**Сгенерированный код:**
```python
import csv
import sys


def read_csv_head(filepath: str, n: int = 5) -> list[list[str]]:
    if not isinstance(filepath, str):
        raise TypeError(
            f"filepath должен быть строкой, получен {type(filepath).__name__}"
        )
    if not isinstance(n, int) or isinstance(n, bool):
        raise TypeError(
            f"n должно быть целым числом, получен {type(n).__name__}: {n!r}"
        )
    if n < 0:
        raise ValueError(f"n должно быть неотрицательным, получено: {n}")
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
Тесты:

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
    with pytest.raises(TypeError, match="filepath должен быть строкой"):
        read_csv_head(123)


def test_read_csv_head_invalid_n_type() -> None:
    with pytest.raises(TypeError, match="n должно быть целым числом"):
        read_csv_head("file.csv", "five")


def test_read_csv_head_negative_n() -> None:
    with pytest.raises(ValueError, match="n должно быть неотрицательным"):
        read_csv_head("file.csv", -1)


def test_read_csv_head_bool_n() -> None:
    with pytest.raises(TypeError, match="n должно быть целым числом"):
        read_csv_head("file.csv", True)
QA-анализ:

text
Уверенность: HIGH

Потенциальные проблемы:
- Диалект CSV: csv.reader использует диалект по умолчанию (excel). Файлы с другими разделителями (табуляция, точка с запятой) будут прочитаны как одноколоночные строки. Серьёзность: НИЗКАЯ. Где: read_csv_head.
- Большие файлы: функция читает до n строк и останавливается — эффективна по памяти для любого n. Риск: НИЗКИЙ.

Соответствие правилам:
Все правила: OK.

Непротестированные крайние случаи:
- CSV-файлы с другой кодировкой (latin-1, cp1251): функция использует utf-8, вызовет UnicodeDecodeError. Не тестировалось, но поведение корректно — ошибка пробрасывается естественно. Риск: НИЗКИЙ.
- CSV-файлы с полями в кавычках, содержащими переносы строк: csv.reader обрабатывает это корректно согласно RFC 4180. Риск: НИЗКИЙ.
Сводка для коррекции:

text
Версия: csv_head_v1
Статус: PASS
Найдено проблем: 0

Исправления: (не требуются)

Паттерны для запоминания:
- ПАТТЕРН-1: Для функций с файловым вводом-выводом всегда перевыбрасывай FileNotFoundError и PermissionError с информативными сообщениями, включающими путь к файлу. Используй from None, чтобы подавить шум в цепочке traceback. Применять: когда любая функция открывает файлы.
- ПАТТЕРН-2: Используй фикстуру tmp_path pytest для тестирования файлового ввода-вывода — никогда не создавай реальные файлы. Применять: при тестировании кода, который читает/пишет файлы.
- ПАТТЕРН-3: Проверяй bool как подкласс int — isinstance(True, int) возвращает True. Защищайся проверкой isinstance(n, bool) перед проверкой на int. Применять: когда функция принимает int и bool должен быть отвергнут.

Директива для самокоррекции:
В будущих генерациях, связанных с файловым вводом-выводом: всегда перевыбрасывай файловые исключения с описательными сообщениями, включающими путь; всегда используй tmp_path в тестах; всегда защищай целочисленные параметры от bool (проверка isinstance на bool перед проверкой на int). Для операций с CSV отмечай, что диалект по умолчанию обрабатывает стандартные CSV, но не варианты с TSV/точкой с запятой — упоминай это ограничение в QA-ANALYSIS, если в задаче не указан разделитель.
