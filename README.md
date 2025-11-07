# 🗺️ Problème du Voyageur de Commerce (TSP)

Résolution du Travelling Salesman Problem avec différentes métaheuristiques d'optimisation.

## 📋 Description du Projet

Ce projet implémente trois algorithmes d'optimisation pour résoudre le problème du voyageur de commerce :
- **Recherche Tabou** (branche `main`)
- **Recuit Simulé** (branche `Méthode_RécuitSimulé`)
- **Algorithme Génétique** (branche `AlgorithmeGénétique`)

Le point de départ du projet est l'implémentation de la **recherche tabou**, qui sert de référence pour comparer les performances des autres algorithmes.

## 🎯 Problème

Le voyageur de commerce doit visiter un ensemble de villes exactement une fois et revenir à son point de départ, en minimisant la distance totale parcourue.

### Instance du problème
- **Nombre de villes** : 10
- **Matrice de distances** : 10×10 (distances symétriques entre chaque paire de villes)

## 🔍 Branche `main` : Recherche Tabou

### Principe
La recherche tabou évite de revisiter des solutions récentes en maintenant une **liste tabou** qui mémorise les dernières solutions explorées.

### Algorithme
1. **Initialisation** : Solution aléatoire
2. **Génération de voisins** : Échange de deux villes (swap)
3. **Sélection** : Choisir le meilleur voisin non-tabou
4. **Mise à jour** : Ajouter la solution à la liste tabou
5. **Critère d'arrêt** : Nombre d'itérations fixé

### Paramètres
```python
nombre_iterations = 1000  # Nombre d'itérations de l'algorithme
taille_tabu = 50          # Taille de la liste tabou (mémoire)
```

### Fonctions principales

#### `calculer_distance_totale(solution, matrice_distances)`
Calcule la distance totale d'un parcours en additionnant les distances entre villes consécutives, incluant le retour au point de départ.

**Paramètres :**
- `solution` : Liste des villes dans l'ordre du parcours
- `matrice_distances` : Matrice des distances entre villes

**Retour :** Distance totale du parcours

#### `generer_voisins(solution)`
Génère tous les voisins d'une solution en échangeant chaque paire de villes possible.

**Paramètres :**
- `solution` : Solution actuelle

**Retour :** Liste de toutes les solutions voisines

#### `tabu_search(matrice_distances, nombre_iterations, taille_tabu)`
Implémente l'algorithme de recherche tabou pour optimiser le parcours.

**Paramètres :**
- `matrice_distances` : Matrice des distances
- `nombre_iterations` : Nombre d'itérations
- `taille_tabu` : Taille de la liste tabou

**Retour :** Tuple (meilleure_solution, meilleure_distance)

### Utilisation
```python
# Exécution de l'algorithme
meilleure_solution, meilleure_distance = tabu_search(
    matrice_distances, 
    nombre_iterations=1000, 
    taille_tabu=50
)

print(f"Meilleure solution trouvée: {meilleure_solution}")
print(f"Distance minimale: {meilleure_distance}")
```

### Affichage attendu
```
Meilleure solution trouvée (Recherche Tabou): [2, 3, 4, 7, 8, 9, 5, 0, 1, 6]
Distance minimale: 17
```

Le programme affiche directement la meilleure solution trouvée et sa distance totale après avoir exploré l'espace des solutions pendant 1000 itérations.

### Caractéristiques
- ✅ Explore systématiquement le voisinage
- ✅ Évite les cycles grâce à la liste tabou
- ✅ Simple à implémenter
- ⚠️ Génère beaucoup de voisins (n×(n-1)/2)
- ⚠️ Peut être lent pour de grandes instances

## 🔥 Branche `Méthode_RécuitSimulé` : Recuit Simulé

### Principe
Algorithme inspiré du processus physique de recuit métallurgique. Il accepte parfois des solutions moins bonnes pour éviter les minima locaux, avec une probabilité qui diminue au cours du temps (refroidissement).

### Paramètres clés
```python
temperature_initiale = 50      # Température de départ (exploration)
temperature_finale = 0.1       # Température finale (exploitation)
taux_refroidissement = 0.995   # Coefficient alpha (0 < α < 1)
nombre_iterations = 1000       # Nombre total d'itérations
```

### Formule d'acceptation (Metropolis)
```
P(accepter) = exp(-Δ / T)
```
- `Δ` : différence de distance (nouvelle - actuelle)
- `T` : température courante

### Avantages
- ✅ Échappe aux minima locaux
- ✅ Un seul voisin généré par itération
- ✅ Peu de paramètres à ajuster
- ✅ Convergence garantie théoriquement

