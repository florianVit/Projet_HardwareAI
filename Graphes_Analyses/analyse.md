# 📈 Analyse détaillée des résultats bruts — CPU vs GPU

Cette section présente une analyse approfondie des mesures obtenues lors des benchmarks CPU et GPU. L’objectif est de comprendre **le comportement réel des architectures** face à différents workloads, et d’identifier les facteurs qui influencent les performances et l’efficacité énergétique.

Les tests couvrent principalement :

- des multiplications matricielles de tailles croissantes,
- des inférences de réseaux convolutionnels,
- une décomposition du temps GPU (transferts mémoire vs calcul).

---

## 🔬 Analyse — Multiplication matricielle (MatMul)

La multiplication matricielle est un workload fortement intensif en calcul, typique des applications scientifiques et d’intelligence artificielle. Elle permet d’observer la montée en charge des architectures.

### ➜ Petites matrices

Pour les tailles réduites :

- le CPU reste compétitif,
- le GPU apporte un gain limité.

L’explication principale réside dans le coût des transferts mémoire CPU → GPU → CPU. Le GPU doit d’abord recevoir les données avant de pouvoir calculer, ce qui introduit une latence fixe non négligeable.

Dans ce régime :

- le calcul est rapide,
- mais les transferts dominent le temps total.

👉 Le GPU est sous-exploité car l’intensité de calcul ne compense pas l’overhead mémoire.

---

### ➜ Tailles intermédiaires

Lorsque la taille des matrices augmente :

- le temps de calcul devient prédominant,
- le parallélisme GPU commence à être pleinement exploité.

Le GPU montre alors une accélération nette par rapport au CPU, car :

- il exécute massivement des opérations en parallèle,
- sa bande passante mémoire interne est élevée.

On observe une transition vers un régime **compute-bound** (limité par le calcul plutôt que par les transferts).

---

### ➜ Grandes matrices

Pour les tailles importantes :

- le GPU atteint son régime optimal,
- le CPU montre une saturation progressive.

Le GPU devient largement dominant grâce à :

- son architecture massivement parallèle,
- ses unités spécialisées pour les opérations matricielles.

Même si les transferts mémoire restent présents, ils représentent une part relative plus faible du temps total.

👉 Le gain de performance devient significatif et stable.

---

## ⚙ Analyse — Réseaux neuronaux convolutionnels (CNN)

Les CNN représentent un workload réaliste pour l’IA moderne :

- calcul parallèle massif,
- forte localité mémoire,
- opérations répétitives.

Les mesures montrent :

- une latence GPU nettement inférieure,
- un débit largement supérieur.

Même avec le coût des transferts mémoire, le calcul GPU domine rapidement.

Cela s’explique par :

- l’optimisation matérielle des convolutions,
- la forte parallélisation des couches,
- la réutilisation efficace des données en mémoire.

👉 Les CNN exploitent naturellement les avantages du GPU.

---

## 🔄 Décomposition du temps GPU

Le temps GPU total se décompose en :

```
Temps total = H2D + Compute + D2H
```

où :

- **H2D** : transfert CPU → GPU  
- **Compute** : calcul réel GPU  
- **D2H** : transfert GPU → CPU  

### Observations

- petites charges → transferts dominants  
- grandes charges → calcul dominant  

Cette évolution montre que le GPU devient réellement efficace lorsque le coût fixe des transferts est amorti par une charge de calcul importante.

👉 Cela met en évidence un principe clé :

> plus l’intensité de calcul est élevée, plus le GPU est rentable.

---

## 🔋 Analyse énergétique

L’énergie estimée suit la relation :

```
Énergie = Puissance × Temps
```

Même si le GPU consomme plus de puissance instantanée :

- il termine les calculs beaucoup plus rapidement,
- l’énergie totale consommée est souvent inférieure.

Cela indique que :

- l’accélération matérielle peut réduire le coût énergétique global,
- la rapidité d’exécution est un facteur clé d’efficacité énergétique.

---

# 📊 Analyse spécifique — CNN ResNet50 (CPU vs CUDA)

Les mesures suivantes ont été obtenues lors du benchmark d’inférence de ResNet50 avec un batch size de 256 :

### Résultats bruts

| Device | Mean Latency | P95 Latency | Throughput |
|--------|--------------|------------|------------|
| CPU    | 15 789.59 ms | 18 360.40 ms | 16 img/s |
| CUDA   | 477.01 ms    | 477.37 ms   | 537 img/s |

---

