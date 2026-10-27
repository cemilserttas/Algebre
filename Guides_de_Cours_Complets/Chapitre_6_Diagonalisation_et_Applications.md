# 📘 Chapitre 6 : Diagonalisation et Systèmes Dynamiques
### *Cours d'Algèbre Linéaire et Calcul Matriciel — Business Analyst (ICHEC)*

---

## 🧒 1. L'Intuition pour un Enfant : Le Trampoline et la Girouette

### A. Le Problème du Trampoline qui Mélange Tout
Imagine que tu marches sur un trampoline très élastique.
- Si tu fais un pas vers l'avant, le tissu s'enfonce de travers et te pousse à la fois vers l'avant **et** vers la droite !
- Les deux directions ($x$ et $y$) sont **mélangées**. C'est ce que fait une matrice ordinaire avec plein de chiffres : elle tord l'espace, mélange toutes les variables, et rend les calculs compliqués.

### B. Les Directions Magiques (Les Vecteurs Propres)
Mais si tu cherches bien sur le trampoline, il existe deux lignes droites très spéciales :
- Si tu marches exactement le long de la **ligne magique 1**, le trampoline s'enfonce parfaitement droit sans dévier ! Tu avances tout droit, simplement plus vite (tu es multiplié par un facteur $\lambda_1$).
- Si tu marches le long de la **ligne magique 2**, tu avances aussi parfaitement droit, multiplié par un facteur $\lambda_2$.

Ces lignes droites magiques sont les **directions propres (vecteurs propres)**, et les facteurs d'agrandissement $\lambda$ sont les **valeurs propres** !
Si nous décidons de tracer notre carte du monde en utilisant ces deux lignes magiques comme axes de référence, **la matrice devient DIAGONALE** !

```mermaid
graph TD
    BASE_COMPLEXE["Base Canonique (Axes classiques)<br>La matrice A est pleine de chiffres.<br>Les variables se mélangent à chaque étape.<br>Calculer A¹⁰⁰ est quasi impossible à la main."] 
    -->|Changement de Base vers les Vecteurs Propres| 
    BASE_DIAGONALE["Base de Vecteurs Propres<br>La matrice D est DIAGONALE.<br>Chaque variable évolue toute seule dans son couloir.<br>Calculer D¹⁰⁰ prend 2 secondes : λ¹⁰⁰ !"]
    style BASE_COMPLEXE fill:#ffebee,stroke:#c62828,stroke-width:2px;
    style BASE_DIAGONALE fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
```

---

## 🧭 2. Valeurs Propres et Vecteurs Propres

### A. Définition Formelle d'un Vecteur Propre
Soit $A \in \mathbb{R}^{n \times n}$ une matrice carrée.
Un vecteur **non nul** $\mathbf{v} \in \mathbb{R}^n$ ($\mathbf{v} \neq \mathbf{0}$) est un **vecteur propre** de $A$ s'il existe un nombre réel $\lambda \in \mathbb{R}$ tel que :
$$
\mathbf{A \mathbf{v} = \lambda \mathbf{v}}
$$
*Autrement dit :* Lorsqu'on applique la matrice $A$ au vecteur $\mathbf{v}$, le résultat est un simple multiple scalaire de $\mathbf{v}$ (il reste sur la même droite, sans tourner !).

> [!CAUTION]
> **LE VECTEUR NUL $\mathbf{0}$ N'EST JAMAIS UN VECTEUR PROPRE !**
> Même s'il vérifie toujours $A\mathbf{0} = \lambda \mathbf{0}$ pour n'importe quel $\lambda$, on l'exclut par définition car il n'indique aucune direction dans l'espace.

---

### B. Définition Formelle d'une Valeur Propre
Le nombre réel $\lambda \in \mathbb{R}$ est appelé la **valeur propre** associée au vecteur propre $\mathbf{v}$.

> [!NOTE]
> Le nombre **$\lambda = 0$ PEUT être une valeur propre** !
> Si $\lambda = 0$, alors $A\mathbf{v} = 0\mathbf{v} = \mathbf{0}$. Cela signifie simplement que $\mathbf{v} \in \ker(A)$, c'est-à-dire que le noyau n'est pas réduit à $\{0\}$, et donc que la matrice $A$ n'est pas inversible.

