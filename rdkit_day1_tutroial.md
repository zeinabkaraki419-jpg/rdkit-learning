
## What is a SMILES String?
SMILES stands for Simplified Molecular Input Line Entry System. It is a way to represent a 3D chemical structure using simple line text (letters, numbers, and symbols).
## 5 Basic Rules of SMILES Strings
### 1. Atoms (Letters)
 * Capital letters represent standard atoms: C (Carbon), O (Oxygen), N (Nitrogen), S (Sulfur), P (Phosphorus).
 * Two-letter elements keep their standard periodic table casing: Cl (Chlorine), Br (Bromine), Na (Sodium).
 * Lowercase letters represent atoms in aromatic rings (like benzene): c, n, o.
 * Hydrogen atoms are usually implicit (RDKit automatically calculates how many hydrogens are attached based on valency).
### 2. Bonds (Symbols)
If no symbol is written between two atoms, a single bond is assumed.

| Symbol | Bond Type | Example | Result |
| :--- | :--- | :--- | :--- |
| *(none)* | Single Bond | CC | Ethane (CH_3-CH_3) |
| = | Double Bond | C=C | Ethene (CH_2=CH_2) |
| # | Triple Bond | C#C | Ethyne ( ($CH \equiv CH$). ) |
| : | Aromatic Bond | c1ccccc1 | Benzene ring |

### 3. Branches (Parentheses ())
Branches in a molecular chain are enclosed in round brackets (). The atom inside the bracket is attached to the atom right before the bracket.
 * Example 1: CC(=O)O (Acetic Acid)
   * The =O is inside parentheses, meaning Oxygen has a double bond to the middle Carbon.
 * Example 2: CC(C)C (Isobutane)
   * A central Carbon connected to three Methyl groups.
### 4. Rings (Numbers)
Rings are written by breaking one bond in the loop and labeling both ends with the same number.
 * Example (Cyclohexane): C1CCCCC1
   * The 1 after the first C and the 1 after the last C tell RDKit that these two carbons are connected to close the ring.
 * Example (Benzene): c:1:c:c:c:c:c1
   * Lowercase c means aromatic ring, and the 1s close the 6-carbon loop.
### 5. Charges & Explicit Hydrogens (Square Brackets [])
When an atom has a charge or specific isotopic/stereochemical rules, it is enclosed in square brackets [].
 * [Na+] ->Sodium ion with a +1 charge.
 * [OH-] -> Hydroxide ion with a -1 charge.
 * [nH]  -> Nitrogen in an aromatic ring with an attached Hydrogen.
## Quick Examples Summary
# Simple linear chain (Ethanol)
ethanol = "CCO"
# Molecule with a branch and double bond (Acetone)
acetone = "CC(=O)C"
# Ring structure (Cyclopropane)
cyclopropane = "C1CC1"
# Aromatic Ring (Benzene)
benzene = "c1ccccc1"
# Charged compound (Sodium Acetate)
sodium_acetate = "CC(=O)[O-].[Na+]"  # '.' separates disconnected molecules/ions
