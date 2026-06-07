# Universal Local AI Benchmark & Recommendation System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub issues](https://img.shields.io/github/issues/micoco1985/universal-local-ai-benchmark)](https://github.com/micoco1985/universal-local-ai-benchmark/issues)
[![GitHub stars](https://img.shields.io/github/stars/micoco1985/universal-local-ai-benchmark)](https://github.com/micoco1985/universal-local-ai-benchmark/stargazers)
[![GitHub last commit](https://img.shields.io/github/last-commit/micoco1985/universal-local-ai-benchmark)](https://github.com/micoco1985/universal-local-ai-benchmark/commits/main)
[![Python version](https://img.shields.io/badge/python-3.12%2B-blue)](https://www.python.org/downloads/)

## 🚀 Vision
Créer une plateforme libre, open source et communautaire permettant à n'importe quel utilisateur de déterminer automatiquement quels modèles d'intelligence artificielle locaux sont les plus adaptés à sa machine.

## 📋 Fonctionnalités prévues
- Détection automatique du hardware (CPU, RAM, GPU, swap, température, etc.)
- Découverte des modèles IA locaux via Ollama, Hugging Face, llama.cpp, etc.
- Benchmark automatisé (tokens/s, latence, consommation RAM/VRAM, etc.)
- Scores multiples (qualité, vitesse, mémoire, énergie, compatibilité, communauté)
- Base de données communautaire anonymisée pour améliorer les recommandations
- Modes de sélection : rapide, équilibré, qualité maximale, expérimental, communautaire
- Gestion automatique des modèles (conserver, archiver, supprimer, top N)

## 🛠️ Installation
```bash
# Cloner le dépôt
git clone https://github.com/micoco1985/universal-local-ai-benchmark.git
cd universal-local-ai-benchmark

# (Optionnel) créer un environnement virtuel
python -m venv venv
source venv/bin/activate   # sous Windows: venv\Scripts\activate

# Installer les dépendances
pip install -r requirements.txt
```

## 🧪 Utilisation rapide
```bash
# Détecter le hardware de votre machine
python -m hardware detect

# Lister les modèles disponibles (gratuits uniquement)
python -m models list

# Benchmarker un modèle spécifique (ex: TinyLlama via Ollama)
python -m benchmark run --model tinyllama

# Obtenir une recommandation personnalisée
python -m recommend
```

## 📊 Exemple de sortie
Après un benchmark, vous obtenez un tableau similaire :

| Modèle       | Tokens/s | Latence (ms) | RAM (Go) | Qualité | Score Global |
|--------------|----------:|-------------:|---------:|--------:|-------------:|
| TinyLlama    |      12.4 |          85  |     2.1  |   0.78  |        0.81 |
| Phi-2-mini   |       9.1 |         112  |     1.8  |   0.74  |        0.76 |
| ...          |        ...|          ... |      ... |     ... |          ... |

## 🤝 Contribuer
Les contributions sont les bienvenues ! Merci de suivre :
1. Fork le repository
2. Créer une branche (`git checkout -b feature/amazing`)
3. Commit vos changements (`git commit -m 'Add amazing feature'`)
4. Push vers la branche (`git push origin feature/amazing`)
5. Ouvrir une Pull Request

Consultez le fichier [CONTRIBUTING.md](CONTRIBUTING.md) pour plus de détails (style de code, tests, etc.).

## 📜 Licence
Ce projet est sous licence MIT - voir le fichier [LICENSE](LICENSE) pour plus de détails.

## 🙏 Remerciements
Merci à la communauté open source pour les outils et modèles gratuits qui rendent ce projet possible.