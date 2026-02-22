# Пример 1 — Простая функция с полным пайплайном QA

**Запрос:** Напиши функцию, которая принимает список чисел и возвращает сумму квадратов чётных чисел.

**Входные данные:**
```python
[1, 2, 3, 4, 5, 6]
Ожидаемый результат:

python
56  # 2² + 4² + 6² = 4 + 16 + 36
Сгенерированный код:

python
def sum_of_squares_of_evens(numbers: list[int | float]) -> int | float:
    if not isinstance(numbers, list):
        raise TypeError(
            f"Ожидался список, получен {type(numbers).__name__}"
        )
    for i, x in enumerate(numbers):
        if not isinstance(x, (int, float)):
            raise TypeError(
                f"Элемент [{i}]: ожидался int или float, "
                f"получен {type(x).__name__}: {x!r}"
            )
    return sum(x ** 2 for x in numbers if x % 2 == 0)


if __name__ == "__main__":
    print(sum_of_squares_of_evens([1, 2, 3, 4, 5, 6]))
Тесты:

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
    with pytest.raises(TypeError, match="Ожидался список"):
        sum_of_squares_of_evens("not a list")


def test_sum_of_squares_of_evens_invalid_type_none() -> None:
    with pytest.raises(TypeError, match="Ожидался список"):
        sum_of_squares_of_evens(None)


def test_sum_of_squares_of_evens_invalid_element_type() -> None:
    with pytest.raises(TypeError, match=r"Элемент \[1\]"):
        sum_of_squares_of_evens([1, "two", 3])
QA-анализ:

text
Уверенность: MEDIUM

Потенциальные проблемы:
- Крайний случай с float: x % 2 для чисел с плавающей точкой типа 2.0 возвращает 0.0 (считается чётным), что математически верно, но может удивить пользователей, ожидающих только целые числа. Серьёзность: НИЗКАЯ. Где: sum_of_squares_of_evens.
- Булевы значения: isinstance(True, int) в Python возвращает True, поэтому True/False проходят проверку типа и обрабатываются как 1/0. Технически это верно, но может быть неожиданно. Серьёзность: НИЗКАЯ. Где: валидация элементов sum_of_squares_of_evens.

Соответствие правилам:
Все правила: OK.

Непротестированные крайние случаи:
- Очень большие списки (10M+ элементов): производительность не тестировалась, но генераторное выражение эффективно по памяти. Риск: НИЗКИЙ.
- Булевы значения в списке ([True, False, 2]): True % 2 == 1 (нечётное), False % 2 == 0 (чётное, квадрат = 0). Математически верно, но может удивить. Риск: НИЗКИЙ.
Сводка для коррекции:

text
Версия: sum_squares_v1
Статус: PASS
Найдено проблем: 0

Исправления: (не требуются)

Паттерны для запоминания:
- ПАТТЕРН-1: Когда функция принимает list[int | float], валидируй типы элементов — не только тип контейнера. Применять: для любой публичной функции, принимающей типизированную коллекцию.
- ПАТТЕРН-2: Учитывай крайний случай с bool как подтипом int при валидации числовых типов. Применять: когда функция принимает int/float и поведение с bool имеет значение.

Директива для самокоррекции:
В будущих генерациях, включающих функции, которые принимают типизированные коллекции (list[int], list[str], dict[str, int] и т.д.), всегда валидируй И тип контейнера, И типы элементов в публичных функциях. Выбрасывай TypeError с сообщением, указывающим индекс и фактический тип невалидного элемента. Кроме того, учитывай, нужно ли специально обрабатывать булевы значения (которые проходят проверку isinstance int) для конкретного случая использования.