#### Exemples Rapides de Reconnaissance (Slide 10) :
- Soit $A = \begin{pmatrix} -1 & 2 \\ 1 & 0 \end{pmatrix}$ et $\mathbf{v} = \begin{pmatrix} 2 \\ -1 \end{pmatrix}$.
  Calculons :
  $$A\mathbf{v} = \begin{pmatrix} -1 & 2 \\ 1 & 0 \end{pmatrix} \begin{pmatrix} 2 \\ -1 \end{pmatrix} = \begin{pmatrix} -2 - 2 \\ 2 + 0 \end{pmatrix} = \begin{pmatrix} -4 \\ 2 \end{pmatrix} = \mathbf{-2 \begin{pmatrix} 2 \\ -1 \end{pmatrix}} = -2\mathbf{v}$$
  Puisque $A\mathbf{v} = -2\mathbf{v}$, **$\mathbf{v}$ est un vecteur propre** de $A$ associé à la valeur propre $\lambda = -2$.
- Soit $A = \begin{pmatrix} 3 & 1 \\ -1 & 2 \end{pmatrix}$ et $\mathbf{v} = \begin{pmatrix} 1 \\ 3 \end{pmatrix}$.
  $$A\mathbf{v} = \begin{pmatrix} 3 & 1 \\ -1 & 2 \end{pmatrix} \begin{pmatrix} 1 \\ 3 \end{pmatrix} = \begin{pmatrix} 3 + 3 \\ -1 + 6 \end{pmatrix} = \begin{pmatrix} 6 \\ 5 \end{pmatrix}$$
  Puisque $\begin{pmatrix} 6 \\ 5 \end{pmatrix}$ n'est pas colinéaire à $\begin{pmatrix} 1 \\ 3 \end{pmatrix}$ (car $6/1 \neq 5/3$), **$\mathbf{v}$ n'est pas un vecteur propre** de $A$.

---

## 🧮 3. Comment Trouver les Valeurs Propres ? Le Polynôme Caractéristique

### A. Le Critère du Déterminant Nul
Transformons l'équation fondamentale :
$$
A\mathbf{v} = \lambda \mathbf{v} \iff A\mathbf{v} - \lambda I_n \mathbf{v} = \mathbf{0} \iff \mathbf{(A - \lambda I_n)\mathbf{v} = \mathbf{0}}
$$
C'est un système d'équations linéaires homogène.
Nous cherchons des solutions $\mathbf{v} \neq \mathbf{0}$ (non triviales). Un tel système admet des solutions non nulles **si et seulement si la matrice $(A - \lambda I_n)$ n'est pas inversible**, c'est-à-dire :
$$
\mathbf{\det(A - \lambda I_n) = 0}
$$

### B. Le Polynôme Caractéristique
Le polynôme défini par :
$$
\mathbf{p_A(\lambda) = \det(A - \lambda I_n)}
$$
est appelé le **polynôme caractéristique** de $A$.
Les valeurs propres de $A$ sont **exactement les racines réelles** de ce polynôme !

---

### C. Exemple de Calcul Pas à Pas en Dimension $3 \times 3$ (Slide 14)
Soit $A = \begin{pmatrix} 1 & 0 & 1 \\ -1 & 3 & 0 \\ 3 & -3 & 2 \end{pmatrix}$.
Soustrayons $\lambda$ sur toute la diagonale principale :
$$
A - \lambda I_3 = \begin{pmatrix} 1 - \lambda & 0 & 1 \\ -1 & 3 - \lambda & 0 \\ 3 & -3 & 2 - \lambda \end{pmatrix}
$$
Développons le déterminant selon la 1ère ligne (qui contient un zéro très pratique) :
$$
p_A(\lambda) = (1 - \lambda) \begin{vmatrix} 3 - \lambda & 0 \\ -3 & 2 - \lambda \end{vmatrix} - 0 + 1 \begin{vmatrix} -1 & 3 - \lambda \\ 3 & -3 \end{vmatrix}
$$
Calculons les deux mineurs $2 \times 2$ :
- 1er mineur : $(3 - \lambda)(2 - \lambda) - 0 = \lambda^2 - 5\lambda + 6$.
- 2ème mineur : $(-1 \times -3) - 3(3 - \lambda) = 3 - 9 + 3\lambda = 3\lambda - 6$.
Remplaçons :
$$
p_A(\lambda) = (1 - \lambda)(\lambda^2 - 5\lambda + 6) + (3\lambda - 6)
$$
Factorisons par $(3 - \lambda)$ ou développons tout :
$$
p_A(\lambda) = (\lambda^2 - 5\lambda + 6 - \lambda^3 + 5\lambda^2 - 6\lambda) + (3\lambda - 6) = -\lambda^3 + 6\lambda^2 - 8\lambda
$$
Factorisons immédiatement par $-\lambda$ :
$$
p_A(\lambda) = -\lambda(\lambda^2 - 6\lambda + 8) = \mathbf{-\lambda(\lambda - 2)(\lambda - 4)}
$$
Les racines sont donc :
$$
\mathbf{\lambda_1 = 0, \quad \lambda_2 = 2, \quad \lambda_3 = 4}
$$
Ce sont les 3 valeurs propres de la matrice $A$ !

