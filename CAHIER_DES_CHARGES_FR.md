# Cahier des charges – Universal Local AI Benchmark & Recommendation System

## 1. Vision
Créer une plateforme libre, open source et communautaire permettant à n’importe quel utilisateur de déterminer automatiquement quels modèles d’intelligence artificielle locaux sont les plus adaptés à sa machine, même avec du matériel ancien ou modeste.

## 2. Objectifs principaux
- **Détection automatique du matériel** : analyser CPU (architecture, cœurs, threads, fréquence réelle, instructions), RAM (totale, libre, disponible, swap), GPU (présence, VRAM), stockage, système d’exploitation, version du noyau, température et consommation énergétique (si disponible).
- **Découverte des modèles IA** : interroger automatiquement plusieurs catalogues publics (Ollama, Hugging Face, llama.cpp, LM Studio, GPT4All, OpenRouter, KoboldCPP, vLLM, MLX, etc.) pour récupérer métadonnées, licences, tailles, besoins mémoire/CPU/GPU estimés.
- **Sélection intelligente** : proposer plusieurs stratégies de recherche :
  - Mode rapide : meilleur modèle utilisable immédiatement.
  - Mode équilibré : meilleur compromis qualité‑vitesse‑mémoire.
  - Mode qualité maximale : meilleur modèle possible.
  - Mode expérimental : tester tous les modèles compatibles.
  - Mode communautaire : tester uniquement les modèles recommandés par la communauté.
- **Téléchargement automatique** : télécharger, vérifier l’intégrité, mesurer la taille réelle et installer le modèle choisi.
- **Benchmark automatique** : pour chaque modèle mesurer
  - Consommation RAM/Swap, charge CPU/GPU, température, temps de chargement.
  - Temps avant premier token, tokens/seconde, temps total de réponse, longueur maximale de contexte, temps de traitement image/document/audio (si applicable).
- **Évaluation qualitative** : jeu de tests standardisé couvrant raisonnement (logique, maths, déduction), programmation (Python, C, Bash, JS), langues (français, anglais, multilingue), connaissances générales (histoire, sciences, informatique), résumé de documents, analyse de fichiers, vision (si applicable).
- **Système de notation** : attribuer à chaque modèle un score qualité, vitesse, mémoire, énergie, compatibilité et communauté ; calculer un score global pondéré.
- **Gestion automatique des modèles** : selon préférences utilisateur (conserver, archiver, supprimer, garder uniquement le Top N, etc.).
- **Base de données communautaire** : permettre le partage anonyme de configuration matérielle, résultats de benchmark, scores, consommation mémoire, vitesse observée ; aucune donnée personnelle collectée.
- **Intelligence collective** : avec suffisamment de participants, répondre automatiquement à des requêtes du type « Pour un Intel i3‑2310M avec 16 Go de RAM sous Linux, les modèles les plus performants sont … ».
- **Classements mondiaux** : produire des tops par catégorie (CPU ancien, CPU moderne, Raspberry Pi, Mini‑PC, GPU NVIDIA/AMD/Intel, RAM limitée, faible consommation énergétique, etc.).
- **Objectif long terme** : devenir le plus grand observatoire open source mondial des performances réelles des modèles IA locaux, rester libre, open source, transparent, communautaire, multiplateforme et indépendant des fournisseurs commerciaux.

## 3. Fonctionnalités détaillées

### 3.1 Détection matérielle
- Exécution de commandes système (`lscpu`, `dmidecode`, `lsblk`, `free`, `nvidia-smi`, `rocminfo`, `glxinfo`, `vcgencmd`, etc.) ou utilisation de bibliothèques cross‑platform (psutil, GPUtil, cpuinfo, etc.).
- Normalisation des valeurs (fréquence réelle vs. fréquence de base, RAM disponible vs. totale, etc.).
- Export au format JSON pour consommation par les autres modules.

### 3.2 Catalogue de modèles
- Connecteurs dédiés à chaque source (API REST, GraphQL, scrapping léger, dépôts Git, etc.).
- Filtrage pour exclure les modèles propriétaires ou payants sauf autorisation explicite de l’utilisateur.
- Mise en cache des métadonnées (nom, taille, licence, architecture, quantisation, URL de téléchargement).
- Possibilité d’ajouter/supprimer des sources via fichier de configuration.

### 3.3 Sélection & stratégie
- Module de stratégie qui reçoit la profil hardware et renvoie une liste ordonnée de modèles à tester selon le mode choisi.
- Possibilité de définir des seuils personnalisés (ex. : RAM max, temps de réponse max, consommation énergie max).

### 3.4 Téléchargement & intégrité
- Téléchargement parallèle avec reprise (curl/wget ou bibliothèque HTTP).
- Vérification de somme de contrôle (SHA256) lorsqu’elle est fournie.
- Extraction éventuelle (archives .tar, .zip) et placement dans un répertoire de modèles local (`~/.local-ai-benchmark/models/`).

