---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.2'
      jupytext_version: 1.3.0
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

<!-- #raw raw_mimetype="text/restructuredtext" -->
.. meta::
   :description: Topic: Writing tests for your code, Difficulty: Easy, Category: Section
   :keywords: test, automated, pytest, parametrize, fixture, suite  
<!-- #endraw -->

# Introducing the Pytest Framework

Thus far, our process for running tests has been a entirely manual one. It is time for us to arranging our test functions into a proper "test suite" and to learn to leverage [the pytest framework](https://docs.pytest.org/en/latest/) to run them.
We will begin by reorganizing our source code to create an installable [Python package](https://www.pythonlikeyoumeanit.com/Module5_OddsAndEnds/Modules_and_Packages.html#Packages).
We will then learn how to structure and run a test suite for this Python package, using pytest.

The pytest framework does much more than just run tests;
for instance, it will enrich the assertions in our tests to produce verbose, informative error messages.
Furthermore it provides valuable means for enhancing our tests via mechanisms like fixtures and parameterizing decorators.
Ultimately, all of this functionality helps to eliminate manual aspects from the testing process.

You may want to [create a separate conda environment](https://www.pythonlikeyoumeanit.com/Module1_GettingStartedWithPython/Installing_Python.html#A-Brief-Introduction-to-Conda-Environments), so that you can work through this material starting from a blank slate.
If you do, be sure to activate that environment and install NumPy and Jupyter notebook: `conda install numpy notebook`  

Let's install pytest. In your terminal where you can access your conda environment, run:

```shell
conda install -c conda-forge pytest
```

Installing from [the conda-forge channel](https://conda-forge.org/) will install the most up-to-date version of pytest.
Or you can install pytest via pip:

```shell
pip install pytest
```

<!-- #region -->
## Organizing a Test Suite

It's time to create a proper test suite.
Before proceeding any further, we should reread the material presented in [Module 5 - Import: Modules and Packages](https://www.pythonlikeyoumeanit.com/Module5_OddsAndEnds/Modules_and_Packages.html) and recall the essentials of import statements, modules, and Python packages.
This material serves as the foundation for this section.

### Organizing our Source Code
Let's create the a Python package, which we will call `plymi_mod6`, with the following directory structure:

```
project_dir/     # the "parent directory" houses our source code, tests, and all other relevant files
  - setup.py     # script responsible for installing `plymi_mod6` package
  - plymi_mod6/  # directory containing source code of `plymi_mod6` package
      |-- __init__.py
      |-- basic_functions.py
      |-- numpy_functions.py
  - tests/        # test-suite for `plymi_mod6` package (to be run using pytest)
      |-- conf.py # optional configuration file for pytest
      |-- test_basic_functions.py
      |-- test_numpy_functions.py
```

A reference implementation of this package can be found [in this GitHub repository](https://github.com/rsokl/plymi_mod6).
Populate the _basic_functions.py_ file with the two functions that we were using as our source code in the previous section: `count_vowels` and `merge_max_mappings`.
In in the _numpy_functions.py_ module, add the `pairwise_dists` function that appears in [Module 3's discussion of optimized pairwise distances](https://www.pythonlikeyoumeanit.com/Module3_IntroducingNumpy/Broadcasting.html#Optimized-Pairwise-Distances).
Don't forget to include `import numpy as np` in your script in accordance with how `pairwise_dists` calls NumPy functions. 

We have arranged these functions so that they can be imported from the `basic_functions` module and the `numpy_functions` module, respectively, which reside in our `plymi_mod6` package.
Let's fill out our _setup.py_ script and install this package so that we can import it regardless of our current working directory. The content of _setup.py_ will be:

```python
from setuptools import find_packages, setup

setup(
    name="plymi_mod6",
    packages=find_packages(exclude=["tests", "tests.*"]),
    version="1.0.0",
    author="Your Name",
    author_email="your.email@email.com",
    description="A template Python package for learning about testing",
    install_requires=["numpy >= 1.10.0"],
    tests_require=["pytest", "hypothesis"],
    python_requires=">=3.6",
)
```

This setup file dictates that a user must have Python 3.6+ installed - we will bar Python 3.5 and below so that we are free to make use of [f-strings](https://www.pythonlikeyoumeanit.com/Module2_EssentialsOfPython/Basic_Objects.html#Formatting-strings) in our code, which were introduced in Python 3.6.

Finally, let's install our package locally [in development mode](https://www.pythonlikeyoumeanit.com/Module5_OddsAndEnds/Modules_and_Packages.html#Installing-Your-Own-Python-Package).
Navigate to the directory containing _setup.py_ and run:

```shell
python setup.py develop
```

Now, we should be able to start a python console or Jupyter notebook in and directory and import our package:

```shell
(plymi_test_env) C:\Users\plymi_person>ipython
Python 3.7.5 (default, Oct 32 2200, 15:18:51) [MSC v.1916 64 bit (AMD64)]
IPython 7.10.2 -- An enhanced Interactive Python. Type '?' for help.

In [1]: from plymi_mod6.basic_functions import count_vowels

In [2]: count_vowels("Happy birthday", include_y=True)
Out[2]: 5
```
<!-- #endregion -->

## Links to Official Documentation

- [pytest](https://docs.pytest.org/en/latest/)


## Reading Comprehension Solutions