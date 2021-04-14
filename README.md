# ComplexMod
## DESCRIPTION
The function of this program is to reconstruct biological macrocomplexes. It can build them using protein-DNA/RNA interactions as well as protein-protein interactions. The input is a set of binary interactions and the desired number of chains of the target complex. Moreover, you can add extra arguments, as RMSD treshold. This program needs as input the folder where the input files are, and you have to select an output directory to save the output files. If you do not select the output folder, the program automatically will create one.

## BACKGROUND
An important problem in protein structure determination and modeling is to position a given group of protein structures in three-dimensional space once they are determined, i.e., protein structure superposition or alignment. The structures may be determined for the same protein but at different times, such as those obtained in NMR structure determination. It is then important to find the best superimposition for the structures, one that truly reflects the dynamic changes of the structures over time. The structures may also refer to different proteins, such as those obtained for mutated proteins or proteins from a specific gene family. It is then critical to find the best alignment for the structures, in order to reveal some shared structural or functional motifs among the structures.

The aim of this project is to, given molecules of DNA or proteins that are interacting with each other, make a program that generates macro complexes. In order to achieve it, we would create an algorithm that uses a value named RMSD to build that macro complex.  

A conventional approach to superimposing a group of structures is to translate and rotate the structures so that the arithmetic average of the coordinate differences of the corresponding atoms in the structures, called the root-mean-square deviation of the structures, is minimized. Here, the best superimposition of the structures is obtained when the minimal possible root-mean-square deviation is reached. The latter is called the RMSD value of the structures and is used as a measure of the similarity of the structures. RMSD can be calculated for any type and subset of atoms; for example, Cα atoms of the entire protein, Cα atoms of all residues in a specific subset (e.g. the transmembrane helices, binding pocket, or a loop), all heavy atoms of a specific subset of residues, or all heavy atoms in a small-molecule ligands. For obtaining the RMSD value we have this formula: 

