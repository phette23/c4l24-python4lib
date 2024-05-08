# Python{4}Lib (Code4Lib 2024 Workshop)

Post-conference workshop for Code4Lib 2024. Time and more specific list of topics TBD.

## Session Description

Python{4}Lib is an informal discussion group which meets biweekly over Zoom. The first hour of this workshop will be one of these informal discussions; a lightly guided chat about all things python in GLAM. For the rest of the session, we will focus on a variety of topics related to the python language with an eye towards improving our understanding of concepts as well as learning new things. Topics will adapt based on the attendees' interests, but may include: pymarc, pandas, spaCy, pytest, csvkit, Jupyter Notebooks, typing, common python patterns, powerful but underutilized components in the standard library, (relatively) new python 3 features, asynchronous code, code editor configuration (linting, pylance), and dependency management (virtualenv, poetry, pipenv).

In general, the session is intended for people with some experience programming in python. Novice to intermediate programmers should expect to learn new techniques but also see some familiar code. Beginners may feel a bit lost during conversations, but there will be beginner exercises and instructor assistance available.

## Workshop Prologue

We will abide by the [Code4Lib Code of Conduct](https://2024.code4lib.org/conduct/) during this workshop. Please be respectful of others and their opinions. If you need to [contact a Community Support Volunteer](https://2024.code4lib.org/conduct/#volunteers), you can do so in a number of ways:

- Email c4lcommunitysupport@googlegroups.com
- Use the [anonymous incident report form](https://css4csv.clir.org/anonymous-incident-report-form/)
- Message the `@css` group in the Code4Lib Slack. The group is also notified by messages containing the text `c4lcss` or `c4lcsv`.
- Message `@community_support_volunteers` on the Code4Lib Discord

**Other bits of housekeeping here** — where are the bathrooms? Water fountains? We will have a break about an hour in.

Then a round of introductions. Please share at least your name and a note about your relationship with Python, whether that's your expertise level, what kinds of coding you usually do, or something you're looking forward to talking about today.

## Getting Started

To get the most out of this session, attendees will need to download this repository and have Python 3. Python 3.10 should be able to utilize all features discussed but earlier versions should be able to run most of the code samples.

Links:

- [Download this repo](https://github.com/phette23/c4l24-python4lib/archive/refs/heads/main.zip) (GitHub > Code > Download ZIP)
  - or `git clone https://github.com/phette23/c4l24-python4lib` on the command line
- [Python](https://www.python.org/downloads/)
  - `brew install python` if you use homebrew on macOS

The repo contains Jupyter Notebooks, which most code editors should be able to open. You can also view them on GitHub. If you're not able to run the notebooks, you can either write code into a Python script or run it in an interactive Python shell.

If you have [poetry](https://python-poetry.org/) installed, you can run `poetry install` to create a virtual environment with all the packages mentioned in notebooks. It will include the ipykernel needed to run the notebooks. To get VS Code to give you the option to choose poetry's venv as a kernel, you may want to `mkdir .venv` in this repo before running `poetry install`.

A requirements.txt file is also included with the packages needed to run the notebooks. Here's how you would create a virtual environment and install the packages with pip:

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

## License

[CC0 Public Domain](https://creativecommons.org/publicdomain/zero/1.0/).
