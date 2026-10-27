# 📘 Chapitre 0 : Introduction, Vision Globale et Modèle de Leontief
### *Cours d'Algèbre Linéaire et Calcul Matriciel — Business Analyst (ICHEC)*

---

## 🧒 1. L'Intuition pour un Enfant : Le Village des 3 Métiers

Imagine un petit village avec seulement trois habitants :
1. **Le Fermier** (qui produit du blé et des légumes),
2. **L'Artisan** (qui fabrique des outils en métal et des machines),
3. **Le Meunier/Électricien** (qui produit de l'énergie avec son moulin).

Pour faire pousser du blé, le fermier a besoin d'énergie pour son moulin et d'outils en métal fabriqués par l'artisan. Mais pour fabriquer des outils, l'artisan a besoin de manger du blé et d'utiliser de l'énergie ! Et pour faire tourner ses éoliennes, le meunier a besoin de pièces de rechange métalliques et de nourriture !

Tout le monde dépend de tout le monde. Si les habitants du village d'à côté (les clients extérieurs) viennent acheter **plus de pain**, le fermier doit produire plus de blé. Pour cela, il va commander **plus d'outils**, ce qui oblige l'artisan à produire plus, et celui-ci va consommer **plus d'énergie** et **plus de blé** !

Comment savoir **exactement** combien de blé, d'outils et d'énergie il faut fabriquer au total pour que tout le monde ait ce dont il a besoin, sans jamais tomber en panne ni gaspiller ? 
C'est le **problème de Leontief** (qui lui a valu le Prix Nobel d'Économie en 1973). Et la seule façon de résoudre ce casse-tête sans devenir fou, c'est **l'algèbre linéaire** !

---

## 📐 2. Les 3 Idées Fondamentales : Représenter, Transformer, Résoudre

Le cours d'algèbre linéaire repose sur un triptyque fondamental :

```mermaid
graph LR
    A["1. REPRÉSENTER<br>Organiser l'information"] --> B["2. TRANSFORMER<br>Créer de nouveaux indicateurs"]
    B --> C["3. RÉSOUDRE<br>Trouver les inconnues sous contraintes"]
```

### A. Représenter : Du simple chiffre au vecteur et à la matrice
Un Business Analyst ne travaille jamais avec une seule donnée isolée. Pour comprendre un client sur un site e-commerce, on observe simultanément 4 indicateurs :
- Nombre de commandes effectuées : $12$
- Montant total dépensé : $1\,248$ €
- Nombre de produits retournés : $2$
- Jours écoulés depuis la dernière commande : $18$

Plutôt que d'avoir 4 chiffres éparpillés, on les rassemble dans un **vecteur colonne** :
$$
\mathbf{x} = \begin{pmatrix} 12 \\ 1248 \\ 2 \\ 18 \end{pmatrix} \in \mathbb{R}^4
$$
- **Interprétation :** Chaque ligne (composante) représente une variable précise décrivant ce client.
- **Convention :** Les vecteurs sont toujours écrits **en colonne** et notés en **caractères gras** ($\mathbf{x}$), tandis que les nombres simples (scalaires) sont notés en caractères ordinaires.

Si l'entreprise a maintenant **10 000 clients**, on assemble leurs vecteurs côte à côte pour former une **matrice de données** $X$ :
$$
X = \begin{pmatrix} 
12 & 4 & 8 & 1 & 5 & \cdots & 2 \\
1248 & 94 & 458 & 79 & 322 & \cdots & 122 \\
2 & 0 & 1 & 0 & 3 & \cdots & 1 \\
18 & 53 & 23 & 4 & 142 & \cdots & 11 
\end{pmatrix} \in \mathbb{R}^{4 \times 10\,000}
$$
- **Chaque colonne** = 1 client unique (une observation).
- **Chaque ligne** = 1 caractéristique spécifique (une variable).
- Ici, $X$ possède **4 lignes** et **10 000 colonnes**.

---

### B. Transformer : Créer de la valeur à partir des données
Avoir des données brutes ne suffit pas. On cherche souvent à calculer un **score de fidélité** ou un **risque de désabonnement (churn)** à partir de ces variables.
Une telle transformation s'écrit mathématiquement sous la forme :
$$
\mathbf{y} = A \mathbf{x}
$$
Par exemple, pour transformer un vecteur à 4 variables $\mathbf{x} \in \mathbb{R}^4$ en 2 indicateurs synthétiques $\mathbf{y} = \begin{pmatrix} y_1 \\ y_2 \end{pmatrix} \in \mathbb{R}^2$ :
$$
\begin{cases}
y_1 = a_{11}x_1 + a_{12}x_2 + a_{13}x_3 + a_{14}x_4 \\
y_2 = a_{21}x_1 + a_{22}x_2 + a_{23}x_3 + a_{24}x_4
\end{cases}
$$
C'est le rôle des **applications linéaires** (Chapitre 2) et des **matrices** (Chapitre 3) !

