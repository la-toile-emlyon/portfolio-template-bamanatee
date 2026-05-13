# LaToile — Exercice JS : fetch + DOM

Le HTML et le CSS sont déjà en place. La page est vide.  
Votre mission : compléter `script.js` pour que tout le contenu de `data.json` peuple la page.

---

## Fichiers

| Fichier | Rôle | À modifier ? |
|---------|------|--------------|
| `index.html` | Structure (vide) | ❌ |
| `style.css`  | Mise en forme    | ❌ |
| `data.json`  | Toutes les données du site | ❌ |
| `script.js`  | Votre code JS    | ✅ |

---

## data.json — structure complète

```json
{
  "logo": "logo",

  "nav": [
    { "label": "MES COMPÉTENCES", "href": "#competences" },
    ...
  ],

  "cta": { "label": "CONTACTEZ MOI ↗", "href": "#contact" },

  "hero": {
    "titre":     "Je donne",
    "accent":    "vie",
    "suite":     "à vos idées",
    "sousTitre": "Je travaille avec vous..."
  },

  "competences": [
    { "titre": "...", "description": "...", "tags": ["...", "..."] }
  ],

  "projets": [
    { "titre": "...", "description": "...", "tags": [...],
      "image": "assets/...", "lien": "#", "reverse": false }
  ],

  "parcours": [
    { "annee": "2024", "titre": "...", "lieu": "..." }
  ]
}
```

---

## Le pattern fetch — à comprendre avant de coder

```js
fetch('data.json')              // charge le fichier
  .then(function(reponse) {
    return reponse.json();      // convertit en objet JS
  })
  .then(function(data) {
    // ici data contient tout le JSON
    // data.logo, data.nav, data.hero, data.projets...
  });
```

> ⚠️ Tout votre code doit être **à l'intérieur** du dernier `.then()`.  
> C'est là que `data` existe.

---

## Les étapes dans script.js

### Étape 1 — `genererTags(tags)` *(tableau → HTML)*
Reçoit un tableau de strings, retourne une chaîne de `<span class="tag">`.

```
["HTML", "CSS"]  →  '<span class="tag">HTML</span><span class="tag">CSS</span>'
```

### Étape 2 — `genererLiens(liens)` *(tableau → HTML)*
Reçoit le tableau `data.nav`, retourne une chaîne de `<li><a>`.

```
[{ label: "PROJETS", href: "#projets" }]
→  '<li><a href="#projets">PROJETS ↗</a></li>'
```

### Étape 3 — Nav
```js
logoNav.textContent  = data.logo
liensNav.innerHTML   = genererLiens(data.nav)
ctaNav.textContent   = data.cta.label
ctaNav.href          = data.cta.href
```

### Étape 4 — Hero
Le `<h1>` contient une balise `<em>` → utiliser `innerHTML` :
```js
heroTitre.innerHTML     = `${data.hero.titre} <em>${data.hero.accent}</em><br>${data.hero.suite}`
heroSousTitre.textContent = data.hero.sousTitre
```

### Étape 5 — Compétences
`forEach` sur `data.competences` → `insertAdjacentHTML('beforeend', carte)`

```html
<div class="competence-card">
  <h3>titre</h3>
  <p>description</p>
  <div class="tags"> genererTags(tags) </div>
</div>
```

### Étape 6 — Projets
`forEach` sur `data.projets` → toutes les cartes ont la **même structure**.

> Le CSS s'occupe d'inverser automatiquement les cartes paires avec `:nth-of-type(even)`.  
> Pas besoin de condition dans le JS !

```html
<article class="projet-card">
  <div class="projet-content">
    <div class="projet-top">
      <h3>titre</h3>
      <div class="tags"> genererTags(tags) </div>
    </div>
    <div class="projet-bottom">
      <p>description</p>
      <a href="lien" class="btn-projet">VOIR LE PROJET ↗</a>
    </div>
  </div>
  <div class="projet-image">
    <img src="image" alt="titre">
  </div>
</article>
```

### Étape 7 — Parcours
`forEach` sur `data.parcours` → `insertAdjacentHTML('beforeend', item)`

```html
<li class="parcours-item">
  <p class="parcours-titre">2024 - Titre</p>
  <p class="parcours-lieu">Lieu</p>
</li>
```

### Étape 8 — Footer
Même logique que la nav (réutilisez `genererLiens`) :
```js
logoFooter.textContent = data.logo
liensFooter.innerHTML  = genererLiens(data.nav)
...
```

---

## Résultat attendu

✅ Logo affiché dans nav et footer  
✅ Liens de navigation générés  
✅ Hero avec titre en italique  
✅ 2 cartes compétences avec leurs tags  
✅ 3 cartes projets (la 2e inversée)  
✅ 4 items dans le parcours  

> Pour tester : modifiez `data.json` (changez le logo, ajoutez un projet...)  
> La page doit se mettre à jour sans toucher au HTML ni au JS.