---

### D. Multiplicité Algébrique ($m_a$) et Sous-Espaces Propres ($E_\lambda$)

- **Multiplicité algébrique $m_a(\lambda)$ :** C'est le nombre de fois que la racine $\lambda$ apparaît dans la factorisation du polynôme caractéristique (sa puissance).
  - Si $( \lambda - 3 )^1 \implies m_a(3) = 1$ (valeur propre simple).
  - Si $( \lambda - 3 )^2 \implies m_a(3) = 2$ (valeur propre double).
- **Le Sous-Espace Propre $E_\lambda$ :** C'est l'ensemble de tous les vecteurs propres associés à $\lambda$, plus le vecteur nul :
  $$
  E_\lambda = \ker(A - \lambda I_n) = \{ \mathbf{v} \in \mathbb{R}^n \mid (A - \lambda I_n)\mathbf{v} = \mathbf{0} \}
  $$
- **Multiplicité géométrique $m_g(\lambda)$ :** C'est la dimension du sous-espace propre :
  $$m_g(\lambda) = \dim(E_\lambda)$$
- **La Règle d'Encadrement Universelle :**
  Pour toute valeur propre réelle $\lambda$ :
  $$
  \mathbf{1 \le \dim(E_\lambda) \le m_a(\lambda)}
  $$
  *(La dimension géométrique vaut au minimum 1, et ne peut jamais dépasser la multiplicité algébrique).*

---

## 🏛️ 4. Les 4 Cas Possibles de Diagonalisation

Comment savoir si une matrice $A \in \mathbb{R}^{n \times n}$ est diagonalisable ?
Il faut passer par **l'arbre de décision des 4 cas officiels du cours** :

```mermaid
flowchart TD
    P["Calcul de p_A(λ) = det(A - λ I)"] --> FACT{"Le polynôme est-il entièrement<br>factorisable dans ℝ ?"}
    FACT -->|NON (racines complexes, Δ < 0)| CAS1["CAS 1 : NON DIAGONALISABLE dans ℝ"]
    FACT -->|OUI| RAC{"Les n racines sont-elles<br>toutes DISTINCTES ?"}
    RAC -->|OUI (n valeurs propres simples)| CAS2["CAS 2 : AUTOMATIQUEMENT DIAGONALISABLE !"]
    RAC -->|NON (au moins une racine multiple)| TEST_DIM{"Pour chaque racine multiple,<br>a-t-on dim(E_λ) = m_a(λ) ?"}
    TEST_DIM -->|NON (déficit de vecteurs propres)| CAS3["CAS 3 : NON DIAGONALISABLE"]
    TEST_DIM -->|OUI (assez de vecteurs propres)| CAS4["CAS 4 : DIAGONALISABLE !"]
    style CAS1 fill:#ffebee,stroke:#c62828,stroke-width:2px;
    style CAS2 fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
    style CAS3 fill:#ffebee,stroke:#c62828,stroke-width:2px;
    style CAS4 fill:#e8f5e9,stroke:#2e7d32,stroke-width:2px;
```

---

### Étude Détaillée des 4 Cas avec les Exemples des Slides

#### 🔴 Cas 1 : $p_A(\lambda)$ non entièrement factorisable dans $\mathbb{R}$ (Slide 20)
Soit $A = \begin{pmatrix} 0 & 1 & 0 \\ 2 & 1 & -1 \\ -2 & 3 & 1 \end{pmatrix}$.
Calcul du polynôme :
$$p_A(\lambda) = -\lambda^3 + 2\lambda^2 - 2\lambda = -\lambda(\lambda^2 - 2\lambda + 2)$$
Le facteur du second degré a pour discriminant :
$$\Delta = (-2)^2 - 4(1)(2) = 4 - 8 = \mathbf{-4 < 0}$$
Le polynôme possède des racines complexes non réelles.
**Conclusion immédiate : $A$ n'est pas diagonalisable dans $\mathbb{R}$.**

