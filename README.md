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
meilleure_solution, meilleure_distance = tabu_search(matrice_distances, nombre_iterations=1000, taille_tabu=50)

print(f"Meilleure solution trouvée: {meilleure_solution}")
print(f"Distance minimale: {meilleure_distance}")
```

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

## 📊 Comparaison des Algorithmes

| Critère | Recherche Tabou | Recuit Simulé | Algo Génétique |
|---------|----------------|---------------|----------------|
| **Approche** | Recherche locale | Recherche stochastique | Évolutionnaire |
| **Mémoire** | Liste tabou | Température | Population |
| **Diversification** | Faible | Moyenne | Forte |
| **Complexité** | Moyenne | Faible | Élevée |
| **Paramètres** | 2 | 4 | 5+ |
| **Vitesse** | Moyenne | Rapide | Lente |



## 📈 Résultats Attendus

Chaque algorithme affiche :
- La meilleure solution trouvée (ordre des villes)
- La distance minimale du parcours
- Statistiques de convergence



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
