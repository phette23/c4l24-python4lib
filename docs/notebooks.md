# Jupyter Notebooks

Jupyter Notebooks, which you may also hear referred to as IPython Notebooks, are documents with embedded blocks of executable code. It's as if you could run, and see the output of, code blocks in a Markdown file. The ".ipynb" files in this repository are all notebooks.

I don't use notebooks regularly, so this will only be a brief overview meant to show you how to use them in the context of this workshop.

## What are notebooks good for?

Why would you use a notebook? Being able to write code alongside its documentation helps you explain the code in greater detail. These use cases all seem suitable:

- Data processing
- Lessons (like the ones here!)
- Exploration

With data processing, the code itself can be quite precise but the underlying reasoning very complex. It might take a couple paragraphs to explain one line of Pandas code that filters your data. Data processing also tends to have multiple semi-independent steps (load, filter, aggregate, etc.) which lend themselves to being broken up into separate cells.

Lessons are a great fit because you can write explanatory text and then a demonstration. Learners can modify the cells, run them, and see what the effect is. Similarly, exploring different tools or approaches works well because the notebook lets you outline your thinking while also rerunning particular segments of code.

## Running Notebooks

There may be other ways to run notebooks, I did not have time to research them all, but here are a few options so everyone should be able to use at least one method that suits them.

### Google Colab

[Google Colab](https://colab.research.google.com/) is the easiest way to run notebooks because you don't need any extra software or setup steps, but you have to access them through the Colab website.

You can open the notebooks in this repo directly from GitHub in the **Open Notebook** menu, select GitHub, then paste this repo's URL https://github.com/phette23/c4l24-python4lib/ or `phette23/c4l24-python4lib` into the search menu. Each notebook has a button to open it in a new tab. There will be a warning about running code from a non-Google source the first time you execute code.

You can also **Upload** notebook files from your hard drive. So you could download this repo, then upload the notebooks to Colab.

### VS Code

See [Jupyter Notebooks in VS Code](https://code.visualstudio.com/docs/datascience/jupyter-notebooks). VS Code supports notebooks out of the box, but when you try to run one you will need to configure the Python interpeter, or kernel, that the notebooks uses. The interpeter must have the `ipykernel` package installed. I was able to create a virtual inside the project directory and install `ipykernel` in it, then select that kernel in the notebook (see [my notes below](#running-notebooks-in-a-virtual-environment)). If you choose your system python, make sure to run `pip install ipykernel`.

### PyCharm

The first time you open a Notebook in PyCharm, it'll prompt you to install the Jupyter plugin. If code cells don't run immediately, it's probably because the notebook's interpreter doesn't have ipykernel. You can change the interpreter in the notebook's Jupyter Server settings. For instance, if you have a .venv virtual environment in your project with ipykernel, you can select that interpreter.

## Running Notebooks in a Virtual Environment

These were the the exact steps I took to use Poetry to create a virtual environment and then choose that venv's interpreter as the kernel for these notebooks in VS Code.

```shell
mkdir .venv # poetry will create the venv here by default
poetry install # uses this project's pyproject.toml
```

Then in VS Code, open a notebook and use the **Select Kernel** button in the upper right, look through the Python Environments and the .venv is listed first with star beside its name.

## Important Notes

A notebook line that begins with an exclamation point `!` is a shell command. This is how we can install packages from within a notebook, with a line like `!pip install pymarc`.

Each code block prints the output of shell commands and `print` statements immediately after it. These outputs are actually stored within the notebook file and can be viewed later. For instance, when you look at the [type-hints notebook](type-hints.ipynb), you can see the output (some of which is randomized) from when I last ran the code on my machine.
