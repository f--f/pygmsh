# PyGmsh

[![Build Status](https://travis-ci.org/nschloe/pygmsh.svg)](https://travis-ci.org/nschloe/pygmsh)
[![Code Health](https://landscape.io/github/nschloe/pygmsh/master/landscape.png)](https://landscape.io/github/nschloe/pygmsh/master)
[![codecov](https://codecov.io/gh/nschloe/pygmsh/branch/master/graph/badge.svg)](https://codecov.io/gh/nschloe/pygmsh)
[![Documentation Status](https://readthedocs.org/projects/pygmsh/badge/?version=latest)](http://pygmsh.readthedocs.org/en/latest/?badge=latest)
[![PyPi Version](https://img.shields.io/pypi/v/pygmsh.svg)](https://pypi.python.org/pypi/pygmsh)
[![GitHub stars](https://img.shields.io/github/stars/nschloe/pygmsh.svg?style=social&label=Star&maxAge=2592000)](https://github.com/nschloe/pygmsh)

[Gmsh](http://geuz.org/gmsh/) is a powerful mesh generation tool with a
scripting language that is notoriously hard to write.

The goal of PyGmsh is to combine the power of Gmsh with the versatility of
Python and to provide useful abstractions from the Gmsh scripting language
so you can create complex geometries more easily.

![](https://nschloe.github.io/pygmsh/screw.png)

To create the above mesh, simply do
```python
import pygmsh as pg
import numpy as np

geom = pg.Geometry()

# Draw a cross.
poly = geom.add_polygon([
    [0.0,   0.5, 0.0],
    [-0.1,  0.1, 0.0],
    [-0.5,  0.0, 0.0],
    [-0.1, -0.1, 0.0],
    [0.0,  -0.5, 0.0],
    [0.1,  -0.1, 0.0],
    [0.5,   0.0, 0.0],
    [0.1,   0.1, 0.0]
    ],
    lcar=0.05
    )

axis = [0, 0, 1]

geom.extrude(
    'Surface{%s}' % poly,
    translation_axis=axis,
    rotation_axis=axis,
    point_on_axis=[0, 0, 0],
    angle=2.0 / 6.0 * np.pi
    )

points, cells = pg.generate_mesh(geom)
```
to retrieve all points and cells of the mesh for the specified geometry.
To store the mesh, you can use [meshio](https://pypi.python.org/pypi/meshio);
for example
```python
import meshio
meshio.write('test.vtu', points, cells)
```
The output file can be visualized with various tools, e.g.,
[ParaView](http://www.paraview.org/).

You will find the above mesh in the directory `test/examples/` along with other
small examples.

### Installation

#### Python Package Index

PyGmsh is [available from the Python Package
Index](https://pypi.python.org/pypi/pygmsh/), so simply type
```
pip install pygmsh
```
to install or
```
pip install pygmsh -U
```
to upgrade.

#### Manual installation

Download PyGmsh from [PyPi](https://pypi.python.org/pypi/pygmsh/)
or [GitHub](https://github.com/nschloe/pygmsh) and
install it with
```
python setup.py install
```

### Requirements

PyGmsh depends on

 * [NumPy](http://www.numpy.org/)

and, obviously, [Gmsh](http://geuz.org/gmsh/).

### Usage

Just
```
import pygmsh as pg
```
and make use of all the goodies the module provides. The
[documentation](http://pygmsh.readthedocs.org/) and the examples under
`test/examples/` might inspire you.


### Testing

To run the PyGmsh unit tests, check out this repository and type
```
nosetests
```
or
```
nose2 -s test
```

### Distribution

To create a new release

1. bump the `__version__` number,

2. create a Git tag,
    ```
    git tag v0.3.1
    git push --tags
    ```
    and

3. upload to PyPi:
    ```
    make upload
    ```


### License

PyGmsh is published under the [MIT license](https://en.wikipedia.org/wiki/MIT_License).
