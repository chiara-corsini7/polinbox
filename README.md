# Polymer in a box

In this tutorial we will learn how to run a simulation of an omopolymer in a box.

## Step 0. Generate initial coordinates

Generate initial coordianets and set the as a 3 column file named coords.raw containing x, y and z positions of the monomer beads. I trial version is here included by you can substitute it with oyur own if you wish.

## Step 1. Generate the polymer.lt

To set up the system we will use moltemplate (add link). Moltemplate allows to generate data.lmp and in.lmp for lammps by providing the .lt files. It is very powerful and can be used to create initial configurations for very complex systems.
In this case we will need 4 .lt files to set up our system:

* forcefield.lt(add link)
* monomer.lt(add link)
* polymer.lt
* system.lt(add link)

Fill in the forcefield.lt file with the wanted functional form and functional parameters wanted to run the simulation. 

We will then generate the polymer.lt file using genpoly_lt.py to add the connectivity between the monomers.

```
genpoly_lt.py -monomer-name 'Monomer' \
              -polymer-name 'Polymer' \
              -inherits 'Monomer' \
              -header 'import "monomer.lt"' \
              -header 'import "forcefield.lt"' \
             -bond Backbone a a \
             < coords.raw > polymer.lt 
```


## Step 2. Generate data and input files

We will use the system.lt file to position the polymer in the simulation box and create the input files for LAMMPS using 

```moltemplate.sh  system.lt```

you will get three files

* system.data
* system.in.init
* system.in setting

The first one is a complete LAMMPS data file. Check if all the informations needed are actually present and correct (Number of atoms and bonds, masses, box parameters, atoms and bonds list etc.)

The following two files contain sections of an input file of lammps. You can combine them and add the remaining informations as explained in in.lmp file.

