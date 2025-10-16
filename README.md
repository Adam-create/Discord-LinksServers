# Bot Discord Récupérateur de Liens - Guide Complet

Ce projet consiste à créer un bot Discord capable de récupérer automatiquement tous les liens partagés dans les salons Discord et de les organiser dans des fichiers structurés. Voici comment procéder pour créer et déployer ce bot sur vos deux serveurs Discord.

## Architecture et Fonctionnement

Le bot fonctionne selon le principe suivant : il surveille en permanence tous les messages postés dans les salons Discord où il a accès, extrait automatiquement les URLs présentes avec des expressions régulières, et les sauvegarde avec des métadonnées complètes (auteur, salon, serveur, timestamp, etc.) dans des fichiers CSV ou JSON.

### Fonctionnalités principales

**Collecte automatique** : Le bot détecte et sauvegarde automatiquement tous les liens partagés dans les messages.

**Commandes manuelles** : 
- `!scrape <limite>` pour scanner l'historique d'un salon spécifique
- `!scrape_server <limite>` pour scanner tous les salons d'un serveur
- `!export <format>` pour exporter les données collectées
- `!stats` pour afficher les statistiques

**Gestion intelligente des rate limits** : Le bot respecte automatiquement les limites d'API Discord avec des délais et retry automatiques.

**Stockage flexible** : Sauvegarde en CSV ou JSON avec métadonnées complètes.

## Structure du Projet - Arborescence du Code

```plaintext
discord-link-bot/
├── src/
│   ├── bot.py                 # Fichier principal du bot
│   ├── commands/
│   │   ├── scrape.py          # Commandes de récupération
│   │   └── export.py          # Commandes d'export
│   ├── utils/
│   │   ├── link_extractor.py  # Logique d'extraction d'URLs
│   │   ├── data_manager.py    # Gestion du stockage des données
│   │   └── rate_limiter.py    # Gestion des rate limits
│   └── models/
│       └── link_data.py       # Modèles de données
├── config/
│   ├── config.json            # Configuration du bot
│   └── .env.example           # Template des variables d'environnement
├── data/
│   ├── links.csv              # Fichiers CSV générés
│   └── logs/                  # Fichiers de logs
├── tests/
│   ├── test_scraper.py        # Tests du scraper
│   └── test_utils.py          # Tests des utilitaires
├── Dockerfile                 # Conteneurisation Docker
├── docker-compose.yml         # Orchestration Docker
├── requirements.txt           # Dépendances Python
├── .gitignore                 # Fichiers ignorés par Git
└── README.md                  # Documentation du projet
```

## Implémentation Technique

Le code principal utilise discord.py avec une architecture orientée objets. Le bot hérite de `commands.Bot` et intègre :

- **Détection automatique des URLs** via regex optimisées
- **Métadonnées complètes** : serveur, salon, auteur, timestamp, contenu partiel du message
- **Gestion d'erreurs robuste** avec logging détaillé
- **Rate limiting intelligent** pour éviter les bannissements temporaires

### Extraction des liens

Le bot utilise une expression régulière sophistiquée pour détecter tous les types d'URLs :

```python
url_pattern = re.compile(
    r'http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\\(\\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+'
)
```

## Configuration et Installation

### Prérequis Discord

1. **Créer l'application** sur le Discord Developer Portal
2. **Activer les intents privilégiés** : Message Content Intent est essentiel pour lire le contenu des messages
3. **Générer les liens d'invitation** avec les permissions appropriées
4. **Inviter le bot** sur vos deux serveurs Discord

### Installation Python

Le projet nécessite Python 3.9+ et les dépendances suivantes :
- discord.py >= 2.3.0
- python-dotenv >= 1.0.0
- aiofiles >= 23.1.0
- asyncio-throttle >= 1.0.2

### Configuration des variables d'environnement

Créez un fichier `.env` :

```env
DISCORD_TOKEN=votre_token_bot_ici
DISCORD_CLIENT_ID=votre_client_id_ici
LOG_LEVEL=INFO
MAX_HISTORY=1000
```

## Stockage et Gestion des Données

**CSV** : Format tabulaire idéal pour l'analyse

**JSON** : Format structuré conservant tous les types de données

### Métadonnées collectées

Pour chaque lien détecté, le bot sauvegarde : URL, identifiants serveur/salon, auteur, timestamp, extrait du message, ID du message.

## Déploiement et Hébergement

### Recommandations de déploiement

- **Oracle Cloud Free Tier** : Gratuit, 24GB RAM, 4 vCPUs
- **Railway** : ~5$/mois, auto-scaling
- **VPS classique** : Contrôle total
- **Docker self-hosted** : Portabilité via Docker

### Exemple Dockerfile

```dockerfile
FROM python:3.11-slim
WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY . .
CMD ["python", "src/bot.py"]
```

## Considérations Légales et Éthiques

- Respecter les rate limits Discord
- Obtenir le consentement des utilisateurs
- Ne pas stocker de données sensibles sans autorisation

## Monitoring et Maintenance

- **Logs** : Fichiers journaux des opérations et erreurs
- **Sauvegarde automatique** : Tâches périodiques pour exporter les données
- **Nettoyage des données** : Suppression des anciennes données

## Évolutions et Personnalisations

- Filtrage avancé des domaines
- Interface web pour visualiser les statistiques
- Intégration CI/CD via GitHub Actions
