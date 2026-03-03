# KomiLab.org — Guide complet

> **Build. Secure. Govern. Scale.**
> Laboratoire digital d'ingénierie IT open source

---

## Structure du projet

```
komilab/
├── index.html               ← Homepage (flux RSS + labs + architecture)
├── labs.html                ← Catalogue complet avec filtres
├── governance.html          ← GRC, ISO 27001, KPI, PSSI
├── architecture.html        ← Blueprints, ADR, topologies
├── veille.html              ← Intelligence technologique live
├── about.html               ← Profil, certifications, newsletter
│
├── _template-lab.html       ← TEMPLATE pour nouveaux labs
├── labs/                    ← Articles labs individuels
│   └── proxmox-cluster-truenas.html  ← Exemple complet
│
├── assets/
│   ├── css/style.css        ← Design system complet
│   └── js/main.js           ← Copy-button, RSS, animations
│
├── data/
│   ├── config.json          ← Config globale du site
│   └── labs.json            ← Base de données des labs
│
├── _headers                 ← Sécurité + Cache Cloudflare
├── _redirects               ← Redirections URL
└── .github/workflows/
    └── deploy.yml           ← CI/CD → Cloudflare Pages auto
```

---

## Ajouter un nouveau Lab (3 étapes)

### Étape 1 — Dupliquer le template
```bash
cp _template-lab.html labs/mon-nouveau-lab.html
```

### Étape 2 — Remplir les balises `[CROCHETS]`
Ouvrir `labs/mon-nouveau-lab.html` dans VS Code et remplacer **toutes** les valeurs entre crochets :

| Balise | Valeur à mettre |
|--------|----------------|
| `[LAB_TITRE]` | Titre complet du lab |
| `[LAB_DESCRIPTION_COURTE]` | Description SEO (160 car.) |
| `[LAB_LEVEL]` | `Lab` / `Pilote` / `Production` / `Expert` |
| `[LAB_LEVEL_CSS]` | `lab` / `pilote` / `production` / `expert` |
| `[LAB_CATEGORY]` | ID catégorie (ex: `infrastructure`) |
| `[LAB_CATEGORY_LABEL]` | Label (ex: `Infrastructure`) |
| `[LAB_ENVIRONMENT]` | `KOMI.LAB` ou `Production SGMT` |
| `[LAB_DURATION]` | Ex: `2h30` |
| `[LAB_DATE]` | Ex: `15 mars 2025` |
| `[LAB_TAGS_COMMA]` | Ex: `Proxmox, Linux, ZFS` |
| `[TAG_1]` ... | Tags individuels sidebar |
| `[LAB_GITHUB_PATH]` | Chemin repo GitHub configs |

Pour chaque bloc de code : remplacer `[COMMANDE_X]` et `[LANG]`.

### Étape 3 — Ajouter dans data/labs.json

```json
{
  "id": "mon-nouveau-lab",
  "title": "Titre du lab",
  "excerpt": "Description courte (2-3 lignes max).",
  "category": "infrastructure",
  "tags": ["Tag1", "Tag2", "Tag3"],
  "level": "Production",
  "duration": "2h",
  "environment": "KOMI.LAB",
  "date": "2025-04-01",
  "href": "labs/mon-nouveau-lab.html",
  "featured": false
}
```

> ⚠️ Ajouter la virgule après l'entrée précédente dans le tableau JSON.

### Déploiement automatique
```bash
git add .
git commit -m "feat: lab - Mon Nouveau Lab"
git push origin main
# → Cloudflare Pages déploie en < 60 secondes
```

---

## Modifier les informations du site

### Config globale → `data/config.json`
```json
{
  "site": {
    "name": "KomiLab",          ← Nom du site
    "email": "contact@komilab.org",
    "github": "https://github.com/TON_USERNAME",
    "linkedin": "https://linkedin.com/in/TON_PROFIL"
  }
}
```

### Design → `assets/css/style.css` (lignes 1-40)
```css
:root {
  --cyan: #00D4FF;      /* Couleur accent principale */
  --bg-main: #0A0E1A;   /* Fond principal */
  --font-mono: 'JetBrains Mono'; /* Police titres */
}
```
Un seul changement → impact sur tout le site.

---

## Sources RSS (flux actualités)

Modifier dans `assets/js/main.js` :
```javascript
const RSS_SOURCES = {
  cyber: [
    { name: 'Nouvelle Source', url: 'https://example.com/feed' },
  ],
  tech: [
    { name: 'Ma Source Tech', url: 'https://example.com/rss' },
  ]
};
```

---

## Configuration Cloudflare Pages

### Connexion initiale (une seule fois)
1. Aller sur [dash.cloudflare.com](https://dash.cloudflare.com)
2. Pages → Create a project → Connect to Git
3. Sélectionner le repo GitHub `komilab`
4. **Build command** : laisser vide (site statique)
5. **Output directory** : `/` (racine)
6. Cliquer Deploy

### Secrets GitHub pour CI/CD automatique
Dans GitHub → Settings → Secrets → Actions :

| Secret | Valeur |
|--------|--------|
| `CLOUDFLARE_API_TOKEN` | Token API Cloudflare (permission Pages:Edit) |
| `CLOUDFLARE_ACCOUNT_ID` | ID compte Cloudflare (visible dans l'URL dash) |

Créer le token : dash.cloudflare.com → Mon profil → API Tokens → Create Token → Custom → Pages: Edit

---

## Catégories disponibles

| ID | Label | Icône |
|----|-------|-------|
| `infrastructure` | Infrastructure & Virtualisation | ⬡ |
| `reseau-securite` | Réseau & Sécurité | ◈ |
| `cybersecurite` | Cybersécurité | ◉ |
| `automatisation` | Automatisation | ◎ |
| `gouvernance` | Gouvernance IT | ◇ |
| `linux` | Systèmes Linux | ○ |

## Niveaux disponibles

| Niveau | CSS class | Usage |
|--------|-----------|-------|
| `Lab` | `level-lab` | Expérimentation pure HomeLab |
| `Pilote` | `level-pilote` | Testé, pas encore en prod |
| `Production` | `level-production` | Déployé en production réelle |
| `Expert` | `level-expert` | Prérequis avancés requis |

---

## Workflow quotidien

```
1. VSCode → éditer/créer fichiers
2. git add . && git commit -m "feat: ..."
3. git push origin main
4. Cloudflare Pages déploie automatiquement (< 60s)
```

**Aucun serveur à gérer. Aucun CMS. Aucune base de données.**

---

*KomiLab · Komi Kpodohoui · CCNP · ISO 27001 Lead Implementer*
