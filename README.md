Solv@TUM is a solvation free energy database which contains partition coefficients
of 658 neutrale organic molekules and inorganic gases dissolved in organic solvents.
The values were collected form different work by Prof. Dr. Acree Jr.
All publications of these values are listed in the [bib file](solvatum/data/solvatum_references.bib).
The actual [database file](solvatum/data/solvatum.sdf) is a structured data file (sdf).

For a simple and fast access to the data one can use the
[user interface](solvatum/ui.py), which is written in the Python programming language.

The installation and usage of this user interface is described in the following sections.

# Dependencies

The Solv@TUM interface currently only supports Python 2.7.

Installation requires [OpenBabel](http://openbabel.org/wiki/Main_Page) and its Python Interface [Pybel](https://openbabel.org/docs/dev/UseTheLibrary/Python_Pybel.html).

Some functions will optionally use [pandas](https://pandas.pydata.org/), [bibtexparser](https://bibtexparser.readthedocs.io/) and [ase](https://wiki.fysik.dtu.dk/ase/).

# Installation

## Installation of Pybel/OpenBabel

To handle the sdf-format we are using the OpenBabel chemistry library and its Python interface 'pybel'.
This has to be installed before you can install the Solv@TUM database interface.
Installation guidelines can be found here: 

* [Pybel](https://openbabel.org/docs/dev/UseTheLibrary/Python_Pybel.html)
* [OpenBabel](http://openbabel.org/wiki/Main_Page) 

We recommend to install Pybel/OpenBabel with the conda package manager,
because then no manual installation of OpenBabel is necessary.
For this just type 
    
    $ conda install -c openbabel openbabel

in your command line.

## Installation of the Solv@TUM user interface:

The latest stable release can be downloade from:

[https://mediatum.ub.tum.de/1452573](https://mediatum.ub.tum.de/1452573)

and installed with
    
    $ pip install solvatum-1.0.0.tar.gz

# Usage of the database interface

## Filtering & Properties

Open a Python prompt and type in:

```python
>>> from solvatum.ui import Database
>>> d = Database()
```

With this command the database gets initialized.
Now all solvents or solutes can be listed with:

```python
>>> print d.solvents
>>> print d.solutes
```

One can filter for one solvent, solute or both:

```python
>>> d.filtering(solvent='chloroform')
>>> d.filtering(solute='hexane')
>>> d.filtering(solvent='chloroform', solute='hexane')
{'deltaG_solv': -0.16979, 'logK': 2.87}

```
The output of the first lines is a dictionary with molecule specific propperties
and a list with all corresponding solutes and solvents, respectively.
The output of the latter, is a dictionary with the logK value 
and the solvation free energy calculated from it.
The unit of the solvation free energy is per default eV,
but one can switch to kcal/mol or J with

```python
>>> d.energy_unit='kcal/mol'
{'deltaG_solv': -3.92, 'logK': 2.87}
```

With
```python
>>> d.get_molecule_properties(<molecule>)
```
all properties (data) which are stored in the database, except the logK values,  are shwon
(e.g. formula, InChI, name, mean polarizability, dipole moment, etc.).
Adding new properties is simple done with

```python
>>> d.add_property(<molecule>, <property_name>, <property_value>)
```    
## Working with the molecule geometries

```python
>>> d.one_mol_from_sdf(<solute>)
```
returns the pybel.Molecule object of a given solute molecule. To that you can apply any Pybel or OpenBabel method.

If you are using the [Atomic Simulation Environment (ASE)](https://wiki.fysik.dtu.dk/ase/) the database interface can directly convert the pybel.Molecule object to an ASE Atomas object with:
```python
>>> d.mol_to_ase(<molecule>)
```

The database interface also provides the possibility to draw depictions of the molecules:

```python
>>> d.draw(<molecule>)
```

This method take the keywords 'save=True', for saving a png figure of the depiction and
'notebook=True' for showing the depiction inline in jupyter notebooks.

# Known Issues
There is a known issue in Pybel in the drawing functionality, which was already reported and is also fixed in the officiall Pybel source code.
But by installing Pybel with conda or PIP and not directly from source it is possible that the following error still appears:

```
ImportError: Tkinter or Python Imaging Library not found, but is required for image display. See installation instructions for more information.
```

For this we provided a fixed version of [pybel.py](fixed_pybel). Just locate your installation path of Pybel and replace the files.

