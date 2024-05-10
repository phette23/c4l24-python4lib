# Dependency Management

We've spoken about managing dependencies at a past Python{4}lib, specifically virtual environments, but here we'll look more specifically at tools that bundle virtual environment and dependency management.

Since most of the "code" in this file is shell commands, it is an ordinary markdown document and not a Jupyter notebook.

## Levels of Dependency Management

There are roughly three levels of sophistication in managing your Python dependencies:

1. Static **requirements.txt** file
2. Virtual environment populated from requirements.txt
3. Pipenv or Poetry

I personally have not used [Conda](https://conda.io/projects/conda/en/latest/user-guide/getting-started.html) in a long time but it fits in here somewhere, too. I would place it somewhere between levels 2 and 3.

## Requirements

Managing dependencies via the static requirements.txt file is an old and well-supported method of managing dependencies. You use `pip` to install dependencies.

```shell
# add a dependency
pip install requests
# write your current dependencies to a file
pip freeze > requirements.txt
# install all dependencies from a file
pip install -r requirements.txt
# remove a dependency
pip uninstall requests
# remember to update the requirements.txt file!
pip freeze > requirements.txt
```

### Requirements Advantages

Very simple and requires no extra software. `pip` comes with Python.

### Requirements Disadvantages

As soon as we have multiple projects with contradictory requirements (one needs version X but the other needs version Y), this approach no longer works.

We have to remember to update the requirements.txt file every time our dependencies change.

Not easy to manage different versions of Python. Easy to become confused about what is installed where.

Inevitably leads to extraneous dependencies that are not needed staying in your environment. Extraneous dependencies take up disk space, lead to conflicts, and increase the number of security vulnerabilities we're exposed to.

## Virtual Environments

Create a "virtual environment" (often shortened to "venv") where our dependencies can be isolated from the system Python and from other projects. Virtual environments address the problem of conflicting dependencies.

```shell
# create a virtual environment in the .venv directory
Python -m venv .venv
# "activate" the virtual environment (tell your shell to use its Python)
source .venv/bin/activate
# install, freeze, etc. steps are same as above
# packages are installed into the venv and not the system Python
```

### Venv Advantages

Like requirements, it's simple and requires no extra software. There are some wrappers around `venv` which make it easier to use.

### Venv Disadvantages

We have to remember to activate the virtual environment before running our code.

As above, keeping our requirements.txt file up-to-date is always an extra step besides installation/removal.

Adding other tools combined with venvs lets us manage multiple Python versions but out of the box `venv` doesn't do this for you.

## Pyenv

Pyenv lets you manage multiple versions of Python. It's a good companion to virtual environments.

```shell
# install a version of Python
pyenv install 3.8.5
# specify a version of Python for a directory
pyenv local 3.8.5
# this creates a .Python-version file in your project directory and Python commands
# (including creating a venv with one) will use that version
```

Pyenv on its own does not do dependency management so I won't discuss it further here, but it is a good way to get more out of your virtual environments.

## Pipenv

Pipenv is a tool that combines virtual environments and dependency management. It has its own file format, `Pipfile`, that is more informative and human-readable than `requirements.txt`, as well as a `Pipfile.lock` which is similar to requirements.txt in that it specifies the exact versions of our dependencies.

```shell
# create a new project with a Pipfile
pipenv install requests
```

### Pipenv Advantages

The Pipfile contains more structured information about our project's dependencies than a requirements.txt file. In particular, it's easy to separate development dependencies from production dependencies.

Pipenv automatically creates a virtual environment for our project with the first `pipenv install`.

Pipenv updates your dependencies in the Pipfile when we run `pipenv install` or `pipenv uninstall`, we don't need to run `pip freeze` after every change.

Pipfile stores a _dependency graph_ of our project, so we can see what packages depend on what other packages. See the digression after the next section.

Pipenv lets us specify a valid range of _Python versions_ for our project in the Pipfile, so if our code relies on a feature that was introduced in Python 3.7, we can specify that in the Pipfile.

We can specify scripts in the Pipfile that can be run with `pipenv run scriptname`. If running our app involves several steps, like starting a database and web server, we can put all those steps in [a Pipfile script](https://pipenv.pypa.io/en/stable/scripts.html).

I don't have the numbers but I suspect pipenv is the most popular tool for dependency management in the Python community. So many projects use it that we might need to use it eventually even if we use other tools for our own projects.

### Pipenv Disadvantages

We still need to remember to run commands in the venv that pipenv creates with either `pipenv shell`, which enters a shell with the venv activated, or `pipenv run` which runs a single command in the venv, e.g. `pipenv run pytest` or `pipenv run Python script.py`.

Pipenv is more complex, with many more subcommands than `pip` or `venv`. For instance, the two dependency files (Pipfile and Pipfile.lock) introduce an extra layer of complexity which necessitates commands like `pipenv lock` and `pipenv sync`.

Pipenv is rather slow because it is constantly calculating and verifying our dependency graph. `pipenv install` is slower than `pip install`. The larger the project, the slower pipenv gets, and `pipenv graph` in particular takes a while on a large project. For an InvenioRDM repository, `pipenv graph` took 14 seconds while `pip list` took only 4.

While pipenv's dependency graph makes it possible to remove extraneous packages, it's an extra step (running `pipenv clean`) that we have to remember. Other package managers like Node's `npm` simply do this as part of the `uninstall` command which makes more sense. There is no value to keeping extraneous packages in our environment.

While Pipenv lets us specify a minor version of Python, like 3.5 or 3.7, it doesn't let us specify a range of valid versions (e.g. ">=3.7") like it does with packages. The Pipenv developers have stated they do not intend to support version ranges for Python.

## Dependency Graphs Digression

Many packages we install have their own dependencies. For example, `requests` depends on `urllib3`. When we install `requests`, `urllib3` is installed as well. A _dependency graph_ is a hierarchical representation of dependencies. Requirements.txt is a flat, non-hierarchical list of dependencies which is inferior to graphs for a few reasons:

1. We cannot easily tell _why_ a particular package is present (was it a direct dependency or an indirect one?)
2. It is hard to notice when an indirect dependency is no longer needed
3. Removing indirect dependencies is a manual process
4. 2 and 3 above mean that extraneous dependencies accumulate over time

Below, I'll illustrate a flat list versus a graph using the `requests` package.

```shell
# create & activate a virtual environment
Python -m venv .venv && source .venv/bin/activate
# install requests
pip install requests
# list installed packages
pip freeze > requirements.txt
```

When I ran these steps on my machine, the `requirements.txt` file contained:

```txt
certifi==2024.2.2
charset-normalizer==3.3.2
idna==3.7
requests==2.31.0
urllib3==2.2.1
```

Only `requests` is a direct dependency of my project. The rest were installed by requests. Say I were to uninstall `requests` and run a second `pip freeze` to update my requirements; I still have the other packages installed.

```shell
> pip uninstall requests && pip freeze > requirements.txt
> cat requirements.txt
certifi==2024.2.2
charset-normalizer==3.3.2
idna==3.7
urllib3==2.2.1
```

What if `requests` makes a change and no longer depends upon `charset-normalizer`? I have to manually remove it from the `requirements.txt` file. But what if I have several other direct dependencies? I have to check all my other dependencies to see if they depend on `charset-normalizer` before knowing it's safe to remove.

On the other hand, a Pipfile only lists direct dependencies. It uses a separate Pipfile.lock for specifying every package that was installed. Both these files are kept current by `pipenv install`.

```shell
> pipenv install pymarc requests # install 2 direct dependencies
> pipenv graph
pymarc==5.1.2
requests==2.31.0
├── certifi [required: >=2017.4.17, installed: 2024.2.2]
├── charset-normalizer [required: >=2,<4, installed: 3.3.2]
├── idna [required: >=2.5,<4, installed: 3.7]
└── urllib3 [required: >=1.21.1,<3, installed: 2.2.1]
> pipenv uninstall requests
> pipenv clean # extra step to remove the extraneous dependencies
> pipenv graph
pymarc==5.1.2
```

Pipenv not only shows us that `charset-normalizer` is a dependency of requests but it shows the acceptable range of potential versions as well as the installed one. If multiple direct dependencies use the same indirect dependency, it's only installed once, and pipenv negotiates a version which satisfies all the direct dependencies. `pipenv clean` removes extraneous dependencies.

## Poetry

Poetry is analogous to Pipenv; it is a higher order tool which manages our venv and dependency graph for us.

```shell
poetry init # interactively asks you questions about your project
poetry add pymarc requests # if you didn't add deps interactively
poetry show --tree # print the dependency graph
```

Poetry shares all the general advantages and disadvantages of Pipenv; it creates the venv automatically and understands your dependencies as a graph and not a flat list. Below, I focus on the ways it differs from Pipenv.

### Poetry Advantages

Poetry uses the pyproject.toml file format which is used by other Python tools including package building and distribution ones. If we are writing a library that will be distriubted on PyPI you probably want to use a pyproject.toml file anyways and Poetry gives you other functionality for free.

`poetry remove` removes a package from your dependencies _and our venv_ in one step, it is equivalent to `pipenv uninstall $PACKAGE && pipenv clean`.

`poetry env` commands provide nice insight into our project's virtual environment(s) in a manner I find more convenient than Pipenv's commands.

Poetry has a `build` command for building distributions if we are publishing a our code on PyPI.

Poetry allows for a range of Python versions in the pyproject.toml file.

I think the Poetry CLI is prettier (makes better use of colorization) than Pipenv's. I haven't done a systematic study but it seems to be faster, too.

### Poetry Disadvantages

While Poetry has a scripts section like Pipenv, it doesn't have a way to run multiple commands in sequence like Pipenv does. We have to specify a function in a Python file to run. If our project benefits from convenient shell scripts or chains of commands, Poetry is slightly less convenient than Pipenv.

It seems to be less popular than pipenv and perhaps receiving a little less development effort.

Some of the Poetry CLI choices are unorthodox. `poetry list` lists _poetry's own subcommands_ and not our dependencies, it is a verbose version of `poetry --help`, which only shows global flags. Dependencies are added with `poetry add` and `poetry install` merely installs the ones listed (it's analogous to `pipenv sync`). `poetry show` prints a flat list of dependencies by default and requires the `--tree` flag to produce a graph. Honestly, none of these choices is terribly odd on its own, it's the fact that Pipenv and Poetry fulfill the same role but with nuanced differences that makes their divergence frustrating.

## Conclusion

I strongly recommend using either Pipenv or Poetry for Python projects and switching to one of them soon so you become comfortable with them. I don't think it matters much which one you choose. If you are going to be writing and distributing packages, then Poetry might be more convenient because it uses the pyproject.toml file format. If you are writing applications, especially ones with many components some of which are non-Python binaries, then Pipenv might be more convenient because its scripts are more flexible.
