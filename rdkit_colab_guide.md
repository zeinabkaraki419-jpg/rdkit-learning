
# RDKit Guide & Google Colab Setup
## 1. What is RDKit?
RDKit is an open-source software toolkit used for cheminformatics and machine learning in chemistry. It allows scientists and developers to process, analyze, and visualize chemical compounds using computer code.
## 2. Why Do We Use RDKit?
We use RDKit because it simplifies working with chemical data:
 * SMILES Parsing: Converts chemical structures (SMILES strings like CCO for ethanol) into digital molecules.
 * Molecule Visualization: Draws 2D and 3D images of chemical structures.
 * Molecular Descriptors: Calculates chemical properties (such as molecular weight, LogP, and atom count).
 * Fingerprinting: Turns molecules into numerical vectors so machine learning algorithms can read them.
 * Substructure Searching: Finds specific chemical functional groups within larger molecules.
## 3. How to Set Up RDKit in Google Colab
Installing RDKit in Google Colab takes only one line of code using pip.
### Step 1: Install RDKit
Open a code cell in Google Colab and run:
!pip install rdkit

### Step 2: Quick Example Code
Run this simple Python code in your Colab notebook to check if RDKit is working properly:
from rdkit import Chem
from rdkit.Chem import Draw

# Create a molecule from a SMILES string (Aspirin)
smiles = "CC(=O)OC1=CC=CC=C1C(=O)O"
mol = Chem.MolFromSmiles(smiles)

# Print Molecular Weight
from rdkit.Chem import Descriptors
mw = Descriptors.ExactMolWt(mol)
print(f"Molecular Weight: {mw:.2f}")

# Draw the molecule
Draw.MolToImage(mol)
