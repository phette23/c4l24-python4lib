# Topics

- [ ] pandas
- [ ] spaCy / NLP, see [their free course](https://course.spacy.io/en/)
- [ ] pydantic
- [ ] async code https://realpython.com/async-io-python/
- [x] new features ([What's in Which Python](https://nedbatchelder.com/text/which-py.html) is a useful reference)
  - [x] dataclasses (3.7) really easy way to create typed classes of properties (e.g. for JSON data)
  - [x] `:=` walrus operator (3.8) asignments in expressions (e.g. `if (match := re.search(pattern, line)) is not None:`), probably most useful [in list comprehensions](https://realpython.com/python-walrus-operator/#list-comprehensions)
  - [x] `match...case` ([3.10](https://peps.python.org/pep-0622/)) especially stuff like [real-world match case](https://nedbatchelder.com/blog/202312/realworld_matchcase.html) where it's used to introspect variable data structures (like JSON API responses that vary in structure)
- [x] common patterns (list comprehensions instead of loops, creating a hashable dict?, deduping with `set`, access index in loop with `enumerate`)
- [x] pymarc patterns  (e.g. iterating over fields in a record instead of using the builtin `record.isbn` properties)
- [x] pytest (parametrization)
- [x] notebooks
- [x] type hints (3.5 but more & more features continue to be added like union types `dict|list` in 3.10)
- [x] dependency management (virtualenv, pyenv, poetry, pipenv)
  - [x] differences between poetry and pipenv (poetry better for writing libraries, pipenv better for applications?)

My favorites of these:

- [ ] spaCy (but hardest to have others play with due to model download)
- [x] pytest parametrization
- [x] f-strings
- [x] type hints
- [x] dependency management
