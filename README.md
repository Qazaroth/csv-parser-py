# README.md

# Python CSV Parser

A learning-focused CSV parser written in Python.

This project exists primarily to practice:

* Test-Driven Development (TDD)
* Parser architecture
* State machines
* Error handling
* Refactoring
* Streaming parsers
* Software design

The goal is NOT just to parse CSV files.

The real goal is to understand how real parsers evolve from simple implementations into robust systems.

---

# Project Status

Current Status:

```text
Early Development
```

This project is being developed incrementally using strict TDD principles.

Features will be added gradually through failing tests and refactoring.

---

# Goals

By the end of development, this parser aims to support:

* Standard CSV parsing
* Quoted fields
* Escaped quotes
* Multiline quoted fields
* Custom delimiters
* Header support
* Strict validation mode
* Detailed parser errors
* Streaming support
* CSV writing/serialization
* RFC 4180 compliance

---

# Why Build Another CSV Parser?

Because CSV parsing is deceptively difficult.

At first glance, CSV seems simple:

```csv
a,b,c
1,2,3
```

But real-world CSV parsing quickly becomes complicated:

```csv
name,note
Alice,"likes apples, bananas"
Bob,"Line 1
Line 2"
Charlie,"She said ""hello"""
```

This project is intended as a deep engineering exercise.

---

# Learning Objectives

This project is specifically designed to teach:

## Parser Design

* finite-state machines
* tokenization
* incremental parsing
* streaming architecture

## TDD Workflow

* red → green → refactor
* behavior-driven testing
* regression prevention
* edge-case discovery

## Software Engineering

* refactoring safely
* clean architecture
* error reporting
* performance optimization

---

# Tech Stack

## Language

```text
Python 3.12+
```

## Testing

```text
unittest
```

The project intentionally uses Python's built-in testing framework to better understand testing fundamentals.

---

# Project Structure

```text
csv-parser/
├── src/
│   └── csv_parser/
│       ├── __init__.py
│       ├── parser.py
│       ├── tokenizer.py
│       ├── states.py
│       ├── errors.py
│       ├── writer.py
│       └── streaming.py
│
├── tests/
│   ├── test_basic.py
│   ├── test_quotes.py
│   ├── test_multiline.py
│   ├── test_headers.py
│   ├── test_errors.py
│   ├── test_writer.py
│   └── test_streaming.py
│
├── examples/
├── benchmarks/
├── ROADMAP.md
└── README.md
```

---

# Development Philosophy

## TDD First

Every feature begins with a failing test.

Workflow:

1. Write failing test
2. Implement minimal solution
3. Make tests pass
4. Refactor safely
5. Repeat

---

## Incremental Complexity

The parser intentionally evolves gradually.

Example progression:

```text
Simple split parser
        ↓
Quoted field support
        ↓
State machine parser
        ↓
Multiline parsing
        ↓
Streaming parser
```

The purpose is to experience how architecture changes as requirements grow.

---

# Planned Features

## Phase 1

* basic row parsing
* comma-separated fields
* simple split parser

## Phase 2

* configurable delimiters
* semicolon support
* tab-separated values

## Phase 3

* quoted fields
* commas inside quotes

## Phase 4

* escaped quotes
* quote escaping rules

## Phase 5

* multiline quoted fields

## Phase 6

* strict validation
* detailed parser errors

## Phase 7

* header support
* dictionary/object rows

## Phase 8

* CSV writer/serializer

## Phase 9

* streaming parser
* large file support

## Phase 10

* performance optimization
* benchmarking

---

# Example Usage

## Basic Parsing

```python
from csv_parser import parse

result = parse("""
name,age
Alice,20
Bob,25
""")

print(result)
```

Expected:

```python
[
    ["name", "age"],
    ["Alice", "20"],
    ["Bob", "25"]
]
```

---

## Headers Mode

```python
from csv_parser import parse

result = parse(data, headers=True)
```

Expected:

```python
[
    {
        "name": "Alice",
        "age": "20"
    }
]
```

---

# Running Tests

## Run All Tests

```bash
python -m unittest discover
```

---

## Run Specific Test File

```bash
python -m unittest tests.test_quotes
```

---

# Example Test

```python
import unittest

from csv_parser import parse


class TestBasicParsing(unittest.TestCase):
    def test_parses_single_row(self):
        self.assertEqual(
            parse("a,b,c"),
            [["a", "b", "c"]]
        )


if __name__ == "__main__":
    unittest.main()
```

---

# Important Edge Cases

The parser will eventually support:

## Empty Fields

```csv
a,,c
```

---

## Escaped Quotes

```csv
"She said ""hello"""
```

---

## Multiline Fields

```csv
"Line 1
Line 2"
```

---

## Unicode

```csv
こんにちは,😀
```

---

## Different Line Endings

```text
\n
\r\n
```

---

# Architecture Notes

The parser will likely evolve into a finite-state machine.

Possible parser states:

```text
START_FIELD
IN_FIELD
IN_QUOTED_FIELD
AFTER_QUOTE
END_ROW
```

This transition is intentional and part of the learning process.

---

# Future Improvements

Potential future features:

* async streaming parser
* property-based testing
* fuzz testing
* parser benchmarking
* type inference
* schema validation
* pandas interoperability

---

# Non-Goals

This project is NOT trying to replace:

* Python's built-in csv module
* pandas.read_csv()

This is primarily an educational and engineering-focused project.

---

# Inspiration

This project is inspired by the fact that many seemingly simple formats become surprisingly complex under real-world requirements.

CSV is one of the best examples.

---

# AI Assistance

AI tools were occasionally used during development for:

- brainstorming architecture ideas
- discussing parser edge cases
- reviewing design decisions
- generating roadmap/documentation drafts

All implementation decisions, debugging, testing, and refactoring were reviewed and adapted manually as part of the learning process.

---

# Contributing

Currently experimental.

Expect:

* breaking changes
* refactors
* architecture rewrites
* incomplete features

---

# License

MIT
