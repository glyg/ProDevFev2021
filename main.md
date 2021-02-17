---
title: Jupyter Notebooks et al.
author: Guillaume Gay, CENTURI multi-engineering
logo: images/logo.png
fontsize: 8pt
width: 1080
height: 800
theme: solarized
data-transition: none
center: 0
...

----

![](images/Jupyter_logo.svg){ width=60% }


# a brief history


* Fernando Perez developed IPython during his fundamental physics PhD
(in University of Colorado in 2001), along the rest of the scipy stack: numpy (2005), scipy and matplotlib.

* Around 2010 the authors of IPython developed ipython notebook, creating a web interface

* In 2014, the IPython project became Jupyter as the communication protocol became language agnostic, & funding from Berkley Data Science Initiative allowed to pay for development



# core architecture

:::{.columns}::::
:::{.column}
![[Jupyter messaging system](https://jupyter-client.readthedocs.io/en/latest/messaging.html)](https://jupyter-client.readthedocs.io/en/latest/_images/frontend-kernel.png)
:::
:::{.column}
* At the core is a messaging system between a **kernel** and clients.

* This protocol based on [ZeroMQ](https://zeromq.org) is **language agnostic**

* Kernels in Python, R, Julia, Matlab even C++ (with the
  [cling](https://github.com/root-project/cling) interpreter) and much
  [more](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)
:::
::::::

## Jupyter notebook and jupyter lab

::: {.columns } :::
:::{.column}
(Notebook: flat, linear)[https://jupyter.org/assets/jupyterpreview.png]
:::
:::{.column}
(Lab: richier)[https://jupyter.org/assets/labpreview.png]
:::


# use cases

## As a REPL

I use jupyter notebook as a Read-Eval-Print Loop system

I like:

* Completion, inline doc, inline outputs

* Editable history

* Example

Some dislike:

* Heavier than a console
* Confusing exectution order (more on that later)

## As a UI

Notebooks are intended to be *programable documents*
Publish code and documentation within the same object

### Markdown and multi-media

* Pandoc-markdown support (maths, inline html)
* Imbed with html e.g. `<video> </video>`
* Export to pdf (poor) and html
* Extensions (e.g. Jupyter Book, see P. Navaro's [work](https://pnavaro.github.io/big-data/intro.html)


### Widget system

* ipywidgets
* examples


### Voilà

* example

### IPython Parallel

Harness ipython messaging architecture for parallel execution:

https://ipyparallel.readthedocs.io/en/latest/intro.html

```python
In [1]: import ipyparallel as ipp

In [2]: c = ipp.Client()

In [3]: c.ids
Out[3]: [0, 1, 2, 3]

In [4]: c[:].apply_sync(lambda : "Hello, World")
Out[4]: [ 'Hello, World', 'Hello, World', 'Hello, World', 'Hello, World' ]
```

* Easy to setup
* But very platform specific (I prefer joblib)

## caveats

### Execution order

* When you re-run a cell, all the variables within that cell are updated
* State can change
* Cell that produced the change can disappear
* Not worse than "underline & evaluate" in IDEs


-----

####  Solution

* Restart and run all
* Ensure linear execution


### Spaghetti notebooks

* Push to write imperative programming
* Cells 100s of lines long
* Intractable notebooks

----

#### Solution

1. Refactor early
2. Companion utils.py
3. Gradually evolve in a project architecture


### Versioning

* Notebooks are JSON documents
* Output (e.g. images) are serialized
* Execution number and session uuids change

. . .

> Diffs are ugly
> Merges are a mess

----

> Solutions

* pre-commit hook to delete output before push
* [nbdime](https://nbdime.readthedocs.io/en/latest/): visually acurate and efficient diff & merge for the notebooks

# Code Distribution

* Well intergrated with `conda`
* Built-in client / server architecture
* "Easy" to setup remote execution
* Security is not easy

## [BinderHub](https://binderhub.readthedocs.io/en/latest/index.html)

* Free service for OSS projects
* Just need `environment.yml`
* [An example](https://github.com/DamCB/tyssue-demo)

. . .

(also google colab)


## [JupyterHub](https://hub.jupyter.org)

* Manage remote access & authentication
* Manage multiple users
* Kubernetes capable (mutliple containers, load-balancing)