### 3.5 Benchmark
- Lancement du modèle via son moteur d’inférence approprié (ollama run, llama.cpp, ect.).
- Pré‑chauffage (warm‑up) de quelques inferences pour stabiliser les fréquences CPU/GPU.
- Mesure précise du temps (time.perf_counter) et utilisation de psutil pour suivre ressources pendant l’inférence.
- Génération d’un rapport détaillé par modèle (JSON + éventuellement markdown).

### 3.6 Évaluation qualitative
- Jeux de questions/réponses pré‑définis stockés dans un répertoire `tests/`.
- Script d’évaluation qui invoque le modèle, collecte réponses, compare à des références (exactitude, pertinence) via métriques simples (exactitude, BLEU approximatif, etc.).
- Possibilité d’étendre les jeux de tests via contributions communautaires.

### 3.7 Notation & agrégation
- Fonction de score normalisée (0‑100) pour chaque critère.
- Scores globaux calculés selon poids configurables (par défaut : qualité 30 %, vitesse 25 %, mémoire 20 %, énergie 15 %, communauté 10 %).
- Mise à jour incrémentale du classement lorsqu’un nouveau résultat est soumis.

### 3.8 Base communautaire
- API légère (ou simples fichiers JSON lignes) permettant d’envoyer anonymement :
  ```
  {
    "cpu": "...",
    "ram_total_gb": ...,
    "gpu": "...",
    "os": "...",
    "model": "...",
    "tokens_per_sec": ...,
    "latency_ms": ...,
    "mem_used_mb": ...
  }
  ```
- Serveur de agrégation (peut être statique GitHub Pages ou petit service) qui calcule moyennes, médianes et écarts‑type par configuration.

### 3.9 Classements & recommandations
- Génération de tableaux markdown mis à jour périodiquement (via cron ou GitHub Actions).
- Endpoint de recommandation qui, donné un profil matériel, retourne le top N selon la base communautaire.

## 4. Contraintes non fonctionnelles
- **Licence** : Open source (MIT ou Apache‑2.0).
- **Gratuité** : seuls les modèles et outils libres/open source sont utilisés par défaut ; aucun appel à des modèles payants ou services propriétaires sauf consentement explicite de l’utilisateur.
- **Multiplateforme** : Linux (distributions principales), Windows, macOS, Raspberry Pi (ARM), autres SBC.
- **Confidentialité** : aucune donnée personnelle n’est collectée ni transmise sans anonymisation explicite.
- **Extensibilité** : architecture modulaire (plug‑in) pour ajouter facilement de nouvelles sources de modèles, de nouveaux benchmarks ou de nouveaux moteurs d’inférence.
- **Performance** : le détecteur et le planificateur doivent être légers (< 50 Mo RAM, < 2 s de démarrage) afin de ne pas fausser les mesures.

## 5. Livrables
- Code source hébergé sur GitHub (référentiel `universal-local-ai-benchmark`).
- Documentation (`README.md`, `CONTRIBUTING.md`, licence).
- Scripts d’installation (script shell ou `make`).
- Exécutables / paquets pour les principales plateformes (AppImage, .exe, brew, chocolatey, etc.).
- Tableau de bord communautaire (GitHub Pages ou similaire) montrant les classements et permettant la soumission de résultats.
- CI/CD basique (GitHub Actions) garantissant que le détecteur hardware et le benchmark d’un modèle tiny (ex. : `TinyLlama` ou `phi‑2`) passent sur chaque push.

## 6. Planning indicatif (phases)
| Phase | Objectif | Durée estimée |
|-------|----------|---------------|
| 0 – Initialisation | Repo, licence, README, .gitignore | 1 sem |
| 1 – Détection matériel | Module hardware, sortie JSON | 2 sem |
| 2 – Catalogue modèles | Connecteurs Ollama + HF + llama.cpp, cache | 3 sem |
| 3 – Stratégie de sélection | Modes rapides/équilibrés/qualité | 2 sem |
| 4 – Téléchargement & intégrité | Téléchargement resumable, vérif SHA | 2 sem |
| 5 – Benchmark de base | Lancement d’un modèle simple, mesure tokens/s | 3 sem |
| 6 – Évaluation qualitative | Jeux de tests raisonnement & programmation | 3 sem |
| 7 – Système de notation | Scores, agrégation, poids configurables | 2 sem |
| 8 – Base communautaire | API de soumission, agrégation simple | 3 sem |
| 9 – Classements & recommandations | Génération de tableaux, endpoint de recommandation | 2 sem |
|10 – Packaging & CI/CD | Builds multiplateforme, tests automatisés | 2 sem |
|11 – Documentation & mise en communauté | Guides, tutoriels, appel à contributions | continu |

**Total estimé** : ~6 mois pour une première version utilisable, puis améliorations itératives grâce aux retours communautaires.

---

*Ce cahier des charges constitue la base de référence pour le développement du projet Universal Local AI Benchmark & Recommendation System. Toute évolution devra y être conforme ou fera l’objet d’une mise à jour concertée avec la communauté.* 