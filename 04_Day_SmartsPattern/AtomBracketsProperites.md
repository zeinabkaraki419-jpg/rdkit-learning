
# 📦 Atom Bracket Properties in SMILES & SMARTS

In SMILES and SMARTS syntax, **square brackets `[...]`** are used to explicitly define detailed properties of a single atom. 
While simple organic atoms (like `C`, `N`, `O`) can be written without brackets in SMILES when using implicit hydrogens, using brackets `[...]` unlocks precise control over atomic features.

## 1. Syntax Structure Inside Brackets
Inside `[...]`, properties are evaluated in a specific order:
$$\text{[ (Isotope) + (Symbol) + (Chirality) + (Hydrogens) + (Charge) ]}$$

## 2. Core Atom Bracket Properties
### A. Formal Charge (`+` / `-`)
Specifies the electrical charge on the atom.
* **`[NH4+]`**: Ammonium ion with a $+1$ charge.
* **`[O-]`**: Oxygen with a $-1$ charge.
* **`[Fe+3]`** or **`[Fe+++]`**: Iron with a $+3$ oxidation state.
### B. Explicit Hydrogens (`H`)
Specifies the exact number of attached hydrogen atoms inside brackets.
* **`[CH4]`**: Methane (Carbon with 4 explicit hydrogens).
* **`[OH-]`**: Hydroxide ion (Oxygen with 1 hydrogen and $-1$ charge).
### C. Isotopes (`[Mass]`)
Preceding the atomic symbol with a number specifies the atomic mass (isotope).
* **`[13C]`**: Carbon-13 isotope.
* **`[2H]`** or **`[D]`**: Deuterium (Hydrogen-2).
### D. Stereochemistry / Chirality (`@` / `@@`)
Specifies tetrahedral chirality around a chiral center:
* **`@`**: Anti-clockwise / Counter-clockwise configuration.
* **`@@`**: Clockwise configuration.
* **Example:** `N[C@@H](C)C(=O)O` (L-Alanine).
---
## 3. Advanced SMARTS Bracket Properties
In **SMARTS**, bracket properties can include logical queries and topological features for substructure matching:

| Property Symbol | Meaning | Example | Explanation |
| :--- | :--- | :--- | :--- |
| **`#N`** | Atomic Number | `[#7]` | Matches any Nitrogen atom. |
| **`vN`** | Valence | `[v4]` | Matches any atom with a valence of 4. |
| **`XN`** | Total Connections (Degree + H) | `[X3]` | Matches any atom with 3 total bonds. |
| **`rN`** | Ring Size | `[r6]` | Matches an atom in a 6-membered ring. |
| **`RN`** | Ring Membership Count | `[R1]` | Matches an atom present in exactly 1 ring. |
| **`a` / `A`** | Aromatic / Aliphatic | `[a]` / `[A]` | Matches aromatic or aliphatic atom. |

---
## 4. RDKit Code Example
Here is how RDKit processes and inspects bracket properties from SMARTS/SMILES:
```python
from rdkit import Chem
# Create molecules with bracket properties
chiral_center = Chem.MolFromSmiles("N[C@@H](C)C(=O)O") # L-Alanine
isotope_mol = Chem.MolFromSmiles("[13CH3]C(=O)O")      # Carbon-13 Acetic Acid
# 1. Inspect Isotope
c13_atom = isotope_mol.GetAtomWithIdx(0)
print(f"Atom 0 Isotope Mass: {c13_atom.GetIsotope()}") # Returns 13
# 2. Inspect Formal Charge
ion = Chem.MolFromSmiles("[NH4+]")
print(f"Nitrogen Charge: {ion.GetAtomWithIdx(0).GetFormalCharge()}") # Returns +1
# 3. SMARTS Query using Ring & Valence properties
# Pattern: Nitrogen in a 6-membered ring
smarts_query = Chem.MolFromSmarts("[#7;r6]")
pyridine = Chem.MolFromSmiles("c1cnccc1")
print(f"Pyridine matches pattern: {py[13C]sSubstructMatch(smarts[CH3]) # True
```

---
## 5. Summary Reference
 * Use **[13C]** for Isotopes.
 * Use **[CH3]** for Explicit Hydrogens.
 * Use **[NH4+]** or **[O-]** for Formal Charges.
 * Use **[C@@H]** for Chirality.
 * Use **[#6;r6]** in SMARTS for advanced queries (e.g., Carbon in a 6-membered ring).
<ElicitationsGroup message="What would you like to do next?">
