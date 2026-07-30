
## 🧪 Molecular Descriptors & LogP in RDKit
Molecular descriptors are numerical properties computed from a chemical structure (2D or 3D). They transform visual chemical representations into data that algorithms, machine learning models, and medicinal chemists can analyze.
## 1. What are Molecular Descriptors?
In cheminformatics, a descriptor quantitatively represents a physical, chemical, or topological property of a molecule.
Common descriptors include:
 * Molecular Weight (MW): Sum of atomic masses in the molecule.
 * Heavy Atom Count: Number of non-hydrogen atoms.
 * Number of Hydrogen Bond Donors (HBD): Typically N-H or O-H groups.
 * Number of Hydrogen Bond Acceptors (HBA): Typically N or O atoms with lone pairs.
 * Rotatable Bonds (RB): Measures molecular flexibility.
   
## 2. Understanding Rotatable Bonds (RB)
### What is a Rotatable Bond?
A Rotatable Bond is a single, non-ring bond that can freely rotate around its bond axis, allowing the molecule to change its overall three-dimensional shape (conformation).
* In RDKit: RDKit counts the number of such single bonds in the molecule. It excludes single bonds within rings or double/triple bonds, which are rigid and cannot rotate.
### Significance of Rotatable Bonds:
1. Molecular Flexibility: More rotatable bonds mean the molecule is more flexible, while fewer rotatable bonds indicate a more rigid molecule.
2. Receptor Binding: When a drug molecule binds to its target protein receptor, it often needs to adopt a specific shape. A moderate amount of flexibility is good, but too much flexibility can make it harder for the molecule to "find" the correct shape to bind effectively.
3. Bioavailability Guidelines: Some guidelines (like Veber's rules) suggest that orally active drugs should ideally have no more than 10 rotatable bonds.
### Visual Example:
Think of a linear molecule vs. a cyclic one. A chain can twist (high flexibility), but a ring structure cannot (high rigidity).

| Rigid Structure | Flexible Structure |
| :--- | :--- |
| Benzene (0 Rotatable Bonds) | n-Hexane (5 Rotatable Bonds) |
| c1ccccc1 | CCCCCC |

## . Deep Dive: LogP (Partition Coefficient)
### What is LogP?
logP measures a molecule's lipophilicity (how much it prefers fats/lipids vs. water). It represents the equilibrium ratio of a compound's concentration between two immiscible phases: Octanol (representing lipids/fats) and Water.

 measures a molecule's lipophilicity (how much it prefers fats/lipids vs. water). 

$$\text{LogP} = \log_{10}\left(\frac{\text{Concentration in Octanol}}{\text{Concentration in Water}}\right)$$
# Why is LogP Critical in Real-World Drug Discovery?
 1. Membrane Permeability: A drug needs a balanced logP to cross lipid cell membranes (too hydrophilic/water-soluble = cannot cross lipid bilayers).
 2. Solubility & Bioavailability: If logP is too high (too lipophilic/fat-soluble), the drug will not dissolve in blood/water, leading to poor absorption or toxicity.
 3. Lipinski's Rule of 5: A classic drug-likeness guideline states that an oral drug candidate should ideally have a logP ≤ 5.
## . RDKit Implementation
RDKit provides two main modules for descriptors:
 * rdkit.**Chem.Descriptors**: Calculates individual named properties (e.g., MW, LogP).
 * rdkit.**Chem.Crippen**: Specifically computes Wildman-Crippen LogP (MolLogP) and Molar Refractivity (MR).
### Python Code for Google Colab:
```python
from rdkit import Chem
from rdkit.Chem import Descriptors, Crippen
```

---
# 1. Define sample molecule (Aspirin)
```python 
smiles = "CC(=O)OC1=CC=CC=C1C(=O)O"
mol = Chem.MolFromSmiles(smiles)
```

---
# 2. Calculate Key Molecular Descriptors
```python 
mol_weight = Descriptors.MolWt(mol)          # Molecular Weight
logp = Crippen.MolLogP(mol)                   # Wildman-Crippen LogP (or Descriptors.MolLogP)
hbd = Descriptors.NumHDonors(mol)             # H-Bond Donors
hba = Descriptors.NumHAcceptors(mol)          # H-Bond Acceptors
rotatable_bonds = Descriptors.NumRotatableBonds(mol)  # Rotatable Bonds
# 3. Print Results
print("=== Aspirin Descriptors Summary ===")
print(f"Molecular Weight : {mol_weight:.2f} g/mol")
print(f"Calculated LogP  : {logp:.2f}")
print(f"H-Bond Donors    : {hbd}")
print(f"H-Bond Acceptors : {hba}")
print(f"Rotatable Bonds  : {rotatable_bonds}")
```

---
## 4. Lipinski's Rule of 5 (Drug-Likeness Test)
Lipinski's Rule evaluates if a chemical compound with a certain pharmacological or biological activity has chemical and physical properties that would make it likely to be an orally active drug in humans.

| Descriptor | Rule Threshold | RDKit Function |
| :--- | :--- | :--- |
| Molecular Weight |  ≤500{ g/mol} | Descriptors.MolWt(mol) |
| LogP | ≤ 5 | Descriptors.MolLogP(mol) |
| H-Bond Donors | ≤ 5 | Descriptors.NumHDonors(mol) |
| H-Bond Acceptors | ≤ 10 | Descriptors.NumHAcceptors(mol) |

```python
def check_lipinski(mol):
    """Checks Lipinski's Rule of 5 for a given Mol object."""
    mw = Descriptors.MolWt(mol) ≤ 500
    logp = Descriptors.MolLogP(mol) ≤ 5
    hbd = Descriptors.NumHDonors(mol) ≤ 5
    hba = Descriptors.NumHAcceptors(mol) ≤ 10
    
    passes_all = all([mw, logp, hbd, hba])
    return passes_all
```

---
# Test Aspirin
```python
print(f"Passes Lipinski's Rule: {check_lipinski(mol)}")
```

---
## 5. Summary Table for Your Notes

| Function | Purpose | Return Value Type |
| :--- | :--- | :--- |
| Descriptors.MolWt(mol) | Calculates molecular weight (g/mol). | float |
| Descriptors.MolLogP(mol) | Estimates octanol-water partition coefficient (LogP). | float |
| Descriptors.NumHDonors(mol) | Counts hydrogen bond donors (OH, NH). | int |
| Descriptors.NumHAcceptors(mol) | Counts hydrogen bond acceptors (O, N). | int |
| Descriptors.TPSA(mol) | Topological Polar Surface Area (\text{Å}^2). | float |
    
