# Configuration du Workflow Update-Prompt

## 📋 Étapes de configuration

### 1. Créer les credentials API n8n

Dans n8n, allez dans **Settings > Credentials** et créez :

**Credential Type:** `HTTP Header Auth`
**Name:** `N8N API Token`
**Header Name:** `X-N8N-API-KEY`
**Header Value:** `[VOTRE_API_TOKEN_N8N]`

### 2. Configurer les variables d'environnement

Dans n8n, allez dans **Settings > Environment Variables** et ajoutez :

```
N8N_HOST=https://polaris-ia.app.n8n.cloud
MAIN_WORKFLOW_ID=[ID_DE_VOTRE_WORKFLOW_PRINCIPAL]
```

### 3. Trouver l'ID de votre workflow principal

1. Ouvrez votre workflow "n8n-workflow-defis-ia-updated"
2. L'ID se trouve dans l'URL : `https://polaris-ia.app.n8n.cloud/workflow/[WORKFLOW_ID]`
3. Copiez cet ID dans la variable `MAIN_WORKFLOW_ID`

### 4. Obtenir votre API Token n8n

1. Dans n8n, allez dans **Settings > API Keys**
2. Créez une nouvelle API Key
3. Copiez le token dans le credential `N8N API Token`

### 5. Importer et activer le workflow

1. Importez le fichier `workflow-update-prompt.json` dans n8n
2. Configurez les credentials sur les nœuds HTTP Request
3. Activez le workflow

### 6. Tester la configuration

Le webhook sera disponible à :
```
https://polaris-ia.app.n8n.cloud/webhook/update-prompt
```

## 🔧 Comment ça fonctionne

1. **Interface Web** envoie le nouveau prompt via POST
2. **Webhook** reçoit et parse les données
3. **API n8n** récupère le workflow principal actuel
4. **Modification** du nœud "AI Agent" avec le nouveau prompt
5. **Sauvegarde** du workflow modifié
6. **Réponse** de confirmation à l'interface

## ⚠️ Points importants

- Le workflow principal doit avoir un nœud nommé exactement **"AI Agent"**
- L'API Token doit avoir les permissions de lecture/écriture sur les workflows
- Les variables d'environnement sont sensibles à la casse
- Testez d'abord avec un workflow de test avant de modifier le principal

## 🧪 Test manuel

Vous pouvez tester avec curl :

```bash
curl -X POST https://polaris-ia.app.n8n.cloud/webhook/update-prompt \
  -H "Content-Type: application/json" \
  -d '{
    "prompt": "Nouveau prompt de test...",
    "timestamp": "2025-01-21T09:50:00Z"
  }'
```
