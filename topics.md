# Topics

- [ ] pymarc patterns  (e.g. iterating over fields in a record instead of using the builtin `record.isbn` properties)
- [ ] pandas
- [ ] spaCy / NLP, see [their free course](https://course.spacy.io/en/)
- [x] pytest (parametrization)
- [ ] csvkit CLI
- [ ] notebooks
- [x] type hints (3.5 but more & more features continue to be added like union types `dict|list` in 3.10)
- [ ] pydantic
- [ ] common patterns (list comprehensions instead of loops, creating a hashable dict?)
- [ ] powerful but underutilized components in the standard library (deduping with `set`, access index in loop with `enumerate`)
- [ ] new features ([What's in Which Python](https://nedbatchelder.com/text/which-py.html) is a useful reference)
  - [x] f-strings (3.6) are arguably the easiest way to format strings now
  - [ ] dataclasses (3.7) really easy way to create typed classes of properties (e.g. for JSON data)
  - [ ] `:=` walrus operator (3.8) asignments in expressions (e.g. `if (match := re.search(pattern, line)) is not None:`), probably most useful [in list comprehensions](https://realpython.com/python-walrus-operator/#list-comprehensions)
  - [ ] `match...case` ([3.10](https://peps.python.org/pep-0622/)) especially stuff like [real-world match case](https://nedbatchelder.com/blog/202312/realworld_matchcase.html) where it's used to introspect variable data structures (like JSON API responses that vary in structure)
- [ ] async code https://realpython.com/async-io-python/
- [ ] code editor configuration (linting, pylance)
- [ ] dependency management (virtualenv, pyenv, poetry, pipenv)
  - [ ] differences between poetry and pipenv (poetry better for writing libraries, pipenv better for applications?)

My favorites of these:

- [ ] spaCy (but hardest to have others play with due to model download)
- [x] pytest parametrization
- [x] f-strings
- [x] type hints
- [ ] dependency management
