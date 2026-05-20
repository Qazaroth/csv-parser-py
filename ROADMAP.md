# ROADMAP.md

# Python CSV Parser Roadmap

A learning-focused roadmap for building a CSV parser in Python while practicing:

* Test-Driven Development (TDD)
* Parser design
* State machines
* Error handling
* Refactoring
* Performance optimization
* Streaming architecture

---

# Project Goals

By the end of this project, you should understand:

* How parsers work internally
* How CSV escaping rules actually function
* How to structure a parser cleanly
* How to use TDD effectively
* How to evolve architecture incrementally
* How to handle malformed input safely
* How streaming parsers work

This project is less about CSV itself and more about becoming a better software engineer.

---

# Recommended Stack

## Python Version

Recommended:

```text
Python 3.12+
```

## Testing Framework

Recommended:

```text
pytest
```

Install:

```bash
pip install pytest
```

Optional tools:

```bash
pip install pytest-cov
pip install hypothesis
pip install ruff
pip install black
```

---

# Recommended Project Structure

```text
csv-parser/
├── src/
│   └── csv_parser/
│       ├── __init__.py
│       ├── parser.py
│       ├── tokenizer.py
│       ├── states.py
│       ├── writer.py
│       ├── errors.py
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
├── pyproject.toml
├── README.md
└── ROADMAP.md
```

---

# Development Philosophy

## IMPORTANT RULES

### 1. Write tests FIRST

Never implement features before writing tests.

### 2. Implement the smallest possible solution

Avoid overengineering.

### 3. Refactor only after tests pass

Keep behavior stable.

### 4. Commit often

Small commits help track architectural evolution.

---

# TDD Workflow

For EVERY feature:

## Step 1 — Write failing test

Example:

```python
from csv_parser import parse


def test_parses_single_row():
    assert parse("a,b,c") == [["a", "b", "c"]]
```

## Step 2 — Run tests

See it fail.

## Step 3 — Implement minimum code

Do the bare minimum.

## Step 4 — Run tests again

Make sure they pass.

## Step 5 — Refactor

Improve code quality without changing behavior.

---

# Phase 1 — Minimal Parser

## Goal

Create the simplest possible parser.

## Features

* Parse CSV from string
* Parse rows
* Parse comma-separated fields
* Return list[list[str]]

## Example

Input:

```csv
a,b,c
1,2,3
```

Output:

```python
[
    ["a", "b", "c"],
    ["1", "2", "3"]
]
```

---

## Suggested Tests

```python
def test_parses_single_row():
    pass


def test_parses_multiple_rows():
    pass


def test_parses_empty_input():
    pass


def test_parses_trailing_newline():
    pass
```

---

## Suggested Implementation

Initially, using:

```python
splitlines()
split(",")
```

is acceptable.

DO NOT support quotes yet.

---

# Phase 2 — Configurable Delimiters

## Goal

Support different delimiters.

## Features

* Semicolon delimiter
* Tab delimiter
* Custom delimiter argument

## Example

```python
parse(data, delimiter=";")
```

## Tests

```python
def test_parses_semicolon_delimiter():
    pass


def test_parses_tab_delimiter():
    pass
```

---

# Phase 3 — Quoted Fields

## Goal

Support actual CSV rules.

## Features

* Quoted fields
* Commas inside quoted fields

## Example

Input:

```csv
name,note
Alice,"likes apples, bananas"
```

Expected:

```python
[
    ["name", "note"],
    ["Alice", "likes apples, bananas"]
]
```

---

## Why This Phase Matters

This is where naive split-based parsing breaks.

You will likely need:

* character-by-character parsing
* parser states
* a finite-state machine

---

## Suggested States

```text
START_FIELD
IN_FIELD
IN_QUOTED_FIELD
AFTER_QUOTE
END_ROW
```

---

## Suggested Tests

```python
def test_parses_quoted_field():
    pass


def test_parses_comma_inside_quotes():
    pass


def test_parses_empty_quoted_field():
    pass
```

---

# Phase 4 — Escaped Quotes

## Goal

Support escaped quotes.

## Example

Input:

```csv
"She said ""hello"""
```

Expected:

```python
[["She said \"hello\""]]
```

---

## Tests

```python
def test_parses_escaped_quotes():
    pass


def test_parses_multiple_escaped_quotes():
    pass
```

---

# Phase 5 — Multiline Fields

## Goal

Support newlines inside quoted fields.

## Example

```csv
name,bio
Alice,"Line 1
Line 2"
```

Expected:

