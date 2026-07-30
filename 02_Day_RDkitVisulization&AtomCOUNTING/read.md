<img width="1280" height="698" alt="image" src="https://github.com/user-attachments/assets/8b2a1e45-5994-42a2-9865-37492ea34f16" />

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
<img width="1280" height="698" alt="image" src="https://github.com/user-attachments/assets/f6299ae5-e9f7-48d6-8758-3fefb2903e6d" />

##
🎨 2D Chemical Structure Visualization
RDKit provides built-in utilities in the `rdkit.Chem.Draw` module to render 2D images of chemical structures, either individually or in a grid format.
---
### 1. Importing the Drawing Module
Before rendering any molecules, you must explicitly import the `Draw` module along with `Chem`:
```python
from rdkit import Chem
from rdkit.Chem import Draw
```

### 2. Rendering a Single Molecule
To convert a single Mol object into a 2D image, use **Draw.MolToImage()**.
```python
# Create a Mol object (Ethanol)
mol = Chem.MolFromSmiles("CCO")
# Generate a 2D PIL Image
img = Draw.MolToImage(mol, size=(300, 300), legend="Ethanol (CCO)")
# Display the image inside Google Colab
display(img)
```
---
#### Understanding Key Arguments:
 * **size (Optional):** Defines the pixel dimensions (width, height) of the output image. Default is typically (300, 300). If omitted, RDKit uses standard default dimensions.
 * **legend (Optional):** Adds a text label or caption below the rendered molecule image. It is useful for displaying compound names or SMILES strings, but it is not required for rendering.
### 3. Grid Drawing for Multiple Molecules
When comparing multiple molecules, rendering them one by one is inefficient. **Draw.MolsToGridImage()** solves this by organizing multiple structures into a clean table/grid layout.
```python
# Define SMILES and corresponding labels
smiles_list = ["CC(=O)O", "c1ccccc1", "CC(=O)C", "C1CC1"]
labels = ["Acetic Acid", "Benzene", "Acetone", "Cyclopropane"]
# Convert SMILES strings to Mol objects
mols = [Chem.MolFromSmiles(s) for s in smiles_list]
# Render a grid image (2 molecules per row)
grid_img = Draw.MolsToGridImage(
    mols, 
    molsPerRow=2, 
    subImgSize=(250, 250), 
    legends=labels
)
# Display grid in Colab
display(grid_img)
```

#### Why and When to Use MolsToGridImage:
 * **When to use:** Use it whenever you are dealing with datasets, screening lists, or performing structural comparisons.
 * **Key Benefits:** It automatically aligns molecules, maintains consistent sizing across all items, and saves visual space by rendering a single matrix image instead of multiple separate output blocks.
### 4. Comparison: MolToImage vs MolsToGridImage

| Feature | Draw.MolToImage() | Draw.MolsToGridImage() |
| :--- | :--- | :--- |
| **Primary Use Case** | Visualizing a **single** molecule structure. | Visualizing **multiple** molecules side-by-side. |
| **Input Type** | A single Mol object. | A list of Mol objects ([mol1, mol2, ...]). |
| **Image Layout** | Single frame. | Matrix / Grid layout with custom rows/columns. |
| **Sizing Argument** | size=(width, height) | subImgSize=(width, height) per cell. |
| **Labeling Argument** | legend="Single Label" | legends=["Label 1", "Label 2", ...] |

---
