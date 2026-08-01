

# 🐼 RDKit Integration with Pandas (`PandasTools`)
In data science, **Pandas DataFrames** are the standard structure for handling tabular data. RDKit provides **`rdkit.Chem.PandasTools`**, which seamlessly integrates chemical structures into DataFrames.
This allows you to render 2D molecular structures directly inside DataFrame columns, perform batch calculations, and filter chemical datasets using SMARTS or numerical descriptors.

## 1. Key Features of `PandasTools`
* **Visual Molecule Rendering:** Displays 2D drawings of molecules directly inside Jupyter / Colab DataFrames.
* **SDF File I/O:** Load and save Structure-Data Files (`.sdf`) directly to and from DataFrames.
* **Substructure Filtering:** Filter DataFrames instantly using SMARTS patterns.
* **Descriptor Calculation:** Apply RDKit functions across entire DataFrame columns.
---
## 2. Basic Setup & Molecule Column Creation
To display chemical structures properly, always call `PandasTools.ChangeMoleculeRendering(df)` after adding molecule columns.
```python
import pandas as pd
from rdkit import Chem
from rdkit.Chem import PandasTools, Descriptors
# 1. Create a sample dataset with SMILES
data = {
    'Name': ['Ethanol', 'Aspirin', 'Ibuprofen', 'Caffeine'],
    'SMILES': ['CCO', 'CC(=O)Oc1ccccc1C(=O)O', 'CC(C)CC1=CC=C(C=C1)C(C)C(=O)O', 'Cn1cnc2c1c(=O)n(C)c(=O)n2C']
}
df = pd.DataFrame(data)
# 2. Add a Molecule Column from SMILES
PandasTools.AddMoleculeColumnToFrame(df, smilesCol='SMILES', molCol='ROMol')
# 3. Enable 2D structure rendering in the DataFrame
PandasTools.ChangeMoleculeRendering(df)
# Display DataFrame
df
```
## 3. Calculating Descriptors Across Columns
You can use standard Pandas syntax to add molecular descriptors ((LogP) or Molecular Weight) for all molecules in batch:
```python
# Calculate Molecular Weight and LogP for each molecule
df['MW'] = df['ROMol'].apply(Descriptors.MolWt)
df['LogP'] = df['ROMol'].apply(Descriptors.MolLogP)
df['Rotatable_Bonds'] = df['ROMol'].apply(Descriptors.NumRotatableBonds)
# View updated DataFrame
df[['Name', 'MW', 'LogP', 'Rotatable_Bonds']]
```
## 4. Substructure Filtering on DataFrames
You can filter an entire DataFrame using a SMARTS pattern with PandasTools.FindMatches():
```python
# Define a SMARTS query (e.g., Aromatic Ring)
aromatic_pattern = Chem.MolFromSmarts('a1aaaaa1')
# Filter DataFrame to keep only molecules containing an aromatic ring
aromatic_df = df[df['ROMol'].apply(lambda mol: mol.HasSubstructMatch(aromatic_pattern))]
aromatic_df[['Name', 'SMILES']]
```

## 5. Loading and Saving SDF Files
PandasTools makes reading and writing .sdf files extremely straightforward:
### A. Load an SDF File into a DataFrame:
```python
# Load SDF directly into Pandas DataFrame
df = PandasTools.LoadSDF("molecules.sdf", idName="ID", molColName="ROMol")
```
### B. Save a DataFrame to an SDF File:
```python
# Export DataFrame with molecules back to SDF
PandasTools.WriteSDF(df, "output_dataset.sdf", molColName="ROMol", properties=list(df.columns))
```
## 6. Quick Function Reference

| Function | Purpose | Example / Syntax |
| :--- | :--- | :--- |
| **AddMoleculeColumnToFrame()** | Converts a SMILES column into RDKit Mol objects column. | PandasTools.AddMoleculeColumnToFrame(df, 'SMILES', 'ROMol') |
| **LoadSDF()** | Reads an .sdf file directly into a Pandas DataFrame. | df = PandasTools.LoadSDF('file.sdf') |
| **WriteSDF()** | Exports a DataFrame containing molecules to an .sdf file. | PandasTools.WriteSDF(df, 'output.sdf') |
| **df['Mol'].apply()** | Applies RDKit descriptor functions across all rows. | df['MW'] = df['ROMol'].apply(Descriptors.MolWt) |

```
