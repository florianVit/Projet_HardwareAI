## 1️⃣ CNN simple, modèle pédagogique

### Pourquoi ce modèle existe

Il permet de :

- tester rapidement le pipeline de benchmark  
- observer le comportement CPU/GPU/TPU sur une **charge modérée**  
- isoler les effets :
  - du parallélisme  
  - du transfert mémoire  
  - de la latence pure  

### Caractéristiques

- seulement **2 couches convolutionnelles**  
- images **64×64**  
- **batch size faible (32)**  
- **peu de paramètres et peu de FLOPs**  

👉 Donc :

- calcul **relativement léger**  
- **GPU pas encore exploité à fond**  
- **CPU encore compétitif**  


---

## 2️⃣ CNN profond, ResNet-50

### Pourquoi utiliser ResNet-50

Parce que c’est :

- un **standard de la vision par ordinateur**  
- un **réseau profond (50 couches)**  
- représentatif d’un **vrai workload industriel IA**  

Ce modèle sert à mesurer :

- la performance en **conditions réalistes**  
- la capacité des accélérateurs à gérer  
  un **volume massif d’opérations tensorielles**  

### Caractéristiques

- **50 couches** avec connexions résiduelles  
- images **224×224**  
- **batch size élevé**  
- **milliards d’opérations par batch**  

👉 Donc :

- calcul **très intensif**  
- **parallélisme pleinement exploité**  
- **écart énorme CPU vs GPU/TPU**  
