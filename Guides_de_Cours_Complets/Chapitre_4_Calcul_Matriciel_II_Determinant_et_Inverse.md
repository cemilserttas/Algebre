# 📘 Chapitre 4 : Calcul Matriciel II — Déterminant et Inverse
### *Cours d'Algèbre Linéaire et Calcul Matriciel — Business Analyst (ICHEC)*

---

## 🧒 1. L'Intuition pour un Enfant : Le Facteur de Zoom et le Bouton "Annuler"

### A. Le Déterminant : La Loupe Volumique
Imagine que tu dessines un petit bonhomme en pâte à modeler sur une table. Tu lui fais subir une transformation magique (une matrice $A$).
- Le **déterminant**, c'est le **facteur multiplicateur de volume** de ta transformation !
- Si $\det(A) = 3$, ton bonhomme devient **3 fois plus volumineux**.
- Si $\det(A) = 1$, le volume ne change pas (par exemple une simple rotation).
- Si $\det(A) < 0$, le bonhomme a été retourné comme dans un miroir (inversion d'orientation).
- **Et si $\det(A) = 0$ ?**
  C'est la catastrophe : la machine a **complètement écrasé ton bonhomme en une galette plate ou en un point** ! Le volume devient strictement nul ($0$).

---

### B. L'Inverse : Le Bouton "Retour en Arrière" (Ctrl+Z)
Si tu as simplement étiré ou tourné ton bonhomme ($\det A \neq 0$), tu peux facilement faire la transformation inverse pour lui redonner sa forme exacte d'origine. C'est la **matrice inverse $A^{-1}$**.
Mais si tu as **écrasé un objet 3D en une feuille plate 2D** ($\det A = 0$), tu as perdu de la hauteur à jamais ! Impossible de savoir si la pâte écrasée venait de la tête, du ventre ou des jambes.
**Il est impossible de faire marche arrière quand on a écrasé l'espace !**
C'est pour cela qu'une matrice est **inversible si et seulement si son déterminant est différent de zéro** !

```mermaid
graph TD
    DET["DÉTERMINANT : det(A)"]
    DET -->|det(A) ≠ 0| INV["Matrice INVERSIBLE (Régulière)<br>L'espace n'est pas écrasé.<br>Il existe une unique matrice inverse A⁻¹.<br>Le système AX = B a une solution unique."]
    DET -->|det(A) = 0| NON_INV["Matrice NON INVERSIBLE (Singulière)<br>L'espace est aplati / comprimé.<br>Impossible de revenir en arrière.<br>Aucune inverse n'existe."]
    style INV fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    style NON_INV fill:#ffebee,stroke:#c62828,stroke-width:2px;
```

---

## 🔑 2. Qu'est-ce qu'une Matrice Inversible ?

### A. Définition Formelle
Une matrice carrée $A \in \mathbb{R}^{n \times n}$ est dite **inversible** s'il existe une matrice $B \in \mathbb{R}^{n \times n}$ telle que :
$$
A B = B A = I_n
$$
Cette matrice $B$ est unique et est notée **$A^{-1}$** (la matrice inverse de $A$).

### B. Propriétés Remarquables de l'Inverse
Pour toutes matrices inversibles $A$ et $B$ de même dimension :
1. **L'inverse de l'inverse redonne la matrice :**
   $$(A^{-1})^{-1} = A$$
2. **L'inverse de la transposée est la transposée de l'inverse :**
   $$(A^\top)^{-1} = (A^{-1})^\top$$
3. **LA RÈGLE PIÈGE : L'inverse d'un produit :**
   $$\mathbf{(AB)^{-1} = B^{-1} A^{-1}}$$
   *(Comme pour les chaussures et les chaussettes : pour défaire l'enchaînement "chaussettes puis chaussures", on retire d'abord les chaussures ($B^{-1}$), puis les chaussettes ($A^{-1}$) !)*

---

## 🧮 3. Le Calcul du Déterminant

Le déterminant ne se calcule **QUE pour des matrices carrées** ($n \times n$).

### A. En Dimension $2 \times 2$ (Le Produit en Croix)
Pour une matrice $A = \begin{pmatrix} a & b \\ c & d \end{pmatrix}$, le déterminant est :
$$
\det(A) = \begin{vmatrix} a & b \\ c & d \end{vmatrix} = a d - b c
$$
*(Diagonale descendante MOINS diagonale montante).*

#### Exemple :
$$
\det \begin{pmatrix} 3 & -2 \\ 4 & 1 \end{pmatrix} = (3 \times 1) - (-2 \times 4) = 3 - (-8) = \mathbf{11}
$$

---

### B. En Dimension $3 \times 3$ : Méthode 1 — La Règle de Sarrus

La règle de Sarrus est une méthode visuelle rapide réservée aux matrices $3 \times 3$.
Soit $A = \begin{pmatrix} a & b & c \\ d & e & f \\ g & h & i \end{pmatrix}$.
On recopie les deux premières colonnes à droite du tableau :
$$
\begin{matrix}
a & b & c & \mathbf{a} & \mathbf{b} \\
d & e & f & \mathbf{d} & \mathbf{e} \\
g & h & i & \mathbf{g} & \mathbf{h}
\end{matrix}
$$
On additionne les trois diagonales descendantes et on soustrait les trois diagonales montantes :
$$
\det(A) = (aei + bfg + cdh) - (gec + hfa + idb)
$$

#### Exemple Numérique Complet (Slide 16) :
Calculons le déterminant de $A = \begin{pmatrix} 2 & -1 & 3 \\ 3 & -1 & -2 \\ -1 & 0 & 4 \end{pmatrix}$ par Sarrus :
Recopions les deux premières colonnes :
$$
\begin{matrix}
2 & -1 & 3 & \mathbf{2} & \mathbf{-1} \\
3 & -1 & -2 & \mathbf{3} & \mathbf{-1} \\
-1 & 0 & 4 & \mathbf{-1} & \mathbf{0}
\end{matrix}
$$
- **Diagonales descendantes ($+$) :**
  - $2 \times (-1) \times 4 = -8$
  - $(-1) \times (-2) \times (-1) = -2$
  - $3 \times 3 \times 0 = 0$
  - Somme = $(-8) + (-2) + 0 = \mathbf{-10}$
- **Diagonales montantes ($-$) :**
  - $(-1) \times (-1) \times 3 = 3$
  - $0 \times (-2) \times 2 = 0$
  - $4 \times 3 \times (-1) = -12$
  - Somme = $3 + 0 + (-12) = \mathbf{-9}$

Résultat final :
$$
\det(A) = (-10) - (-9) = -10 + 9 = \mathbf{-1}
$$

> [!CAUTION]
> **LE PIÈGE ABSOLU À L'EXAMEN :**
> La règle de Sarrus **NE FONCTIONNE QU'EN DIMENSION $3 \times 3$** !
> L'appliquer à une matrice $4 \times 4$ est une erreur mathématique grave qui donne 0 point. En dimension $\ge 4$, il faut obligatoirement utiliser le développement de Laplace !

---

### C. En Dimension Quelconque : Méthode 2 — Le Développement de Laplace

On peut développer le déterminant suivant **n'importe quelle ligne** ou **n'importe quelle colonne**.

#### 1. Le Damier des Signes (La Grille) :
Chaque position $(i, j)$ possède un signe dicté par $(-1)^{i+j}$ :
$$
\begin{pmatrix}
+ & - & + & - & \cdots \\
- & + & - & + & \cdots \\
+ & - & + & - & \cdots \\
\vdots & \vdots & \vdots & \ddots & \vdots
\end{pmatrix}
$$

#### 2. Principe du Développement :
1. On choisit une ligne (ou une colonne).
2. Pour chaque coefficient $a_{ij}$ de cette ligne, on calcule le sous-déterminant d'ordre inférieur obtenu en **rayant la ligne $i$ et la colonne $j$**.
3. On multiplie ce sous-déterminant par le coefficient $a_{ij}$ et par son signe dans le damier.
4. On additionne le tout.

#### Exemple de calcul par la 1ère ligne (Slide 13) :
Reprenons $A = \begin{pmatrix} 2 & -1 & 3 \\ 3 & -1 & -2 \\ -1 & 0 & 4 \end{pmatrix}$. Développons selon la ligne 1 :
$$
\det(A) = +(2) \begin{vmatrix} -1 & -2 \\ 0 & 4 \end{vmatrix} - (-1) \begin{vmatrix} 3 & -2 \\ -1 & 4 \end{vmatrix} + (3) \begin{vmatrix} 3 & -1 \\ -1 & 0 \end{vmatrix}
$$
Calculons chaque sous-déterminant $2 \times 2$ :
- $\begin{vmatrix} -1 & -2 \\ 0 & 4 \end{vmatrix} = (-1 \times 4) - (-2 \times 0) = -4$
- $\begin{vmatrix} 3 & -2 \\ -1 & 4 \end{vmatrix} = (3 \times 4) - (-2 \times -1) = 12 - 2 = 10$
- $\begin{vmatrix} 3 & -1 \\ -1 & 0 \end{vmatrix} = (3 \times 0) - (-1 \times -1) = 0 - 1 = -1$

Assemblons :
$$
\det(A) = 2(-4) + 1(10) + 3(-1) = -8 + 10 - 3 = \mathbf{-1}
$$
On retrouve exactement le même résultat qu'avec Sarrus !

> [!TIP]
> **Conseil d'Expert pour l'Examen :**
> Choisis TOUJOURS la ligne ou la colonne qui contient **le plus grand nombre de zéros** ! Chaque zéro annule complètement un sous-déterminant à calculer, ce qui divise ton temps de calcul par deux !

---

### D. En Dimension $4 \times 4$ (Slide 17)
Pour calculer le déterminant d'une matrice $4 \times 4$, on développe suivant une ligne ou colonne pour se ramener à une somme de quatre déterminants $3 \times 3$, que l'on calcule ensuite par Sarrus ou Laplace.

---

### E. Les Propriétés Magiques du Déterminant
Soient $A, B \in \mathbb{R}^{n \times n}$ et $\lambda \in \mathbb{R}$ :
1. **Transposition :** $\det(A^\top) = \det(A)$.
2. **Produit :** $\det(AB) = \det(A) \times \det(B)$.
3. **Inverse :** $\det(A^{-1}) = \frac{1}{\det(A)}$.
4. **Multiplication par un scalaire (PIÈGE FRÉQUENT) :**
   $$\mathbf{\det(\lambda A) = \lambda^n \det(A)}$$
   *(Car le facteur $\lambda$ multiplie chacune des $n$ lignes de la matrice !)*
5. **Matrice Triangulaire ou Diagonale :** Le déterminant est tout simplement le **produit des éléments de la diagonale principale** :
   $$\det(A) = a_{11} \times a_{22} \times \dots \times a_{nn}$$
6. **Ligne ou colonne de zéros :** $\det(A) = 0$.
7. **Deux lignes (ou colonnes) proportionnelles ou identiques :** $\det(A) = 0$.

---

## 🛠️ 4. Inverser une Matrice : Méthode 1 — La Formule de la Comatrice

C'est la méthode théorique exacte par excellence.

### A. Les Cofacteurs et la Matrice des Cofacteurs
Pour une matrice carrée $A = (a_{ij}) \in \mathbb{R}^{n \times n}$, le **cofacteur** $c_{ij}$ est le sous-déterminant obtenu en supprimant la ligne $i$ et la colonne $j$, affublé de son signe dans le damier :
$$
c_{ij} = (-1)^{i+j} \det(M_{ij})
$$
La matrice de tous les cofacteurs s'appelle la **Comatrice**, notée $\operatorname{Cof}(A)$.

### B. La Formule d'Inversion Suprême
Si $\det(A) \neq 0$, alors la matrice inverse est donnée par :
$$
\mathbf{A^{-1} = \frac{1}{\det(A)} \left( \operatorname{Cof}(A) \right)^\top}
$$

> [!CAUTION]
> **LE PIÈGE N°1 DES ÉTUDIANTS :**
> N'oublie JAMAIS de **transposer** la matrice des cofacteurs avant de la diviser par le déterminant !

---

### C. Exemple d'Inversion en Dimension $2 \times 2$ (Slide 23)
Soit $A = \begin{pmatrix} 3 & 1 \\ -2 & 1 \end{pmatrix}$.
1. **Déterminant :** $\det(A) = (3)(1) - (1)(-2) = 3 + 2 = \mathbf{5} \neq 0$ (inversible).
2. **Comatrice :**
   - $c_{11} = +1$
   - $c_{12} = -(-2) = 2$
   - $c_{21} = -(1) = -1$
   - $c_{22} = +3$
   $$\operatorname{Cof}(A) = \begin{pmatrix} 1 & 2 \\ -1 & 3 \end{pmatrix}$$
3. **Transposition de la comatrice :**
   $$\left( \operatorname{Cof}(A) \right)^\top = \begin{pmatrix} 1 & -1 \\ 2 & 3 \end{pmatrix}$$
4. **Matrice inverse :**
   $$A^{-1} = \frac{1}{5} \begin{pmatrix} 1 & -1 \\ 2 & 3 \end{pmatrix} = \begin{pmatrix} 1/5 & -1/5 \\ 2/5 & 3/5 \end{pmatrix}$$

---

### D. Exemple Complet d'Inversion en Dimension $3 \times 3$ (Slide 24)
Reprenons $A = \begin{pmatrix} 2 & -1 & 3 \\ 3 & -1 & -2 \\ -1 & 0 & 4 \end{pmatrix}$ dont on sait déjà que $\det(A) = -1$.
Calculons les 9 cofacteurs :
- **Ligne 1 :**
  - $c_{11} = + \begin{vmatrix} -1 & -2 \\ 0 & 4 \end{vmatrix} = -4$
  - $c_{12} = - \begin{vmatrix} 3 & -2 \\ -1 & 4 \end{vmatrix} = -(12 - 2) = -10$
  - $c_{13} = + \begin{vmatrix} 3 & -1 \\ -1 & 0 \end{vmatrix} = 0 - 1 = -1$
- **Ligne 2 :**
  - $c_{21} = - \begin{vmatrix} -1 & 3 \\ 0 & 4 \end{vmatrix} = -(-4 - 0) = 4$
  - $c_{22} = + \begin{vmatrix} 2 & 3 \\ -1 & 4 \end{vmatrix} = 8 - (-3) = 11$
  - $c_{23} = - \begin{vmatrix} 2 & -1 \\ -1 & 0 \end{vmatrix} = -(0 - 1) = 1$
- **Ligne 3 :**
  - $c_{31} = + \begin{vmatrix} -1 & 3 \\ -1 & -2 \end{vmatrix} = 2 - (-3) = 5$
  - $c_{32} = - \begin{vmatrix} 2 & 3 \\ 3 & -2 \end{vmatrix} = -(-4 - 9) = 13$
  - $c_{33} = + \begin{vmatrix} 2 & -1 \\ 3 & -1 \end{vmatrix} = -2 - (-3) = 1$

Matrice des cofacteurs :
$$
\operatorname{Cof}(A) = \begin{pmatrix} -4 & -10 & -1 \\ 4 & 11 & 1 \\ 5 & 13 & 1 \end{pmatrix}
$$
Transposons cette comatrice :
$$
\left( \operatorname{Cof}(A) \right)^\top = \begin{pmatrix} -4 & 4 & 5 \\ -10 & 11 & 13 \\ -1 & 1 & 1 \end{pmatrix}
$$
Divisons par $\det(A) = -1$ :
$$
A^{-1} = \frac{1}{-1} \begin{pmatrix} -4 & 4 & 5 \\ -10 & 11 & 13 \\ -1 & 1 & 1 \end{pmatrix} = \begin{pmatrix} 4 & -4 & -5 \\ 10 & -11 & -13 \\ 1 & -1 & -1 \end{pmatrix}
$$

*Vérification éclair :* Multiplions la ligne 1 de $A$ par la colonne 1 de $A^{-1}$ :
$2(4) + (-1)(10) + 3(1) = 8 - 10 + 3 = 1$. C'est parfaitement juste !

---

## ⚡ 5. Inverser une Matrice : Méthode 2 — Le Pivot de Gauss-Jordan

### A. Principe de la Matrice Augmentée
On colle la matrice $A$ et la matrice identité $I_n$ côte à côte pour former la **matrice augmentée** :
$$
\left( A \;\middle|\; I_n \right)
$$
On effectue ensuite des **opérations élémentaires sur les lignes** appliquées simultanément des deux côtés, avec pour objectif de **transformer la partie gauche en $I_n$** :
$$
\left( A \;\middle|\; I_n \right) \xrightarrow{\quad \text{Opérations sur les lignes} \quad} \left( I_n \;\middle|\; A^{-1} \right)
$$
Une fois la partie gauche devenue l'identité, **la partie droite est automatiquement devenue $A^{-1}$** !

> [!NOTE]
> Les 3 opérations élémentaires autorisées sur les lignes sont :
> 1. Échanger deux lignes : $L_i \leftrightarrow L_j$
> 2. Multiplier une ligne par un réel non nul : $L_i \leftarrow \alpha L_i$ ($\alpha \neq 0$)
> 3. Ajouter à une ligne un multiple d'une autre : $L_i \leftarrow L_i + \beta L_j$

---

### B. Exemple Pas à Pas du Pivot de Gauss (Slide 28)
Inversons $A = \begin{pmatrix} 1 & 2 \\ 3 & 5 \end{pmatrix}$.

1. **Écriture de la matrice augmentée :**
   $$
   \left(\begin{array}{cc|cc} 1 & 2 & 1 & 0 \\ 3 & 5 & 0 & 1 \end{array}\right)
   $$
2. **Élimination sous le premier pivot ($L_2 \leftarrow L_2 - 3L_1$) :**
   - Ligne 2 : $(3 - 3\times 1 \quad 5 - 3\times 2 \quad\mid\quad 0 - 3\times 1 \quad 1 - 3\times 0) = (0 \quad -1 \quad\mid\quad -3 \quad 1)$
   $$
   \left(\begin{array}{cc|cc} 1 & 2 & 1 & 0 \\ 0 & -1 & -3 & 1 \end{array}\right)
   $$
3. **Rendre le pivot unitaire ($L_2 \leftarrow -L_2$) :**
   $$
   \left(\begin{array}{cc|cc} 1 & 2 & 1 & 0 \\ 0 & 1 & 3 & -1 \end{array}\right)
   $$
4. **Élimination au-dessus du pivot ($L_1 \leftarrow L_1 - 2L_2$) :**
   - Ligne 1 : $(1 - 2\times 0 \quad 2 - 2\times 1 \quad\mid\quad 1 - 2\times 3 \quad 0 - 2\times(-1)) = (1 \quad 0 \quad\mid\quad -5 \quad 2)$
   $$
   \left(\begin{array}{cc|cc} \mathbf{1} & \mathbf{0} & -5 & 2 \\ \mathbf{0} & \mathbf{1} & 3 & -1 \end{array}\right)
   $$

La partie gauche est devenue $I_2$ ! On lit immédiatement la réponse à droite :
$$
A^{-1} = \begin{pmatrix} -5 & 2 \\ 3 & -1 \end{pmatrix}
$$

---

## ⚖️ 6. Quand Choisir la Comatrice ou le Pivot de Gauss ?

| Méthode | Avantages | Inconvénients | Quand l'utiliser ? |
| :--- | :--- | :--- | :--- |
| **Comatrice** | - Formule directe et mécanique.<br>- Très rapide en dimension $2 \times 2$.<br>- Ne risque pas de propager une erreur de ligne. | - Très long en dimension $\ge 3$ (il faut calculer 9 ou 16 déterminants !). | **Recommandée en $2 \times 2$** ou si la matrice contient un paramètre littéral ($m$ ou $k$). |
| **Pivot de Gauss** | - Beaucoup plus rapide pour les matrices $3 \times 3$ ou $4 \times 4$.<br>- Algorithme universel. | - Une petite erreur d'inattention au début fausse toute la suite. | **Recommandée en $3 \times 3$ et plus** avec des valeurs numériques claires. |

---

## 🗣️ 7. Le Guide de l'Oral : Comment décrire ces calculs à l'examen

- **Pour justifier l'inversibilité d'une matrice :**
  > *"Je commence par calculer le déterminant de la matrice carrée $A$. Puisque $\det(A) \neq 0$, la transformation ne compresse pas l'espace vers une dimension inférieure : la matrice est régulière, donc inversible."*
- **Pour décrire le développement de Laplace :**
  > *"Je développe le déterminant suivant la ligne (ou colonne) contenant le plus de zéros. Pour chaque élément, je multiplie sa valeur par son signe dans le damier $(-1)^{i+j}$ et par le mineur obtenu en éliminant sa ligne et sa colonne."*
- **Pour décrire le calcul de l'inverse par la comatrice :**
  > *"Je construis la matrice des cofacteurs en calculant les sous-déterminants signés. J'applique ensuite la formule d'inversion en prenant la transposée de cette comatrice et en divisant chaque coefficient par le déterminant de $A$."*

---

## 📌 8. À Retenir Absolument (Points Clés du Chapitre 4)

1. $A$ est inversible $\iff \det(A) \neq 0$.
2. En $2 \times 2$ : $\det \begin{pmatrix} a & b \\ c & d \end{pmatrix} = ad - bc$.
3. En $3 \times 3$ : Sarrus (attention, **interdit en dimension $\ge 4$** !).
4. Laplace : choix de la ligne/colonne avec le maximum de zéros.
5. $\det(AB) = \det(A) \det(B)$ et $\det(\lambda A) = \lambda^n \det(A)$.
6. Inversion par comatrice : $A^{-1} = \frac{1}{\det(A)} (\operatorname{Cof} A)^\top$ (attention à la transposée !).
7. Inversion par Gauss : $(A \mid I_n) \to (I_n \mid A^{-1})$.
8. $(AB)^{-1} = B^{-1} A^{-1}$.

---

➡️ **Passer au chapitre suivant :** [Chapitre 5 : Applications du Calcul Matriciel](file:///c:/Users/cemmi/OneDrive%20-%20ICHEC/Math/Alg%C3%A8bre/Guides_de_Cours_Complets/Chapitre_5_Applications_du_Calcul_Matriciel.md)