---

#### 🟢 Cas 2 : $n$ valeurs propres réelles distinctes (Slide 21)
Soit $A = \begin{pmatrix} 2 & 1 \\ 8 & -5 \end{pmatrix} \in \mathbb{R}^{2 \times 2}$.
$$
p_A(\lambda) = \det \begin{pmatrix} 2 - \lambda & 1 \\ 8 & -5 - \lambda \end{pmatrix} = (2 - \lambda)(-5 - \lambda) - 8 = \lambda^2 + 3\lambda - 18 = (\lambda - 3)(\lambda + 6)
$$
Les racines sont $\lambda_1 = 3$ et $\lambda_2 = -6$.
Ce sont **2 racines réelles distinctes** pour une matrice d'ordre 2 ($n = 2$).
**Conclusion immédiate : $A$ est diagonalisable !** (Pas besoin de vérifier les dimensions, chaque $m_a = 1 \implies m_g = 1$).

---

#### 🔴 Cas 3 : Racine multiple avec déficit de vecteurs propres (Slides 23-24)
Soit $A = \begin{pmatrix} 1 & -2 \\ 2 & 5 \end{pmatrix}$.
$$
p_A(\lambda) = \det \begin{pmatrix} 1 - \lambda & -2 \\ 2 & 5 - \lambda \end{pmatrix} = (1 - \lambda)(5 - \lambda) + 4 = \lambda^2 - 6\lambda + 9 = \mathbf{(\lambda - 3)^2}
$$
La valeur propre $\lambda = 3$ est **double** ($m_a(3) = 2$).
Calculons son sous-espace propre $E_3 = \ker(A - 3I_2)$ :
$$
(A - 3I_2)\mathbf{v} = \mathbf{0} \iff \begin{pmatrix} 1-3 & -2 \\ 2 & 5-3 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} \iff \begin{pmatrix} -2 & -2 \\ 2 & 2 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix}
$$
Ce qui donne l'unique équation : $-2x - 2y = 0 \iff y = -x$.
Les vecteurs s'écrivent :
$$
\begin{pmatrix} x \\ -x \end{pmatrix} = x \begin{pmatrix} 1 \\ -1 \end{pmatrix} \implies E_3 = \operatorname{Vect}\left( \begin{pmatrix} 1 \\ -1 \end{pmatrix} \right) \implies \mathbf{\dim(E_3) = 1}
$$
Or la multiplicité algébrique était $m_a(3) = 2$.
Puisque $\dim(E_3) = 1 < 2$, **il nous manque un vecteur propre pour fabriquer une base de $\mathbb{R}^2$**.
**Conclusion : $A$ n'est pas diagonalisable !**

---

#### 🟢 Cas 4 : Racine multiple avec dimension égale (Slides 25-27)
Soit $A = \begin{pmatrix} 0 & -1 & 1 \\ -1 & 0 & -1 \\ 1 & -1 & 0 \end{pmatrix} \in \mathbb{R}^{3 \times 3}$.
Son polynôme caractéristique est :
$$
p_A(\lambda) = -(\lambda - 2)(\lambda + 1)^2
$$
- Valeur propre simple : $\lambda_1 = 2$ ($m_a = 1 \implies \dim(E_2) = 1$).
- Valeur propre double : $\lambda_2 = -1$ ($m_a = 2$).

Calculons les deux sous-espaces propres :
1. **Pour $\lambda = 2$ :**
   $$
   (A - 2I_3)\mathbf{v} = \mathbf{0} \iff \begin{pmatrix} -2 & -1 & 1 \\ -1 & -2 & -1 \\ 1 & -1 & -2 \end{pmatrix} \begin{pmatrix} x \\ y \\ z \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix}
   $$
   Par substitution : $x = z$ et $y = -z$.
   $$E_2 = \operatorname{Vect}\left( \begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix} \right) \quad (\dim = 1)$$
