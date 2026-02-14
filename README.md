# Comparaison de Performance CPU/GPU/TPU

## Objectif

Comparer les performances de différents types de processeurs (CPU, GPU, TPU) sur des opérations de calcul intensif. Analyse des caractéristiques en termes de latence, débit (GFLOPS), decomposition des étapes et efficacité énergétique.

## Configuration matérielle

| Composant | Modèle | Puissance |
|-----------|--------|-----------|
| **CPU** | Intel Core i7-11800H | 45W (TDP) |
| **GPU** | NVIDIA GeForce RTX 3060 Laptop | 80W (TGP) |
| **TPU** | À tester via Google Colab | - |

## Méthodologie

### Opérations de référence

**MatMul (Multiplication matricielle FP32)** - Principal benchmark
- Matrice carrée de taille N × N
- Tailles testées : S (1024), M (4096), L (8192)
- Warmup : 20 itérations
- Mesures : 100 itérations

### Métriques collectées

| Métrique | Description |
|----------|-------------|
| **mean_ms** | Temps moyen d'exécution (ms) |
| **p50_ms** | Percentile 50 (temps typique) |
| **p95_ms** | Percentile 95 (cas lent) |
| **gflops** | Débit de calcul (Giga FLOPs/sec) |
| **h2d_ms** (GPU) | Transfert Host→Device (ms) |
| **compute_ms** | Temps de calcul pur (ms) |
| **d2h_ms** (GPU) | Transfert Device→Host (ms) |
| **energy_j** | Énergie totale estimée (Joules) |


## Comment lancer le benchmark

### 1. Exécuter le benchmark local

Dans le notebook `test.ipynb` :
- Cellule 1 : Vérifier l'installation (Torch, CUDA)
- Cellule 2 : Importer les dépendances
- Cellule 5 : Lancer la configuration et les benchmarks

```python
# Les résultats seront sauvegardés dans results/matmul_cpu_gpu.csv
python -c "from test import main; main()"
```

### 2. Adapter à votre matériel

Modifiez la cellule 5 :
```python
SIZES = {"S": 1024, "M": 4096, "L": 8192}  # Ajuster selon votre RAM
CPU_THREADS = None  # Laisser None pour auto, ou spécifier (1, 4, 8, etc), en soit ça permet surtout de limiter ou d’augmenter le parallélisme CPU,
                    # ce qui est utile pour analyser l’influence du nombre de cœurs sur les performances et l’énergie
POWER_W = {
    "cpu": 45,   # Ici j'ai mit mes composants donc à vous de check les votres
    "gpu": 80,   #
}
```

### 3. Benchmark sur TPU (Google Colab)

À ajouter : Notebook Colab avec torch_xla ou JAX, exportant un CSV identique.

## Résultats attendus

Le file **results/matmul_cpu_gpu.csv** contient une ligne par (device, size) :

```csv
device,size,N,mean_ms,p50_ms,p95_ms,gflops,h2d_ms,compute_ms,d2h_ms,power_w,energy_j,compute_energy_j
cpu,S,1024,...
cpu,M,4096,...
cpu,L,8192,...
gpu,S,1024,...
gpu,M,4096,...
gpu,L,8192,...
```

## Analyse et interprétation

### Cas d'usage recommandés

| Processeur | Avantages | Limitations | Usages |
|-----------|-----------|------------|--------|
| **CPU** | Latence basse, flexibilité, versatilité | Débit limité pour FP operations | Calculs légers, embarqué, CPU-bound |
| **GPU** | Massive parallelism, débit élevé (TFLOPS) | Overhead mémoire, consommation | ML training, science computing, gaming |
| **TPU** | Efficacité énergétique, optimisé ML | Spécialisé (ML uniquement) | Inference/training réseaux neurones, TPU pods |

### Éléments à considérer

1. **Breakeven point** : Taille N à partir de laquelle GPU > CPU
2. **Efficacité énergétique** : GFLOPS par Watt
3. **Decomposition latence** : H2D impact vs compute time
4. **Scalabilité** : Performance avec matrice grand N

## Limitations



## Prochaines étapes

- [ ] Lancer benchmark CPU/GPU complet
- [ ] Ajouter benchmark TPU (Colab)
- [ ] Analyser les résultats bruts
- [ ] Générer graphiques de comparaison
- [ ] Rédiger conclusions et recommandations
