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
