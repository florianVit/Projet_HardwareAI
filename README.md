# Comparaison de Performance CPU/GPU/TPU

## Objectif

Comparer les performances de différents types de processeurs (CPU, GPU, TPU) sur des opérations de calcul intensif, afin d'analyser leurs caractéristiques en termes de latence, débit et efficacité énergétique.

## Méthodologie

### Plateformes testées
- **CPU** : Tests locaux sur PC
- **GPU** : Tests locaux (ex: RTX 3060 pour Florian, )
- **TPU** : Tests via Google Colab (TPU device: xla:0, v5e1)

### Métriques analysées
- **Performance** : GFLOPS, temps d'exécution (moyenne, p50, p95)
- **Latence** : Décomposition des étapes (transfert mémoire, calcul, récupération)
- **Efficacité énergétique** : Comparaison basée sur les specs constructeur

### Tests de référence
1. **Multiplication matricielle (MatMul)**
   - Différentes tailles de matrices (S, M, L)
   - Mesure des temps de transfert (H2D, D2H) et de calcul
   
2. à décider, CNN ?

3. à décider (ou juste rien xD)

## Résultats 


## Applications (pourquoi chosir)

Cas d'usage :
- **CPU** : Calculs légers, latence faible, flexibilité
- **GPU** : Calculs massifs parallélisables, throughput élevé
- **TPU** : Inférence/entraînement réseaux de neurones, efficacité énergétique

## Limitations

