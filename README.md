# Biophysics

Here is the source code for an exercise about the **evaluation energy Spike RBD-ACE2 protein-protein interface analysis** using Jupyter-notebook. The objective of this project was to evaluate the contribution of each of the interface residues to the interaction energy in a specific protein-protein complex. 

## Index

## Preparation

From the PDB load the structure **6m0J**, which corresponds to the **Crystal structure of SARS-CoV-2 spike receptor-binding domain bound with ACE2**. Since we want only to know how interface residues contribute to the complex, we will perform a quality checking on the structure: add missing side-chains and
hydrogen atoms and atom charges. This 'pre-step' will be performed with the file named **'check_structure.ipynb'**. 
The following are the results before running the script and after. If we observe closely we'll se that at first the number of heteroatoms, ligands or water molecules were several, and after cleaning the structure, to obtain the biological unit, we removed them.

![1](https://user-images.githubusercontent.com/93529369/203752913-4be44493-0acd-4827-84c3-687933191961.png) ![2](https://user-images.githubusercontent.com/93529369/203753259-abc318f1-33e2-4e26-a2f3-571686be08bc.png)


## Steps

### First Step

To begin with we have to define a suitable list of interface residues, but before running the script we wanted to visualize and differentiate the two structures as well as select the atoms that could be a part of the interface and find polar bonds. To do so we used Pymol following the next steps:

1.- Show both chains Angiotensin Converting Enzyme (ACE2) and SPIKE protein S1 as lines.
![3](https://user-images.githubusercontent.com/93529369/203757404-227e1479-45a0-40b4-9e95-a66bed6134b7.png)

2.- We looked for the polar contacts between any atoms and then using the tool wizard we measured the distance between those that visually seem to be interacting on the surface.

As seen in the different distances, the longest one was **3.5 Å** (between 505(TYR) and 37(GLU)), extent which we'll use to create a variable in the script.


## Second Step

This second step consisted in evaluating the Interaction energy among chains (between components of A-E complex). This interaction energy is defined by the difference between the total energy of each chain in the bound state and the unbound state. But to simplify, we assumed the structure does not change from the complex to the isolation of both chains in solution. Additionaly, we considered solvation energies for all atom types.
Thus, if we have the interface residues defined correctly, obtained values will be very similar. 

To do that we used Biopython as a package tool iterating over the structure to get both chains and its corresponding residues ID's. At that point it was just a matter of calling the different functions with the MAXDIST variable set to 3.5 Å which was previously selected in the first step.



we iterated residue by residue and computed the necessary energies for each one. We added everything using the formula to obtain the total energy.
We repeated this process twice, one for all the residues and another one for the interface residues.


