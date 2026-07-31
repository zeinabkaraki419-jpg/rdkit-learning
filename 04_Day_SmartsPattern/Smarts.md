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
