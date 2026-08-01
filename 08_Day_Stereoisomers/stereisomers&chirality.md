
# 🧬 Stereoisomers & Chirality in RDKit
**Stereoisomers** are molecules that have the **same chemical formula and same atom connectivity**, but differ in the **3D spatial orientation** of their atoms. 
In drug discovery, stereoisomerism is crucial because two enantiomers of the same compound can have drastically different biological activities (e.g., one may be an effective drug while the other is inactive or toxic).
## 1. Types of Stereoisomers
1. **Enantiomers:** Non-superimposable mirror images (chiral molecules with opposite configuration at all chiral centers).
2. **Diastereomers:** Stereoisomers that are **not** mirror images (e.g., $cis/trans$ double bond isomers or molecules with multiple chiral centers where only some configurations change).
---
## 2. Representing Chirality in SMILES (`@` vs `@@`)
RDKit uses `@` and `@@` notation inside square brackets `[...]` to define tetrahedral chirality based on atom ordering:
* **`@` (Anti-Clockwise / Counter-Clockwise):** Looking down the bond to the lowest-priority atom (usually H), the priority sequence goes counter-clockwise.
* **`@@` (Clockwise):** Looking down the bond, the sequence goes clockwise.
### Example: L-Alanine vs. D-Alanine
```python
from rdkit import Chem
# L-Alanine vs D-Alanine SMILES
l_alanine = Chem.MolFromSmiles("N[C@@H](C)C(=O)O") # Clockwise configuration
d_alanine = Chem.MolFromSmiles("N[C@H](C)C(=O)O")  # Counter-clockwise configuration
```
## 3. Double Bond Stereochemistry (E/Z Isomers)
RDKit uses slashes (/ and \) around double bonds to represent directional orientation:
 * **C/C=C/C**: *trans*-2-butene (E-isomer).
 * **C/C=C\C**: *cis*-2-butene (Z-isomer).

## 4. RDKit Code Implementation
### A. Detecting Chiral Centers & R/S Designation
RDKit can automatically assign Cahn-Ingold-Prelog (CIP) stereochemical labels (R or S) to chiral centers using *Chem.FindMolChiralCenters()*:
```python
from rdkit import Chem
# Define a molecule with a chiral center (L-Alanine)
mol = Chem.MolFromSmiles("N[C@@H](C)C(=O)O")
# Find chiral centers and assign R/S stereochemistry
chiral_centers = Chem.FindMolChiralCenters(mol, includeUnassigned=True)
print("Chiral Centers (Atom Index, Configuration):")
print(chiral_centers)
# Output: [(1, 'S')]
```
### B. Enumerating All Possible Stereoisomers
You can use EnumerateStereoisomers to generate all possible 3D spatial combinations of a molecule:
```python
from rdkit import Chem
from rdkit.Chem.EnumerateStereoisomers import EnumerateStereoisomers, StereoEnumerationOptions
# Molecule with undefined stereochemistry
mol = Chem.MolFromSmiles("CC(O)C(F)C") # 2 chiral centers -> 2^2 = 4 stereoisomers
# Set options to enumerate all isomers
options = StereoEnumerationOptions(unique=True)
isomers = list(EnumerateStereoisomers(mol, options=options))
print(f"Total Stereoisomers Generated: {len(isomers)}\n")
for i, iso in enumerate(isomers):
    smiles = Chem.MolToSmiles(iso, isomericSmiles=True)
    centers = Chem.FindMolChiralCenters(iso)
    print(f"Isomer {i+1}: {smiles} | Centers: {centers}")
```
## 5. Quick Reference Summary

| Stereochemical Feature | SMILES / RDKit Notation | Description |
| :--- | :--- | :--- |
| **Clockwise Tetrahedral** | [C@@H] | R/S chirality center notation. |
| **Counter-Clockwise Tetrahedral** | [C@H] | R/S chirality center notation. |
| ***trans* Double Bond (E)** | C/C=C/C | Directional single bonds around double bond. |
| ***cis* Double Bond (Z)** | C/C=C\C | Directional single bonds around double bond. |
| **Detect R/S Centers** | Chem.FindMolChiralCenters(mol) | Returns atom indices and CIP designations (R/S). |

```
