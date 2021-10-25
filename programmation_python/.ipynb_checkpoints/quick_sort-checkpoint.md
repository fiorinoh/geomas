---
jupyter:
  jupytext:
    text_representation:
      extension: .md
      format_name: markdown
      format_version: '1.3'
      jupytext_version: 1.13.0
  kernelspec:
    display_name: Python 3
    language: python
    name: python3
---

<!-- #region -->
# Quick Sort (tri rapide)

## Principe

Le quick sort est l'un des algorithme de tri les plus rapides. Voici son principe.  

Soit une liste (= tableau) d'entiers à trier. Par exemple : [1, 33, 6, 89, 5]. On enlève le dernier élément de la liste, ici 5, qu'on appelle le *pivot* et que l'on place dans une liste [5].  

Pour améliorer le fonctionnement du quick sort, le choix du pivot doit être plus précis. Mais pour notre version du quick sort, nous nous bornerons à prendre le dernier élément de la liste. On crée ensuite 2 listes vides qu'on appelle conventionnellement *agauche* et *adroite*. On a donc :

| agauche     | pivot | adroite     | liste à trier     |
| :---:      |    :----:   |    :----:   |    :----:   |
| [ ]      | [5]       | [ ]      | [1, 33, 6, 89]      |

Puis on place tous les éléments de la liste à trier plus petits ou égaux au pivot dans la liste agauche, et tous les plus grands dans la liste adroite :

| agauche     | pivot | adroite    | liste à trier     |
| :---:      |    :----:   |    :----:   |    :----:   |
| [1]      | [5]       | [33, 6, 89]      | [ ]      |

On applique (récursivement) cette procédure aux listes agauche et adroite qui deviennent les nouvelles listes à trier. Une liste vide ou singleton (ne contenant qu'un seul élément) à trier est cette même liste : le résultat du tri de [1] est [1]. Le résultat du tri de [33, 6, 89] est [6, 33, 89]. Le résultat final est la concaténation de ces tris récursifs : [1] + [5] + [6, 33, 89] soit [1, 5, 6, 33, 89].



## Exercice 1 : Tester si une liste en triée

- Ecrire un algorithme permettant de tester si une liste d'entiers est triée
- Traduisez-le en python
<!-- #endregion -->

```python
def est_trie(tableau_entiers):
    est_trie = True
    for i in range(len(tableau_entiers) - 1):
        if (tableau_entiers[i] > tableau_entiers[i + 1]):
            est_trie = False
    return est_trie
```

```python
est_trie([1,33,6,89,5])
```

```python
est_trie([1, 5, 6, 33, 89])
```

```python
est_trie([])
```

```python
est_trie([5])
```

### Pythonic (pour info)

```python
def pythonic_est_trie(tableau_entier):
    return all(tableau_entier[i] <= tableau_entier[i + 1] for i in range(len(tableau_entier)-1))
```

```python
pythonic_est_trie([1,33,6,89,5])
```

```python
pythonic_est_trie([1, 5, 6, 33, 89])
```

## Exercice 2 : Ecrire l'algorithme du quicksort


## Exercice 3 : Ecrire le quick sort en python

```python
def quick_sort(atrier):
    if len(atrier) <= 1:
        return atrier
    else:
        agauche = []
        adroite = []
        pivot = atrier.pop()
        
        for elem in atrier:
            if elem > pivot:
                adroite.append(elem)
            else:
                agauche.append(elem)
    return quick_sort(agauche) + [pivot] + quick_sort(adroite)
```

```python
quick_sort([1,33,6,89,5])
```

## Exercice 4 : Comparer les performances du quick sort avec celles de la fonction sorted

```python
import random
#Generate 5 random numbers between 10 and 30
randomlist = random.sample(range(10, 30), 5)
print(randomlist)
```

```python
import time

# Création d'un fichier csv

head = "size;algo;time\n"
f = open('quick_sort.csv', 'w')
f.write(head)

# Tailles des tableaux : 10, 100, 1000, 10000
for i in range(2, 6):
    # 100 tableaux de chaque taille
    for j in range (0, 100):
        
        randomlist = random.sample(range(1, 10 ** i), 10 ** (i - 1))
        start_time = time.time()
        sorted(randomlist)
        duration = time.time() - start_time
        f.write(str(10 ** (i - 1)) + ";" + "sorted()" + ";" + str(duration) + "\n")
        
        start_time = time.time()
        quick_sort(randomlist)
        duration = time.time() - start_time
        f.write(str(10 ** (i - 1)) + ";" + "quick_sort()" + ";" + str(duration) + "\n")
        
# Ferme le fichier
f.close()
print("Fini !")
```

```python
import pandas as pd
import matplotlib.pyplot as plt

# Lecture d'un fichier csv
df = pd.read_csv("./quick_sort.csv", sep = ";")
df
```

```python
# time en ms
df["time"] = df["time"] * 1000000
```

```python
# Calcul de la moyenne
ag = df.groupby(["size", "algo"]).mean().reset_index()
ag
```

```python
x = ag["size"].drop_duplicates()
y_sorted = ag[ag["algo"] == "sorted()"]["time"]
y_custom = ag[ag["algo"] == "quick_sort()"]["time"]
```

```python
plt.figure()
plt.xscale("log")
plt.plot(x, y_sorted, label = "Sorted")
plt.plot(x, y_custom, label = "Quick Sort")
plt.legend(framealpha = 1, frameon = True)
plt.title("Algorithmes de tri")
```

```python

```