![image](https://user-images.githubusercontent.com/78853932/114619446-de20d880-9caa-11eb-8fec-dd53153a2be9.png)

RMSD values are presented in Å and calculated by where the averaging is performed over the n pairs of equivalent atoms and di is the distance between the two atoms in the i-th pair. In DNA interactions we can use P, C2′ and C4' atoms. All three DNA atoms appear in any nucleotide, regardless of type (A, C, G or T). The P atom is situated in the DNA backbone, C2′ in the DNA sugar ring, and C4' is in the nucleobase. This allows for easy computations of the RMSD between DNA molecules containing the same number of nucleotides but different sequences. In our particular case, ComplexMod calculates the RMSD of the Cα atoms if there is a protein-protein interaction, and the RMSD of the C4' for the DNA/RNA interactions. In order to understand better this technique, we would make an example: 

| No Changes | Re-centered | Rotated | 
| ------------- | ------------- | ------------- |
| ![image](./img/no_center.png) | ![image](./img/recentered.png) | ![image](./img/rotated.png) |

You have molecule A and B and want to calculate the structural difference between those two. If you just calculate the RMSD straight-forward you might get a too big of a value. For that reason, you would need to first recenter the two molecules and then rotate them unto each other until you get the true minimal RMSD. ComplexMod performs this approximation with several chains (if the input have more than two chains). After obtaining the minimal RMSD between two chains, we are interested in the chain of the sample structure that actually has not been superimposed, as the final goal is trying to build a multi-chain complex. Because of the superimposition, the atom coordinates of this non-superimposed chain has changed, thus, they have rotated towards the reference structure. To accept this new rotation, we must check whether the atoms of this chain clashes with the atoms of any of the chains of the reference structure. Clashes are unfavorable interactions where atoms are too close together. 

They can be calculated using a K-dimensional tree data structure (KDTree), which uses N-dimensional vectors to find all points within a radius of a given point. Thus, we can know how many atoms have at least one atom within radius of center. In a real proteins, clashes cannot happen, because if the distance between two atoms is minimum, the energy is maximum. For instance, repulsive forces prevail in Van Der Waals interactions, due to the collision of external electron clouds, making this interaction unfavorable. If the number of clashes is below a given threshold, we can allow this new rotation and add the chain in the reference structure.


### LIMITATIONS
This approach have some limitations: 
- Calculates only structures with the same number of atoms. 
- Has a low flexibility: Scale bad the cases that two structures that are identical with the exception of a position of a single loop or a flexible terminus typically have a large global backbone. This is not a particular case about our program, it happens to any algorithm that optimizes the global RMSD.
- Any kind of RMSD-based measurement requires prior assignment of atom correspondences.
- GUI interface is not develop yet

### OTHER APPROACHES 
This program uses the common way to build a macro complex, that as we have explained before is called the root-mean-square deviation (RMSD). But there are other approaches that we will briefly explain: 

1. **G-RMSD**: Generalized Root Mean Square Deviation (G-RMSD) method calculates the minimal RMSD value of two atomic structures by optimal superimposition. G-RMSD is not restricted to systems with an equal number of atoms to compare or a unique atom mapping between two molecules. The method can handle any type of chemical structures, including transition states and structures which cannot be explained only with valence bond (VB) theory (non-VB structures). It requires only Cartesian coordinates for the structures.
2. **Dynamically weighted RMSD**: This approach takes into account the weight of the different molecules. Different atoms may have different properties and they should be compared differently. For this reason, when superimposed with RMSD, the coordinate differences of different atoms should be evaluated with different weights. This method the thermal motions of the atoms can be obtained from several sources such as the mean-square fluctuations that can be estimated by Gaussian network model analysis. 
3. **RMS of dihedral angles**: An approach complementary to Cartesian backbone RMSD is based on the representation of the protein structure in the internal coordinates that include bond lengths, planar bond angles, and dihedral torsion angles.
4. **Global Distance Test (GDT)**: As described above, RMSD heavily depends on the precise superimposition of the two structures and is strongly affected by the most deviated fragments. GDT ( used for CASP model evaluation) performs multiple superimpositions, each including the largest superimposable subset for one of the residues,  between the two structures. The output of a GDT calculation represents a curve that plots the distance cutoff against the percent of residues that can be fitted under this distance cutoff. A larger area under the curve corresponds to more accurate prediction.

This does not mean that one approach is better than the other. All approaches have their pros and cons, but for future versions of the program it is important to take into account all possible improvements, because some of this approaches could solve some actual limitations that our tool presents. 

## INSTALLATION
## REQUERIMENTS
## ALGORITHM
### FUTURE CODE IMPROVMENT
- Functions could be further spitted
- Use of one liners (the pythonic way)
- Use of generator functions instead of lists (memory costs)
- Use of composition over inheritance (since in python everything is an object, the easy  to adapt existent code to our program purposes providing  flexibility, but this has a drawback and it's that since the program works adding new features on top of predefined functions, if something needs to be modified its a bit messy (hindering code mantainance), so adding more composition to our code could make this task easier)
## EXAMPLES
## REFERENCES 
We have extract some of the information about protein-protein interaction superimposition, RMSD value and things related to this project from this references: 
- Bottaro, S., Di Palma, F., & Bussi, G. (2014). The role of nucleobase interactions in RNA structure and dynamics. Nucleic Acids Research, 42(21), 13306–13314.
- Garcia‐Garcia, J., Bonet, J., Guney, E., Fornes, O., Planas, J., & Oliva, B. (2012). Networks of Protein Protein Interactions: From Uncertainty to Molecular Details. Molecular Informatics, 31(5), 342-362.
- Kufareva, I., & Abagyan, R. (2012). Methods of protein structure comparison. Methods in Molecular Biology (Clifton, N.J.), 857, 231–257.
- Wu, D., & Wu, Z. (2010). Superimposition of protein structures with dynamically weighted RMSD. Journal of Molecular Modeling, 16(2), 211–222.