```python
[
    ["name", "bio"],
    ["Alice", "Line 1\nLine 2"]
]
```

---

## Learning Focus

This phase teaches:

* state persistence
* buffering
* tokenizer design
* parsing complexity

---

## Tests

```python
def test_parses_multiline_field():
    pass


def test_parses_multiple_multiline_records():
    pass
```

---

# Phase 6 — Error Handling

## Goal

Create meaningful parser errors.

## Features

* line numbers
* column numbers
* strict mode
* malformed CSV detection

---

## Example Errors

### Unclosed quote

```text
ParseError:
Line 4, Column 12
Unclosed quoted field
```

### Invalid escape sequence

```text
ParseError:
Unexpected quote character
```

---

## Suggested Error Class

```python
class CSVParseError(Exception):
    pass
```

---

## Tests

```python
def test_raises_on_unclosed_quote():
    pass


def test_reports_correct_line_number():
    pass


def test_reports_correct_column_number():
    pass
```

---

# Phase 7 — Header Support

## Goal

Support dictionary/object rows.

## Example

Input:

```csv
name,age
Alice,20
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

## Features

* optional headers=True
* duplicate header handling
* missing value handling

## Tests

```python
def test_parses_headers():
    pass


def test_maps_rows_to_dicts():
    pass
```

---

# Phase 8 — CSV Writer

## Goal

Serialize data back into CSV.

## Features

* escape quotes correctly
* quote fields only when necessary
* configurable delimiter

---

## Example

Input:

```python
[
    ["Alice", "hello, world"]
]
```

Expected CSV:

```csv
Alice,"hello, world"
```

---

## Tests

```python
def test_writes_basic_csv():
    pass


def test_writes_quoted_fields():
    pass


def test_roundtrip_parse_write():
    pass
```

---

# Phase 9 — Streaming Parser

## Goal

Parse large files efficiently.

## Features

* generator API
* chunked parsing
* low memory usage

---

## Example API

```python
for row in parse_stream(file):
    print(row)
```

---

## Learning Focus

This phase teaches:

* generators
* iterators
* chunk processing
* memory-efficient architecture

---

## Tests

```python
def test_streaming_large_file():
    pass


def test_chunk_boundaries():
    pass
```

---

# Phase 10 — Performance Optimization

## Goal

Make the parser efficient.

## Ideas

* reduce allocations
* avoid unnecessary string copies
* benchmark parsing speed
* optimize hot paths

---

## Benchmark Ideas

Compare against:

* Python csv module
* pandas.read_csv()

Metrics:

* rows/sec
* memory usage
* parse latency

---

# Phase 11 — RFC 4180 Compliance

## Goal

Support official CSV specification behavior.

## Topics

* CRLF handling
* proper quote escaping
* whitespace behavior
* edge-case compatibility

---

# Phase 12 — Property-Based Testing

## Goal

Find edge cases automatically.

## Recommended Library

```text
hypothesis
```

Install:

```bash
pip install hypothesis
```

---

## Example

Generate random CSV input and ensure:

```text
write(parse(x)) == x
```

for valid CSV.

---

# Phase 13 — Fuzz Testing

## Goal

Break your parser intentionally.

## Inputs To Generate

* random quotes
* malformed rows
* huge fields
* unicode characters
* random delimiters

---

# Important Edge Cases

## Empty fields

```csv
a,,c
```

---

## Empty quoted field

```csv
""
```

---

## Trailing delimiter

```csv
a,b,
```

---

## Escaped quotes

```csv
""""
```

Expected:

```text
"
```

---

## Unicode

```csv
こんにちは,😀
```

---

## Different line endings

```text
\n
\r\n
```

---

# Recommended Final API

## Basic Parsing

```python
parse(data)
```

## Headers Mode

```python
parse(data, headers=True)
```

## Custom Delimiter

```python
parse(data, delimiter=";")
```

## Strict Mode

```python
parse(data, strict=True)
```

## Streaming

```python
parse_stream(file)
```

## Writing CSV

```python
write(rows)
```

---

# Suggested Milestones

## Beginner

* basic parser
* delimiters
* quoted fields

## Intermediate

* multiline support
* escaped quotes
* error handling
* headers

## Advanced

* streaming parser
* writer
* RFC compliance
* fuzz/property testing
* performance optimization

---

# Final Recommendation

DO NOT rush into advanced features.

The real educational value comes from:

* incremental improvements
* constant refactoring
* handling edge cases
* evolving architecture naturally

A CSV parser seems simple at first.

That is exactly why it is such a good engineering exercise.
