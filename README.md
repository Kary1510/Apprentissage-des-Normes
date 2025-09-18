
# ISO 26000 Escape Builder (Vite + React + Tailwind)

Générateur **client-side** d'escape games pédagogiques pour sensibiliser à **ISO 26000**.  
Inclut un **éditeur visuel** de scènes/énigmes et un **runner** avec minuteur, score, indices et progression.

## ✨ Fonctionnalités
- Édition de **scènes** (titre, narration, média, tag **ISO 26000**).
- Types d'énigmes : **QCM**, **Code secret**, **Associations**, **Ordonnancement**, **Hotspots (image)**.
- **Minuteur**, **score**, **indices**, message de réussite par scène.
- **Export/Import JSON** du jeu complet.
- **Partage par URL** (jeu encodé dans la query string).
- **Persistance locale** via `localStorage`.
- **Banque de QCM** ISO 26000 (exemples inclus).

---

## 🚀 Démarrage rapide

```bash
# 1) Installer
npm install

# 2) Dev
npm run dev

# 3) Build de prod
npm run build
npm run preview
```

> Stack : **Vite** (+ React), **TailwindCSS**, **Framer Motion**, **Lucide**.

---

## 🧩 Utilisation (éditeur)
- **Ajouter une scène** → choisir un **type d’énigme**.
- Remplir **narration**, **indice**, **message de réussite**, et **média** (URL d’image/vidéo).
- Boutons :
  - **Exporter JSON** : sauvegarde le jeu.
  - **Importer JSON** : recharge un jeu.
  - **Tester le jeu** : ouvre le **runner** (minuteur + score).
  - **Partager** : copie une **URL** encodée du jeu dans le presse-papiers.
  - **Ajouter QCM ISO 26000** : injecte des scènes QCM depuis la **banque** intégrée.

### Types d’énigmes
- **QCM** : cochez les options correctes (plusieurs possibles).
- **Code secret** : saisie d’un code (`answer`) à trouver.
- **Associations** : associer `left → right` (menu déroulant).
- **Ordonnancement** : réordonner des étapes via ↑/↓ (l’ordre **correct** peut être défini via `orderingCorrect`, sinon l’ordre initial fait foi).
- **Hotspots (image)** : cliquez sur une **image** aux coordonnées correctes (zones définies en **%** : `x, y, w, h`).

---

## 🗂️ Structure du projet

```
iso26000-escape-builder/
├─ index.html
├─ package.json
├─ postcss.config.js
├─ tailwind.config.js
├─ vite.config.js
└─ src/
   ├─ App.jsx          # Éditeur & Runner
   ├─ main.jsx         # Bootstrap React
   └─ styles.css       # Tailwind
```

---

## 🧱 Modèle de données (simplifié)

Chaque **scène** ressemble à :
```json
{
  "id": "uuid",
  "title": "Titre",
  "narrative": "Contexte de la scène…",
  "mediaUrl": "https://...",
  "isoTag": "Gouvernance de l'organisation",
  "puzzle": {
    "type": "qcm|codelock|match|ordering|hotspot",
    "question": "Question pour QCM ou indice pour Code",
    "options": [{ "id": 1, "text": "Option A", "correct": false }],
    "answer": "26000",
    "pairs": [{ "left": "Gouvernance", "right": "Décision responsable" }],
    "ordering": ["Identifier", "Planifier", "Agir", "Évaluer", "Améliorer"],
    "orderingCorrect": ["Identifier", "Planifier", "Agir", "Évaluer", "Améliorer"],
    "hotspots": {
      "image": "https://image",
      "targets": [{ "x": 10, "y": 10, "w": 20, "h": 20, "correct": true }]
    },
    "hint": "Indice optionnel",
    "successText": "Bravo !"
  }
}
```

> **Note** : `orderingCorrect` est prioritaire si défini.

---

## 🌐 Déploiement

### Option 1 — **Vercel** (build auto recommandé)
1. Poussez ce dossier dans un **repo Git** (GitHub/GitLab).
2. Sur Vercel : *Import Project* → sélectionnez le repo.
3. Vercel détecte Vite. Vérifiez :
   - **Build Command** : `npm run build`
   - **Output Directory** : `dist`
