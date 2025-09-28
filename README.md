# Rapport - Value Iteration (BookGrid)

## Question 1 : Calculs Value Iteration

### Itération 0 - États Initiaux
- **États non terminaux** : Valeurs initiales = 0
- **États terminaux** :
  - `V(4,3) = +1` (case verte, sortie positive)
  - `V(4,2) = -1` (case rouge, sortie négative)

**Résultat de test :**

<img src="https://github.com/user-attachments/assets/13298067-5ad8-413a-8366-72b663cef271" width="50%" alt="Itération 0"/>

---

### Itération 1

#### Calcul de V₁(4,3)
- Action `EXIT` : Q(Exit) = +1
- **Résultat** : V₁(4,3) = 1.00

#### Calcul de V₁(4,2)
- Action `EXIT` : Q(Exit) = -1
- **Résultat** : V₁(4,2) = -1.00

**Résultat de test :**

<img src="https://github.com/user-attachments/assets/05f4f65e-dc2b-48f2-bf4d-b055ec810859" width="50%" alt="Itération 1"/>

---

### Itération 2

#### Calcul de V₂(3,3)
**Action optimale** : `RIGHT` vers (4,3)

**Formule** :
```
Q₂(RIGHT) = 0.8(0 + 0.9 × V₁(4,3)) + 0.1(0 + 0.9 × V₁(3,3)) + 0.1(0 + 0.9 × V₁(3,2))
```

**Calcul détaillé** :
- 0.9 × V₁(4,3) = 0.9 × 1 = 0.90 → 0.8 × 0.90 = 0.72
- Les deux autres termes = 0

**Résultat** : V₂(3,3) = 0.72

**Résultat de test :**

<img src="https://github.com/user-attachments/assets/5f87ca33-5971-483b-831f-c73fc4142dba" width="50%" alt="Itération 2"/>

---

### Itération 3

#### Calcul de V₃(3,3)
**Formule** :
```
Q₃(RIGHT) = 0.8(0 + 0.9 × V₂(4,3)) + 0.1(0 + 0.9 × V₂(3,3)) + 0.1(0 + 0.9 × V₂(3,2))
```

**Calcul détaillé** :
- 0.9 × V₂(4,3) = 0.9 × 1.00 = 0.900 → 0.8 × 0.900 = 0.7200
- 0.9 × V₂(3,3) = 0.9 × 0.72 = 0.648 → 0.1 × 0.648 = 0.0648
- 0.9 × V₂(3,2) = 0.9 × 0.00 = 0.000 → 0.1 × 0.000 = 0.0000

**Somme** : 0.7200 + 0.0648 + 0.0000 = 0.7848  
**Résultat** : V₃(3,3) = 0.78

#### Calcul de V₃(2,3)
**Action optimale** : `RIGHT` vers (3,3)

**Formule** :
```
Q₃(RIGHT) = 0.8(0 + 0.9 × V₂(3,3))
```

**Calcul détaillé** :
- 0.9 × V₂(3,3) = 0.9 × 0.72 = 0.648
- 0.8 × 0.648 = 0.5184

**Résultat** : V₃(2,3) = 0.52

#### Calcul de V₃(3,2)
**Action optimale** : `UP` vers (3,3) avec risque de déviation vers (4,2)

**Formule** :
```
Q₃(UP) = 0.8(0 + 0.9 × V₂(3,3)) + 0.1(0 + 0.9 × V₂(4,2)) + 0.1(0 + 0.9 × 0)
```

**Calcul détaillé** :
- 0.9 × V₂(3,3) = 0.9 × 0.72 = 0.648 → 0.8 × 0.648 = 0.5184
- 0.9 × V₂(4,2) = 0.9 × (-1.00) = -0.900 → 0.1 × -0.900 = -0.0900
- 0.1 × 0 = 0

**Somme** : 0.5184 - 0.0900 + 0 = 0.4284  
**Résultat** : V₃(3,2) = 0.43

#### Résultats Finaux Itération 3
- V₃(3,3) = 0.78
- V₃(2,3) = 0.52
- V₃(3,2) = 0.43

**Résultat de test :**

<img src="https://github.com/user-attachments/assets/c32fee71-02cd-442b-a957-7776117f4056" width="50%" alt="Itération 3"/>

**Conclusion** : Les valeurs calculées manuellement correspondent exactement aux résultats obtenus par le code lors des tests.

---

## Question 2 : Modification BridgeGrid

### Commande Utilisée
```bash
python gridworld.py -g BridgeGrid -a value -k 1 --noise 0.01
```