## ⏱ Analyse de la latence

### 🔹 CPU

- Latence moyenne : **~15.8 secondes**
- P95 : **~18.4 secondes**

L’écart significatif entre la moyenne et le P95 (~2.6 secondes) indique une variabilité notable.  
Cela peut s’expliquer par :

- la contention CPU,
- la gestion mémoire,
- l’absence de parallélisme massif.

Le CPU exécute les convolutions de manière beaucoup plus séquentielle comparé au GPU.

---

### 🔹 GPU (CUDA)

- Latence moyenne : **~477 ms**
- P95 : **~477 ms**

La moyenne et le P95 sont presque identiques.

👉 Cela indique une **exécution extrêmement stable**.  
Le GPU maintient une performance constante sur les itérations.

La latence est environ :

```
15 789 ms / 477 ms ≈ 33x plus rapide
```

Le GPU offre donc une accélération d’environ **33×** sur ce workload.

---

## 🚀 Analyse du throughput

### CPU

- **16 images par seconde**

Cela correspond à un traitement relativement lent pour un batch de 256 images.

---

### GPU

- **537 images par seconde**

Le GPU traite plus d’un demi-millier d’images par seconde, ce qui montre :

- une exploitation efficace du parallélisme,
- une excellente optimisation des convolutions,
- une bande passante mémoire élevée.

---

## 📈 Facteur d’accélération global

Le gain de performance est spectaculaire :

| Métrique | Accélération GPU |
|----------|------------------|
| Latence  | ~33× plus rapide |
| Throughput | ~33× plus élevé |

Cette cohérence confirme que le GPU est en régime pleinement **compute-bound**, où le calcul domine largement les coûts de transfert mémoire.

---

## 🔬 Interprétation architecturale

ResNet50 est composé principalement de :

- convolutions 2D,
- opérations matricielles massives,
- normalisations batch,
- activations non linéaires.

Ces opérations :

- sont hautement parallélisables,
- correspondent exactement aux unités spécialisées du GPU,
- exploitent efficacement la mémoire interne rapide.

Le CPU, bien que performant en généraliste, ne dispose pas :

- d’un nombre suffisant d’unités parallèles,
- d’une architecture optimisée pour ce type de workload massif.

---

## 🔋 Implication énergétique

Même si le GPU consomme plus de puissance instantanée :

- il termine le calcul 33 fois plus vite,
- l’énergie totale consommée est probablement inférieure.

Cela suggère une **meilleure efficacité énergétique globale du GPU pour l’inférence CNN à grande échelle**.

---

## 🎯 Conclusion spécifique ResNet50

Les résultats démontrent clairement que :

- Le CPU n’est pas adapté à l’inférence CNN batch intensive.
- Le GPU exploite pleinement le parallélisme du modèle.
- L’écart devient massif dès que la charge de calcul est importante.

Ce benchmark confirme que pour des workloads deep learning réalistes :

> le GPU n’apporte pas une simple amélioration marginale,  
> mais un changement d’ordre de grandeur en performance.

---


## 📊 Tendances générales observées

Les résultats mettent en évidence plusieurs régimes :

### Workloads légers

- overhead mémoire dominant
- avantage CPU possible

### Workloads intermédiaires

- accélération GPU progressive
- meilleur ratio calcul/transfert

### Workloads intensifs

- GPU pleinement exploité
- gains majeurs en temps et énergie

---

## 🎯 Implications pratiques

Ces observations permettent de dégager plusieurs recommandations :

### CPU adapté pour

- petites tâches,
- traitements séquentiels,
- opérations à faible parallélisme.

### GPU adapté pour

- calcul scientifique,
- deep learning,
- traitement batch,
- multiplication matricielle massive.

Le facteur déterminant reste **l’intensité de calcul par rapport aux transferts mémoire**.

---

## 🧠 Conclusion de l’analyse brute

Les résultats confirment un principe fondamental de l’accélération matérielle :

> le GPU excelle lorsque le calcul massif amortit les coûts de transfert.

Cette étude montre que le choix CPU/GPU ne dépend pas uniquement de la puissance brute, mais de la nature du workload, de la taille des données et du rapport calcul/mémoire.

Les benchmarks illustrent clairement :

- la transition d’un régime limité par les transferts vers un régime dominé par le calcul,
- l’importance de structurer les pipelines pour exploiter efficacement le GPU,
- le potentiel énergétique des architectures accélérées.

Ces conclusions servent de base pour optimiser les stratégies de calcul dans des contextes réels.
