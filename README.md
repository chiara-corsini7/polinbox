# Polymer in a box

In this tutorial you will learn how to run a simulation of an omopolymer in a box.

## Dependencies

* LAMMPS
* [moltemplate](https://www.moltemplate.org/download.html)
*  [genpoly_lt.py](https://github.com/jewettaij/moltemplate/blob/master/moltemplate/genpoly_lt.py) (Included in moltemplate-master)

## Step 0. Generate initial coordinates

Generate initial coordinates and save them in a 3 column file named [coords.raw](coords.raw) containing x, y and z positions of the monomer beads. A trial version is included but you can substitute it with your own. 

## Step 1. Generate the polymer.lt

To set up the system you will use [moltemplate](https://www.moltemplate.org/). Moltemplate allows to generate data.lmp and in.lmp for lammps by providing the `.lt` files. It is very powerful and can be used to create initial configurations for very complex systems.
In this case we will need four `.lt` files to set up our system:

* [`forcefield.lt`](forcefield.lt)
* [`monomer.lt`](monomer.lt)
* `polymer.lt`
* [`system.lt`](system.lt)

Complete the [`forcefield.lt`](forcefield.lt) file with the functional forms and functional parameters wanted to run the simulation, using [LAMMPS documentation](https://docs.lammps.org/commands_list.html). 

You will then generate the `polymer.lt` file using `genpoly_lt.py` to add the connectivity between the monomers. (Before running the command make sure genpoly_lt.py is somewhere accessible by your `$PATH`).

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

We will use the `system.lt` file to position the polymer in the simulation box and create the input files for LAMMPS using 

```moltemplate.sh  system.lt```

you will get three files

* `system.data`
* `system.in.init`
* `system.in setting`

The first one is a complete [LAMMPS data file](https://docs.lammps.org/2001/data_format.html). Check if all the informations needed are actually present and correct (Number of atoms and bonds, masses, box parameters, atoms and bonds list etc.)

The following two files contain sections of a LAMMPS input file. You can combine them and add the remaining informations as explained in [`in.lmp`](in.lmp) file.

## Step 3. Run the simulation

Run the simulation using LAMMPS. Since it is a small non-charged system there is no need to parallelize the simulation. (`lmp` must be substituted with the name of your LAMMPS executable which must be somewhere reached by your `$PATH`)

```lmp -in in.lmp > out.lmp```

## Step 4. Check the output

Look for any possible warnings or errors in `out.lmp` you might have gotten and try to debug them.

Check if the values in `out.lmp` make sense for the simulation you just ran. 

Visually check the dump file using OVITO or any other trajectory visualizer at your disposal.

## Training

Run an equilibration simulation  using a `soft` potential and scaling the pair coefficient using `variable ramp` and `fix adapt pair soft` commands.

Understand how to use moltemplate to generate a heteropolymer composed of 2 beads with different masses.

Now intoduce positive- and negative-charged beads in the system. Modify the input file to simulate a charged system (`pair_style lj/cut/coul/long` and `kspace_stype pppm`). ATTENTION: the overall simulation box has to be charge-neutral!