2. **Pour $\lambda = -1$ :**
   $$
   (A + I_3)\mathbf{v} = \mathbf{0} \iff \begin{pmatrix} 1 & -1 & 1 \\ -1 & 1 & -1 \\ 1 & -1 & 1 \end{pmatrix} \begin{pmatrix} x \\ y \\ z \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \\ 0 \end{pmatrix}
   $$
   Les 3 lignes sont identiques ! Il ne reste qu'une seule équation :
   $$x - y + z = 0 \iff \mathbf{x = y - z}$$
   Les deux variables $y$ et $z$ sont libres !
   $$
   \begin{pmatrix} y - z \\ y \\ z \end{pmatrix} = y \begin{pmatrix} 1 \\ 1 \\ 0 \end{pmatrix} + z \begin{pmatrix} -1 \\ 0 \\ 1 \end{pmatrix}
   $$
   $$E_{-1} = \operatorname{Vect}\left( \begin{pmatrix} 1 \\ 1 \\ 0 \end{pmatrix}, \begin{pmatrix} -1 \\ 0 \\ 1 \end{pmatrix} \right) \quad \implies \mathbf{\dim(E_{-1}) = 2}$$

Puisque $\dim(E_{-1}) = 2 = m_a(-1)$ et $\dim(E_2) = 1 = m_a(2)$, **la matrice $A$ est parfaitement diagonalisable** !

---

### E. L'Écriture Matricielle de la Diagonalisation : $A = P D P^{-1}$

On assemble la base de vecteurs propres $\mathcal{B} = (\mathbf{v}_1, \mathbf{v}_2, \mathbf{v}_3)$ :
$$
P = \begin{pmatrix} 1 & 1 & -1 \\ -1 & 1 & 0 \\ 1 & 0 & 1 \end{pmatrix}
$$
Dans cette nouvelle base, la matrice devient la matrice diagonale $D$ :
$$
D = \begin{pmatrix} 2 & 0 & 0 \\ 0 & -1 & 0 \\ 0 & 0 & -1 \end{pmatrix}
$$
On a alors la relation fondamentale :
$$
\mathbf{D = P^{-1} A P \iff A = P D P^{-1}}
$$

> [!WARNING]
> **Attention à l'ordre des colonnes :**
> Si tu mets le vecteur propre $\begin{pmatrix} 1 \\ -1 \\ 1 \end{pmatrix}$ en colonne 1 de $P$, tu dois obligatoirement mettre sa valeur propre ($2$) en position $(1, 1)$ sur la diagonale de $D$ !

---

## ⚡ 5. Calcul des Puissances d'une Matrice ($A^n$)

Pourquoi s'est-on donné tout ce mal pour diagonaliser ?
Regarde ce qui se passe quand on élève $A$ à la puissance $n$ :
$$
A^2 = (P D P^{-1})(P D P^{-1}) = P D \underbrace{(P^{-1} P)}_{I} D P^{-1} = P D^2 P^{-1}
$$
Par récurrence immédiate :
$$
\mathbf{A^n = P D^n P^{-1}}
$$
Et la puissance d'une matrice diagonale est un jeu d'enfant :
$$
D^n = \begin{pmatrix} \lambda_1^n & 0 & \cdots & 0 \\ 0 & \lambda_2^n & \cdots & 0 \\ \vdots & \vdots & \ddots & \vdots \\ 0 & 0 & \cdots & \lambda_p^n \end{pmatrix}
$$
On peut calculer $A^{100}$ ou $A^{1000}$ en **quelques secondes** !

---

## 📈 6. La Grande Application Business : Dynamique de Marché et Chaînes de Markov

C'est l'application économique phare du cours (Slides 29-34).

### A. Le Problème de Gestion
Deux opérateurs de téléphonie mobile, **A** et **B**, se partagent un marché total de **600 000 clients**.
Chaque année :
- Chez **A** : 90% restent fidèles, 10% partent chez B (*churn*).
- Chez **B** : 95% restent fidèles, 5% partent chez A (*churn*).
Au départ (Année 0) :
- Opérateur A : $a_0 = 350\,000$ clients.
- Opérateur B : $b_0 = 250\,000$ clients.

**Question stratégique :** Vers quelles parts de marché l'économie va-t-elle se stabiliser à long terme ($n \to \infty$) ?

---

### B. Modélisation par la Matrice de Transition
D'une année à la suivante :
$$
\begin{cases}
a_{n+1} = 0{,}90 a_n + 0{,}05 b_n \\
b_{n+1} = 0{,}10 a_n + 0{,}95 b_n
\end{cases}
\iff
\begin{pmatrix} a_{n+1} \\ b_{n+1} \end{pmatrix} = \underbrace{\begin{pmatrix} 0{,}90 & 0{,}05 \\ 0{,}10 & 0{,}95 \end{pmatrix}}_{A} \begin{pmatrix} a_n \\ b_n \end{pmatrix}
$$
Après $n$ années, la répartition est donnée par :
$$
\begin{pmatrix} a_n \\ b_n \end{pmatrix} = A^n \begin{pmatrix} a_0 \\ b_0 \end{pmatrix}
$$