### Affichage attendu
```
Distance initiale: 67
Itération 100, Température: 30.2885, Distance actuelle: 66, Meilleure distance: 28
Itération 200, Température: 18.3479, Distance actuelle: 37, Meilleure distance: 28
Itération 300, Température: 11.1146, Distance actuelle: 42, Meilleure distance: 28
Itération 400, Température: 6.7329, Distance actuelle: 38, Meilleure distance: 25
Itération 500, Température: 4.0786, Distance actuelle: 35, Meilleure distance: 21
Itération 600, Température: 2.4707, Distance actuelle: 31, Meilleure distance: 21
Itération 700, Température: 1.4967, Distance actuelle: 28, Meilleure distance: 21
Itération 800, Température: 0.9066, Distance actuelle: 22, Meilleure distance: 17
Itération 900, Température: 0.5492, Distance actuelle: 20, Meilleure distance: 17
Itération 1000, Température: 0.3327, Distance actuelle: 20, Meilleure distance: 17

Distance minimale trouvée: 17

Meilleure solution trouvée (Recuit Simulé): [4, 3, 2, 9, 1, 0, 5, 6, 8, 7]
Distance minimale: 17
```

Le programme affiche :
- La distance initiale de la solution aléatoire
- Des mises à jour périodiques (toutes les 100 itérations) montrant :
  - Le numéro de l'itération
  - La température courante (qui diminue progressivement)
  - La distance de la solution actuelle
  - La meilleure distance trouvée jusqu'ici
- Le résultat final

## 🧬 Branche `AlgorithmeGénétique` : Algorithme Génétique

### Principe
Algorithme inspiré de l'évolution biologique : une population de solutions évolue par sélection, croisement et mutation.

### Paramètres clés
```python
taille_population = 50          # Nombre d'individus par génération
nombre_generations = 100        # Nombre de générations
taux_croisement = 0.8          # Probabilité de croisement (80%)
taux_mutation = 0.2            # Probabilité de mutation (20%)
```

### Opérateurs de Sélection
- **Roulette** : Probabilité proportionnelle au fitness
- **Rang** : Probabilité basée sur le classement

### Opérateurs de Croisement
- **Simple (OX)** : Croisement en un point avec ordre préservé
- **Double** : Croisement en deux points
- **Barycentrique (PMX)** : Partially Mapped Crossover avec mapping
- **Uniforme** : Sélection aléatoire avec masque binaire

### Opérateurs de Mutation
- **Échange** : Swap de deux villes aléatoires
- **Inversion** : Inversion d'un segment du parcours
- **Insertion** : Déplacement d'une ville vers une nouvelle position

### Avantages
- ✅ Exploration parallèle de l'espace de solutions
- ✅ Grande flexibilité (nombreux opérateurs)
- ✅ Élitisme : conservation des meilleures solutions
- ⚠️ Nombreux paramètres à calibrer
- ⚠️ Plus coûteux en calcul (population entière)

### Affichage attendu
```
=== Résolution du TSP avec Algorithme Génétique ===

Test de différentes configurations:


--- Configuration: roulette/simple/echange ---
Sélection: roulette, Croisement: simple, Mutation: echange

Génération 10: Meilleure distance = 24
Génération 20: Meilleure distance = 23
Génération 30: Meilleure distance = 18
Génération 40: Meilleure distance = 18
Génération 50: Meilleure distance = 18
Génération 60: Meilleure distance = 18
Génération 70: Meilleure distance = 18
Génération 80: Meilleure distance = 18
Génération 90: Meilleure distance = 18
Génération 100: Meilleure distance = 18

Distance finale: 18
Solution: [9, 6, 5, 8, 7, 4, 3, 2, 0, 1]
Distance: 18


--- Configuration: rang/double/inversion ---
Sélection: rang, Croisement: double, Mutation: inversion

Génération 10: Meilleure distance = 22
Génération 20: Meilleure distance = 20
Génération 30: Meilleure distance = 19
Génération 40: Meilleure distance = 17
Génération 50: Meilleure distance = 17
Génération 60: Meilleure distance = 17
Génération 70: Meilleure distance = 17
Génération 80: Meilleure distance = 17
Génération 90: Meilleure distance = 17
Génération 100: Meilleure distance = 17

Distance finale: 17
Solution: [8, 7, 4, 3, 2, 9, 1, 0, 5, 6]
Distance: 17


--- Configuration: roulette/barycentrique/insertion ---
Sélection: roulette, Croisement: barycentrique, Mutation: insertion

Génération 10: Meilleure distance = 25
Génération 20: Meilleure distance = 24
Génération 30: Meilleure distance = 20
Génération 40: Meilleure distance = 20
Génération 50: Meilleure distance = 17
Génération 60: Meilleure distance = 17
Génération 70: Meilleure distance = 17
Génération 80: Meilleure distance = 17
Génération 90: Meilleure distance = 17
Génération 100: Meilleure distance = 17

Distance finale: 17
Solution: [2, 3, 4, 7, 8, 6, 5, 0, 1, 9]
Distance: 17


--- Configuration: rang/uniforme/echange ---
Sélection: rang, Croisement: uniforme, Mutation: echange

Génération 10: Meilleure distance = 22
Génération 20: Meilleure distance = 22
Génération 30: Meilleure distance = 20
Génération 40: Meilleure distance = 20
Génération 50: Meilleure distance = 20
Génération 60: Meilleure distance = 20
Génération 70: Meilleure distance = 20
Génération 80: Meilleure distance = 20
Génération 90: Meilleure distance = 20
Génération 100: Meilleure distance = 20

Distance finale: 20
Solution: [3, 4, 7, 6, 9, 1, 0, 5, 8, 2]
Distance: 20

```

