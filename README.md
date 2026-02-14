# Comparaison de Performance CPU/GPU/TPU

## Objectif

Comparer les performances de différents types de processeurs (CPU, GPU, TPU) sur des opérations de calcul intensif, afin d'analyser leurs caractéristiques en termes de latence, débit et efficacité énergétique.

## Méthodologie

### Plateformes testées
- **CPU** : Tests locaux sur PC
- **GPU** : Tests locaux (ex: RTX 3060)
- **TPU** : Tests via Google Colab

### Métriques analysées
- **Performance** : GFLOPS, temps d'exécution (moyenne, p50, p95)
- **Latence** : Décomposition des étapes (transfert mémoire, calcul, récupération)
- **Efficacité énergétique** : Comparaison basée sur les specs constructeur

### Tests de référence
1. **Multiplication matricielle (MatMul)** - Implémenté
   - Différentes tailles de matrices (S, M, L)
   - Mesure des temps de transfert (H2D, D2H) et de calcul
   
2. **Réseaux de neurones convolutifs (CNN)** - À venir

## Structure du projet

```
.
├── test.ipynb                  # Notebook principal avec benchmarks
├── results/
│   └── matmul_cpu_gpu.csv     # Résultats des tests MatMul
└── README.md
```

## Résultats préliminaires

Les premiers tests sur MatMul montrent que :
- **Petites matrices** : CPU compétitif (overhead du GPU)
- **Grandes matrices** : GPU apporte un gain significatif (×6 sur RTX 3060)
- **Transferts mémoire** : Impact important sur les performances GPU

## Applications pratiques

Selon les résultats, identification des cas d'usage optimaux :
- **CPU** : Calculs légers, latence faible, flexibilité
- **GPU** : Calculs massifs parallélisables, throughput élevé
- **TPU** : Inférence/entraînement réseaux de neurones, efficacité énergétique

## Limitations

- Dépendance au matériel disponible
- Variabilité des environnements de test
- Overhead des transferts mémoire pour GPU/TPU
