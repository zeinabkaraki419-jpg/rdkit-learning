# SMARTS with RDKit

A quick guide to SMARTS (SMiles ARbitrary Target Specification) — a language for describing molecular substructure patterns — and how to use it with RDKit.

## What is SMARTS?

SMARTS extends SMILES notation to describe substructure *patterns* rather than specific molecules. While SMILES describes one exact molecule (e.g. CCO for ethanol), SMARTS describes a *query* that can match many molecules (e.g. "any carbon bonded to an oxygen").

SMARTS is used for:
- Substructure searching (does this molecule contain this pattern?)
- Filtering compound libraries (e.g. removing molecules with reactive groups)
- Reaction definition (combined with reaction SMARTS/SMIRKS)
- Highlighting functional groups

---
## 1. SMILES vs. SMARTS: What is the Difference?

| Feature | SMILES | SMARTS |
| :--- | :--- | :--- |
| Primary Purpose | Represents a complete molecule. | Defines a pattern or substructure. |
| Usage in RDKit | Chem.MolFromSmiles() | Chem.MolFromSmarts() |
| Specificity | Exact atomic types and explicit bonds. | Logical operators, wildcards, and atom environment constraints. |
| Example | c1ccccc1 (Benzene molecule) | [a] (Any aromatic atom) |

---
## 2. Common SMARTS Notation & Wildcards
SMARTS allows you to write flexible chemical queries using special symbols:
### A. Atom Primitives
* **`*`**: Any atom (wildcard)
* `a`: Any aromatic atom
* `A`: Any aliphatic atom.
* **`[#6]`**: Carbon atom (specified by atomic number)
* **`[#7]`**: Nitrogen atom (specified by atomic number)
* **`[OH]`**: A hydroxyl group (Oxygen bound to 1 Hydrogen).
### B. Bond Primitives
* **`-`**: Single Bond
* **`=`**: Double Bond
* **`#`**: Triple Bond
* **`:`**: Aromatic Bond
* **`~`**: Any bond (wildcard bond)
### C. Logical Operators
* **`, ` (OR):** `[O,N]` means Oxygen OR Nitogen
*  **`;` (AND):** `[C;H1]` means Carbon AND attached to 1 Hydrogen
*  **`!` (NOT):** `[!#6]` means ANY atom that is NOT Carbon.
---
## 3. RDKit Implementation Code
In RDKit:
* Use Chem.MolFromSmiles() to target moleculesecules.
* Use Chem.MolFromSmarts() to query patternstterns.
  
To perform substructure searches using SMARTS in RDKit, use **mol.HasSubstructMatch()** or **mol.GetSubstructMatches()**.

```python
from rdkit import Chem
from rdkit.Chem import Draw
# 1. Define target molecules
aspirin = Chem.MolFromSmiles("CC(=O)OC1=CC=CC=C1C(=O)O")
ethanol = Chem.MolFromSmiles("CCO")
# 2. Define SMARTS patterns
# Pattern A: Carboxylic Acid group (-C(=O)OH)
carboxylic_acid_pattern = Chem.MolFromSmarts("C(=O)[OH]")
# Pattern B: Any Aromatic Ring
aromatic_ring_pattern = Chem.MolFromSmarts("a1aaaaa1")
# 3. Check for Substructure Matches
print("=== Substructure Matching Results ===")
# Check Aspirin
has_acid = aspirin.HasSubstructMatch(carboxylic_acid_pattern)
has_aromatic = aspirin.HasSubstructMatch(aromatic_ring_pattern)
print(f"Aspirin has Carboxylic Acid: {has_acid}")     # Returns True
print(f"Aspirin has Aromatic Ring:    {has_aromatic}") # Returns True
# Check Ethanol
ethanol_has_acid = ethanol.HasSubstructMatch(carboxylic_acid_pattern)
print(f"Ethanol has Carboxylic Acid: {ethanol_has_acid}") # Returns False
# 4. Get Matching Atom Indices
# Returns tuple of atom indices matching the SMARTS pattern
matches = aspirin.GetSubstructMatches(carboxylic_acid_pattern)
print(f"Matching Atom Indices in Aspirin: {matches}")
```
---