---

### C. Diagonalisation Complète de la Matrice de Transition $A$

#### 1. Valeurs Propres :
$$
p_A(\lambda) = \det \begin{pmatrix} 0{,}90 - \lambda & 0{,}05 \\ 0{,}10 & 0{,}95 - \lambda \end{pmatrix} = (0{,}90 - \lambda)(0{,}95 - \lambda) - 0{,}005 = \lambda^2 - 1{,}85\lambda + 0{,}85 = \mathbf{(\lambda - 1)(\lambda - 0{,}85)}
$$
Les deux valeurs propres sont :
$$\mathbf{\lambda_1 = 1} \quad \text{et} \quad \mathbf{\lambda_2 = 0{,}85}$$

#### 2. Vecteurs Propres Associés :
- **Pour $\lambda_1 = 1$ (L'état d'équilibre stationnaire) :**
  $$(A - I_2)\mathbf{v} = \mathbf{0} \iff \begin{pmatrix} -0{,}10 & 0{,}05 \\ 0{,}10 & -0{,}05 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} \iff -0{,}10x + 0{,}05y = 0 \iff \mathbf{y = 2x}$$
  Vecteur propre : $\mathbf{v}_1 = \begin{pmatrix} 1 \\ 2 \end{pmatrix}$.
- **Pour $\lambda_2 = 0{,}85$ (Le mode transitoire amorti) :**
  $$(A - 0{,}85I_2)\mathbf{v} = \mathbf{0} \iff \begin{pmatrix} 0{,}05 & 0{,}05 \\ 0{,}10 & 0{,}10 \end{pmatrix} \begin{pmatrix} x \\ y \end{pmatrix} = \begin{pmatrix} 0 \\ 0 \end{pmatrix} \iff x + y = 0 \iff \mathbf{y = -x}$$
  Vecteur propre : $\mathbf{v}_2 = \begin{pmatrix} 1 \\ -1 \end{pmatrix}$.

#### 3. Matrices $P$, $D$ et $P^{-1}$ :
$$
P = \begin{pmatrix} 1 & 1 \\ 2 & -1 \end{pmatrix}, \quad D = \begin{pmatrix} 1 & 0 \\ 0 & 0{,}85 \end{pmatrix}
$$
Inversons $P$ : $\det(P) = 1(-1) - 1(2) = -3$.
$$
P^{-1} = \frac{1}{-3} \begin{pmatrix} -1 & -1 \\ -2 & 1 \end{pmatrix} = \mathbf{\frac{1}{3} \begin{pmatrix} 1 & 1 \\ 2 & -1 \end{pmatrix}}
$$

---

### D. Expression de $A^n$ et Équilibre à Long Terme ($n \to \infty$)

Calculons le produit $A^n = P D^n P^{-1}$ :
$$
A^n = \begin{pmatrix} 1 & 1 \\ 2 & -1 \end{pmatrix} \begin{pmatrix} 1^n & 0 \\ 0 & 0{,}85^n \end{pmatrix} \frac{1}{3} \begin{pmatrix} 1 & 1 \\ 2 & -1 \end{pmatrix} = \frac{1}{3} \begin{pmatrix} 1 & 0{,}85^n \\ 2 & -0{,}85^n \end{pmatrix} \begin{pmatrix} 1 & 1 \\ 2 & -1 \end{pmatrix}
$$
$$
\mathbf{A^n = \frac{1}{3} \begin{pmatrix} 1 + 2(0{,}85)^n & 1 - 0{,}85^n \\ 2 - 2(0{,}85)^n & 2 + 0{,}85^n \end{pmatrix}}
$$

Quand le temps passe et que $n \to \infty$ :
Comme $|0{,}85| < 1$, on a :
$$
\mathbf{\lim_{n \to \infty} (0{,}85)^n = 0}
$$
Le terme transitoire s'éteint complètement ! La matrice de puissance converge vers :
$$
\lim_{n \to \infty} A^n = \frac{1}{3} \begin{pmatrix} 1 & 1 \\ 2 & 2 \end{pmatrix}
$$

