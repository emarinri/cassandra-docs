Utilities
=========

.. _sec:mcfgen:

Generate a Molecular Connectivity File
--------------------------------------

The script ``mcfgen.py`` is a tool that aims to ease the setup of
molecular connectivity files from scratch (see section
`[sec:MCF_File] <#sec:MCF_File>`__ to learn more about MCFs), as the
generation of these files by hand can be error prone. In this section, a
pentane MCF will be generated to demonstrate the use of this tool. The
Transferable Potentials for Phase Equilibria (TraPPE) force field will
be used to represent the pentane molecular interactions. This force
field involves a pairwise-additive 12-6 Lennard-Jones potential to
represent the dispersion-repulsion interactions. Additionally, bond
angles and dihedral angles are represented through harmonic and OPLS
functional forms, respectively. Bond lengths are kept constant. The
force field mathematical expression becomes

.. math::

   \begin{aligned}
   U = \sum_{angles} K_\theta(\theta-\theta_0)^2 +
   \sum_{dihedrals} \frac{1}{2}K_1[1+cos(\phi)]+\frac{1}{2}K_2[1-cos(2\phi)] + \frac{1}{2}K_3[1+cos(3\phi)]+\frac{1}{2}K_4[1-cos(4\phi)] + \\
   \sum_{i} \sum_{i>j} 4 \epsilon_{ij} \left [  \left ( \frac {\sigma_{ij}} { r_{ij} }\right )^{12} - \left ( \frac {\sigma_{ij}} { r_{ij} }\right )^{6}\ \right ]\end{aligned}

First, generate (or obtain) a PDB file or a CML file. To generate a
PDB or CML file, software such as Gaussview or Avogadro can be used.
Alternatively, PDB files can be downloaded from the internet (e.g.
www.rcsb.org). In this example, a pentane PDB file using the program
Gaussview v5.08 was be generated, as shown below.

.. image:: resources/pdbfile_final.eps
    :align: center
    :height: 1in

Append a column containing the atom types.

.. image:: resources/pdbfile_edited_final.eps
    :height: 1in

Avogadro v1.1.1 can also be used to generate CML files. Below is an
example of a CML file generated using Avogadro.

.. image:: resources/pentane_cml.eps
    :height: 1.5in

Modify the pentane united atom CML file. Note that the atom type is
appended as a last column between quotation marks.

.. image:: resources/pentane_cml_modified.eps
    :height: 1.5in

In the terminal, run the following command:

.. code-block:: bash

    python mcfgen.py pentane.pdb –ffTemplate

This command will create an .ff file. The first three sections of the FF file
are displayed next. Do not modify these.

.. image:: resources/top_ff.eps
    :height: 2in

The force field parameters for non-bonded (not shown), bonded, angle, dihedral
(not shown) and coulombic interactions (not shown) must be entered next to the
corresponding keyword. For example, the angle type CH3 CH2 CH2 has an angle of
114.0. This value must be placed next to the “Angle” keyword.

.. image:: resources/body_ff.eps
    :height: 2in

For more examples of filled ff files, please refer to the examples
contained in the ``/Scripts/MCF_Generation/`` directory. Using the filled
.ff file, run:

.. code-block:: bash

    python mcfgen.py pentane.pdb

Check the file newly created pentane.mcf for any possible errors. This example
can be found in the directory ``/Scripts/MCF_Generation/PDB/``

Note that if an MCF for a rigid solid is being created, this last step
must include the ``--solid`` flag, as

.. code-block:: bash

    python mcfgen.py zeolite.pdb --solid


.. _sec:libgen:

Generate Library of Fragment Configurations
-------------------------------------------

The goal of the script ``library_setup.py`` is to automate the generation of
fragment libraries. As a starting point, the script requires the simulation
input file, and the MCF and PDB files for each of the species. To run this
script, type

.. code-block:: bash

    python library_setup.py $PATH$/cassandra.exe input_file.inp pdbfilespecies1.pdb pdfilespecies2.pdb ...

This script will create the necessary files to create the fragment libraries. It
will also run Cassandra to generate these libraries, whose location will be at
``/species?/frag?/frag?.inp``, where ’?’ refers to the species number, for
example, species 1, species 2 etc. Note that the script overwrites the section
of the input file where needed (i.e. ``# Fragment_Files``) with the aforementioned
directory locations.

