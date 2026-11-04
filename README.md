# 🎓 Cheatsheets ICHEC : Mathématiques, Statistique & Algèbre Linéaire
### *Programme Business Analyst — ICHEC Brussels Management School*

Ce dépôt contient les fiches d'aide-mémoire (Cheatsheets) interactives et exhaustives pour réviser et résoudre l'ensemble des exercices et examens du cours.

---

## 🌐 Liens vers les Cheatsheets en Ligne (GitHub Pages)

- 🏠 **Portail d'Accueil :** [https://cemilserttas.github.io/Algebre/](https://cemilserttas.github.io/Algebre/)
- 📐 **Cheatsheet Algèbre Linéaire & Calcul Matriciel :** [https://cemilserttas.github.io/Algebre/Cheatsheet_Algebre_Lineaire.html](https://cemilserttas.github.io/Algebre/Cheatsheet_Algebre_Lineaire.html)
- 📊 **Cheatsheet Statistiques & Probabilités :** [https://cemilserttas.github.io/Algebre/Cheatsheet_Statistiques_et_Probabilites.html](https://cemilserttas.github.io/Algebre/Cheatsheet_Statistiques_et_Probabilites.html)

---

## 📚 Contenu Détaillé des Fiches

### 1. Algèbre Linéaire & Calcul Matriciel *(Réf. Prof. M. Bui)*
- **0. Modèle de Leontief :** Équilibre macroéconomique $\mathbf{p} = (I - M)^{-1}\mathbf{d}$ et matrice des coefficients techniques.
- **1. Calcul Matriciel :** Opérations, ordres de compatibilité, trace $\operatorname{tr}(A)$, transposée $A^t$ et non-commutativité ($AB \neq BA$).
- **2. Échelonnement de Gauss :** Les 3 opérations élémentaires autorisées et calcul du rang $\operatorname{rg}(A)$.
- **3. Déterminants & Laplace :** Déterminants $2 \times 2$, règle de Sarrus ($3 \times 3$), mineurs $M_{ij}$, cofacteurs $C_{ij}$ et les 7 propriétés fondamentales.
- **4. Inversion de Matrice :** Méthode de la matrice adjointe $A^{-1} = \frac{1}{\det(A)}\operatorname{Adj}(A)$, formule directe $2 \times 2$ et Gauss-Jordan.
- **5. Systèmes Linéaires :** Pivot de Gauss, classification de Rouché-Capelli (unique, impossible, infinité) et règle de Cramer.
- **6. Espaces Vectoriels :** Familles libres, génératrices, bases de $\mathbb{R}^n$ et les 3 règles d'or des cardinaux.
- **7. Applications Linéaires :** Démonstration en 2 étapes (additivité et homogénéité), automorphismes, théorème du rang et matrices de passage $P$.
- **8. Diagonalisation & Puissances $A^n$ :** Valeurs propres, vecteurs propres, sous-espaces propres $E_\lambda$, matrices $S$ et $D$, formule $A^n = S D^n S^{-1}$ et calcul de $A^{10}$.
- **9. Modèles de Rédaction & Outils :** Phrases clés pour l'examen et simulateur matriciel $2 \times 2$.

### 2. Statistiques & Probabilités *(Réf. Prof. Dr Sarah Dendievel)*
- **1. Statistique Descriptive :** Typologie des variables (nominale, ordinale, discrète, continue) et tableaux de distribution.
- **2. Mesures de Position :** Mode $M$, moyenne arithmétique $\bar{x}$, médiane $Me$ et diagnostic de l'asymétrie.
- **3. Dispersion & Boxplot :** Étendue $E$, écart interquartile $IQR$, détection des outliers par Tukey, formule de Koenig-Huygens $s^2 = \frac{1}{N}\sum n_i x_i^2 - \bar{x}^2$ et variance débiaisée $s'^2$.
- **4. Données Groupées :** Histogramme à amplitudes inégales (densité de fréquence $d_i$), interpolation linéaire sur l'ogive cumulative.
- **5. Logique Propositionnelle :** Connecteurs, tables de vérité, tautologie, contradiction et contingence.
- **6. Probabilités :** Démarche fréquentielle (empirique) vs classique a priori, probabilités conditionnelles, indépendance et théorème de Bayes.
- **7. Variables Aléatoires :** Espérance $E(X)$, variance $\operatorname{Var}(X)$, épreuve de Bernoulli et loi binomiale $\mathcal{B}(n, p)$.
- **8. Loi Normale :** Standardisation $Z = \frac{X - \mu}{\sigma}$, règle des 3 sigmas et guide des 6 cas de lecture de la table normale.
- **9. Échantillonnage & TCL :** Théorème Central Limite pour $n \ge 30$, distributions de la moyenne $\bar{X}$ et de la proportion $\hat{P}$.
- **10. Intervalles de Confiance :** Arbre de décision pour la moyenne $\mu$ (Normale vs Student $t_{n-1}$), IC pour une proportion $p$ et dimensionnement $n$.
- **11. Tests d'Hypothèses :** Analogie du procès pénal (risques $\alpha$ et $\beta$), protocole officiel en 5 étapes, tests $Z$ et $t$ sur la moyenne et la proportion.
- **12. Modèles de Rédaction & Outils :** Phrases types pour l'examen et simulateur $Z \sim \mathcal{N}(0, 1)$.

---

## 💻 Fonctionnalités Techniques
- Rendu mathématique dynamique avec MathJax 3 (LaTeX).
- Bascule Mode Clair / Mode Sombre avec mémorisation locale.
- Filtrage et recherche textuelle en temps réel.
- Entièrement responsive (ordinateur, tablette, mobile).
- Feuilles de style optimisées pour impression et export PDF.
