# Quantum-Protein-Folding
Proteins in nature fold in continuous 3D structures in their Quaternary structure. The process of simulating continuous positions is too expensive, hence the researchers use lattice based models, where amino acids sit on discrete points. Our code uses a hybrid lattice which combines:
1. BCC - Body Centered Cubic -which has 8 directions (±1,±1,±1) - Good for hydrophobic core packing
2. FCC - Face Centered Cubic - which has 12 directions (±1,0,±1),(0,±1,±1),(±1,±1,0) - Excellent for modelling compact structures
3. Step sizes of 0.5(small step for fine detail) and 1(normal step for backbone extension) - doubles the diversity of movements


This provides us with more biological realism, better folding diversity and is closer to the real molecular geometry.


Instead of running the full algorithm, this code simulates the post-processing pipeline:
* Bitstring - direction mapping
* Direction - 3D Co-ordinates
* Energy evaluation using physically motivated potentials
* Interactive 3D visualization for better understanding
* Stepwise folding animation




QAOA outputs bitstrings from measurement, since we are using 5 qubits(5 bits per residue) we get 32 possible directions. 


idx = int(bitstring[i*5:(i+1)*5], 2)
dirs.append(ALL_DIRECTIONS[idx])


This snippet simulates the collapse of a QAOA state into a definite folding path.


Hamiltonian Interaction:
* Hydrophobic - Hydrophobic attraction (H-H -> -1.0)
* Electrostatic attraction((H,K,R)⁺ + (D,E)⁻ -> -2.0)
* Electrostatic repulsion((H,K,R)⁺ + (H,K,R)⁺ ->+1.5)
* Weak generic contact(0.5)
* Steric penalty (self-intersection)(+100 if two residues overlap)
These approximate the short-range residue-residue potentials similar to HP mode and Miyazawa-Jernigan matrix.


The total fold energy includes overlap penalty and pairwise residue interaction for residues closer than L1 - distance 1.5. The goal of the solver is to find the bitstring that minimizes this energy. This project simulates the evaluation of energy once that bitstring is known.


The fold is rendered with a Backbone line where Red represents hydrophobic residue and blue represents the polar residue.
The program also prints the bitstring for each residue, the direction selected and the actual 3D vector used.
<img width="855" height="313" alt="image" src="https://github.com/user-attachments/assets/6cb3aa09-b84d-415a-b2ce-b37333bb6097" />




