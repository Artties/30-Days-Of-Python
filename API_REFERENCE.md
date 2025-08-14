## API Reference

This repository contains Python modules, a minimal Flask web application, and data modules. This document lists all public APIs, functions, routes, and data components, with examples and usage instructions.

- Python version: 3.8+
- Package layout of interest:
  - `mypackage/` — small utility package (`arithmetics`, `greet`)
  - `mymodule.py` — example module with functions and constants
  - `python_for_web/` — Flask app with routes and templates
  - `data/` — public data structures (stop words, countries)

## Installation and quick start

### Use the Python modules locally
```bash
# From the repo root
python3 -q  # or use your preferred virtual environment
```
```python
# Examples
from mypackage.arithmetics import add_numbers, division
from mypackage.greet import greet_person
import mymodule

print(add_numbers(1, 2, 3))          # 6
print(division(10, 4))               # 2.5
print(greet_person('Jane', 'Doe'))   # Jane Doe, welcome to 30DaysOfPython Challenge!
print(mymodule.generate_full_name('Ada', 'Lovelace'))  # Ada Lovelace
```

### Run the Flask app
```bash
cd python_for_web
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
python app.py  # runs on http://0.0.0.0:5000
```

---

## Modules

### `mypackage.arithmetics`

- **add_numbers(*args) -> int | float**
  - Sums any number of numeric arguments.
  - Returns 0 for no arguments.
  - Example:
    ```python
    from mypackage.arithmetics import add_numbers
    add_numbers()                # 0
    add_numbers(1, 2, 3)         # 6
    add_numbers(1.5, 2.5, 3)     # 7.0
    ```

- **subtract(a: int | float, b: int | float) -> int | float**
  - Returns the result of a − b.
  - Example:
    ```python
    from mypackage.arithmetics import subtract
    subtract(10, 3)  # 7
    ```

- **multiple(a: int | float, b: int | float) -> int | float**
  - Returns a × b.
  - Example:
    ```python
    from mypackage.arithmetics import multiple
    multiple(6, 7)  # 42
    ```

- **division(a: int | float, b: int | float) -> float**
  - Returns a ÷ b (true division).
  - Raises `ZeroDivisionError` if b is 0.
  - Example:
    ```python
    from mypackage.arithmetics import division
    division(7, 2)   # 3.5
    ```

- **remainder(a: int | float, b: int | float) -> int | float**
  - Returns a % b.
  - Example:
    ```python
    from mypackage.arithmetics import remainder
    remainder(10, 3)  # 1
    ```

- **power(a: int | float, b: int | float) -> int | float**
  - Returns a ** b (exponentiation).
  - Example:
    ```python
    from mypackage.arithmetics import power
    power(2, 10)  # 1024
    ```

### `mypackage.greet`

- **greet_person(firstname: str, lastname: str) -> str**
  - Returns a friendly greeting string: "<firstname> <lastname>, welcome to 30DaysOfPython Challenge!"
  - Example:
    ```python
    from mypackage.greet import greet_person
    greet_person('Grace', 'Hopper')
    # 'Grace Hopper, welcome to 30DaysOfPython Challenge!'
    ```

### `mymodule`

- **generate_full_name(firstname: str, lastname: str) -> str**
  - Concatenates first and last name with a single space.
  - Example:
    ```python
    import mymodule
    mymodule.generate_full_name('Linus', 'Torvalds')  # 'Linus Torvalds'
    ```

- **sum_two_nums(num1: int | float, num2: int | float) -> int | float**
  - Returns the sum of two numbers.
  - Example:
    ```python
    import mymodule
    mymodule.sum_two_nums(2, 3.5)  # 5.5
    ```

- **gravity: float**
  - Constant representing standard gravity (9.81).
  - Example:
    ```python
    import mymodule
    mymodule.gravity  # 9.81
    ```

- **person: dict**
  - Example person profile dictionary with keys: `firstname`, `age`, `country`, `city`.
  - Example:
    ```python
    import mymodule
    mymodule.person['country']  # 'Finland'
    ```

Note: `12_Day_Modules/mymodule.py` and `20_Day_Python_package_manager/arithmetic.py|greet.py` contain tutorial duplicates of the above APIs with the same behavior. Prefer importing from the root-level `mymodule.py` and `mypackage/` for production-like use.

---

## Data modules

### `data.stop_words`

- **stop_words: list[str]**
  - A list of common English stop words useful for text processing.
  - Example:
    ```python
    from data.stop_words import stop_words

    tokens = 'This is only a simple example'.lower().split()
    filtered = [t for t in tokens if t not in stop_words]
    # filtered -> ['simple', 'example']
    ```

### `data.countries`

- **countries: list[str]**
  - A list of country names.
  - Example:
    ```python
    from data.countries import countries
    'Finland' in countries  # True
    len(countries)          # e.g. 193+
    ```

### `data.countries-data`

- A large list of dictionaries with fields: `name`, `capital`, `languages` (list[str]), `population`, `flag` (URL), `currency`.
- This file is a valid Python list literal but its hyphenated filename is not importable as a normal module.
- Recommended usage:
  - Rename the file to `countries_data.py` and then:
    ```python
    from data.countries_data import countries_data  # if you wrap the list in a variable named countries_data
    ```
  - Or read and parse it directly:
    ```python
    import ast, pathlib

    text = pathlib.Path('data/countries-data.py').read_text(encoding='utf-8')
    countries = ast.literal_eval(text)
    print(countries[0]['name'])
    ```

---

## Web application (`python_for_web`)

A minimal Flask application with a small set of routes and templates.

### Dependencies
- Pinned in `python_for_web/requirements.txt` (Flask 1.1.1 and friends).

### Running locally
```bash
cd python_for_web
python app.py  # starts the development server on http://0.0.0.0:5000
```

### Routes

- **GET /** — Home page
  - Renders `templates/home.html`.
  - Template context:
    - `name: str` — title string
    - `techs: list[str]` — technologies to list

- **GET /about** — About page
  - Renders `templates/about.html`.
  - Template context:
    - `name: str`

- **GET /result** — Result page
  - Renders `templates/result.html`.

- **GET, POST /post** — Text Analyzer form
  - GET: Renders `templates/post.html` with a `<textarea name="content">`.
  - POST: Reads `content` from `request.form` and redirects to `/result`.

### Example requests

- Fetch home page:
  ```bash
  curl -i http://localhost:5000/
  ```

- Submit the text analyzer form (POST /post):
  ```bash
  curl -i -X POST \
    -F 'content=This is a sample text.' \
    http://localhost:5000/post
  ```

### Templating notes
- Templates extend `templates/layout.html`.
- `home.html` expects `name` and `techs`.
- `post.html` contains a form that posts to `/post` with `content` as the only field.

---

## Error handling and edge cases

- `division(a, b)` will raise `ZeroDivisionError` when `b == 0`.
- Arithmetic functions do not coerce types; pass numeric types.
- `data/countries-data.py` has a hyphenated filename; import with a workaround (see above) or rename it.

---

## Conventions

- Public APIs are defined by top-level functions and variables in `mypackage/*` and `mymodule.py`.
- Tutorial duplicates exist under `12_Day_Modules/` and `20_Day_Python_package_manager/`; treat them as examples rather than canonical imports.

---

## License

If a license file exists in this repository, refer to it for usage terms. Otherwise, treat these modules and data as educational examples.