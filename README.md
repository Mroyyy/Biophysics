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

Considering these points, interaction energy between components of a A-B complex (in this case A-E) will come from the following formula:
![formula](https://user-images.githubusercontent.com/93529369/203762378-233e1e8f-fdf9-4a99-ba2f-e75f913fda6a.png)

Basically, what is stating is that the binding energy comes from the difference of electrostatic, Van der Waals and solvation energy between A and B, minus the solvation energy individually of each chain.
Thus, if we have the interface residues defined correctly, obtained values will be very similar. 

To do that we used Biopython as a package tool iterating over the structure to get both chains and its corresponding residues ID's. At that point it was just a matter of calling the different functions with the MAXDIST (cut-off distance) variable set to 3.5 Å which was previously selected in the first step. (All script commented at file named 'energy_evaluation.ipynb').
This process is repeated twice, first for those residues on the interface and the next cell for all residues of complex. What changes in this second cell of the script is that the MAXDIST variable is set to zero, these will provide information of the energy of all residues on the protein, not just the interface.

Therefore, we achieved these results:
![res](https://user-images.githubusercontent.com/93529369/203763545-f00af95a-afc0-4012-82da-69193e287e51.png)

We obtained a final dG of -172 selecting all residues and -82 for those residues participating on the contacts.

## Third step

In this step we had to determine the effect of replacing each interface residue with Ala throughout ΔGA-B and plot the obtained results, highlighting those residues that are more relevant to the interface stability.
To calculate this we just added some lines of code:

```ruby
with open("res_ala.txt", "a") as res_ala:
    for ch in st[0]:
        for res in ch.get_residues():
            if MAXDIST > 0 and res not in interface[ch.id]:
                continue
            print('{:1}, {:1.4}'.format(residue_id(res),
                    - elec[res] + elec_ala[res] - vdw[res] + vdw_ala[res] -solvAB[res] +\
                        solvAB_ala[res] -solvA[res] + solvA_ala[res]), file = res_ala)
```

Since the side-chain of Alanine is part of all the other residues, we just had to take into account the atoms that form this residue. Thus, before running this piece of code we established:

```ruby
#Possible Atom names that correspond to Ala atoms"
ala_atoms = {'N', 'H', 'CA', 'HA', 'C', 'O', 'CB', 'HB', 'HB1', 'HB2', 'HB3', 'HA1', 'HA2', 'HA3'}
```

As we can see from this image (with a few examples of residues) the side-chain of Alanine is what constitutes the others basic structures. With an excepcion of the Glycine, which is the simplest residues we'll find, with Glycine there is no nedd to replace anything because it won't make a huge change.

![amino](https://user-images.githubusercontent.com/93529369/203766681-b0d994cf-30cb-408e-8abb-e97b2dbbed61.png)
