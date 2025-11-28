About symbolite-feedstock
=========================

Feedstock license: [BSD-3-Clause](https://github.com/conda-forge/symbolite-feedstock/blob/main/LICENSE.txt)



Package license: 

Summary: A minimalistic symbolic package.

![Package](https://img.shields.io/pypi/v/symbolite?label=symbolite)
![CodeStyle](https://img.shields.io/badge/code%20style-black-000000.svg)
![License](https://img.shields.io/pypi/l/symbolite?label=license)
![PyVersion](https://img.shields.io/pypi/pyversions/symbolite?label=python)
[![CI](https://github.com/dyscolab/symbolite/actions/workflows/ci.yml/badge.svg)](https://github.com/dyscolab/symbolite/actions/workflows/ci.yml)
[![Lint](https://github.com/dyscolab/symbolite/actions/workflows/lint.yml/badge.svg)](https://github.com/dyscolab/symbolite/actions/workflows/lint.yml)

# symbolite: a minimalistic symbolic python package

______________________________________________________________________

Symbolite allows you to create symbolic mathematical
expressions. Just create a symbol (or more) and operate with them as you
will normally do in Python.

```python
>>> from symbolite import Symbol, substitute, translate
>>> x = Symbol("x")
>>> y = Symbol("y")
>>> expr1 = x + 3 * y
>>> print(expr1)
x + 3 * y
```

An expression is just an unnamed Symbol.
You can easily replace the symbols by the desired value.

```python
>>> expr2 = substitute(expr1, {x: 5, y: 2})
>>> print(expr2)
5 + 3 * 2
```

The output is still a symbolic expression, which you can translate:

```python
>>> translate(expr2)
11
```

Notice that we also got a warning (`No libsl provided, defaulting to Python standard library.`).
This is because translating an expression requires an actual library implementation,
named usually as `libsl`. The default one just uses python's math module.

You can avoid this warning by explicitely providing an `libsl` implementation.

```python
>>> from symbolite.impl import libstd
>>> translate(expr2, libstd)
11
```

You can also import it with the right name and it will be found

```python
>>> from symbolite.impl import libstd as libsl
>>> translate(expr2)
11
```

In addition to the `Symbol` class, there is also a `Scalar` and `Vector` classes
to represent integer, floats or complex numbers, and an array of those.

```python
>>> from symbolite import real, Vector
>>> x = Scalar("x")
>>> y = Scalar("y")
>>> v = Vector("v")
>>> expr1 = x + 3 * y
>>> print(expr1)
x + 3 * y
>>> print(2 * v)
2 * v
```

Mathematical functions that operate on scalars are available in the `scalar` module.

```python
>>> from symbolite import real
>>> expr3 = 3. * scalar.cos(0.5)
>>> print(expr3)
3.0 * scalar.cos(0.5)
```

Mathematical functions that operate on vectors are available in the `vector` module.

```python
>>> from symbolite import vector
>>> expr4 = 3. * vector.sum((1, 2, 3))
>>> print(expr4)
3.0 * vector.sum((1, 2, 3))
```

Notice that functions are named according to the python math module.
Again, this is a symbolic expression until translated.

```python
>>> translate(expr3)
2.6327476856711
>>> translate(expr4)
18.0
```

Three other implementations are provided:
[NumPy](https://numpy.org/),
[SymPy](https://www.sympy.org),
[JAX](https://jax.readthedocs.io).

```python
>>> from symbolite.impl import libnumpy
>>> translate(expr3, libsl=libnumpy)
np.float64(2.6327476856711183)
>>> from symbolite.impl import libsympy
>>> translate(expr3, libsl=libsympy)
2.6327476856711
```

(notice that the way that the different libraries round and
display may vary)

In general, all symbols must be replaced by values in order
to translate an expression. However, when using an implementation
like SymPy that contains a Scalar object you can still translate.

```python
>>> from symbolite.impl import libsympy as libsl
>>> translate(3. * scalar.cos(x), libsl)
3.0*cos(x)
```

which is actually a SymPy expression with a SymPy symbol (`x`).

And by the way, checkout `vectorize` and `auto_vectorize` functions
in the vector module.

We provide a simple way to call user defined functions.

```python
>>> from symbolite import UserFunction
>>> def abs_times_two(x: float) -> float:
...     return 2 * abs(x)
>>> uf = UserFunction.from_function(abs_times_two)
>>> uf
UserFunction(name='abs_times_two', namespace='user')
>>> translate(uf(-1))
2
```

and you can register implementations for other backends:

```python
>>> def np_abs_times_two(x: float) -> float:
...     return 2 * np.abs(x)
>>> uf.register_impl(libnumpy, np_abs_times_two)
>>> translate(uf(-1), libnumpy)
2
```

### Installing:

```bash
pip install -U symbolite
```

### FAQ

**Q: Is symbolite a replacement for SymPy?**

**A:** No

**Q: Does it aim to be a replacement for SymPy in the future?**

**A:** No

Current build status
====================


<table><tr><td>All platforms:</td>
    <td>
      <a href="https://dev.azure.com/conda-forge/feedstock-builds/_build/latest?definitionId=21062&branchName=main">
        <img src="https://dev.azure.com/conda-forge/feedstock-builds/_apis/build/status/symbolite-feedstock?branchName=main">
      </a>
    </td>
  </tr>
</table>

Current release info
====================

| Name | Downloads | Version | Platforms |
| --- | --- | --- | --- |
| [![Conda Recipe](https://img.shields.io/badge/recipe-symbolite-green.svg)](https://anaconda.org/conda-forge/symbolite) | [![Conda Downloads](https://img.shields.io/conda/dn/conda-forge/symbolite.svg)](https://anaconda.org/conda-forge/symbolite) | [![Conda Version](https://img.shields.io/conda/vn/conda-forge/symbolite.svg)](https://anaconda.org/conda-forge/symbolite) | [![Conda Platforms](https://img.shields.io/conda/pn/conda-forge/symbolite.svg)](https://anaconda.org/conda-forge/symbolite) |

Installing symbolite
====================

Installing `symbolite` from the `conda-forge` channel can be achieved by adding `conda-forge` to your channels with:

```
conda config --add channels conda-forge
conda config --set channel_priority strict
```

Once the `conda-forge` channel has been enabled, `symbolite` can be installed with `conda`:

```
conda install symbolite
```

or with `mamba`:

```
mamba install symbolite
```

It is possible to list all of the versions of `symbolite` available on your platform with `conda`:

```
conda search symbolite --channel conda-forge
```

or with `mamba`:

```
mamba search symbolite --channel conda-forge
```

Alternatively, `mamba repoquery` may provide more information:

```
# Search all versions available on your platform:
mamba repoquery search symbolite --channel conda-forge

# List packages depending on `symbolite`:
mamba repoquery whoneeds symbolite --channel conda-forge

# List dependencies of `symbolite`:
mamba repoquery depends symbolite --channel conda-forge
```


About conda-forge
=================

[![Powered by
NumFOCUS](https://img.shields.io/badge/powered%20by-NumFOCUS-orange.svg?style=flat&colorA=E1523D&colorB=007D8A)](https://numfocus.org)

conda-forge is a community-led conda channel of installable packages.
In order to provide high-quality builds, the process has been automated into the
conda-forge GitHub organization. The conda-forge organization contains one repository
for each of the installable packages. Such a repository is known as a *feedstock*.

A feedstock is made up of a conda recipe (the instructions on what and how to build
the package) and the necessary configurations for automatic building using freely
available continuous integration services. Thanks to the awesome service provided by
[Azure](https://azure.microsoft.com/en-us/services/devops/), [GitHub](https://github.com/),
[CircleCI](https://circleci.com/), [AppVeyor](https://www.appveyor.com/),
[Drone](https://cloud.drone.io/welcome), and [TravisCI](https://travis-ci.com/)
it is possible to build and upload installable packages to the
[conda-forge](https://anaconda.org/conda-forge) [anaconda.org](https://anaconda.org/)
channel for Linux, Windows and OSX respectively.

To manage the continuous integration and simplify feedstock maintenance,
[conda-smithy](https://github.com/conda-forge/conda-smithy) has been developed.
Using the ``conda-forge.yml`` within this repository, it is possible to re-render all of
this feedstock's supporting files (e.g. the CI configuration files) with ``conda smithy rerender``.

For more information, please check the [conda-forge documentation](https://conda-forge.org/docs/).

Terminology
===========

**feedstock** - the conda recipe (raw material), supporting scripts and CI configuration.

**conda-smithy** - the tool which helps orchestrate the feedstock.
                   Its primary use is in the construction of the CI ``.yml`` files
                   and simplify the management of *many* feedstocks.

**conda-forge** - the place where the feedstock and smithy live and work to
                  produce the finished article (built conda distributions)


Updating symbolite-feedstock
============================

If you would like to improve the symbolite recipe or build a new
package version, please fork this repository and submit a PR. Upon submission,
your changes will be run on the appropriate platforms to give the reviewer an
opportunity to confirm that the changes result in a successful build. Once
merged, the recipe will be re-built and uploaded automatically to the
`conda-forge` channel, whereupon the built conda packages will be available for
everybody to install and use from the `conda-forge` channel.
Note that all branches in the conda-forge/symbolite-feedstock are
immediately built and any created packages are uploaded, so PRs should be based
on branches in forks, and branches in the main repository should only be used to
build distinct package versions.

In order to produce a uniquely identifiable distribution:
 * If the version of a package **is not** being increased, please add or increase
   the [``build/number``](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html#build-number-and-string).
 * If the version of a package **is** being increased, please remember to return
   the [``build/number``](https://docs.conda.io/projects/conda-build/en/latest/resources/define-metadata.html#build-number-and-string)
   back to 0.

Feedstock Maintainers
=====================


