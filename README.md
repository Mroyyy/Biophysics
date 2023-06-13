# Biophysics

```ruby
[biohpc-28@aolin-login OpenACC]$ nsys stats report3.qdrep 
Using report3.sqlite for SQL queries.
Running [/soft/nvidia-hpc-sdk-2021-21.2/Linux_x86_64/21.2/profilers/Nsight_Systems/target-linux-x64/reports/cudaapisum.py report3.sqlite]... 

 Time(%)  Total Time (ns)  Num Calls    Average      Minimum     Maximum            Name        
 -------  ---------------  ---------  ------------  ----------  ----------  --------------------
    93,0       12.373.190          1  12.373.190,0  12.373.190  12.373.190  cuMemHostAlloc      
     3,0          464.805          1     464.805,0     464.805     464.805  cuMemAllocHost_v2   
     1,0          143.647          3      47.882,0       4.597     127.226  cuMemAlloc_v2       
     0,0           72.885          1      72.885,0      72.885      72.885  cuModuleLoadDataEx  
     0,0           54.308          3      18.102,0       7.770      37.462  cuLaunchKernel      
     0,0           51.429          5      10.285,0       3.761      31.375  cuMemcpyHtoDAsync_v2
     0,0           25.892          9       2.876,0       1.369       7.266  cuStreamSynchronize 
     0,0           22.904          3       7.634,0       5.039      11.588  cuMemcpyDtoHAsync_v2
     0,0           14.429          1      14.429,0      14.429      14.429  cuStreamCreate      
     0,0            7.129          3       2.376,0       1.387       4.152  cuEventRecord       
     0,0            3.604          2       1.802,0         362       3.242  cuEventCreate       
     0,0            2.974          3         991,0         553       1.558  cuEventSynchronize  

Running [/soft/nvidia-hpc-sdk-2021-21.2/Linux_x86_64/21.2/profilers/Nsight_Systems/target-linux-x64/reports/gpukernsum.py report3.sqlite]... 

 Time(%)  Total Time (ns)  Instances  Average  Minimum  Maximum      Name     
 -------  ---------------  ---------  -------  -------  -------  -------------
    33,0            2.176          1  2.176,0    2.176    2.176  loop_2_38_gpu
    33,0            2.144          1  2.144,0    2.144    2.144  loop_1_21_gpu
    32,0            2.113          1  2.113,0    2.113    2.113  loop_3_55_gpu

Running [/soft/nvidia-hpc-sdk-2021-21.2/Linux_x86_64/21.2/profilers/Nsight_Systems/target-linux-x64/reports/gpumemtimesum.py report3.sqlite]... 

 Time(%)  Total Time (ns)  Operations  Average  Minimum  Maximum      Operation     
 -------  ---------------  ----------  -------  -------  -------  ------------------
    58,0            4.576           5    915,0      832    1.120  [CUDA memcpy HtoD]
    41,0            3.295           3  1.098,0      959    1.376  [CUDA memcpy DtoH]

Running [/soft/nvidia-hpc-sdk-2021-21.2/Linux_x86_64/21.2/profilers/Nsight_Systems/target-linux-x64/reports/gpumemsizesum.py report3.sqlite]... 

Total   Operations  Average  Minimum  Maximum      Operation     
 ------  ----------  -------  -------  -------  ------------------
 19,000           5    3,000    3,000    3,000  [CUDA memcpy HtoD]
 11,000           3    3,000    3,000    3,000  [CUDA memcpy DtoH]

Running [/soft/nvidia-hpc-sdk-2021-21.2/Linux_x86_64/21.2/profilers/Nsight_Systems/target-linux-x64/reports/osrtsum.py report3.sqlite]... SKIPPED: report3.sqlite does not contain OS Runtime trace data

Running [/soft/nvidia-hpc-sdk-2021-21.2/Linux_x86_64/21.2/profilers/Nsight_Systems/target-linux-x64/reports/nvtxppsum.py report3.sqlite]... SKIPPED: report3.sqlite does not contain NV Tools Extension (NVTX) data

Running [/soft/nvidia-hpc-sdk-2021-21.2/Linux_x86_64/21.2/profilers/Nsight_Systems/target-linux-x64/reports/openmpevtsum.py report3.sqlite]... SKIPPED: report3.sqlite does not contain OpenMP event data.


```






```ruby



```

```ruby

```






Here is the source code for an exercise about the **evaluation energy Spike RBD-ACE2 protein-protein interface analysis** using Jupyter-notebook. The objective of this project was to evaluate the contribution of each of the interface residues to the interaction energy in a specific protein-protein complex. 

## Index

* Preparation
* Steps
    - First Step
    - Second Step
    - Third Step
    - Fourth Step
 * Results and discussion

## Preparation

From the PDB load the structure **6m0J**, which corresponds to the **Crystal structure of SARS-CoV-2 spike receptor-binding domain bound with ACE2**. Since we want only to know how interface residues contribute to the complex, we will perform a quality checking on the structure: add missing side-chains and
hydrogen atoms and atom charges. This 'pre-step' will be performed with the file named **'check_structure.ipynb'**. 
The following are the results before running the script and after. If we observe closely we'll se that at first the number of heteroatoms, ligands or water molecules were several, and after cleaning the structure, to obtain the biological unit, we removed them.