4. Déployez → URL en ligne immédiate.

### Option 2 — **Netlify**
- **Depuis Git** : mêmes paramètres (`build: npm run build`, `publish: dist`).
- **Sans Git (Netlify Drop)** :
  ```bash
  npm run build
  ```
  Puis **déposez** le dossier `dist/` sur Netlify Drop.

### Option 3 — **GitHub Pages**
1. Si le repo **n’est pas** `user.github.io`, ajoutez dans `vite.config.js` :
   ```js
   // vite.config.js
   import { defineConfig } from 'vite'
   import react from '@vitejs/plugin-react'
   export default defineConfig({
     plugins: [react()],
     base: '/nom-du-repo/', // ← important pour Pages
   })
   ```
2. Déployez via **GitHub Actions** (ex. `peaceiris/actions-gh-pages`) ou `gh-pages` :
   ```bash
   npm run build
   npx gh-pages -d dist
   ```

---

## 🔧 Personnalisation
- **Styles/UI** : Tailwind dans `styles.css` + classes utilitaires inlined.
- **Animations** : `framer-motion` (ex. transitions cartes de scènes).
- **Icônes** : `lucide-react` (remplaçables).
- **Banque de QCM** : éditez `QUESTION_BANK` dans `App.jsx`.

### Ajouter un nouveau type d’énigme (exemple)
1. Étendez le schéma `puzzle` dans `DEFAULT_SCENE()`.
2. Ajoutez l’éditeur dans `SceneCard` (section “type === ...”).  
3. Implémentez le composant `*Runner` et la logique de validation dans `Runner.handleValidate`.

---

## ❓FAQ

**Puis-je “coller” directement le code sans build ?**  
- Non pour `App.jsx` : c’est du React/Vite → nécessite un **build** ou un hébergeur qui build.
- Oui pour le **dossier `dist/`** après build : déposez-le chez n’importe quel hébergeur statique.

**Où sont conservées mes données ?**  
- En local (**localStorage**) + vos fichiers **JSON** exportés.

**ISO 26000**  
- Le contenu fourni est **illustratif**. Adaptez vos questions/ressources selon vos sources et votre contexte.

---

## 🧭 Scripts NPM
- `npm run dev` — serveur de dev Vite
- `npm run build` — build de prod (sortie `dist/`)
- `npm run preview` — prévisualisation du build

---

## ⚖️ Licence
Aucune licence explicite incluse. Ajoutez la licence qui convient à votre usage (ex. MIT) avant diffusion.

---

## 🤖 CI/CD – GitHub Pages (workflow inclus)

Ce repo contient un workflow **GitHub Actions** : `.github/workflows/deploy.yml` qui :
1. installe les dépendances (`npm ci`),
2. lance `npm run build`,
3. publie le dossier `dist/` sur **GitHub Pages**.

### Configuration
- Assurez-vous d'avoir activé Pages : **Settings → Pages → Source: GitHub Actions**.
- Si votre repo **n’est pas** `user.github.io`, ajoutez la base dans `vite.config.js` :
  ```js
  import { defineConfig } from 'vite'
  import react from '@vitejs/plugin-react'
  export default defineConfig({
    plugins: [react()],
    base: '/nom-du-repo/', // ← important pour Pages
  })
  ```

Poussez sur `main` → le workflow s'exécute → votre app est en ligne 🎉

---

## 📸 Captures d’écran

Ajoutez vos captures (PNG/JPG) dans `public/` (ex. `public/screens/editeur.png`, `public/screens/runner.png`) et référencez-les ici :

![Éditeur](public/screens/editeur.png)
![Runner](public/screens/runner.png)

> Astuce : vous pouvez aussi héberger les images dans le README via des URLs absolues (CDN, GitHub user content, etc.).


---

## 🔗 Déploiement (préconfiguré pour GitHub Pages)

- Utilisatrice : **Karima Marnissi** (GitHub **@kary1510**)
- Repo suggéré : **iso26000-escape-builder**
- URL attendue après déploiement : **https://kary1510.github.io/iso26000-escape-builder/**
- `vite.config.js` inclut déjà `base: '/iso26000-escape-builder/'`

> Si vous changez le nom du repo, modifiez aussi `base` en conséquence.

