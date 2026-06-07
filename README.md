# Universal Local AI Benchmark & Recommendation System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub issues](https://img.shields.io/github/issues/micoco1985/universal-local-ai-benchmark)](https://github.com/micoco1985/universal-local-ai-benchmark/issues)
[![GitHub stars](https://img.shields.io/github/stars/micoco1985/universal-local-ai-benchmark)](https://github.com/micoco1985/universal-local-ai-benchmark/stargazers)
[![GitHub last commit](https://img.shields.io/github/last-commit/micoco1985/universal-local-ai-benchmark)](https://github.com/micoco1985/universal-local-ai-benchmark/commits/main)
[![Python version](https://img.shields.io/badge/python-3.12%2B-blue)](https://www.python.org/downloads/)
[![Code style: black](https://img.shields.io/badge/code%20style-black-000000.svg)](https://github.com/psf/black)

## 🚀 Vision étendue

Notre ambition dépasse la simple comparaison de modèles : nous voulons créer le **premier observatoire mondial ouvert et communautaire des performances réelles des modèles d'IA locaux**, capable de :

- **Démocratiser l'accès à l'IA** en permettant à chacun, quel que soit son matériel (ancien laptop, Raspberry Pi, serveur de bureau, NAS, etc.), de trouver immédiatement le modèle le mieux adapté à ses besoins.
- **Fournir des recommandations fiables et personnalisées** basées sur des milliers de benchmarks réels, agrégés de façon anonyme.
- **Stimuler l'innovation ouverte** en mettant à disposition un cadre modulaire où chacun peut ajouter de nouvelles sources de modèles, de nouveaux benchmarks ou de nouvelles métriques.
- **Assurer la transparence** grâce à des données ouvertes, des méthodes de mesure reproductibles et une licence permissive (MIT).
- **Construire une intelligence collective** où chaque contribution améliore la qualité du service pour tous, à la manière de projets comme SETI@home ou Folding@home, mais pour l'évaluation d'IA locale.

## 🌌 Vision avant‑gardiste

Nous envisageons d’aller bien au‑dessus du simple benchmark :

- **IA neuromorphique & quantique** : dès que des puces neuromorphiques (Intel Loihi, BrainChip Akida) ou des simulateurs quantiques deviennent accessibles, nous les intégrerons comme nouvelles cibles de détection et de benchmark afin de guider les développeurs vers les architectures de demain.
- **Apprentissage fédéré & confidentialité différentielle** : permettre aux utilisateurs de contribuer à l’amélioration des modèles sans jamais quitter leur appareil, en agrégeant uniquement des mises à jour de poids chiffrés.
- **Modèles auto‑optimisables** : utiliser des techniques de recherche d’architecture (NAS, reinforcement learning) pour que le système suggère non seulement quel modèle utiliser, mais aussi comment le ré‑arquitector pour un matériel donné.
- **Jumeaux numériques du matériel** : créer un profil matériel virtuel simulé (via QEMU, gem5, ou des simulateurs de GPU) permettant de prédire les performances d’un modèle avant même de le télécharger, réduisant ainsi le gaspillage énergétique.
- **Tableau de bord en temps réel** : offrir une vue live (WebSocket + Grafana) des tendances mondiales, avec des alertes lorsqu’un nouveau modèle dépasse un seuil d’efficacité énergétique.
- **Intégration avec les écosystèmes Edge & IoT** : fournir des binaires ultra‑légers (< 2 Mo) pour microcontrôleurs (ESP32, STM32) afin d’exécuter le détecteur et de reporter les résultats depuis le terrain.
- **Gouvernance ouverte** : mettre en place un DAO léger (via Snapshot ou Aragon) où la communauté vote sur les nouvelles fonctionnalités, les seuils de consommation énergétique acceptables, et les modèles à promouvoir ou à déprécier.
- **Pédagogie & diffusion** : produire des cours interactifs (Jupyter Books, vidéos) qui enseignent comment mesurer, comparer et optimiser l’IA locale, transformant le projet en véritable plateforme d’apprentissage.
- **Durabilité** : calculer l’empreinte carbone estimée de chaque modèle sur chaque matériel et proposer des recommandations « vertes » qui minimisent l’impact environnemental tout en maintenant la performance.

Ces ambitions placeront **Universal Local AI Benchmark** non seulement comme un outil pratique, mais comme un véritable catalyseur de la prochaine génération d’intelligence artificielle décentralisée, responsable et accessible à tous.

## 📋 Fonctionnalités détaillées

### 1. Détection matérielle autonome
- Analyse fine du CPU : architecture, nombre de cœurs physiques/logiques, fréquence réelle, instruction set (SSE, AVX, AVX2, AVX‑512, NEON, SVE, etc.).
- Mémoire : RAM totale, libre, disponible, swap total/disponible, usage en temps réel.
- GPU : détection NVIDIA (via `nvidia-smi`), AMD (via `rocminfo` ou OpenCL), Intel (via `clinfo`), ainsi que les GPU intégrés.
- Stockage : type (SSD, HDD, eMMC, NVMe), capacité, débit séquentiel/aléatoire approximatif.
- Système : distribution Linux, version du noyau, Windows build, macOS version.
- Capteurs : température (CPU, GPU, disque), consommation énergétique (si disponible via `upower`, `tlp`, `lm-sensors`).
- Export normalisé au format JSON pour consommation par les autres modules.

### 2. Catalogue et découverte de modèles locaux
- Connecteurs dédiés aux principaux hubs gratuits :
  - **Ollama** (registry locale, modèles prêts à l'emploi)
  - **Hugging Face** (modèles `gguf`, `ggml`, `onnx` libres)
  - **llama.cpp** (builds communautaires, quantifications variées)
  - **LM Studio**, **GPT4All**, **KoboldCPP**, **vLLM**, **MLX**, **PrivateGPT**, **Text Generation WebUI**, etc.
- Filtrage automatisé : exclure les modèles propriétaires ou nécessitant une licence commerciale sauf autorisation explicite de l'utilisateur.
- Mise en cache intelligente des métadonnées (nom, taille, architecture, type de quantification, licence, URL de téléchargement, dépendances).
- Possibilité d'ajouter/supprimer des sources via un fichier de configuration `sources.yaml`.
- Gestion des versions : détection des mises à jour et proposition de re‑benchmark.

### 3. Stratégies de sélection intelligente
Chaque stratégie reçoit le profil matériel et renvoie une liste ordonnée de modèles à tester :

| Mode | Objectif | Critères principaux |
|------|----------|--------------------|
| **Rapide** | Trouver le premier modèle utilisable immédiatement | Temps de chargement < 2 s, RAM < 50 % disponible |
| **Équilibré** | Meilleur compromis qualité‑vitesse‑mémoire | Score global pondéré (voir §5) |
| **Qualité maximale** | Maximiser la qualité indépendamment du coût | Qualité ≥ 0,85, pas de contrainte de vitesse |
| **Expérimental** | Explorer l'ensemble des modèles compatibles | Tous les modèles passés les filtres de base |
| **Communautaire** | Se baser exclusivement sur les modèles plébiscités par la communauté | Score communautaire ≥ seuil défini |

Les seuils sont réglables dans `config.yaml`.

### 4. Téléchargement et vérification d'intégrité
- Téléchargement parallèle avec reprise (utilisation de `aiohttp` ou `requests` + `tqdm`).
- Vérification de sommes de contrôle (SHA256, SHA512) lorsque fournies.
- Extraction automatique des archives (`.tar.gz`, `.zip`, `.7z`).
- Installation dans un répertoire utilisateur (`~/.local-ai-benchmark/models/`) afin d'éviter les conflits système.
- Journalisation détaillée des étapes (taille téléchargée, temps, empreinte).

### 5. Benchmark automatisé et reproductible
Pour chaque modèle, nous exécutons un protocole standardisé :

#### Étapes communes
1. **Warm‑up** : 3 inferences sans mesure pour stabiliser les fréquences CPU/GPU et charger le modèle en mémoire.
2. **Mesure des ressources système** : utilisation de `psutil` pour suivre CPU, RAM, swap, température, puissance (si disponible) avant, pendant et après l'inférence.
3. **Chronométrage de haute précision** : `time.perf_counter()` avec résolution microsecondes.
4. **Génération de texte** : prompt fixe (ex. : « Explain the theory of relativity in two paragraphs ») afin de garantir la même charge de travail.
5. **Collecte des métriques IA** :
   - Temps jusqu'au premier token (TTFT)
   - Tokens par seconde (TPS)
   - Latence totale
   - Longueur maximale de contexte testée (augmentation progressive jusqu'à échec)
   - Pour les modèles multimodaux : temps de traitement d'image, de document, d'audio (si applicable).

#### Résultats
- Export JSON par modèle contenant toutes les mesures.
- Optionnel : génération d'un tableau Markdown récapitulatif.

### 6. Évaluation qualitative (benchmark de compétences)
Au-delà de la performance brute, nous testons la pertinence du modèle via des jeux de questions :

| Domaine | Exemples de tests |
|---------|-------------------|
| **Raisonnement** | Problèmes de logique, équations simples, déductions syllogistiques |
| **Programmation** | Génération de fonctions Python/C/Bash/JS à partir de descriptions, débuguage de snippets |
| **Langues** | Traduction FR↔EN, génération de texte dans un style donné, réponse à des questions de culture générale |
| **Connaissances générales** | Questions historiques, scientifiques, technologiques |
| **Résumé de documents** | Condensation d'un passage de 500 mots en 80 mots tout en conservant l'information clé |
| **Analyse de fichiers** | Comptage de lignes, extraction de mots-clés, détection de langage de programmation |
| **Vision** (si modèle multimodal) | Description d'image, réponse à des questions basées sur l'image, OCR simple |

Chaque test retourne un score de 0 à 1 (ou un pourcentage). Ces scores alimentent le critère **qualité** du système de notation.

### 7. Système de notation et agrégation
- Chaque critère (qualité, vitesse, mémoire, énergie, compatibilité, communauté) reçoit un score normalisé entre 0 et 1.
- Formule du score global :

```
Score Global = w_q * Qualité
             + w_v * Vitesse
             + w_m * Mémoire
             + w_e * Énergie
             + w_c * Compatibilité
             + w_com * Communauté
```

où les poids (`w_*`) sont définis dans `config.yaml` (par défaut : 0.30, 0.25, 0.20, 0.15, 0.05, 0.05).

- Les scores sont mis à jour de façon incrémentale lorsqu'un nouveau résultat est soumis.
- Un historique des scores par version de modèle permet de suivre l'impact des mises à jour.

### 8. Gestion automatisée des modèles
Selon les préférences de l'utilisateur (définies dans `preferences.yaml`) :
- **Conserver** le modèle indéfiniment.
- **Archiver** (déplacer vers un répertoire d'archive, préservant les métadonnées).
- **Supprimer** définitivement.
- **Garder uniquement le Top N** (ex. : garder les 3 meilleurs modèles selon le score global, supprimer les autres).
- **Supprimer les modèles trop lents** (seuil de TPS ou de TTFT).
- **Supprimer les modèles dépassant une limite mémoire** (ex. : > 4 Go sur une machine avec 8 Go de RAM).

### 9. Base de données communautaire et intelligence collective
- **Soumission anonyme** : les utilisateurs peuvent envoyer via une simple requête HTTP POST (ou via le CLI) un payload JSON contenant :
  ```json
  {
    "cpu_model": "...",
    "cpu_cores": ...,
    "cpu_threads": ...,
    "ram_total_gb": ...,
    "gpu_model": "...",
    "gpu_vram_gb": ...,
    "os": "...",
    "os_version": "...",
    "model_name": "...",
    "model_version": "...",
    "tokens_per_sec": ...,
    "ttft_ms": ...,
    "latency_ms": ...,
    "ram_used_mb": ...,
    "quality_score": ...,
    "global_score": ...,
    "timestamp": "..."
  }
  ```
- Aucun champ d'identification personnelle (adresse IP, nom d'utilisateur, etc.) n'est stocké ; l'adresse IP est immédiatement hachée si nécessaire pour la lutte contre le spam.
- **Agrégation côté serveur** (GitHub Pages + fonctions serverless ou petit service dédié) : calcul de la moyenne, médiane, écart-type et nombre de contributions par profil matériel.
- **Endpoint de recommandation** : donné un profil matériel, renvoie le Top N modèles les mieux notés dans la base communautaire pour ce profil.

### 10. Classements mondiaux et visualisations
- Génération périodique (via GitHub Actions cron) de tableaux Markdown classant les modèles par catégorie :
  - Meilleur modèle pour CPU ancien (< 2 cœurs, < 2 GHz)
  - Meilleur modèle pour Raspberry Pi 4
  - Meilleur modèle pour GPU NVIDIA d'entrée de gamme (GTX 1650, etc.)
  - Meilleur modèle sous contrainte de RAM (< 4 Go)
  - Meilleur modèle basse consommation (< 10 W)
- Graphiques interactifs (optionnels) utilisant Plotly ou Chart.js hébergés sur GitHub Pages.

### 11. Extensibilité et architecture modulaire
Le projet suit une architecture en couches :

```
+---------------------+
|   CLI / API Layer   |
+---------------------+
|   Orchestrator      |  ← sélecteur de stratégie, planificateur de tâches
+---------------------+
|   Modules           |
|  - HWDetector       |
|  - ModelCatalogue   |
|  - Downloader       |
|  - BenchmarkRunner  |
|  - QualitativeEval  |
|  - Scorer           |
|  - ModelManager     |
|  - CommunityClient  |
+---------------------+
|   Storage / Cache   |
+---------------------+
```

Chaque module expose une API stable (fonctions Python ou point d'entrée CLI) permettant de le remplacer ou de l'étendre via un système de **plug‑ins** (dossier `plugins/`).

### 12. Contraintes non fonctionnelles (rappel et approfondissement)
- **Licence** : MIT (permissive, compatible avec un usage académique et commercial).
- **Gratuité par défaut** : seuls les modèles et outils libres/open‑source sont appelés. Un commutateur `allow_paid_models` (désactivé par défaut) doit être explicitement activé par l'utilisateur pour sortir du périmètre gratuit.
- **Multiplateforme** : Linux (Ubuntu, Debian, Fedora, Arch, openSUSE), Windows 10/11, macOS ≥ 12, Raspberry Pi OS, autres distributions ARM.
- **Confidentialité** : aucune donnée personnelle n'est collectée. Les adresses IP sont hachées (SHA‑256) si besoin pour le filtrage anti‑spam, puis immédiatement supprimées après agrégation.
- **Performance du dispositif de détection** : < 50 Mo RAM, < 2 s de démarrage sur une machine modeste afin de ne pas fausser les mesures.
- **Reproductibilité** : toutes les étapes sont versionnées (code, dépendances, paramètres) et peuvent être relancées avec les mêmes résultats (sauf variations matérielles normales).
- **Extensibilité** : ajout de nouvelles sources, de nouveaux benchmarks ou de nouvelles métriques nécessite uniquement la création d'un nouveau module respectant l'interface définie.

### 13. Livrables attendus
- **Code source** : dépôt GitHub `universal-local-ai-benchmark`.
- **Documentation complète** :
  - `README.md` (ce fichier)
  - `INSTALLATION.md` (guide pas‑à‑pas)
  - `CONFIGURATION.md` (explication des fichiers YAML)
  - `API_REFERENCE.md` (description des points d'entrée programmables)
  - `ARCHITECTURE.md` (diagrammes et explications détaillées)
  - `CHANGELOG.md` (historique des versions)
- **Scripts d'installation** :
  - `install.sh` (Linux/macOS)
  - `install.bat` (Windows)
- **Paquets prêts à l'emploi** :
  - AppImage (Linux universel)
  - `.exe` signé (Windows)
  - Homebrew formula (macOS)
  - Chocolatey package (Windows)
- **Tableau de bord communautaire** : site GitHub Pages montrant les classements, les formulaires de soumission et les visualisations.
- **CI/CD** :
  - Tests unitaires (pytest) sur chaque push.
  - Workflow de benchmark léger (ex. : benchmark du modèle `TinyLlama` via Ollama) pour vérifier que rien ne est cassé.
  - Déploiement automatique de la documentation sur GitHub Pages à chaque tag de version.

### 14. Feuille de route indicative (et étapes suivantes)

| Phase | Objectif | Livrable principal |
|-------|----------|--------------------|
| **0 – Fondations** | Licence, README, .gitignore, structure de dépôt | Repo prêt, documentation de base |
| **1 – Détection matériel** | Module `hardware` détectant CPU, RAM, GPU, température, sortie JSON | `python -m hardware detect` fonctionnel |
| **2 – Catalogue modèles** | Connecteurs Ollama, HuggingFace (gguf/ggml), llama.cpp + cache | `python -m models list` listant les modèles disponibles |
| **3 – Stratégies de sélection** | Modes rapide, équilibré, qualité maximale, expérimental, communautaire | `python -m select --mode équilibré` renvoie une liste ordonnée |
| **4 – Téléchargement & intégrité** | Téléchargement resumable, vérif SHA, installation locale | `python -m download --model tinyllama` |
| **5 – Benchmark de base** | Lancer un modèle (via ollama run ou llama.cpp), mesurer TPS, latence, RAM | `python -m benchmark run --model tinyllama` |
| **6 – Évaluation qualitative** | Jeux de tests raisonnement & programmation, scoring qualité | `python -m evaluate --model tinyllama` |
| **7 – Système de notation** | Agrégation des scores, poids configurables, historique | `python -m score --model tinyllama` |
| **8 – Base communautaire** | Endpoint de soumission anonyme, agrégation simple, recommandation | `python -m recommend` utilise la base distante |
| **9 – Classements mondiaux** | Génération de tableaux Markdown via GitHub Actions, publication sur GH Pages | Page `/rankings` visible publiquement |
| **10 – Packaging & CI/CD** | Création des binaires, tests automatisés, déploiement | Release version `v0.1.0` sur GitHub |
| **11 – Documentation avancée & outreach** | Tutoriels, vidéos, webinars, appel à contributions | Blog, chaîne YouTube, présentations en meetups |
| **12 – Boucle d'amélioration continue** | Intégration des retours communauté, ajout de nouvelles sources, amélioration des métriques | Versions itératives `v0.2.x`, `v0.3.x`… |

**Total estimé** : environ 6 mois pour une première version utilisable (phases 0‑5), suivie d'améliorations itératives guidées par la communauté.

### 15. Comment contribuer ?
Nous accueillons tout type de contribution :
- **Code** : correction de bugs, nouvelles fonctionnalités, amélioration des performances.
- **Documentation** : rédaction de tutoriels, traduction, exemples d'utilisation.
- **Données** : soumettre vos benchmarks de façon anonyme pour enrichir la base communautaire.
- **Idées** : proposer de nouveaux modes de sélection, de nouveaux critères de notation, de nouvelles visualisations.
- **Tests** : écrire des tests unitaires ou d'intégration pour garantir la stabilité.

Consultez le fichier [CONTRIBUTING.md](CONTRIBUTING.md) pour le guide de style de code, le processus de pull‑request et les consignes de licence.

### 16. Licence
Ce projet est sous licence **MIT** – voir le fichier [LICENSE](LICENSE) pour plus de détails.

### 17. Remerciements
Un immense merci à la communauté open source qui met à disposition les modèles gratuits, les cadres d'inférence (ollama, llama.cpp, etc.) et les outils de mesure qui rendent ce projet possible. Merci également aux premiers testeurs qui ont partagé leurs retours précieux.

--- 

*Ce document constitue la référence actuelle du projet. Toute évolution devra y être conforme ou fera l’objet d’une mise à jour concertée avec la communauté.*