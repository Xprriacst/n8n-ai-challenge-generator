# Configuration de l'Approche Base de Données

## 🎯 Avantages de cette approche

**✅ Plus simple** : Un seul workflow au lieu de deux
**✅ Plus fiable** : Pas de dépendance à l'API n8n
**✅ Temps réel** : L'agent lit directement depuis la DB
**✅ Moins de configuration** : Juste une connexion PostgreSQL

## 📋 Étapes de configuration

### 1. Créer la base de données

Exécutez le script `database-setup.sql` dans votre base PostgreSQL :

```sql
-- Création de la table prompts
CREATE TABLE IF NOT EXISTS prompts (
    id SERIAL PRIMARY KEY,
    prompt_text TEXT NOT NULL,
    active BOOLEAN DEFAULT true,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### 2. Configurer la connexion PostgreSQL dans n8n

Dans n8n, allez dans **Settings > Credentials** et créez :

**Credential Type:** `PostgreSQL`
**Name:** `PostgreSQL Database`
**Host:** `[VOTRE_HOST_POSTGRES]`
**Database:** `[NOM_DE_VOTRE_DB]`
**Username:** `[USERNAME]`
**Password:** `[PASSWORD]`
**Port:** `5432` (par défaut)

### 3. Importer le nouveau workflow

1. Importez `workflow-defis-with-database.json`
2. Configurez la credential PostgreSQL sur tous les nœuds DB
3. Activez le workflow

### 4. Tester la configuration

**Webhooks disponibles :**
- `https://polaris-ia.app.n8n.cloud/webhook/ui-defis-final` (génération)
- `https://polaris-ia.app.n8n.cloud/webhook/save-prompt` (sauvegarde prompt)

## 🔧 Comment ça fonctionne

### Génération de défis :
1. **Parse Input Data** → traite la requête
2. **Get Current Prompt** → lit le prompt actif depuis la DB
3. **AI Agent** → utilise le prompt de la DB
4. **Validation & Response** → retourne les défis

### Sauvegarde de prompt :
1. **Webhook Save Prompt** → reçoit le nouveau prompt
2. **Parse Save Prompt** → valide les données
3. **Deactivate Old Prompts** → désactive les anciens prompts
4. **Insert New Prompt** → insère le nouveau prompt actif
5. **Success Response** → confirme la sauvegarde

## 🧪 Test manuel

**Sauvegarder un prompt :**
```bash
curl -X POST https://polaris-ia.app.n8n.cloud/webhook/save-prompt \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Nouveau prompt personnalisé...",
    "timestamp": "2025-01-21T09:50:00Z"
  }'
```

**Générer des défis :**
```bash
curl -X POST https://polaris-ia.app.n8n.cloud/webhook/ui-defis-final \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "Je veux...=Développer mon réseau&Combien de défi à tu relevé=3"
```

## ⚠️ Points importants

- La table `prompts` doit exister avant d'importer le workflow
- Seul le prompt avec `active=true` le plus récent est utilisé
- Les anciens prompts sont conservés mais désactivés
- La connexion PostgreSQL doit être accessible depuis n8n
