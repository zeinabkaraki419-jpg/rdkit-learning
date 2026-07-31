
# 🧬 Molecular Fingerprints & Chemical Similarity in RDKit
In cheminformatics, comparing molecules based on their 2D drawings or SMILES strings directly is computationally slow and inefficient. Instead, we convert molecules into **Molecular Fingerprints** (binary bit vectors) and calculate **similarity scores** mathematically.

## 1. What is a Molecular Fingerprint?
A **Molecular Fingerprint** is a fixed-length vector of bits (`0`s and `1`s) where each bit indicates the presence (`1`) or absence (`0`) of specific chemical substructures or patterns within the molecule.
$$\text{Molecule} \xrightarrow{\text{RDKit}} \text{Bit Vector } [1, 0, 0, 1, 1, 0, \dots, 1]$$
### Common Types of Fingerprints in RDKit:
1. **Morgan Fingerprints (Circular Fingerprints / ECFP):**
   * Based on the **Extended-Connectivity Fingerprints (ECFP)** algorithm.
   * Scans atomic neighborhoods within a specific radius (e.g., `radius=2` corresponds to ECFP4).
   * **Most widely used** for virtual screening and machine learning.
2. **RDKit Topological Fingerprints:**
   * Analyzes paths through the molecular graph up to a certain atom length.
3. **MACCS Keys:**
   * A predefined 166-bit dictionary of specific structural patterns (e.g., "Contains a carbonyl group").
---
## 2. Measuring Chemical Similarity: Tanimoto Coefficient
The most standard metric for comparing two molecular fingerprints is the **Tanimoto Similarity Index** (also known as the Jaccard Index).
$$\text{Tanimoto Similarity} = \frac{c}{a + b - c}$$
* **$a$**: Number of bits set to `1` in Molecule A.
* **$b$**: Number of bits set to `1` in Molecule B.
* **$c$**: Number of bits set to `1` in **both** Molecule A and B (shared bits).
### Score Interpretation:
* **`1.0`**: Identical fingerprints (very high structural similarity).
* **`0.0`**: No shared structural features.
* **`≥ 0.85`**: Commonly used threshold in drug discovery to infer similar biological activity (Neighborhood Behavior Principle).
---
## 3. RDKit Implementation Code
Run this code in Google Colab to compute Morgan Fingerprints and calculate Tanimoto Similarity:
```python
from rdkit import Chem
from rdkit.Chem import rdMolDescriptors, DataStructs
# 1. Define sample molecules
aspirin_smiles = "CC(=O)OC1=CC=CC=C1C(=O)O"
ibuprofen_smiles = "CC(C)CC1=CC=C(C=C1)C(C)C(=O)O"
ethanol_smiles = "CCO"
mol_aspirin = Chem.MolFromSmiles(aspirin_smiles)
mol_ibuprofen = Chem.MolFromSmiles(ibuprofen_smiles)
mol_ethanol = Chem.MolFromSmiles(ethanol_smiles)
```
 2. Generate Morgan Fingerprints (ECFP4 equivalent: radius=2, nBits=2048)
```python
fp_aspirin = rdMolDescriptors.GetMorganFingerprintAsBitVect(mol_aspirin, radius=2, nBits=2048)
fp_ibuprofen = rdMolDescriptors.GetMorganFingerprintAsBitVect(mol_ibuprofen, radius=2, nBits=2048)
fp_ethanol = rdMolDescriptors.GetMorganFingerprintAsBitVect(mol_ethanol, radius=2, nBits=2048)
```
 3. Calculate Tanimoto Similarity
```python
sim_aspirin_ibuprofen = DataStructs.TanimotoSimilarity(fp_aspirin, fp_ibuprofen)
sim_aspirin_ethanol = DataStructs.TanimotoSimilarity(fp_aspirin, fp_ethanol)
```
4. Output Results
```python
print("=== Chemical Similarity Results ===")
print(f"Similarity (Aspirin vs. Ibuprofen) : {sim_aspirin_ibuprofen:.3f}")
print(f"Similarity (Aspirin vs. Ethanol)   : {sim_aspirin_ethanol:.3f}")
```

---
## 4. Quick Reference Summary

| Concept | Description | RDKit Function |
| :--- | :--- | :--- |
| Morgan Fingerprint | Circular fingerprint capturing atomic environments up to a radius. | rdMolDescriptors.GetMorganFingerprintAsBitVect() |
| Tanimoto Similarity | Mathematical metric comparing shared bit features between two fingerprints. | DataStructs.TanimotoSimilarity() |
| MACCS Keys | Structural key fingerprint with 166 predefined bit patterns. | MACCSkeys.GenMACCSKeys(mol) |
