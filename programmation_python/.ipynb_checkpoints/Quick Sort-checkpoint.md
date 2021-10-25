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

# Quick Sort (tri rapide)

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
