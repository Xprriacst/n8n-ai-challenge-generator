# Configuration Google Sheets - Solution Ultra-Simple

## 🎯 Avantages de cette approche

**✅ Ultra-simple** : Juste une cellule A1 dans Google Sheets
**✅ Modification instantanée** : Éditez directement dans Google Sheets
**✅ Synchronisation automatique** : n8n lit la cellule à chaque génération
**✅ Zéro configuration complexe** : Pas de base de données, pas d'API

## 📋 Étapes de configuration (2 minutes)

### 1. Créer le Google Sheet

1. Allez sur [Google Sheets](https://sheets.google.com)
2. Créez un nouveau document
3. Nommez-le **"Prompt GPT Défis"**
4. Dans la cellule **A1**, collez le prompt par défaut :

```
Tu es un expert en développement personnel. Tu vas recevoir une problématique PRÉCISE et tu DOIS créer 15 micro-défis qui correspondent EXACTEMENT à cette problématique.

PROBLÉMATIQUE À TRAITER: {{ $json.problematique }}
NOMBRE DE DÉFIS DÉJÀ RELEVÉS: {{ $json.nombreDefis }}
NIVEAU DÉTECTÉ: {{ $json.niveau }}

RÈGLE STRICTE: Analyse la problématique ci-dessus et identifie le domaine exact. Puis crée 15 défis QUI NE TRAITENT QUE DE CE DOMAINE SPÉCIFIQUE. Ne dévie JAMAIS vers d'autres domaines.

EXEMPLES DE SPÉCIALISATION STRICTE:
- "Développer mon réseau professionnel" → LinkedIn (optimiser profil, publier, commenter), événements networking, cold outreach, cartes de visite, suivi contacts, personal branding professionnel
- "Améliorer mon cardio" → course à pied, vélo, natation, HIIT, escaliers, marche rapide, fréquence cardiaque, endurance
- "Créer mon entreprise" → étude de marché, business plan, MVP, validation concept, pitch, financement, clients, marketing
- "Mieux m'organiser" → planning, to-do lists, méthodes productivité (GTD, Pomodoro), workspace, priorisation, calendrier
- "Renforcer mon estime de soi" → auto-valorisation, reconnaissance succès, affirmations positives, acceptation de soi

INTERDICTION ABSOLUE: Ne mélange JAMAIS les domaines. Si c'est du networking, ne parle PAS de confiance en soi. Si c'est du sport, ne parle PAS de méditation.

Tu dois créer des défis progressifs adaptés au niveau:
- Débutant (0-2 défis): Actions simples, premiers pas
- Intermédiaire (3-7 défis): Défis modérés, approfondissement
- Avancé (8+ défis): Challenges complexes, maîtrise

IMPORTANT: Réponds UNIQUEMENT avec du JSON valide, sans markdown, sans ```json, sans commentaires.

Structure de réponse OBLIGATOIRE:
{
  "problematique": "{{ $json.problematique }}",
  "niveau_detecte": "{{ $json.niveau }}",
  "defis": [
    {
      "numero": 1,
      "nom": "[Nom spécifique au domaine identifié]",
      "mission": "[Action concrète liée UNIQUEMENT au domaine]",
      "pourquoi": "[Explication du bénéfice pour CE domaine précis]",
      "bonus": "[Action supplémentaire ou null]",
      "duree_estimee": "10"
    }
  ]
}

ATTENTION: Reste 100% dans le domaine spécifique. Aucun défi générique autorisé.
```

5. **Partagez le document** : Clic droit → Partager → "Toute personne disposant du lien peut consulter"
6. **Copiez l'ID du document** depuis l'URL : `https://docs.google.com/spreadsheets/d/[DOCUMENT_ID]/edit`

### 2. Configurer Google Sheets OAuth dans n8n

1. Dans n8n, allez dans **Settings > Credentials**
2. Créez un nouveau credential **"Google Sheets OAuth2 API"**
3. Suivez les instructions pour configurer OAuth2
4. Nommez-le **"Google Sheets OAuth2"**

### 3. Importer et configurer le workflow

1. Importez `workflow-defis-google-sheets.json` dans n8n
2. Dans le nœud **"Get Prompt from Sheets"** :
   - Configurez le credential Google Sheets OAuth2
   - Remplacez l'ID du document par le vôtre
   - Vérifiez que la plage est bien **A1:A1**
3. Activez le workflow

## 🔧 Comment ça fonctionne

### Génération de défis :
1. **Parse Input Data** → traite la requête utilisateur
2. **Get Prompt from Sheets** → lit la cellule A1 de votre Google Sheet
3. **AI Agent** → utilise le prompt lu depuis Sheets
4. **Validation & Response** → retourne les 15 défis

### Modification du prompt :
1. Ouvrez votre Google Sheet
2. Modifiez directement la cellule A1
3. Sauvegardez (Ctrl+S)
4. **Effet immédiat** : la prochaine génération utilisera le nouveau prompt

## 🧪 Test

**URL du webhook :** `https://polaris-ia.app.n8n.cloud/webhook/ui-defis-final`

**Test avec curl :**
```bash
curl -X POST https://polaris-ia.app.n8n.cloud/webhook/ui-defis-final \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d "Je veux...=Développer mon réseau professionnel&Combien de défi à tu relevé=3"
```

## ⚠️ Points importants

- Le Google Sheet doit être **partagé publiquement** (lecture seule suffit)
- La cellule **A1** contient tout le prompt (peut être très long)
- Les modifications dans Google Sheets sont **instantanées**
- Pas besoin de redémarrer n8n après modification du prompt

## 🎉 Avantages vs autres solutions

| Solution | Complexité | Temps setup | Modification |
|----------|------------|-------------|--------------|
| **Google Sheets** | ⭐ | 2 min | Instantané |
| Base de données | ⭐⭐⭐ | 15 min | Interface web |
| API n8n | ⭐⭐⭐⭐ | 30 min | Interface web |

**Google Sheets = Solution parfaite pour la simplicité !**
