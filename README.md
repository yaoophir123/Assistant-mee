# 🎓 Assistant MEE — Guide d'installation et de mise en ligne

Chatbot conversationnel avec IA de l'agence **Mes Études à l'Étranger** (Abidjan), conforme au mémoire de licence (qualification à 5 critères, scoring 0–100, fiche prospect, dashboard).

## 📂 Contenu du projet

| Fichier | Rôle |
| --- | --- |
| `server.js` | Serveur Node.js (middleware) : protège la clé API et relaie les conversations vers l'IA |
| `public/index.html` | Le chatbot (interface web complète) |
| `package.json` | Dépendances du projet |
| `.env.example` | Modèle de configuration de la clé API |

⚠️ **Important** : la clé API n'apparaît JAMAIS dans le code. Elle est stockée dans une variable d'environnement côté serveur. Ne mettez jamais une clé dans `index.html`.

---

## ÉTAPE 0 — Obtenir une (nouvelle) clé API

1. Allez sur **https://console.anthropic.com**
2. Si votre ancienne clé (celle qui était dans le fichier HTML) existe encore : **révoquez-la** (API Keys → Delete).
3. Créez une **nouvelle clé** (API Keys → Create Key) et copiez-la. Elle ressemble à `sk-ant-api03-...`
4. Vérifiez que votre compte a du crédit (menu Billing). L'API est payante à l'usage : quelques francs CFA par conversation.

---

## ÉTAPE 1 — Tester sur votre ordinateur (optionnel mais recommandé)

Prérequis : installer **Node.js** version 18 ou plus (https://nodejs.org).

```bash
# 1. Ouvrir un terminal dans le dossier du projet
cd chatbot-mee-production

# 2. Installer les dépendances
npm install

# 3. Créer le fichier de configuration
#    (copiez .env.example en .env puis collez votre clé dedans)
copy .env.example .env        (Windows)
cp .env.example .env          (Mac/Linux)

# 4. Lancer le serveur
npm start
```

Ouvrez ensuite **http://localhost:3000** dans votre navigateur : le chatbot doit répondre.

---

## ÉTAPE 2 — Mettre en ligne gratuitement sur Render.com

C'est l'hébergeur prévu dans le mémoire (chapitre 7, plan gratuit, HTTPS automatique).

1. **Créez un compte GitHub** (https://github.com) si vous n'en avez pas, et créez un nouveau dépôt (repository), par exemple `assistant-mee`.
2. **Envoyez les fichiers du projet** dans ce dépôt (bouton « Add file → Upload files » : glissez `server.js`, `package.json`, `.gitignore` et le dossier `public/`). ⚠️ N'envoyez **jamais** le fichier `.env`.
3. **Créez un compte sur https://render.com** (connexion possible avec GitHub).
4. Cliquez **New → Web Service** puis sélectionnez votre dépôt `assistant-mee`.
5. Render détecte Node.js automatiquement. Vérifiez :
   - **Build Command** : `npm install`
   - **Start Command** : `npm start`
   - **Instance Type** : Free
6. Dans la section **Environment Variables**, ajoutez :
   - Key : `ANTHROPIC_API_KEY`
   - Value : votre clé `sk-ant-api03-...`
7. Cliquez **Deploy**. Au bout de 2–3 minutes, Render vous donne une adresse du type :
   **https://assistant-mee.onrender.com** ← votre chatbot est en ligne ! 🎉

> 💡 Sur le plan gratuit, le serveur « s'endort » après 15 minutes d'inactivité : la première réponse peut alors prendre 30–50 secondes le temps qu'il se réveille. C'est normal. Le plan payant (~7 USD/mois) supprime cette limite — ce coût est déjà prévu dans le budget « Hébergement » du mémoire.

---

## ÉTAPE 3 — Intégrer le chatbot au site de l'agence

Deux possibilités :

**Option A — Lien direct** : ajoutez sur le site un bouton « 💬 Discuter avec l'Assistant MEE » qui pointe vers votre adresse Render.

**Option B — Widget intégré (iframe)** : collez ce code dans la page du site de l'agence (remplacez l'adresse par la vôtre) :

```html
<iframe
  src="https://assistant-mee.onrender.com"
  style="position:fixed;bottom:20px;right:20px;width:420px;height:640px;
         max-width:95vw;max-height:85vh;border:none;border-radius:18px;
         box-shadow:0 12px 48px rgba(0,0,0,.4);z-index:9999"
  title="Assistant MEE">
</iframe>
```

---

## 🛠️ Personnalisation

- **Lien du formulaire de candidature** : dans `public/index.html`, modifiez la constante `FORM_URL` (en haut du script).
- **Connaissances du chatbot** (prix, destinations, processus) : dans `server.js`, modifiez le bloc `SYSTEM_PROMPT`.
- **Limite anti-abus** : 20 messages/minute par visiteur (modifiable dans `server.js`, fonction `rateLimit`).

## ❓ Problèmes fréquents

| Symptôme | Cause probable | Solution |
| --- | --- | --- |
| « Clé API non configurée sur le serveur » | Variable `ANTHROPIC_API_KEY` absente | Ajoutez-la dans Render (Environment) ou dans `.env` en local |
| « invalid x-api-key » | Clé incorrecte ou révoquée | Recréez une clé sur console.anthropic.com |
| « credit balance is too low » | Plus de crédit API | Rechargez sur console.anthropic.com → Billing |
| Première réponse très lente | Serveur gratuit en veille | Normal sur le plan Free de Render |