Calculons la répartition finale des 600 000 clients :
$$
\begin{pmatrix} a_\infty \\ b_\infty \end{pmatrix} = \frac{1}{3} \begin{pmatrix} 1 & 1 \\ 2 & 2 \end{pmatrix} \begin{pmatrix} 350\,000 \\ 250\,000 \end{pmatrix} = \frac{1}{3} \begin{pmatrix} 350\,000 + 250\,000 \\ 2(350\,000) + 2(250\,000) \end{pmatrix} = \frac{1}{3} \begin{pmatrix} 600\,000 \\ 1\,200\,000 \end{pmatrix} = \mathbf{\begin{pmatrix} 200\,000 \\ 400\,000 \end{pmatrix}}
$$

#### 🏆 Conclusion Économique Majeure :
À long terme :
- **L'opérateur A aura 200 000 clients (1/3 du marché soit 33,3%).**
- **L'opérateur B aura 400 000 clients (2/3 du marché soit 66,7%).**
*Le résultat d'équilibre ne dépend absolument pas des parts de marché de départ ($350\,000$ et $250\,000$), mais uniquement des taux de fidélité et de churn !*
Remarque que le vecteur d'équilibre $\begin{pmatrix} 200\,000 \\ 400\,000 \end{pmatrix} = 200\,000 \begin{pmatrix} 1 \\ 2 \end{pmatrix}$ est **strictement aligné sur le vecteur propre associé à $\lambda = 1$** !

---

## 🗣️ 7. Le Guide de l'Oral : Comment décrire ces calculs à l'examen

- **Pour décrire la recherche des valeurs propres :**
  > *"Je pose l'équation caractéristique $\det(A - \lambda I_n) = 0$. Je développe le déterminant pour obtenir le polynôme caractéristique $p_A(\lambda)$. Les valeurs propres sont les racines réelles de ce polynôme. Pour chacune, je note sa multiplicité algébrique $m_a(\lambda)$."*
- **Pour décrire le calcul d'un sous-espace propre :**
  > *"Pour la valeur propre $\lambda$, je résous le système homogène $(A - \lambda I_n)\mathbf{v} = \mathbf{0}$. L'ensemble des solutions forme le sous-espace propre $E_\lambda = \ker(A - \lambda I_n)$. J'en extrais une famille génératrice libre, qui me donne une base et la dimension géométrique $m_g(\lambda)$."*
- **Pour conclure sur la diagonalisabilité :**
  > *"La matrice est diagonalisable si et seulement si son polynôme caractéristique est scindé sur $\mathbb{R}$ et que pour chaque valeur propre, la dimension du sous-espace propre est égale à sa multiplicité algébrique. Nous pouvons alors former une base de $\mathbb{R}^n$ composée de vecteurs propres."*
- **Pour interpréter la dynamique à long terme :**
  > *"Puisque la plus grande valeur propre vaut 1 et que l'autre valeur propre est strictement inférieure à 1 en valeur absolue ($0{,}85 < 1$), le terme associé à $0{,}85^n$ tend vers 0. Le système converge donc vers un état d'équilibre stationnaire dirigé par le vecteur propre associé à $\lambda = 1$."*

---

## 📌 8. À Retenir Absolument (Points Clés du Chapitre 6)

1. $\mathbf{v} \neq \mathbf{0}$ est vecteur propre de $A \iff A\mathbf{v} = \lambda \mathbf{v}$.
2. Valeurs propres = racines de $p_A(\lambda) = \det(A - \lambda I_n) = 0$.
3. Sous-espace propre : $E_\lambda = \ker(A - \lambda I_n)$, avec $1 \le \dim(E_\lambda) \le m_a(\lambda)$.
4. Si $n$ valeurs propres distinctes $\implies$ Toujours diagonalisable.
5. $A$ est diagonalisable $\iff p_A$ entièrement factorisable et $\dim(E_\lambda) = m_a(\lambda)$ pour tout $\lambda$.
6. Formules de diagonalisation : $D = P^{-1} A P$ et $A = P D P^{-1}$.
7. Puissance $n$-ième : $A^n = P D^n P^{-1}$.
8. Modèles de Markov : La valeur propre $\lambda = 1$ pilote l'équilibre stationnaire à long terme.

---

🎉 **Félicitations ! Tu possèdes désormais l'intégralité du cours d'Algèbre Linéaire décortiqué avec rigueur, clarté et pédagogie !**
