# TODO — contenu et informations à fournir

Tout ce qui suit nécessite une information que moi seul peux fournir. Rien n'a été
inventé : chaque champ manquant apparaît soit comme un marqueur jaune
« À COMPLÉTER » sur le site, soit est purement omis des données structurées.

---

## 🔴 Bloquant avant la mise en ligne

### 1. Adresse professionnelle

**Où :** `data/legal.yaml` → `address.street`, `address.postalCode`, `address.city`

Le droit belge (livre VI du Code de droit économique, transposant la directive
e-commerce) impose la mention d'une **adresse géographique** sur le site d'un
professionnel. Elle apparaît aujourd'hui comme `À COMPLÉTER` sur les
[mentions légales](https://datanalyze.be/mentions-legales/) et la
[politique de confidentialité](https://datanalyze.be/politique-de-confidentialite/).

Si tu ne souhaites pas publier ton adresse privée, les options habituelles sont
une adresse de domiciliation d'entreprise ou l'adresse d'un espace de coworking.

### 2. Adresse e-mail professionnelle publiable

**Où :** `data/legal.yaml` → `email`

Également obligatoire dans les mentions légales, et nécessaire pour l'exercice
des droits RGPD (une demande d'accès ou d'effacement doit pouvoir t'être adressée
directement, pas seulement via un formulaire tiers).

Une adresse du type `contact@datanalyze.be` ou `antoine@datanalyze.be` serait
cohérente avec le domaine. Je n'ai volontairement pas publié ton adresse
personnelle.

> Une fois ces deux champs remplis dans `data/legal.yaml`, ils apparaissent
> automatiquement partout : mentions légales, politique de confidentialité, et
> données structurées JSON-LD. **Il n'y a qu'un seul endroit à modifier.**

---

## 🟠 À compléter dès que possible

### 3. Numéro d'entreprise (BCE) et numéro de TVA

**Où :** `data/legal.yaml` → `enterpriseNumber`, `vatNumber`

Démarches prévues à partir du **1er octobre 2026**. Tant que ces champs sont
vides :

- les mentions légales affichent `À COMPLÉTER` ;
- le JSON-LD **omet** les propriétés `identifier` et `vatID` plutôt que d'y mettre
  une valeur bidon.

Dès que tu as les numéros, remplis-les dans `data/legal.yaml` : rien d'autre à
faire.

### 4. Vérification Google Search Console et Bing Webmaster Tools

**Où :** `config/_default/params.toml` → `[verification]` → `google`, `bing`

C'est ta **principale source de données d'audience** (voir « Mesure d'audience »
dans le README). Marche à suivre :

1. Va sur [search.google.com/search-console](https://search.google.com/search-console/).
2. Ajoute une propriété. Deux choix :
   - **Domaine** (`datanalyze.be`) — le plus complet, mais demande un enregistrement
     DNS TXT chez ton registrar. À privilégier.
   - **Préfixe d'URL** (`https://datanalyze.be/`) — plus simple : choisis la méthode
     « balise HTML », copie la valeur de l'attribut `content` (**pas** la balise
     entière) et colle-la dans `params.toml` → `[verification] google = "..."`.
3. Déploie, puis clique sur « Vérifier ».
4. Soumets le sitemap : `https://datanalyze.be/sitemap.xml`.
5. Répète sur [bing.com/webmasters](https://www.bing.com/webmasters/) (champ `bing`).
   Bing permet d'importer directement la propriété depuis Search Console.

La checklist post-déploiement complète est dans le README.

### 5. Confirmer la publication des quatre études de cas

Les pages [références](https://datanalyze.be/references/) nomment la
**Clinique Saint-Jean** et la **Fondation Saint-Luc**. Ces textes existaient déjà
sur l'ancien site et n'ont pas été modifiés sur le fond, mais il vaut la peine de
vérifier que tu as bien l'accord de ces institutions pour les citer nommément —
d'autant que le site va gagner en visibilité.

### 6. Statut de la publication « Clinique Saint-Jean »

**Où :** `content/references/cliniquesaintjean.fr.md` et `.en.md`

Le texte dit encore « publication (en cours de révision) », ce qui datait de
septembre 2024. Si l'article est paru depuis, remplace la phrase par un lien vers
la publication, comme pour les deux autres études de cas. Je ne l'ai pas modifié
faute de pouvoir le vérifier.

---

## 🟡 Améliorations, non bloquantes

### 7. Image de partage sur les réseaux sociaux

Aujourd'hui, l'image Open Graph est un **recadrage automatique 1200×630 de ton
portrait**. C'est correct et fonctionnel, mais une image dessinée (nom, titre
commercial, URL) serait plus efficace lorsqu'un lien est partagé sur LinkedIn.

Pour la remplacer : dépose un fichier 1200×630 dans `assets/img/` et ajoute
`image: "img/ton-fichier.png"` dans le front matter de la page concernée, ou
modifie la valeur par défaut dans `layouts/partials/head.html`.

### 8. Conditions générales — délibérément non rédigées

Je ne les ai **pas** écrites, et c'est un choix argumenté :

- elles ne sont **pas obligatoires** ici, puisque aucune vente ne se conclut sur
  le site (le formulaire et Calendly ne servent qu'à la prise de contact) ;
- en rédiger de génériques reviendrait à **inventer** tes délais de paiement, ta
  politique d'annulation, tes plafonds de responsabilité et tes clauses de
  propriété intellectuelle — des clauses à valeur juridique réelle, qui
  risqueraient de contredire tes futurs devis et contrats.

Quand tu voudras les ajouter, il faudra décider : délais et modalités de paiement,
acompte éventuel, politique d'annulation et de report (surtout pour les
formations), plafond de responsabilité, propriété intellectuelle des livrables
(le site annonce déjà un transfert intégral au client), et droit applicable.
Un comptable ou un guichet d'entreprises couvre généralement ce point.

### 9. Témoignages, logos clients et grille tarifaire

Les trois sections sont **construites et stylées** mais **non affichées**, faute
de contenu réel. Chacune s'active en deux gestes, documentés en tête du partial
correspondant :

| Section | Partial | Drapeau dans `params.toml` | Données |
| --- | --- | --- | --- |
| Témoignages | `layouts/partials/dz-testimonials.html` | `showTestimonials = true` | `data/testimonials.yaml` |
| Logos clients | `layouts/partials/dz-client-logos.html` | `showClientLogos = true` | fichiers dans `assets/img/clients/` |
| Tarifs | `layouts/partials/dz-pricing.html` | `showPricing = true` | `data/pricing.yaml` |

Rien n'est émis tant que le drapeau **et** les données ne sont pas présents.

`static/_index_files/logos-clients-datanalyze.jpeg` reste **inutilisé**, comme
demandé. Si tu actives le bandeau plus tard, préfère des logos individuels dans
`assets/img/clients/` : ils restent nets, acceptent un texte alternatif et se
retirent un par un.

### 10. Activer la mesure d'audience (si tu en ressens le besoin)

Le site part avec **zéro script de mesure et zéro cookie**, donc sans bandeau de
consentement. Si tu veux plus tard le détail du trafic par page :

1. Crée un compte gratuit sur [Cloudflare Web Analytics](https://www.cloudflare.com/web-analytics/)
   (gratuit, sans cookie, sans stockage local, sans donnée personnelle — donc
   utilisable sans bandeau).
2. Ajoute le site, copie le token.
3. Dans `config/_default/params.toml`, remplis :
   `cloudflareAnalyticsToken = "ton-token"`.

C'est tout. **Une seule ligne.** Le script s'ajoute et le paragraphe « mesure
d'audience » apparaît automatiquement dans les deux politiques de confidentialité,
puisque les deux sont pilotés par ce même paramètre. Le bascule a été testé dans
les deux sens.

> ⚠️ Cette clé doit rester **au-dessus de tout en-tête `[table]`** dans
> `params.toml`, sinon TOML l'imbrique dans la table précédente et le paramètre
> n'est jamais lu.

Alternative envisageable : le **palier gratuit d'Umami Cloud** (open source,
sans cookie, hébergé en UE). Il demanderait un partial supplémentaire, non écrit.
Je n'ai activé **aucun** des deux.

### 11. Textes juridiques à faire relire

Les mentions légales et la politique de confidentialité sont des **modèles
génériques** adaptés à une activité d'indépendant en Belgique, pas un conseil
juridique. Une relecture par un professionnel du droit est recommandée avant la
mise en ligne définitive, surtout pour la partie sous-traitance RGPD (tu traites
des données de santé pour tes clients hospitaliers).

### 12. Points de contenu à vérifier

- **easystat.be** est toujours cité comme activité de cours particuliers, sur la
  page « À propos ». À confirmer.
- Le compte **Twitter/X** `@statsandr` a été retiré des liens (le réseau ne fait
  plus partie des canaux utiles pour ce type d'activité). Dis-moi si tu veux le
  remettre : `config/_default/menus.*.toml`, table `[[social]]`.
- La **liste des publications** citées sur « À propos » est volontairement une
  sélection de revues, sans titres ni dates, pour éviter qu'elle ne se périme.
  Le site académique reste la référence.
