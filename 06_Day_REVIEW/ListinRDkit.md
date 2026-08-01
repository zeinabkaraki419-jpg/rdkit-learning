```python
#for atom in mol.GetAtoms()
print(atoms.GetSymbol(), atoms.GetAtomicNum(), atoms.GetDegree())
# for bond in mol.GetBonds()
print( bond.GetBegininAtomIdx(),bond.GetEndAtomIdx(),bond.GetBondTypeAsDouble())
```

# 🧬 RDKit Atom & Bond Methods Reference
When working with molecular graphs in RDKit, molecules are composed of Atoms (nodes) and Bonds (edges). RDKit provides built-in methods to inspect their properties.
## 1. Atom Methods (mol.GetAtoms())
Iterating over mol.GetAtoms() returns Atom objects.
### A. atom.GetSymbol()
 * What it does: Returns the chemical element symbol as a string (e.g., 'C', 'O', 'N').
 * Example Output: 'O' for an oxygen atom.
### B. atom.GetAtomicNum()
 * What it does: Returns the atomic number (number of protons) of the element as an integer.
 * Example Output: 8 for Oxygen, 6 for Carbon.
### C. atom.GetDegree()
 * What it does: Returns the number of directly connected neighbor atoms (excluding implicit hydrogens).
 * Example Output: In Ethanol (CH3-CH2-OH), the middle carbon is connected to 1 Carbon and 1 Oxygen, so its degree is 2.
## 2. Bond Methods (mol.GetBonds())
Iterating over mol.GetBonds() returns Bond objects.
### A. bond.GetBeginAtomIdx()
 * What it does: Returns the zero-based index of the atom where the bond starts.
 * Example Output: 0
### B. bond.GetEndAtomIdx()
 * What it does: Returns the zero-based index of the atom where the bond ends.
 * Example Output: 1
### C. bond.GetBondTypeAsDouble()
 * What it does: Returns the numerical bond order as a float.
   * Single Bond = 1.0
   * Double Bond = 2.0
   * Triple Bond = 3.0
   * Aromatic Bond = 1.5
 * Example Output: 1.0 for a single bond (C-O), 2.0 for a double bond (C=O).
## 💻 Full Practical Code Example
```python
from rdkit import Chem

# Create Ethanol molecule: CH3-CH2-OH
mol = Chem.MolFromSmiles("CCO")

print("--- ATOM PROPERTIES ---")
for atom in mol.GetAtoms():
    idx = atom.GetIdx()          # Atom index
    symbol = atom.GetSymbol()     # Element symbol
    num = atom.GetAtomicNum()    # Atomic number
    degree = atom.GetDegree()    # Number of heavy atom neighbors
    
    print(f"Atom Index {idx}: Symbol={symbol}, AtomicNum={num}, Degree={degree}")

print("\n--- BOND PROPERTIES ---")
for bond in mol.GetBonds():
    begin_idx = bond.GetBeginAtomIdx()  # Start atom index
    end_idx = bond.GetEndAtomIdx()      # End atom index
    order = bond.GetBondTypeAsDouble()  # Bond order (1.0, 2.0, 1.5, etc.)
    
    print(f"Bond between Atom {begin_idx} and Atom {end_idx}: Order={order}")
```



## Expected Output:

*--- ATOM PROPERTIES ---

Atom Index 0: Symbol=C, AtomicNum=6, Degree=1

Atom Index 1: Symbol=C, AtomicNum=6, Degree=2

Atom Index 2: Symbol=O, AtomicNum=8, Degree=1

*--- BOND PROPERTIES ---

Bond between Atom 0 and Atom 1: Order=1.0

Bond between Atom 1 and Atom 2: Order=1.0

---

# 🔍 Batch Substructure Matching with Lists in RDKit
In RDKit, instead of checking one molecule at a time, you often store multiple SMILES strings in a **Python List** and loop through them to evaluate structural patterns (like searching for a Benzene ring) across an entire dataset.

## 1. Step-by-Step Code Breakdown
### A. Input Data Lists
```python
smiles_list = ['CCO', 'CC(=O)Oc1ccccc1C(=O)O', 'c1ccccc1', 'CCCC', 'c1ccncc1']
names = ['ethanol', 'aspirin', 'benzene', 'butane', 'pyridine']
 #smiles_list: A list containing 5 molecular SMILES strings.
 #names: A corresponding list of compound names in the exact same order.
```

### B. Defining the SMARTS Target Pattern

```python
benzene_pattern = Chem.MolFromSmarts('c1ccccc1')
 #Chem.MolFromSmarts('c1ccccc1'): Converts the SMARTS string into a search query pattern representing a **Benzene ring** (a 6-membered aromatic ring of carbons).
```
### C. Looping and Pairwise Iteration (zip)
```python
for smi, name in zip(smiles_list, names):
 #zip(smiles_list, names): Pairs up elements from both lists index-by-index:
   1. ('CCO', 'ethanol')
   2. ('CC(=O)Oc1ccccc1C(=O)O', 'aspirin')
   3. ('c1ccccc1', 'benzene')
   4. ('CCCC', 'butane')
   5. ('c1ccncc1', 'pyridine')
 # In each iteration, smi receives the SMILES string and name receives the corresponding molecule name.
```
### D. Molecule Conversion & Substructure Check
```python
    mol = Chem.MolFromSmiles(smi)
    has_benzene = mol.HasSubstructMatch(benzene_pattern)
    print(name, has_benzene)
 1. #Chem.MolFromSmiles(smi): Converts the current SMILES string into an RDKit Mol object.
 2. #mol.HasSubstructMatch(benzene_pattern): Checks whether the current molecule contains the **Benzene ring** substructure. Returns True or False.
 3. #print(name, has_benzene): Prints the name of the molecule alongside its boolean search result.
```

 2. Execution Output
When you run this loop, RDKit yields:
text
ethanol False
aspirin True
benzene True
butane False
pyridine False

## Explanation of Results:
 * **ethanol** (CCO): Aliphatic alcohol; contains no ring ->False.
 * **aspirin**: Contains a benzene ring fused to functional groups ->True.
 * **benzene** (c1ccccc1): Exact match to the target pattern -> True.
 * **butane** (CCCC): Straight-chain alkane ->False.
 * **pyridine** (c1ccncc1): Contains an aromatic nitrogen atom (heteroaromatic ring), so it does **not** match a pure 6-carbon benzene ring -> False.
   
## 3. Core Takeaways

| Tool / Function | Purpose |
| :--- | :--- |
| **zip(list1, list2)** | Iterates over two or more lists simultaneously in parallel pairs. |
| **Chem.MolFromSmarts()** | Prepares a fixed search query object to reuse across multiple molecules. |
| **mol.HasSubstructMatch()** | Evaluates if a target molecule contains the query substructure (True/False). |

---
