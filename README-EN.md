# Python Coder Prompt V3.2

A production-grade system prompt that turns any LLM into a **senior Python developer + QA engineer**.  
Generates clean, type-safe, well-tested Python code with self-analysis and correction summaries.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE.md)

> 📘 [Русская версия](README-ru.md)

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
   - [`Prompt/PYTHON-CODER-V3.2-EN.md`](Prompt/PYTHON-CODER-V3.2-EN.md) (English version)  
   - [`Prompt/PYTHON-CODER-V3.2-RU.md`](Prompt/PYTHON-CODER-V3.2-RU.md) (Russian version)

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

Examples
See the Examples/ folder for complete examples:

English examples:
Examples/Example-1-EN.md — simple function with full QA pipeline

Examples/Example-2-EN.md — negative example (what NOT to do)

Examples/Example-3-EN.md — file I/O script with mocking

Russian examples:
Examples/Examples-RU/Example-1-RU.md

Examples/Examples-RU/Example-2-RU.md

Examples/Examples-RU/Example-3-RU.md

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
├── README-en.md                 # This file
├── README-ru.md                  # Russian version
├── LICENSE.md                     # MIT license
├── Prompt/
│   ├── PYTHON-CODER-V3.2-EN.md    # English prompt
│   └── PYTHON-CODER-V3.2-RU.md    # Russian prompt
└── Examples/
    ├── Example-1-EN.md             # English example 1
    ├── Example-2-EN.md             # English example 2
    ├── Example-3-EN.md             # English example 3
    └── Examples-RU/                 # Russian examples folder
        ├── Example-1-RU.md
        ├── Example-2-RU.md
        └── Example-3-RU.md
License
MIT © 2026 Sergey Shilov. See LICENSE.md for details.

Author
Sergey Shilov

GitHub: @ren10-14

Location: Russia

16 y.o. self‑taught prompt engineer, Python enthusiast, and future immigrant to Bucharest 🇷🇴

Contributing
Found a bug? Have an idea?
Open an issue or submit a pull request. All contributions welcome!
Open an issue or submit a pull request. All contributions welcome!
