
# 🧩 Molecular Clustering in RDKit
**Molecular Clustering** is an unsupervised machine learning technique used to group a set of chemical structures into clusters based on their structural similarity. Molecules in the same cluster share similar chemical scaffolds and properties, while molecules in different clusters are structurally distinct.
In drug discovery, clustering is widely used to:
* **Diversity Selection:** Pick representative molecules from large chemical libraries.
* **Structure-Activity Relationship (SAR) Analysis:** Group active molecules to identify common core scaffolds.
* **Reduce Redundancy:** Remove duplicate or overly similar compounds from screening datasets.
---
## 1. Popular Clustering Algorithms in Cheminformatics
1. **Butina Clustering Algorithm (Most Popular in RDKit):**
   * **Distance-based & Centroid-driven:** Designed specifically for high-throughput screening data.
   * **Requires a Tanimoto Distance Cutoff:** You define a threshold (e.g., distance $\le 0.3$, meaning similarity $\ge 0.7$).
   * **Deterministic & Fast:** Does not require pre-defining the number of clusters ($k$).
2. **Hierarchical Clustering:**
   * Builds a tree-like hierarchy (dendrogram) of molecules from bottom-up or top-down.
---
## 2. Distance Matrix Calculation
Clustering algorithms require a **Distance Matrix** rather than similarity scores. 
$$\text{Tanimoto Distance} = 1.0 - \text{Tanimoto Similarity}$$
* **Distance = 0.0:** Identical molecular fingerprints.
* **Distance = 1.0:** Completely different molecular fingerprints.
---
## 3. RDKit Implementation: Butina Clustering
Here is the complete Python workflow to cluster a dataset using **Morgan Fingerprints** and the **Butina Algorithm**:
```python
from rdkit import Chem
from rdkit.Chem import rdMolDescriptors, DataStructs
from rdkit.ML.Cluster import Butina
```

 1. Define sample dataset of SMILES
```python
smiles_list = [
    "CC(=O)Oc1ccccc1C(=O)O",          # Aspirin
    "CC(C)CC1=CC=C(C=C1)C(C)C(=O)O",  # Ibuprofen
    "CC1=CC=C(C=C1)C(C)C(=O)O",       # Similar to Ibuprofen
    "Cn1cnc2c1c(=O)n(C)c(=O)n2C",     # Caffeine
    "CN1C=NC2=C1C(=O)N(C)C(=O)N2C",   # Caffeine variant
    "CCO"                             # Ethanol
]
mols = [Chem.MolFromSmiles(s) for s in smiles_list]
```
 2. Generate Morgan Fingerprints (ECFP4)
```python
fps = [rdMolDescriptors.GetMorganFingerprintAsBitVect(m, radius=2, nBits=2048) for m in mols]
```
 3. Calculate Condensed Tanimoto Distance Matrix
```python
n_fps = len(fps)
distance_matrix = []
for i in range(1, n_fps):
    # Calculate Tanimoto distance (1 - similarity) against all previous fingerprints
    sims = DataStructs.BulkTanimotoSimilarity(fps[i], fps[:i])
    dists = [1.0 - x for x in sims]
    distance_matrix.extend(dists)
```
4. Perform Butina Clustering
```python
# distThresh = 0.4 means molecules with Tanimoto similarity >= 0.6 will cluster together
cutoff = 0.4
clusters = Butina.ClusterData(distance_matrix, n_fps, distThresh=cutoff, isDistData=True)
```
5. Display Clustering Results
```python
print(f"Total Clusters Formed: {len(clusters)}\n")
for i, cluster in enumerate(clusters):
    centroid_idx = cluster[0]  # The first element in each tuple is the cluster centroid/representative
    member_indices = cluster
    print(f"Cluster {i+1} (Size: {len(member_indices)}):")
    print(f"  - Representative Molecule Index: {centroid_idx}")
    print(f"  - All Member Indices: {member_indices}\n")
```
## 4. Understanding Butina Output
The output of **Butina.ClusterData()**cluster of tuples:
 * Each inner tuple represents a **cluster**.
 * Clusters are sorted by sfirst indexng order (largest cluster first).
Cluster Centroid** inside each cluster tuple is the **Cluster Centroid** (the most representative molecule with the highest number of neighbors within the cutoff).
## 5. Quick Reference Summary

| Concept | Description | RDKit Function / Tool |
| :--- | :--- | :--- |
| Tanimoto Distance | 1.0 - \text{TanimotoSimilarity} | DataStructs.BulkTanimotoSimilarity() |
| Butina Clustering | Fast, threshold-based clustering for chemical fingerprints. | rdkit.ML.Cluster.Butina.ClusterData() |
| Cluster Centroid | Index 0 of each generated cluster tuple. | cluster[0] |

```
