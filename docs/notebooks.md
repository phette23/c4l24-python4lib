# Jupyter Notebooks

Jupyter Notebooks, which we may also hear referred to by their older name "IPython Notebooks," are documents with embedded blocks of executable code. It's as if we could run, and see the output of, code blocks in a Markdown file. The ".ipynb" files in this repository are all notebooks.

This document is a brief overview of the features of notebooks mostly meant to make it so we can use them during the workshop.

## What are notebooks good for?

Why would we use a notebook? Being able to write code alongside its documentation helps us explain the code in greater detail. These use cases all seem suitable:

- Data processing
- Lessons (like the ones here!)
- Exploration

With data processing, the code itself can be quite precise but the underlying reasoning very complex. It might take a couple paragraphs to explain one line of Pandas code that filters our data. Data processing also tends to have multiple semi-independent steps (load, filter, aggregate, etc.) which lend themselves to being broken up into separate cells.

Lessons are a great fit because we can write explanatory text and then a demonstration. Learners can modify the cells, run them, and see what the effect is. Similarly, exploring different tools or approaches works well because the notebook lets us outline our thinking while also rerunning particular segments of code.

## Running Notebooks

There may be other ways to run notebooks, I did not have time to research them all, but here are a few options so everyone should be able to use at least one method that suits them.

### Google Colab

[Google Colab](https://colab.research.google.com/) is the easiest way to run notebooks because we don't need any extra software or setup steps, but we have to access them through the Colab website. Colab notebooks come with many packages pre-installed, including pandas and spaCy, which is quite convenient.

We can open the notebooks in this repo directly from GitHub in the **Open Notebook** menu, select GitHub, then paste this repo's URL https://github.com/phette23/c4l24-python4lib/ or `phette23/c4l24-python4lib` into the search menu. Each notebook has a button to open it in a new tab. There will be a warning about running code from a non-Google source the first time we execute code.

We can also **Upload** notebook files from our hard drive. So we could download this repo, then upload the notebooks to Colab.

### VS Code

See [Jupyter Notebooks in VS Code](https://code.visualstudio.com/docs/datascience/jupyter-notebooks). VS Code supports notebooks out of the box, but when we run code blocks we need to configure the Python interpeter, or kernel, that the notebooks uses. The interpeter must have the `ipykernel` package installed. I was able to create a virtual inside the project directory and install `ipykernel` in it, then select that kernel in the notebook (see [my notes below](#running-notebooks-in-a-virtual-environment)). If we chose our system python, we need to run `pip install ipykernel`.

### PyCharm

The first time we open a Notebook in PyCharm, it'll prompt us to install the Jupyter plugin. If code cells don't run immediately, it's probably because the notebook's interpreter doesn't have ipykernel. We can change the interpreter in the notebook's Jupyter Server settings. For instance, if we have a .venv virtual environment in our project with ipykernel, we can select that interpreter.

## Running Notebooks in a Virtual Environment

These were the the exact steps I took to use Poetry to create a virtual environment and then choose that venv's interpreter as the kernel for these notebooks in VS Code.

```shell
mkdir .venv # poetry will create the venv here by default
poetry install # uses this project's pyproject.toml
```

Without Poetry, the steps are:

```shell
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Then in VS Code, open a notebook and use the **Select Kernel** button in the upper right, look through the Python Environments and the .venv is listed first with star beside its name.

## Important Notes

A notebook line that begins with an exclamation point `!` is a shell command. This is how we can install packages from within a notebook, with a line like `!pip install pymarc`. That _should_ work if the notebook is using the right Python interpreter (e.g. one in the right virtual environment instead of the system python).

Each code block prints the output of shell commands and `print` statements immediately after it. These outputs are actually stored within the notebook file and can be viewed later. For instance, when we look at the [type-hints notebook](type-hints.ipynb), we see the output (some of which is randomized) from when I last ran the code on my machine.

## Cells & Execution Order

Notebook cells are run in order, and the state of the Python interpreter is maintained between cells. This means that if we run a cell that imports a module, we can use that module in subsequent cells. If we define a variable in one cell, it will still be present (and able to modified!) in the next cell.

A common pattern is to edit and rerun cells multiple times until we get the output we want. This works well, but changes to a cell in the middle of a notebook may break cells later on (e.g. if we delete or change the type of a variable). Thankfully, notebooks typically have a "run all cells before this one" option. This is useful to reset the state of the interpreter when a series of changes has created a mess.
