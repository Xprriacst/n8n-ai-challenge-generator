# Générateur de Défis Personnalisés IA

Un générateur de défis de développement personnel utilisant n8n et OpenAI GPT-4 pour créer des micro-challenges personnalisés basés sur les problématiques des utilisateurs.

## 🚀 Fonctionnalités

- **Interface web moderne** avec 6 catégories thématiques
- **Génération automatique** de 15 défis personnalisés
- **Détection du niveau** (débutant/intermédiaire/avancé) basée sur l'expérience
- **Workflow n8n** avec intégration OpenAI GPT-4
- **Validation et nettoyage** des réponses IA
- **Système de fallback** en cas d'erreur

## 📁 Structure du projet

### Fichiers principaux
- `index.html` - Interface utilisateur principale
- `workflow-defis-google-sheets-v2.json` - **Workflow n8n recommandé** (AI Agent)
- `workflow-defis-google-sheets-fixed.json` - Version alternative (OpenAI Chat)
- `workflow-defis-google-sheets.json` - Version Google Sheets de base

### Documentation et configuration
- `SETUP-*.md` - Guides de configuration détaillés
- `database-setup.sql` - Script de base de données (optionnel)
- `workflow-update-prompt.json` - Workflow de mise à jour des prompts

### Archive
- `archive/` - Versions de développement et fichiers de test archivés

## 🛠 Configuration

### Prérequis
- Compte n8n Cloud ou self-hosted
- Clé API OpenAI
- Accès Google Sheets (optionnel)

### Déploiement
1. Importez le workflow `workflow-defis-google-sheets-v2.json` dans n8n
2. Configurez vos credentials OpenAI
3. Activez le workflow
4. Ouvrez `index.html` dans un navigateur

### URL du webhook
```
https://polaris-ia.app.n8n.cloud/webhook/ui-defis-final
```

## 🎯 Catégories disponibles

- **Mental & émotionnel** - Gestion des émotions, résilience, lâcher-prise
- **Relations & communication** - Écoute active, networking, gestion des conflits
- **Argent & carrière** - Entrepreneuriat, diversification, prise de risque
- **Santé & habitudes** - Dépendances, forme physique
- **Productivité & concentration** - Organisation, motivation, priorités
- **Confiance & identité** - Estime de soi, affirmation, acceptation

## 🔧 Architecture technique

### Workflow n8n
1. **Webhook Reception** - Reçoit les requêtes POST
2. **Parse Input Data** - Traite les données du formulaire
3. **Get Prompt from Sheets** - Récupère le prompt depuis Google Sheets
4. **Create Chat Input** - Prépare le prompt avec variables
5. **AI Agent** - Génère les défis avec OpenAI GPT-4
6. **Validation & Nettoyage** - Valide et nettoie la réponse JSON
7. **Response Final** - Retourne les défis avec headers CORS

### Frontend
- **Tailwind CSS** pour le styling
- **JavaScript vanilla** pour les interactions
- **Fetch API** pour les appels webhook
- **Fallback intelligent** si l'API est indisponible

## 📊 Format de réponse

```json
{
  "problematique": "Je veux travailler sur: [problématique]",
  "niveau_detecte": "débutant|intermédiaire|avancé",
  "defis": [
    {
      "numero": 1,
      "nom": "Nom du défi",
      "mission": "Action concrète à réaliser",
      "pourquoi": "Explication du bénéfice",
      "bonus": "Action supplémentaire (optionnel)",
      "duree_estimee": "15"
    }
  ]
}
```

## 🚀 Utilisation

1. Ouvrez `index.html` dans votre navigateur
2. Sélectionnez le nombre de défis déjà relevés
3. Cliquez sur une catégorie thématique
4. Attendez la génération des 15 défis personnalisés
5. Consultez vos défis avec explications et durées estimées

## 🔒 Sécurité

- Headers CORS configurés
- Validation stricte des réponses JSON
- Gestion d'erreurs robuste
- Possibilité d'ajouter HMAC validation

## 📝 Versions

- **v2** (recommandée) - AI Agent avec prompt optimisé
- **fixed** - OpenAI Chat Model direct
- **original** - Version de base

## 🤝 Contribution

Le projet est prêt pour la production. Pour des améliorations :
1. Forkez le repository
2. Créez une branche feature
3. Committez vos changements
4. Ouvrez une Pull Request

---

Développé avec ❤️ pour le développement personnel automatisé
