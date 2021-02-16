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


# a brief history

::: incremental :::

* Fernando Perez developed IPython during his fundamental physics PhD
(in University of Colorado in 2001), along the rest of the scipy stack: numpy (2005), scipy and matplotlib.

* Around 2010 the authors of IPython developed ipython notebook, creating a web interface

* In 2014, the IPython project became Jupyter as the communication protocol became language agnostic, & funding from Berkley Data Science Initiative allowed to pay for development

:::


# core architecture

::: incremental :::

* At the core is a messaging system between a **kernel** and clients.

![[Jupyter messaging system](https://jupyter-client.readthedocs.io/en/latest/messaging.html)](https://jupyter-client.readthedocs.io/en/latest/_images/frontend-kernel.png)

* This protocol (implemented with [ZeroMQ](https://zeromq.org) is language agnostic

* Kernels in Python, R, Julia, Matlab even C++ (with the
  [cling](https://github.com/root-project/cling) interpreter) and much
  [more](https://github.com/jupyter/jupyter/wiki/Jupyter-kernels)

:::

## Jupyter notebook and jupyter lab

* Notebook: flat, linear

* Lab: richier

# use cases

## As a REPL

I use jupyter notebook as a Read-Eval-Print Loop system

I like:

* Completion, inline doc, inline outputs
* Editable history

Some dislike:

* Heavier than a console
* Confusing exectution order (more on that later)

## As a UI

Notebooks are intended to be *programable documents*
Publish code and documentation within the same object

### Markdown and multi-media


### Widget system

* ipywidgets
*


### Voilà

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


### Proustian notebooks

* Push to write imperative programming
* Cells 100s of lines long
* Intractable notebooks

----

> Solution

1. Refactor early
2. Companion utils.py
3. Gradually evolve in a project architecture


### Versioning

* Notebooks are JSON documents
* Output (e.g. images) are serialized
* Execution number and session uuids change

> Diffs are ugly
> Merges are a mess

----

> Solutions

* pre-commit hook to delete output before push
* nbdime: visually acurate and efficient diff & merge for the notebooks

# Distribution

* Built-in client / server architecture
* "Easy" to setup remote exectution
* Security is not easy


## A word on conda

* Jupyter & scipy rely on the conda package management system
* Mamba is faster and community supported
* environment.yml to define dependencies

* Complicated to distribute code can be cross-compiled with conda-feedstock

## BinderHub (and colab)

* Free service for OSS projects
* Just need enviornment.yml

## JupyterHub

* Manage remote access & authentication
* Manage multiple users
* Kubernetes capable (mutliple containers, load-balancing)