Le programme teste 4 configurations différentes en affichant :
- Le type de sélection, croisement et mutation utilisés
- Des mises à jour toutes les 10 générations montrant la meilleure distance
- La solution finale et sa distance pour chaque configuration
- Permet de comparer l'efficacité des différentes combinaisons d'opérateurs

## 📊 Comparaison des Algorithmes

| Critère | Recherche Tabou | Recuit Simulé | Algo Génétique |
|---------|----------------|---------------|----------------|
| **Approche** | Recherche locale | Recherche stochastique | Évolutionnaire |
| **Mémoire** | Liste tabou | Température | Population |
| **Diversification** | Faible | Moyenne | Forte |
| **Complexité** | Moyenne | Faible | Élevée |
| **Paramètres** | 2 | 4 | 5+ |
| **Vitesse** | Moyenne | Rapide | Lente |

## 🚀 Exécution

### Branche `main` (Recherche Tabou)
```bash
git checkout main
python main.py
```

### Branche `Méthode_RécuitSimulé`
```bash
git checkout Méthode_RécuitSimulé
python recuit_simule.py
```

### Branche `AlgorithmeGénétique`
```bash
git checkout AlgorithmeGénétique
python algo_genetique.py
```

## 📈 Résultats Attendus

### Format de sortie standard

Chaque algorithme affiche :
- La meilleure solution trouvée (ordre des villes)
- La distance minimale du parcours
- Statistiques de convergence (selon l'algorithme)

### Comparaison visuelle des sorties

| Algorithme | Information affichée | Fréquence |
|------------|---------------------|-----------|
| **Recherche Tabou** | Solution finale uniquement | Fin d'exécution |
| **Recuit Simulé** | Température, distances, progrès | Toutes les 100 itérations |
| **Algorithme Génétique** | Génération, meilleure distance | Toutes les 10 générations |

### Interprétation des résultats

**Recherche Tabou :**
- Sortie simple et directe
- Pas de suivi intermédiaire
- Résultat stable mais peut manquer l'optimum global

**Recuit Simulé :**
- Suivi détaillé du refroidissement
- Température élevée au début → exploration large
- Température basse à la fin → exploitation locale
- Pourcentage d'amélioration montrant l'efficacité

**Algorithme Génétique :**
- Évolution progressive sur les générations
- Permet de comparer différentes configurations
- Convergence plus lente mais exploration plus diversifiée
- Variation des résultats selon les opérateurs utilisés



## 📝 Notes Techniques

### Génération de voisins (Tabou)
La fonction `generer_voisins()` génère **n(n-1)/2** voisins pour une solution de taille n en échangeant chaque paire de villes.

### Fitness (Algorithme Génétique)
Le fitness est défini comme l'inverse de la distance :
```python
fitness = 1 / distance
```
Plus la distance est courte, plus le fitness est élevé.

### Critère d'arrêt
- **Tabou & Recuit** : Nombre d'itérations fixé
- **Génétique** : Nombre de générations fixé

Alternative possible : arrêt après N itérations sans amélioration.

## 🎓 Concepts Clés

### Recherche Tabou
- **Mémoire à court terme** : Liste tabou
- **Intensification** : Exploration systématique du voisinage
- **Diversification** : Éviter les solutions récentes

### Recuit Simulé
- **Température** : Contrôle l'acceptation de mauvaises solutions
- **Refroidissement** : Passage progressif exploration → exploitation
- **Équilibre thermodynamique** : Convergence théorique vers l'optimum

### Algorithme Génétique
- **Population** : Diversité des solutions
- **Sélection** : Pression évolutive
- **Croisement** : Combinaison de bonnes caractéristiques
- **Mutation** : Introduction de nouveauté
- **Élitisme** : Préservation des meilleures solutions

## 📚 Références

- Glover, F. (1986). "Future paths for integer programming and links to artificial intelligence"
- Kirkpatrick, S. et al. (1983). "Optimization by Simulated Annealing"
- Holland, J. H. (1975). "Adaptation in Natural and Artificial Systems"

## 👨‍💻 Auteur

Réalisé par : Hsan Khecharem

Filière : Licence en Sciences de l’Informatique

Spécialité : Génie Logiciel et Systèmes d’Information

Faculté : Faculté des Sciences de Sfax

Projet académique - Comparaison d'algorithmes métaheuristiques pour le TSP

## 📄 Licence

Ce projet est à usage éducatif.

---

**Note** : Ce README couvre les trois implémentations disponibles sur différentes branches. Le point de départ du projet est la recherche tabou (branche `main`), les autres algorithmes ayant été développés pour comparaison.
