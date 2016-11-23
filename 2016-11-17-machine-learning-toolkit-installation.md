---
layout: post
title: "Machine Learning Toolkit: Installation"
comments: false
description: "Want to get started with machine learning in python? In this series, We’ll show you the tools you need. We’ll go over what they all do, how to get them, and introduce their most important features. In this post we’ll go over installation, so you’ll be able to interact with the tools while you are learning about them. In the next post we’ll go over what everything actually is and how to use it. Finally, take you through the process of creating a model end to end."
keywords: "Machine Learning, Anaconda, Scikit-learn"
---

*Want to get started with machine learning in python? In this series, We’ll show you the tools you need. We’ll go over what they all do, how to get them, and introduce their most important features. In this post we’ll go over installation, so you’ll be able to interact with the tools while you are learning about them. In the next post we’ll go over what everything actually is and how to use it. Finally, take you through the process of creating a model end to end.*

**tldr; install anaconda;**


### 0. Python
This guide requires that you have python installed. **If you've got it already, skip ahead.** If you don’t, you can download it from [python's website](https://www.python.org/downloads/).

If you don’t know if you have python installed, you can open your terminal and type `python --version`. 

If you have it installed, python will return the version number. If you don’t have it, your terminal will return something like “Command not found”. 

Note: If you're new to python, I recommend using python 3.  It is the future. The main reason to use python 2 is if you need a specific library that only works with python 2. If you’re using python 2 and want to upgrade to python 3, don’t worry. You can manage your python version in anaconda.

### 1.Install Anaconda

Grab the Anaconda installation from [continuum](https://www.continuum.io/downloads)

You need to choose the installer based on your version of python. You can check you version of python in terminal with `python --version`. 

Note: Don’t worry now if you want to use python 3 but only have python 2 installed. You can manage environments using python 3 in anaconda even if you set up anaconda using python 2.



### 2. Verify conda and jupyter are working

To check if you've successfully installed anaconda, in terminal, run:

    conda info

This will give you information on your installation. To check if you have successfully installed jupyter, in terminal, run:

    jupyter --version

If this spits out a version number, you are probably all good!


The two commands you’ll find yourself using most in jupyter are `console` and `notebook`. Console is an improved python REPL, but notebook is where you will find the most magic. It’s a wonderful tool for rapid learning, teaching, exploring tools, and exploring datasets.


### 3. Setting up your conda env


Let’s create a new conda environment named ml:

    conda create -name ml python=3.5

Note: In the line above, you are choosing to use python 3. If you want to use python two, you can change it to:

    conda create -name ml python=2.7

To use your environment:

    source activate ml

You should see your command prompt prepended with the name of your anaconda environment.



### 4. Run jupyter notebook

Running jupyter notebook is easy. Let's make a directory for our first notebook and run it.

    mkdir first_notebook
    cd first_notebook 
    jupyter notebook

`jupyter notebook` will run a notebook server from the current directory. You’ll want to move to a project directory to run jupyter notebook from there. In a later post, we'll dig into using jupyter.

### Conclusion:

Your computer is all ready to build machine learning models, now we just need to get your brain ready. We'll get your brain there in a post that should arrive in the next few days. Sign up if you want to be notified when its ready.

----------


### Follow Up Questions:

*I’ve Got Python 2 installed already. Do I need to do anything special to upgrade to python three?*

No, just create and use anaconda environments with python 3. It's easier, faster, and will help you not break things.


*What’s the difference between pip and conda? Why do I need both?*

Good question. Pip and Anaconda are both package managers based around the python eco system. Pip is the goto package manager for the python community. Anaconda is also a package manager, but there are a few big advantages to using it. 

- Anaconda installs all the packages you need for machine learning in python by default! This means, once you install anaconda, you are basically ready to go. 

- Anaconda allows you to manage and install non python packages. These are packages that you can’t install with pip or easy_install. They don’t have a setup.py so you can just install them with pip. With conda, there are many libraries and dependencies that you can set up that you wouldn't be able to with pip.

- Anaconda is both a package manager and a virtual environment manager. A virtual environment allows you to create isolated environments for python. The barebones virtual environment manager in python is [virtualenv](https://virtualenv.pypa.io/en/stable/). With Anaconda, you don't have to use virtualenv. You get to work with the nicer [conda syntax](http://conda.pydata.org/docs/_downloads/conda-pip-virtualenv-translator.html)

In practice, you'll find that it's important to use both. There are some packages that are easier to install with pip and some that are easier with anaconda. You can install packages using pip onto a virtual environment managed with anaconda. They'll be best friends on your computer. You can learn more from this post from the [anaconda blog](https://www.continuum.io/blog/developer-blog/python-packages-and-environments-conda).

Finally, you can grab the anaconda [cheat sheet](http://conda.pydata.org/docs/_downloads/conda-cheatsheet.pdf) to help you along when you get stuck.