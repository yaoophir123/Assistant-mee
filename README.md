# 🎓 Assistant MEE — Déploiement sur Vercel (gratuit, sans mise en veille)

## 📂 Structure du projet (NE PAS MODIFIER)

```
index.html        ← le chatbot (à la RACINE, pas dans un dossier)
api/
  └── chat.js     ← la fonction qui protège la clé API et parle à l'IA
package.json
.gitignore
```

⚠️ Sur GitHub, ces fichiers doivent être **directement à la racine du dépôt** :
quand vous ouvrez le dépôt, vous devez voir `index.html`, `package.json` et le
dossier `api` — pas de fichier .zip, pas de dossier intermédiaire.

---

## ÉTAPE 1 — Mettre les fichiers sur GitHub

1. Décompressez le zip sur votre ordinateur.
2. Ouvrez votre dépôt sur github.com (ou créez-en un nouveau : bouton **New repository**, nom `assistant-mee`, Public, **Create repository**).
3. **Supprimez les anciens fichiers** s'il y en a (cliquez sur chaque fichier → 🗑️ → Commit changes).
4. **Add file → Upload files** → glissez-déposez en une fois : `index.html`, `package.json`, `.gitignore` ET **le dossier `api` entier**.
5. Cliquez le bouton vert **Commit changes**.

✅ Vérification : la page d'accueil du dépôt montre `api/`, `index.html`, `package.json`.

---

## ÉTAPE 2 — Déployer sur Vercel

1. Allez sur **https://vercel.com** → **Sign Up** → **Continue with GitHub** (autorisez l'accès).
2. Sur le tableau de bord : **Add New… → Project**.
3. Dans la liste, trouvez votre dépôt `assistant-mee` → bouton **Import**.
   (S'il n'apparaît pas : lien « Adjust GitHub App Permissions » → cochez le dépôt.)
4. Sur l'écran de configuration :
   - **Framework Preset** : laissez **Other** (détecté automatiquement).
   - Ne touchez à rien d'autre dans Build settings.
5. Ouvrez la section **Environment Variables** et ajoutez :
   - **Name** : `ANTHROPIC_API_KEY`
   - **Value** : votre clé `sk-ant-api03-...` (créée sur console.anthropic.com)
   - Cliquez **Add**.
6. Cliquez **Deploy** et attendez ~1 minute. 🎉

Vercel vous donne une adresse du type :
**https://assistant-mee.vercel.app** ← votre chatbot, en ligne 24h/24, sans mise en veille.

---

## ÉTAPE 3 — Intégrer au site de l'agence

**Option A — Lien** : un bouton « 💬 Discuter avec l'Assistant MEE » qui pointe vers votre adresse Vercel.

**Option B — Widget flottant (iframe)** à coller dans le site de l'agence :

```html
<iframe
  src="https://assistant-mee.vercel.app"
  style="position:fixed;bottom:20px;right:20px;width:420px;height:640px;
         max-width:95vw;max-height:85vh;border:none;border-radius:18px;
         box-shadow:0 12px 48px rgba(0,0,0,.4);z-index:9999"
  title="Assistant MEE">
</iframe>
```

---

## 🔄 Mettre à jour le chatbot plus tard

Modifiez le fichier directement sur GitHub (ouvrir le fichier → icône crayon ✏️ →
Commit changes). **Vercel redéploie automatiquement** à chaque modification.

- Lien du formulaire : constante `FORM_URL` en haut du script dans `index.html`.
- Connaissances de l'IA (prix, destinations, scoring) : bloc `SYSTEM_PROMPT` dans `api/chat.js`.

## ❓ Problèmes fréquents

| Symptôme | Solution |
| --- | --- |
| « Clé API non configurée » | Vercel → votre projet → **Settings → Environment Variables** → ajoutez `ANTHROPIC_API_KEY`, puis **Deployments → ⋯ → Redeploy** |
| « invalid x-api-key » | La clé est fausse ou révoquée : recréez-en une sur console.anthropic.com |
| « credit balance is too low » | Rechargez le crédit API : console.anthropic.com → Billing |
| Page blanche / 404 | `index.html` n'est pas à la racine du dépôt GitHub — reprenez l'Étape 1 |
| Le chatbot ne répond pas | Vercel → projet → **Logs** (ou Observability) pour voir l'erreur exacte |