---

### C. Résoudre : Trouver les causes à partir des effets
Supposons que nous ayons plusieurs contraintes de gestion liant deux variables $x$ et $y$ :
$$
\begin{cases}
2x + y = 10 \\
x + 3y = 15
\end{cases}
$$
Ce système peut s'écrire sous forme matricielle condensée :
$$
\underbrace{\begin{pmatrix} 2 & 1 \\ 1 & 3 \end{pmatrix}}_{A} \underbrace{\begin{pmatrix} x \\ y \end{pmatrix}}_{\mathbf{x}} = \underbrace{\begin{pmatrix} 10 \\ 15 \end{pmatrix}}_{\mathbf{b}} \iff A\mathbf{x} = \mathbf{b}
$$
Le but de l'algèbre linéaire sera de répondre avec certitude aux questions :
1. Une solution existe-t-elle ? (Compatibilité)
2. La solution est-elle unique ? (Inversibilité)
3. Comment la calculer de manière optimale ? (Méthode de la matrice inverse $x = A^{-1}b$ ou Pivot de Gauss).

---

## 🏭 3. L'Étude de Cas Complète : Le Modèle Économique de Leontief

C'est l'exemple phare du cours qui sert de fil rouge à toute la matière.

### A. Le tableau des échanges intermédiaires
Considérons une économie simplifiée à 3 secteurs : **Agriculture**, **Industrie**, et **Énergie**.

| Secteur Fournisseur (Lignes) | Utilisé par Agriculture | Utilisé par Industrie | Utilisé par Énergie | Demande Finale des Ménages ($d$) | Production Totale ($p$) |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **Agriculture** | 20 | 30 | 10 | **40** | **100** |
| **Industrie** | 20 | 40 | 20 | **120** | **200** |
| **Énergie** | 10 | 20 | 20 | **50** | **100** |

