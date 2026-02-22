# Python Coder Prompt V3.2

A production-grade system prompt that turns any LLM into a **senior Python developer + QA engineer**.  
Generates clean, type-safe, well-tested Python code with self-analysis and correction summaries.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## Features

- ✅ **Complete type hints** — Python 3.10+ syntax (`list[int]`, `dict[str, Any]`, `str | None`)
- ✅ **Input validation** — both container types AND element types
- ✅ **Specific exceptions** — `ValueError`, `TypeError`, `KeyError`, etc. with descriptive messages
- ✅ **Error handling** — `try-except` where needed, no bare `except`, no silent failures
- ✅ **Automatic test suite** — pytest with happy path, edge cases, and error tests
- ✅ **Self-analysis** — `[QA-ANALYSIS]` section with confidence rating, potential issues, and rule compliance
- ✅ **Correction summary** — `[CORRECTION-SUMMARY]` with fix suggestions and reusable patterns
- ✅ **PEP 8 compliance** — 4 spaces, snake_case, PascalCase, 88 char limit
- ✅ **No placeholders, no demo code** — every output is production-ready
- ✅ **Standard library only by default** — no external dependencies unless explicitly requested
- ✅ **Anti-injection protection** — `<user_data>` tags isolate instructions from data

---

## Why Use This Prompt?

Most code-generating prompts produce **incomplete, untested, or unsafe code**.  
This prompt enforces **professional-grade standards**:

- Every function is fully implemented — no `TODO`, no `pass`
- Every public function validates inputs
- Every error has a clear message
- Every piece of code comes with tests
- Every output includes a **self-review** and **lessons learned** for future generations

It's like having a **senior developer and a QA engineer** in your pocket.

---

## How to Use

1. **Copy the prompt**  
   - [`prompt/prompt-en.md`](prompt/prompt-en.md) (English version)  

2. **Send it to an LLM** (Claude, GPT-4, DeepSeek, etc.)  
   Place your task description inside `<user_data>` tags:

```xml
<user_data>
Write a function that takes a list of numbers and returns the sum of squares of even numbers
</user_data>
Get back a complete response with 4 sections separated by ---:

Main Python code (ready to run)

Test suite (pytest)

QA analysis (potential issues, edge cases, rule compliance)

Correction summary (fixes, patterns, self-correction directive)

Example
See the examples/ folder for full examples:

examples/Example-1-EN — simple function with full QA pipeline

examples/Example-2-EN2.md — negative example (what NOT to do)

examples/Example-3-EN.md — file I/O script with mocking

Quick peek (input → output):

Input:

xml
<user_data>
Write a function that takes a list of numbers and returns the sum of squares of even numbers
</user_data>
Output (simplified):

python
def sum_of_squares_of_evens(numbers: list[int | float]) -> int | float:
    if not isinstance(numbers, list):
        raise TypeError(f"Expected list, got {type(numbers).__name__}")
    for i, x in enumerate(numbers):
        if not isinstance(x, (int, float)):
            raise TypeError(f"Element [{i}]: expected int or float, got {type(x).__name__}: {x!r}")
    return sum(x ** 2 for x in numbers if x % 2 == 0)
… plus tests, QA analysis, and correction summary.

Compatibility
Works best with:

Claude (Sonnet, Opus) — XML tags are native

GPT-4, GPT-4o — markdown headers + JSON mode (adjust block order if needed)

DeepSeek — strong at code generation

Gemini — with minor adjustments (add safety preamble)

Also works with many local models (Llama, Mistral) — reduce complexity for smaller models.

Repository Structure
text
python-coder-prompt/
├── README.md                 # This file
├── LICENSE                    # MIT license
├── prompt/
│   ├── prompt-en.md           # English version of the prompt
│   └── prompt-ru.md           # Russian version of the prompt
└── examples/
    ├── en/                     # English examples
    │   ├── example1.md
    │   ├── example2.md
    │   └── example3.md
    └── ru/                     # Russian examples
        ├── example1.md
        ├── example2.md
        └── example3.md
License
MIT © 2026 Sergey Shilov. See LICENSE for details.

Author
Sergey Shilov

GitHub: @ren10-14

Location: Russia

16 y.o. self‑taught prompt engineer, Python enthusiast, and future immigrant to Bucharest 🇷🇴

Contributing
Found a bug? Have an idea?
Open an issue or submit a pull request. All contributions welcome!