![1](https://user-images.githubusercontent.com/93529369/203752913-4be44493-0acd-4827-84c3-687933191961.png) ![2](https://user-images.githubusercontent.com/93529369/203753259-abc318f1-33e2-4e26-a2f3-571686be08bc.png)


## Steps

### First Step

To begin with we have to define a suitable list of interface residues, but before running the script we wanted to visualize and differentiate the two structures as well as select the atoms that could be a part of the interface and find polar bonds. To do so we used Pymol following the next steps:

1.- Show both chains Angiotensin Converting Enzyme (ACE2) and SPIKE protein S1 as lines.

2.- We looked for the polar contacts between any atoms and then using the tool **wizard** we measured the distance between those that visually seem to be interacting on the surface.

![Captura de pantalla 2022-11-15 a las 15 52 01](https://user-images.githubusercontent.com/93529369/203799487-260ea475-c7cb-45a5-a0a8-124621a9591c.png)


As seen in the different distances, the longest one was **3.5 Å** (between 505(TYR) and 37(GLU)), extent which we'll use to create a variable in the script.

```
GLN A 24 OE1 : ASN E 487 ND2 : 2.687678
ASP A 30 OD2 : LYS E 417 NZ : 2.9049046
LYS A 31 NZ : GLN E 493 OE1 : 2.925601
GLU A 35 OE2 : GLN E 493 OE1 : 3.4952347
GLU A 37 OE2 : TYR E 505 OH : 3.4565377
ASP A 38 OD2 : TYR E 449 OH : 2.6951609
TYR A 41 OH : ASN E 501 OD1 : 3.4305587
TYR A 41 OH : THR E 500 OG1 : 2.7074692
GLN A 42 NE2 : GLN E 498 OE1 : 2.9271529
GLN A 42 NE2 : TYR E 449 OH : 2.7875116
GLN A 42 NE2 : GLY E 446 O : 3.2436583
TYR A 83 OH : ASN E 487 OD1 : 2.7878785
LYS A 353 O : GLY E 502 N : 2.7837358
LYS A 353 NZ : GLY E 496 O : 3.084003
ASP A 355 OD2 : THR E 500 O : 3.3421304
```

## Second Step

This second step consisted in **evaluating the Interaction energy among chains** (between components of A-E complex). This interaction energy is defined by the difference between the total energy of each chain in the bound state and the unbound state. But to simplify, **we assumed the structure does not change from the complex to the isolation of both chains in solution**. Additionaly, we considered solvation energies for all atom types.

Considering these points, interaction energy between components of a A-B complex (in this case A-E) will come from the following formula:
![formula](https://user-images.githubusercontent.com/93529369/203762378-233e1e8f-fdf9-4a99-ba2f-e75f913fda6a.png)

Basically, what is stating is that the binding energy comes from the difference of electrostatic, Van der Waals and solvation energy between A and B, minus the solvation energy individually of each chain.
Thus, if we have the interface residues defined correctly, obtained values will be very similar. 

To do that we used **Biopython** as a package tool iterating over the structure to get both chains and its corresponding residues ID's. At that point it was just a matter of calling the different functions with the MAXDIST (cut-off distance) variable set to 3.5 Å which was previously selected in the first step. (All script commented at file named 'energy_evaluation.ipynb').
This process is repeated twice, first for those residues on the interface and the next cell for all residues of complex. What changes in this second cell of the script is that the **MAXDIST variable is set to zero**, these will provide information of the energy of all residues on the protein, not just the interface.

Therefore, we achieved these results:

![res](https://user-images.githubusercontent.com/93529369/203763545-f00af95a-afc0-4012-82da-69193e287e51.png)

We obtained a final dG of -172 selecting all residues and -82 for those residues participating on the contacts.

## Third step

In this step we had to determine the **effect of replacing each interface residue with Alanine** throughout ΔGA-B and plot the obtained results, highlighting those residues that are more relevant to the interface stability.
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

As we can see from this image the side-chain of Alanine is what constitutes the others basic structure. With an exception of the Glycine, which is the simplest residues we'll find, with Glycine there is no need to replace anything because it won't make any change.

![Captura desde 2022-11-24 16-21-09](https://user-images.githubusercontent.com/93529369/203818759-2acfba18-e762-4ac3-b7f0-6cdba1676624.png)

The results obtained are in a file called 'res_ala.txt'
```
GLN A24, 3.334
PHE A28, -0.7887
ASP A30, 6.292
LYS A31, 8.02
HIE A34, 5.351
GLU A35, 4.779
GLU A37, 4.999
ASP A38, 4.695
TYR A41, 3.858
GLN A42, 9.785
TYR A83, 3.01
LYS A353, 7.583
GLY A354, 0.0
ASP A355, 2.562
LYS E417, 4.925
GLY E446, 0.0
TYR E449, -10.57
TYR E453, -2.924
ASN E487, 2.757
TYR E489, 0.843
GLN E493, 10.61
GLY E496, 0.0
GLN E498, 8.258
THR E500, -0.6821
ASN E501, 4.609
GLY E502, 0.0
TYR E505, -1.336
```

In the last column appears the overall ΔG of the change of the residue in the first column for an Alanine. For example, if we look in the first row we make a change from GLN A24 (chain A residue number 24) to an Alanine and we obtain a ΔG of 3.334. With these overall ΔG we are able to tell the impact that the mutation has. 
The results we have obtained are either negative, positive or 0. A **negative ΔΔG** indicates that the **mutation provides stability**.
Lastly, we have to compute a plot where we put the binding energy in the y axis and the residue ID on the x axis. As we wanted to observe the residues that are more relevant to the stability, we decided to put the **binding energy in absolute values** in order to have  everything positive.  In that way,  we are able to see that the values that are higher are the ones that have a **higher impact** on the structure of the protein. However, later we have to look if this change affects the protein positively or negatively.
Here you can observe the plot:

![plot](https://user-images.githubusercontent.com/93529369/203806032-bce383b0-c1c8-442c-9fb0-c05a8ca12096.png)

Since all the magnitudes are in absolute value, the highest peaks will reflect the essential positions in the interface and how it will alter the complex.

## Fourth step

Just to wrap the project we prepared a session on Pymol following a legend, also with the absolute values, since we want to know the impact:

```
Red: from 5 to +10 ΔG difference, therefore a critical or highest impact
Orange: from 0.1 to 4.9 ΔG difference, not a big difference
Grey: 0 difference, basically formed by Glycines
```

![Captura desde 2022-11-24 19-25-58](https://user-images.githubusercontent.com/93529369/203847677-558a091c-2636-40f8-9a8a-3889586cf587.png)  ![Captura desde 2022-11-24 19-29-20](https://user-images.githubusercontent.com/93529369/203848018-66f8279b-d30d-4478-a33f-29864d94f398.png)

## Results and discussion

When we were doing the preparation to obtain a pdb with the molecule fixed, what we obtained was not the one we needed. Therefore, we decided to use the fixed pdb from Josep Lluís Gelpí in order to be able to perform the other steps properly, if not the results we obtained were too big. 

In step 2, when we had to access the file called naccess, we encountered another problem. When we try to access the file while executing the script, it appears that we didn’t have the permissions required. To solve it, we had to change the path of the naccess file and then install csh on the terminal. Finally, execute the install file with this command:
```ruby
csh install.scr 
```
After doing this, we were able to execute the script correctly.

### Conclusions

After following all the steps we’ve obtained some conclusions:
We’ve been able to observe the complex structure and visualize the interactions between residues. Using Pymol we’ve found the interface atoms, polar bonds and distances. In this case, the longest distance was 3.5 Å between aminoacids 505(TYR) and 37(GLU). Furthermore, we have to take into account that it is a *Crystal structure* made with an **X-ray diffraction method**, therefore the distance will not be exactly the same, but we have to know that in reality it may vary a bit.

We have evaluated the energy interaction between the two chains and when comparing the total energy of all the residues with the total energy of the interface residues we have obtained both DG negatives and the results are quite similar which means that the cut-off distance was correctly calculated.

By performing the alanine change we’ve been able to see which are the most relevant residues for the stability of the interface:

To start with the TYR E449, one of the critical residues on these structure,  which gave us an  ΔGA-B of  -10.57 in step 3, we know that the tyrosine is a hydrophobic amino acid, with an aromatic ring that makes it much more hydrophobic. When we substitute it by an alanine (that is less hydrophobic) it is possible that it comes into contact with water and therefore creates more overall stability. 
We have to take into account the distance in which the amino acid is from the other chain, because when we replace it with alanine it can happen that they do not interact. That 's because the alanine can be shorter and therefore it cannot achieve contact. Consequently, in some cases the change of the amino acid stabilizes and others destabilize.
That’s the case of the TYR A41, when we substitute that residue for an alanine it destabilizes because the difference of the binding energy is positive.

Then we have glutamine (GLN E493, GLN A42, GLN E498) which is polar, therefore hydrophilic, it will create contacts with the water that surrounds the structure, if we change to Alanine which is hydrophobic, it destabilizes the structure by not creating contacts with the water.

Moreover we have Lysine (LYS A353) which is a polar positively charged amino acid. Note that the side chain has three methylene groups, so that even though the terminal amino group will be charged under physiological conditions, the side chain does have significant hydrophobic character. Lysines are often found buried with only the amino group exposed to solvent. Thus, if we change this type of residue with an alanine, which is an apolar amino acid, we can observe how the structure destabilizes

Additionally, we have Aspartic acid (ASP A30) that is one of the two acidic amino acids (polar negatively charged). Just as the Lysine, Aspartic Acid is polar, unlike alanine, therefore the change will destabilize the structure, since some bonds that were made with water are no longer there.

Then we find a follow-up of amino acids with little impact, it may be because the change in properties between the two is not limiting in terms of stability or because the distance is more or less maintained in the  previous contact.
