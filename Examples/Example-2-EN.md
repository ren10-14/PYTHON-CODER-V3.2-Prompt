# Example 2 — Negative Example (Missing QA and Analysis)

**Input:** Write a function that reverses a string.

**BAD OUTPUT (what NOT to do):**
```python
def reverse_string(s: str) -> str:
    return s[::-1]
Why it's bad:

No test suite (Rule 14 violated)

No QA analysis (Rule 15 violated)

No correction summary (Rule 16 violated)

No input validation (Rule 5 violated)

No entry point for a simple script (Rule 11 — minor)

Missing SECTIONS 2, 3, 4 in the output format

Missing '---' separators between sections

What the correct output should include:

Complete function with input validation

Test suite with pytest

QA analysis

Correction summary