#### Comment lire ce tableau ?
- **Lecture en ligne (Ce qu'un secteur vend) :**
  L'Agriculture produit au total 100 unités :
  - 20 sont réutilisées par l'Agriculture elle-même (ex: des graines pour replanter),
  - 30 sont vendues à l'Industrie (ex: matières premières agroalimentaires),
  - 10 sont vendues au secteur de l'Énergie (ex: biocarburants),
  - 40 sont vendues aux consommateurs finaux (la demande finale $d_1$).
  $$20 + 30 + 10 + 40 = 100$$
- **Lecture en colonne (Ce qu'un secteur achète pour produire) :**
  Pour fabriquer ses 200 unités, l'Industrie a dû acheter :
  - 30 unités d'Agriculture,
  - 40 unités d'outils industriels,
  - 20 unités d'Énergie.

Nous isolons deux vecteurs essentiels :
- Le **vecteur des productions totales** : $\mathbf{p} = \begin{pmatrix} 100 \\ 200 \\ 100 \end{pmatrix}$
- Le **vecteur de la demande finale** : $\mathbf{d} = \begin{pmatrix} 40 \\ 120 \\ 50 \end{pmatrix}$

---

### B. La Matrice des Coefficients Techniques $M$

#### Définition
Le coefficient technique $m_{ij}$ indique la quantité produite par le secteur $i$ nécessaire pour produire **une seule unité** dans le secteur $j$.
$$
m_{ij} = \frac{\text{Quantité vendue par le secteur } i \text{ au secteur } j}{\text{Production totale du secteur } j (p_j)}
$$

#### Calcul pas à pas des coefficients :
- **Colonne 1 (Besoins pour 1 unité agricole, $p_1 = 100$) :**
  - $m_{11} = 20 / 100 = 0{,}20$
  - $m_{21} = 20 / 100 = 0{,}20$
  - $m_{31} = 10 / 100 = 0{,}10$
- **Colonne 2 (Besoins pour 1 unité industrielle, $p_2 = 200$) :**
  - $m_{12} = 30 / 200 = 0{,}15$ *(Il faut 0,15 unité agricole pour produire 1 unité industrielle)*
  - $m_{22} = 40 / 200 = 0{,}20$
  - $m_{32} = 20 / 200 = 0{,}10$
- **Colonne 3 (Besoins pour 1 unité d'énergie, $p_3 = 100$) :**
  - $m_{13} = 10 / 100 = 0{,}10$
  - $m_{23} = 20 / 100 = 0{,}20$
  - $m_{33} = 20 / 100 = 0{,}20$

On obtient ainsi la **matrice des coefficients techniques** :
$$
M = \begin{pmatrix} 
0{,}20 & 0{,}15 & 0{,}10 \\
0{,}20 & 0{,}20 & 0{,}20 \\
0{,}10 & 0{,}10 & 0{,}20 
\end{pmatrix}
$$

> [!CAUTION]
> **Attention à l'orientation :**
> - Les **lignes** correspondent aux secteurs **fournisseurs** d'intrants.
> - Les **colonnes** correspondent aux secteurs **utilisateurs** (qui consomment les intrants pour produire).

---

### C. La Production Intermédiaire et l'Équation Fondamentale

Si l'économie produit un vecteur total $\mathbf{p} = \begin{pmatrix} p_1 \\ p_2 \\ p_3 \end{pmatrix}$, alors la quantité totale d'intrants consommée par les entreprises elles-mêmes (les **besoins intermédiaires**) est donnée par le produit matrice-vecteur :
$$
M \mathbf{p} = \begin{pmatrix} 
0{,}20 & 0{,}15 & 0{,}10 \\
0{,}20 & 0{,}20 & 0{,}20 \\
0{,}10 & 0{,}10 & 0{,}20 
\end{pmatrix} \begin{pmatrix} 100 \\ 200 \\ 100 \end{pmatrix}
$$

Calculons ligne par ligne :
- Ligne 1 : $0{,}20 \times 100 + 0{,}15 \times 200 + 0{,}10 \times 100 = 20 + 30 + 10 = \mathbf{60}$
- Ligne 2 : $0{,}20 \times 100 + 0{,}20 \times 200 + 0{,}20 \times 100 = 20 + 40 + 20 = \mathbf{80}$
- Ligne 3 : $0{,}10 \times 100 + 0{,}10 \times 200 + 0{,}20 \times 100 = 10 + 20 + 20 = \mathbf{50}$

On retrouve exactement la consommation intermédiaire du tableau :
$$
M\mathbf{p} = \begin{pmatrix} 60 \\ 80 \\ 50 \end{pmatrix}
$$

#### Le Cœur du Modèle : L'Équilibre Économique
La production totale d'un pays ($\mathbf{p}$) doit obligatoirement suffire à couvrir :
1. Ce que les industries consomment entre elles ($M\mathbf{p}$),
2. Ce que les consommateurs finaux réclament ($\mathbf{d}$).

D'où l'équation d'équilibre de Leontief :
$$
\mathbf{p} = M\mathbf{p} + \mathbf{d}
$$

Dans notre exemple numérique, on a bien :
$$
\begin{pmatrix} 100 \\ 200 \\ 100 \end{pmatrix} = \begin{pmatrix} 60 \\ 80 \\ 50 \end{pmatrix} + \begin{pmatrix} 40 \\ 120 \\ 50 \end{pmatrix} \quad \checkmark
$$

---

### D. Vers le Système Linéaire et la Matrice Inverse

En pratique économique, le gouvernement ou l'analyste se pose la question inverse :
> *"Si la demande finale $\mathbf{d}$ augmente (par exemple, un plan de relance de la consommation), quelle quantité totale de production $\mathbf{p}$ chaque usine du pays doit-elle planifier ?"*

Isolons $\mathbf{p}$ dans l'équation :
1. On soustrait $M\mathbf{p}$ des deux côtés :
   $$
   \mathbf{p} - M\mathbf{p} = \mathbf{d}
   $$
2. On sait que $\mathbf{p} = I \mathbf{p}$ (où $I$ est la **matrice identité**, le neutre de la multiplication matricielle) :
   $$
   I\mathbf{p} - M\mathbf{p} = \mathbf{d}
   $$
3. On factorise par $\mathbf{p}$ à droite (car la multiplication matricielle n'est pas commutative) :
   $$
   (I - M)\mathbf{p} = \mathbf{d}
   $$
4. Si la matrice $(I - M)$ est inversible, on multiplie à gauche par son inverse $(I - M)^{-1}$ :
   $$
   \mathbf{p} = (I - M)^{-1} \mathbf{d}
   $$

La matrice $(I - M)^{-1}$ s'appelle le **Multiplicateur de Leontief**. Elle capture tous les effets d'entraînement directs et indirects dans toute l'économie !

```mermaid
flowchart TD
    V[1. Vecteurs : p et d] --> M[2. Matrice des coefficients : M]
    M --> PM[3. Produit matriciel : M p]
    PM --> SL[4. Système linéaire : (I - M) p = d]
    SL --> MI[5. Matrice inverse : p = (I - M)⁻¹ d]
    style V fill:#e3f2fd,stroke:#1565c0;
    style M fill:#e8f5e9,stroke:#2e7d32;
    style PM fill:#fff3e0,stroke:#e65100;
    style SL fill:#fce4ec,stroke:#c2185b;
    style MI fill:#f3e5f5,stroke:#7b1fa2;
```

---

## 💼 4. Où rencontre-t-on l'Algèbre Linéaire en Business Analytics ?

1. **Régression Linéaire Multiple :**
   Prédire les ventes $y$ en fonction des dépenses publicitaires TV, Web et Radio :
   $$
   \mathbf{y} = X\boldsymbol{\beta} + \boldsymbol{\varepsilon}
   $$
   La formule magique de l'estimateur des moindres carrés ordinaires (MCO) repose à 100% sur le calcul matriciel : $\hat{\boldsymbol{\beta}} = (X^\top X)^{-1} X^\top \mathbf{y}$.
2. **Analyse en Composantes Principales (ACP) :**
   Réduire un tableau de 100 variables financières à seulement 2 ou 3 axes synthétiques pour visualiser les entreprises en bonne santé vs celles à risque. Cela repose directement sur les **valeurs propres** et **vecteurs propres** (Chapitre 6).
3. **Optimisation & Gestion des Opérations :**
   Programmation linéaire (algorithme du simplexe), planification des flux logistiques et des stocks.
4. **Intelligence Artificielle et Machine Learning :**
   Les réseaux de neurones (Deep Learning) et les Large Language Models (LLM) ne font rien d'autre que des milliards de produits de matrices et de transformations d'espaces vectoriels chaque seconde !

---

## 🗣️ 5. Le Guide de l'Oral : Comment décrire ces calculs

Quand le professeur te demande d'interpréter le modèle de Leontief :
- **Pour le coefficient $m_{ij}$ :** 
  > *"Le coefficient $m_{ij}$ mesure l'intensité technique. Il représente le montant d'intrant du secteur fournisseur $i$ requis pour produire une unité monétaire de produit fini dans le secteur $j$."*
- **Pour l'expression $M\mathbf{p}$ :** 
  > *"Le produit $M\mathbf{p}$ est le vecteur de la demande intermédiaire globale. Il synthétise tout ce que les secteurs productifs de l'économie s'échangent mutuellement pour fonctionner."*
- **Pour l'équation $(I - M)\mathbf{p} = \mathbf{d}$ :** 
  > *"Cette équation traduit l'équilibre macroéconomique entrée-sortie. La matrice $(I - M)$ représente la part de la production totale qui reste disponible pour la consommation finale après avoir déduit les besoins de fonctionnement de l'outil de production."*
- **Pour la résolution $\mathbf{p} = (I - M)^{-1}\mathbf{d}$ :** 
  > *"L'inverse de la matrice $(I - M)$ est l'opérateur multiplicateur. Il permet de déduire le plan de production national nécessaire pour absorber n'importe quel choc de demande finale."*

---

## 📌 6. À Retenir Absolument (Points Clés d'Examen)

1. Un vecteur colonne regroupe les caractéristiques d'une observation ($\mathbf{x} \in \mathbb{R}^n$).
2. Une matrice regroupe plusieurs observations ou représente une transformation ($X \in \mathbb{R}^{m \times n}$).
3. Le fil conducteur : **Représenter** $\to$ **Transformer** $\to$ **Résoudre**.
4. Dans la matrice de Leontief $M$, $m_{ij} = \frac{\text{Achat de } j \text{ auprès de } i}{\text{Production totale de } j}$.
5. L'équation d'équilibre fondamentale est :
   $$\mathbf{p} = M\mathbf{p} + \mathbf{d} \iff (I - M)\mathbf{p} = \mathbf{d}$$
6. Si $(I - M)$ est inversible, la solution d'équilibre est :
   $$\mathbf{p} = (I - M)^{-1}\mathbf{d}$$

---

➡️ **Passer au chapitre suivant :** [Chapitre 1 : Espaces Vectoriels Réels](file:///c:/Users/cemmi/OneDrive%20-%20ICHEC/Math/Alg%C3%A8bre/Guides_de_Cours_Complets/Chapitre_1_Espaces_Vectoriels_Reels.md)
