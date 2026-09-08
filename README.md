# datanalyze.be

[![Netlify Status](https://api.netlify.com/api/v1/badges/fe94d327-36e4-4f58-876b-5fa29e538389/deploy-status)](https://app.netlify.com/sites/datanalyze/deploys)

Site professionnel de **datanalyze** — consultance et formation en statistique et
science des données.

Site statique **Hugo**, bilingue, **sans cookie, sans traceur et sans aucune
ressource externe**. Déploiement continu sur Netlify.

👉 Ce qui reste à compléter est listé dans **[TODO-CONTENU.md](TODO-CONTENU.md)**.

---

## Démarrer

```sh
hugo server          # http://localhost:1313
hugo --gc --minify   # build de production dans public/
```

**La version de Hugo est épinglée à 0.119.0** en deux endroits qui doivent rester
synchronisés :

| Fichier | Clé |
| --- | --- |
| `netlify.toml` | `HUGO_VERSION` |
| `.Rprofile` | `blogdown.hugo.version` |

> Une version plus récente de Hugo **ne construit pas ce site** : Congo 2.9.0
> utilise `_internal/shortcodes/figure.html`, supprimé après la 0.14x. Si tu
> montes de version, il faudra monter Congo en même temps.

`hugo` seul suffit à produire le site. **Aucun build Node n'est nécessaire**, ni en
local ni sur Netlify — voir « Pourquoi il n'y a pas de Tailwind » plus bas.

---

## Langues

Le **français est la langue principale** et est servi à la **racine** ; l'anglais
est sous **`/en/`**.

```toml
defaultContentLanguage = "fr"
defaultContentLanguageInSubdir = false
```

**Tout fichier de contenu porte un suffixe de langue explicite** : `page.fr.md`
**et** `page.en.md`. Aucun fichier sans suffixe — sans quoi Hugo l'attribuerait
silencieusement au français et la traduction se briserait.

Les slugs sont déclarés en front matter (`slug:`), jamais en `url:` codé en dur,
et diffèrent d'une langue à l'autre :

| Contenu | FR | EN |
| --- | --- | --- |
| `content/about.*.md` | `/a-propos/` | `/en/about/` |
| `content/trainings.*.md` | `/formations/` | `/en/trainings/` |
| `content/services/consulting.*.md` | `/services/consultance/` | `/en/services/consulting/` |

> Conséquence pratique : **ne jamais écrire un chemin d'URL en dur dans un
> layout.** Utiliser `site.GetPage "/about"` puis `.RelPermalink`, qui résout le
> bon slug dans la bonne langue. Les menus utilisent `pageRef` pour la même raison.

Le sélecteur de langue pointe vers la **traduction de la page courante**
(`layouts/partials/language-switch.html`), jamais vers l'accueil.

---

## Structure

```text
config/_default/     configuration éclatée (config, params, languages, menus…)
content/             contenu, en .fr.md et .en.md
data/legal.yaml      ⭐ point unique pour nom, adresse, e-mail, BCE, TVA
i18n/                chaînes d'interface du projet (fr.yaml, en.yaml)
layouts/             tous les gabarits (voir « Overrides du thème »)
assets/css/          design system écrit à la main
assets/fonts/        polices auto-hébergées (Inter, Source Serif 4)
static/_redirects    table de redirections Netlify
themes/congo/        thème vendorisé — NE PAS MODIFIER
```

### Le wrapper blogdown

`index.Rmd`, `.Rprofile` et `R/*.R` sont conservés et fonctionnels. `ignoreFiles`
exclut toujours les `.Rmd`, `.Rmarkdown` et `_cache`. Aucun contenu `.Rmd`
aujourd'hui, mais la chaîne reste utilisable.

---

## Design

### Pourquoi il n'y a pas de Tailwind

Congo embarque un build Tailwind **pré-compilé et figé**
(`themes/congo/assets/css/compiled/main.css`) qui ne contient que les classes
utilisées par le thème. **Toute classe Tailwind écrite dans `layouts/` serait donc
muette**, et la recompiler exigerait soit de modifier le thème vendorisé, soit
d'imposer un build Node à Netlify. Les deux sont exclus.

Le design est donc **écrit à la main en CSS classique**, avec des noms de classes
sémantiques préfixés `dz-`, dans **`assets/css/custom.css`** — que le partial
`head.html` concatène **en dernier**, donc il gagne toujours.

Le CSS compilé de Congo reste chargé **en dessous**, uniquement pour :

- son *preflight*, qui sert de reset ;
- sa typographie `.prose`, utilisée par les corps markdown des pages longues.

### Palette

`assets/css/schemes/datanalyze.css` redéfinit les variables de couleur de Congo.
Comme `params.toml` déclare `colorScheme = "datanalyze"` et que les assets du
projet priment sur ceux du thème, **ce seul fichier recolore tout le Tailwind du
thème et `.prose`** sans y toucher.

Les *tokens* du design maison (typographie, espacements, formes) sont en tête de
`assets/css/custom.css`.

### Polices

Auto-hébergées dans `assets/fonts/`, **sous-ensemble latin** généré depuis les
sources OFL amont — jamais depuis un CDN.

| Police | Usage | Poids | Taille |
| --- | --- | --- | --- |
| Inter (variable) | corps, interface | 400–700 | 80 Ko |
| Source Serif 4 Subhead | titres | 600 | 33 Ko |

Licences SIL OFL 1.1 dans `assets/fonts/LICENSE-*.txt`.

Pour régénérer un sous-ensemble (nécessite `fonttools` et `brotli`) :

```sh
pyftsubset InterVariable.ttf \
  --unicodes="U+0000-00FF,U+0100-017F,U+0180-024F,U+02B0-02FF,U+0300-036F,U+2000-206F,U+2070-209F,U+20A0-20BF,U+2100-214F,U+2190-21BB,U+2212,U+2215,U+2248,U+2260,U+2264,U+2265,U+2713,U+25A0-25CF,U+FEFF,U+FFFD" \
  --layout-features="kern,liga,clig,ccmp,locl,mark,mkmk,rlig,calt,tnum,frac,sups,subs" \
  --flavor=woff2 --output-file=assets/fonts/inter-variable-latin.woff2
```

---

## Overrides du thème

**`themes/congo/` ne doit jamais être modifié.** Tout passe par `layouts/`,
`assets/`, `static/`, `config/`, `i18n/` et `data/`.

Les fichiers suivants **masquent** un fichier du thème. À chaque montée de version
de Congo, il faut les rediffer contre l'original. Version de référence : **Congo 2.9.0**.

| Fichier du projet | Pourquoi il masque celui du thème |
| --- | --- |
| `layouts/_default/baseof.html` | le `<body>` de Congo est contraint (`max-w-7xl`, paddings, `h-screen`), ce qui interdit toute section pleine largeur |
| `layouts/partials/head.html` | `<title>`/description par page, **hreflang** (absent de Congo), preload des polices, image OG générée, pas de lien vers l'index de recherche inutilisé |
| `layouts/partials/footer.html` | mise en page maison, liens légaux, pas d'attribution de thème |
| `layouts/partials/schema.html` | **remplace** le JSON-LD de Congo au lieu de s'y ajouter — c'est ce qui garantit un seul bloc par page |
| `layouts/partials/analytics.html` | rend inatteignables les blocs GA / Fathom / Plausible / Umami du thème |
| `layouts/partials/breadcrumbs.html` | cohérence avec le `BreadcrumbList` du JSON-LD |
| `layouts/_default/sitemap.xml` | neutralise les sitemaps par langue (voir SEO) |
| `layouts/sitemapindex.xml` | remplace l'index par un sitemap plat bilingue |
| `layouts/_default/{list,single}.html`, `layouts/index.html`, `layouts/404.html` | mises en page sectionnées |

**`layouts/partials/header/custom.html` n'est pas un override** : c'est le point
d'extension prévu par Congo, activé par `header.layout = "custom"`.

Gabarits ajoutés, sans équivalent dans le thème : `layouts/services/single.html`,
`layouts/partials/dz-*.html`, `layouts/partials/language-switch.html`,
`layouts/shortcodes/{legal,analytics-notice}.html`.

---

## Mesure d'audience — zéro script, zéro cookie

Le site part en ligne avec **aucun outil de mesure et aucun cookie**, donc **sans
bandeau de consentement** : il n'y a rien à consentir.

`layouts/partials/analytics.html` masque le partial de Congo, ce qui rend ses
blocs Google Analytics, Fathom, Plausible et Umami **inatteignables** — aucune clé
de configuration ne peut réactiver un traceur par inadvertance.

**Les données viennent de trois sources gratuites et sans script :**

1. **Google Search Console** — requêtes, impressions, clics et positions par page.
   C'est le haut de l'entonnoir d'acquisition et la donnée la plus utile ici.
   Voir aussi **Bing Webmaster Tools**. Mise en place : TODO-CONTENU.md § 4.
2. **Les soumissions Airtable et les réservations Calendly** — les conversions
   réelles, déjà comptées par les outils eux-mêmes.
3. **Le tableau de bord Netlify** — déploiements et bande passante.

**Option préparée mais inactive :** Cloudflare Web Analytics (gratuit, sans
cookie, sans bandeau). Tant que `cloudflareAnalyticsToken` est vide, **aucun octet
n'est émis**. L'activer se fait en une ligne — TODO-CONTENU.md § 10.

### Règle : aucune ressource externe

Polices, icônes, images, scripts et styles sont **tous** servis depuis ce domaine.
Concrètement, cela interdit :

- les polices Google Fonts en CDN (elles sont auto-hébergées) ;
- l'**intégration** d'Airtable ou de Calendly — ce sont des **liens sortants**, et
  ils doivent le rester : un iframe déposerait des cookies tiers et
  réintroduirait l'obligation de bandeau ;
- les vidéos YouTube intégrées, les cartes Google Maps, les boutons sociaux.

Vérification (doit ne rien renvoyer d'autre que des liens de navigation) :

```sh
grep -roh 'https\?://[^"'"'"' ]*' public/ | sort -u
```

---

## SEO

- **hreflang** réciproques sur chaque page (`fr-BE`, `en`) avec **`x-default` vers
  le français**. C'est la pièce maîtresse de l'inversion des langues : c'est ce
  qui permet à Google de comprendre que `/` a changé de langue et de retrouver les
  pages anglaises déplacées sous `/en/`.
- `canonical` auto-référent, Open Graph et Twitter Card sur toutes les pages.
- **`/sitemap.xml` est un sitemap unique et plat couvrant les deux langues.** Le
  sitemap index par défaut de Hugo publiait `/fr/sitemap.xml`, qui listait des URLs
  servies à la racine — hors de la portée autorisée pour un sitemap situé sous
  `/fr/`. Les sitemaps par langue sont donc rendus vides et `robots.txt` n'annonce
  que celui de la racine.
- **JSON-LD** : `ProfessionalService` + `Person` + `WebSite` (accueil), `Service`
  (pages services), `FAQPage` (pages avec `faq:` en front matter),
  `BreadcrumbList` + `WebPage` (toutes les autres). Les champs non vérifiables
  (adresse, BCE, TVA, tarifs, avis) sont **omis**, jamais remplis d'une valeur
  bidon.
- Les questions de FAQ sont déclarées **une seule fois**, en front matter (`faq:`),
  et alimentent à la fois l'affichage et le `FAQPage` — les deux ne peuvent pas
  diverger.

---

## Redirections

`static/_redirects` contient **une règle explicite par URL**, classée en trois cas
et commentée dans le fichier.

Deux règles absolues :

- **aucune règle sur `/`** — l'ancienne accueil anglaise est devenue l'accueil
  française ; elle reste une 200 et les `hreflang` gèrent la transition ;
- **aucune règle forcée (`!`)**, à deux exceptions documentées : la redirection
  historique `datanalyze.netlify.app`, et `/fr/` (Hugo y écrit un stub
  meta-refresh que seule une règle forcée peut court-circuiter — cela ne peut
  casser aucune page, puisque aucune page réelle ne vit à `/fr/`).

Sur Netlify, une règle non forcée ne se déclenche **que si aucun fichier ne
correspond**, ce qui est exactement le comportement voulu : les nouvelles pages
françaises ne sont jamais interceptées.

**Une seule ancienne URL ne peut pas être redirigée** : `/faq/`, dont le chemin est
repris par la FAQ française. L'ancienne FAQ anglaise vit désormais à `/en/faq/`,
et les `hreflang` permettent à Google de l'y retrouver.

---

## Vérifications

Le site est vérifié par deux scripts (hors dépôt, à recréer si besoin) et par
quelques commandes :

```sh
hugo --gc --minify                                        # ni ERROR ni WARNING
# Les deux politiques de confidentialité CITENT ces outils pour dire qu'ils ne
# sont pas utilisés : sans l'exclusion, ce contrôle remonte toujours deux pages.
grep -ril "gtag\|googletagmanager\|plausible\|umami" public/ \
  | grep -v "privacy-policy\|politique-de-confidentialite"   # doit être vide
grep -roh 'https\?://[^"'"'"' ]*' public/ | sort -u           # liens de navigation seulement

# Antoine ne doit jamais être décrit comme doctorant (il est docteur depuis 2024).
# Les exclusions sont nécessaires : « doctorants » et « PhD students » décrivent
# légitimement la CLIENTÈLE de la page « Accompagnement des chercheurs », et
# « post-doctorant » est son statut actuel.
grep -rn "doctorant\|PhD researcher\|PhD student" content/ config/ \
  | grep -viE "post-?doctora" \
  | grep -viE "doctorants|PhD students|chercheurs et doctorants|researchers and"   # doit être vide
```

---

## Checklist après un déploiement important

1. `hugo --gc --minify` sans erreur ni avertissement.
2. Vérifier que `/` sert bien le français et `/en/` l'anglais.
3. Vérifier quelques redirections en production, notamment `/fr/` et
   `/portfolio/cliniquesaintjean/`.
4. Dans Search Console : soumettre `sitemap.xml`, puis surveiller le rapport de
   couverture et les 404 pendant quelques semaines.
5. Contrôler le rapport d'internationalisation (hreflang) dans Search Console.
