# 🧪 Day 02: RDKit Visualization & Atom Counting

In this section, we cover how to count atoms in a molecule, handle implicit/explicit hydrogens (Heavy Atoms), and render 2D chemical structure images using RDKit.

---

## 1. Understanding Heavy Atoms & Implicit Hydrogens

In cheminformatics, **Heavy Atoms** refer to **all non-hydrogen atoms** (e.g., Carbon, Oxygen, Nitrogen, Sulfur).

> [!NOTE]
> * Implicit Hydrogens: By default, RDKit omits hydrogen atoms to save memory and simplify molecular graphs.
> * **mol.GetNumAtoms():** Returns the count of heavy atoms in the molecule.
> * **Chem.AddHs(mol):** Adds explicit Hydrogens atoms to the moleculer structure 

---

```python
from rdkit import Chem
mol=Chem.MolFromSmiles('CC') #C2H6
print(mol.GetNumAtoms()) #2 all  heavy atoms
mol_h=Chem.AddHs(mol)
print(mol_h.GetNumAtoms()) #8 include hydrogen atom
```

---


## 🎨 2D Chemical Structure Visualization

RDKit provides built-in utilities in the Chem.Draw module to render 2D images of chemical structures, either individually or in a grid format.


### 1. Rendering a Single Molecule

To convert a single Mol object into a 2D image, use **Draw.MolToImage()**.

```python
from rdkit import Chem
from rdkit.Chem import Draw

# Create a Mol object (Ethanol)
mol = Chem.MolFromSmiles("CCO")

# Generate a 2D PIL Image
img = Draw.MolToImage(mol, size=(300, 300), legend="Ethanol (CCO)")

# Display the image inside Google Colab
display(img)