### Résultats
![BridgeGrid 1](https://github.com/user-attachments/assets/5da404b0-15ed-4569-840b-2cca688d0859)
![BridgeGrid 2](https://github.com/user-attachments/assets/003d19c2-80a3-4d6c-85cb-819f866451cc)

### Solution
**Paramètre modifié** : noise = 0.01 (au lieu de 0.2)

### Justification
J'ai choisi de réduire le bruit car avec noise = 0.2, il y a 20% de chance que l'agent dévie de sa trajectoire, rendant le passage du pont trop risqué (déviation probable dans les cases à -100).

En réduisant le noise à 0.01, l'agent a 99% de chance d'aller dans la direction souhaitée. Cela rend le passage du pont suffisamment sûr pour que la récompense espérée de +10 compense le risque minimal de chute.

**Comparaison des espérances** :
- Espérance avec pont : 0.99 × 10 + 0.01 × (-100) = 8.9
- Alternative sans pont : récompense de 1

Le discount factor n'était pas le bon choix car même avec γ = 1, le risque de 20% reste trop élevé.

---

## Question 3 : Modification DiscountGrid

### 1. Chemin Risqué pour Atteindre +1

#### Commande
```bash
python gridworld.py -g DiscountGrid -a value -d 0.1
```

#### Résultats
![Discount 1a](https://github.com/user-attachments/assets/0c1e6561-d36f-40ac-a839-a283e9f008c7)
![Discount 1b](https://github.com/user-attachments/assets/c20b734d-7e0e-4532-9e8e-4f3da4945d9b)

#### Solution
**Paramètre modifié** : discount = 0.1

**Justification** : Avec un faible discount, l'agent valorise peu les récompenses futures. Il préfère le chemin court et direct vers +1, même s'il passe près du précipice, car les pénalités futures sont fortement dévaluées.

---

### 2. Chemin Risqué pour Atteindre +10

#### Commande
```bash
python gridworld.py -g DiscountGrid -a value -n 0.01
```

#### Résultats
![Discount 2a](https://github.com/user-attachments/assets/4241e4e6-e7e4-4fdc-b69b-dd25af74da4f)
![Discount 2b](https://github.com/user-attachments/assets/a3b5e1e9-28ac-4dfd-8d99-d474eb07af4d)

#### Solution
**Paramètre modifié** : noise = 0.01

**Justification** : En réduisant drastiquement le bruit, l'agent peut emprunter le chemin dangereux près du précipice. La récompense de +10 justifie le risque minimal, et l'agent choisit le chemin le plus court.

---

### 3. Chemin Sûr pour Atteindre +1

#### Commande
```bash
python gridworld.py -g DiscountGrid -a value -n 0.5
```

#### Résultats
![Discount 3a](https://github.com/user-attachments/assets/dc3e3f92-baee-43e8-8779-7114d5aef700)
![Discount 3b](https://github.com/user-attachments/assets/fb923afa-156c-4757-b12b-5d09f7299959)

#### Solution
**Paramètre modifié** : noise = 0.5

**Justification** : Avec un bruit très élevé, l'agent devient très prudent. Il évite complètement les chemins près du précipice car la probabilité de tomber est trop grande. Il préfère le chemin long mais sûr par le haut pour atteindre +1.

---

### 4. Éviter les États Absorbants

#### Commande
```bash
python gridworld.py -g DiscountGrid -a value -r 0.5
```

#### Résultats
![Discount 4a](https://github.com/user-attachments/assets/5b39846b-74fa-49e8-8cb2-deb145c460bd)
![Discount 4b](https://github.com/user-attachments/assets/034996d9-5398-42f6-bfbd-397b40263847)

#### Solution
**Paramètre modifié** : livingReward = 0.5

**Justification** : Avec une récompense positive à chaque pas, l'agent préfère continuer à se déplacer indéfiniment plutôt que d'atteindre un état terminal. Même la récompense de +10 ne compense pas la perte du livingReward constant. L'agent développe une politique qui évite tous les états absorbants pour maximiser la somme des récompenses sur le long terme.
### Question 4: Précision du détail du calcul des valeurs théoriques 

<p align="center">
  <img src="https://github.com/user-attachments/assets/5eacabd3-9b2b-4b1d-8afa-1adb27ebc72f" alt="WhatsApp Image 2025-09-28 at 14 32 38" />
  <img src="https://github.com/user-attachments/assets/b482aebc-21c9-47e6-a62e-cf64a12d7b1f" alt="WhatsApp Image 2025-09-28 at 14 33 46" />
  <img src="https://github.com/user-attachments/assets/a5a6fecf-2775-45da-bc97-7f8a2ff01b52" alt="WhatsApp Image 2025-09-28 at 14 37 38" />
</p>


## Question 8: Analyse des Features de l'ExpertExtractor

### Features Implémentées et leurs Rôles

### 1. Features de Base
- **`bias`** : Feature constante (1.0) qui permet au modèle d'avoir un biais de base
- **`stop-action`** : Pénalise l'action STOP pour encourager le mouvement et éviter que Pacman reste immobile

### 2. Features de Gestion des Fantômes

#### Features pour les fantômes normaux (dangereux)
- **`#-of-normal-ghosts-1-step-away`** : Compte le nombre de fantômes dangereux à proximité immédiate
  - *Rôle* : Signal de danger pour éviter les collisions fatales
- **`closest-normal-ghost`** : Distance au fantôme normal le plus proche (valeur négative)
  - *Rôle* : Encourage Pacman à maintenir une distance de sécurité avec les fantômes dangereux

#### Features pour les fantômes apeurés (cibles)
- **`#-of-scared-ghosts-1-step-away`** : Compte les fantômes apeurés à proximité
  - *Rôle* : Détecte les opportunités de capture
- **`eats-scared-ghost`** : Indique si l'action permet de manger un fantôme apeuré
  - *Rôle* : Récompense directe pour la capture de fantômes (200 points)
- **`closest-scared-ghost`** : Distance au fantôme apeuré le plus proche
  - *Rôle* : Guide Pacman vers les fantômes chassables
- **`min-scared-timer`** : Temps restant avant que les fantômes redeviennent dangereux
  - *Rôle* : Crée un sens d'urgence pour maximiser les captures

### 3. Features liées aux Capsules
- **`eats-capsule`** : Indique si l'action permet de manger une capsule
  - *Rôle* : Récompense l'activation du mode de chasse (50 points + capacité de chasser)
- **`closest-capsule`** : Distance à la capsule la plus proche
  - *Rôle* : Guide Pacman vers les capsules quand c'est stratégique

### 4. Features liées à la Nourriture
- **`eats-food`** : Indique si l'action permet de manger de la nourriture (seulement en sécurité)
  - *Rôle* : Objectif principal du jeu (10 points par pastille)
- **`closest-food`** : Distance à la nourriture la plus proche
  - *Rôle* : Guide l'exploration et la collecte systématique

## Analyse des Résultats

### Comparaison des Performances

#### SimpleExtractor vs ExpertExtractor

**Limitations de SimpleExtractor :**
- Ignore complètement les capsules et les fantômes apeurés
- Comportement purement défensif (fuite systématique)
- Performance limitée dans les labyrinthes avec capsules

**Avantages d'ExpertExtractor :**
- Comportement adaptatif selon le contexte
- Exploitation optimale des capsules pour maximiser les scores
- Stratégie offensive quand les fantômes sont apeurés

### Résultats Attendus

#### Sur `smallClassic` et `capsuleClassic` :
1. **Phase d'apprentissage plus rapide** : Les features spécialisées accélèrent la convergence
2. **Scores plus élevés** : Exploitation des capsules (50 points + 200 par fantôme)
3. **Comportement stratégique** :
   - Recherche active des capsules quand menacé
   - Chasse agressive des fantômes après avoir mangé une capsule
   - Retour au comportement défensif quand les fantômes redeviennent dangereux

#### Métriques de Performance :
- **Taux de victoire** : Amélioration significative (>90% vs ~70%)
- **Score moyen** : Augmentation due aux bonus de capture
- **Temps de survie** : Amélioration grâce à une meilleure gestion des dangers

### Analyse Comportementale

#### Comportement Émergent Observé :
1. **Priorisation dynamique** : L'agent apprend à arbitrer entre nourriture, capsules et chasse
2. **Gestion du timing** : Optimisation du temps passé en mode chasse
3. **Évaluation risque/récompense** : Prise de risques calculés pour maximiser les gains

#### Stratégies Apprises :
- **Fuite tactique** : Éviter les fantômes tout en se dirigeant vers une capsule
- **Chasse efficace** : Poursuite systématique des fantômes apeurés
- **Maximisation des bonus** : Exploitation complète de la période d'immunité

## Conclusion

L'ExpertExtractor transforme fondamentalement le comportement de Pacman d'une stratégie purement réactive à une approche proactive et stratégique. Les features implémentées permettent à l'agent de :

1. **Comprendre le contexte** : Différenciation entre situations de danger et d'opportunité
2. **Planifier à court terme** : Anticipation des changements d'état des fantômes
3. **Optimiser les gains** : Exploitation maximale des mécaniques de jeu

Cette approche démontre l'importance critique du choix des features dans l'apprentissage par renforcement approximé, où une représentation appropriée de l'état peut drastiquement améliorer les performances de l'agent.

