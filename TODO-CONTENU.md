## 🟠 À compléter dès que possible

### 1. Numéro d'entreprise (BCE) et numéro de TVA

**Où :** `data/legal.yaml` → `enterpriseNumber`, `vatNumber`

Démarches prévues à partir du **1er octobre 2026**. Ce sont désormais les
**seuls** champs encore vides du fichier. Tant qu'ils le sont :

- les mentions légales affichent un marqueur jaune `À COMPLÉTER` ;
- le JSON-LD **omet** les propriétés `identifier` et `vatID` plutôt que d'y mettre
  une valeur bidon.

Ce n'est pas bloquant : tant que l'activité n'est pas enregistrée, il n'y a pas
de numéro à mentionner. Dès que tu les as, remplis-les dans `data/legal.yaml` :
rien d'autre à faire.

### 2. Profil Google Business (visibilité locale)

**Quand :** après l'inscription à la BCE, pas avant.

Si tu veux apparaître dans le bloc local de Google et sur Maps — pour des
requêtes du type « statisticien près de chez moi » ou « consultant statistique
Brabant wallon » — c'est **un profil Google Business** qu'il faut, pas du
balisage sur le site.

C'est une confusion fréquente et elle a une conséquence concrète ici : j'ai
retiré la propriété `areaServed` des données structurées parce qu'elle ne sert à
rien en référencement (elle ne figure pas parmi les propriétés que Google
documente pour les résultats enrichis d'établissement local, et n'est pas un
signal de classement). L'équivalent qui compte réellement, c'est le champ
« zone de service » du profil Google Business.

Deux points à arbitrer avant de te lancer :

- **Il faut une activité vérifiable.** Google demande de prouver l'existence de
  l'entreprise, d'où l'attente de la BCE.
- **Ton adresse est à ton domicile.** Deux options : la publier, ou configurer le
  profil en *établissement de zone de service*, ce qui la masque sur la fiche et
  n'affiche qu'un rayon d'intervention. À noter : ton adresse figure déjà dans
  les mentions légales, l'arbitrage porte donc surtout sur son apparition sur une
  carte.

Non bloquant, et à évaluer selon l'importance réelle des clients de proximité
pour toi : une bonne partie de ton marché (chercheurs, hôpitaux, entreprises)
te trouvera par recherche classique plutôt que par la carte.

### 5. Conditions générales — délibérément non rédigées

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

### 7. Activer la mesure d'audience (si tu en ressens le besoin)

Le site part avec **zéro script de mesure et zéro cookie**, donc sans bandeau de
consentement. Si tu veux plus tard le détail du trafic par page :

1. Crée un compte gratuit sur [Cloudflare Web Analytics](https://www.cloudflare.com/web-analytics/)
   (gratuit, sans cookie, sans stockage local, sans donnée personnelle — donc
   utilisable sans bandeau).
2. Ajoute le site, copie le token.
3. Dans `config/_default/params.toml`, remplis :
   `cloudflareAnalyticsToken = "ton-token"`.
4. Dans `netlify.toml`, ouvre le `Content-Security-Policy` aux **deux hôtes** de
   Cloudflare — le script est servi par l'un et rapporte à l'autre :
   - `script-src` : ajoute `https://static.cloudflareinsights.com` ;
   - `connect-src` : ajoute `https://cloudflareinsights.com`.

**Deux fichiers, pas un.** L'étape 4 n'existait pas tant que le site ne servait
aucun en-tête de sécurité ; depuis, le CSP n'autorise plus aucun script tiers, et
sans elle le beacon est bloqué **sans rien de visible** — ni erreur à l'écran, ni
statistique dans Cloudflare. La même mise en garde figure dans `params.toml`,
dans `layouts/partials/analytics.html` et dans `netlify.toml`.

Cela fait, le script s'ajoute et le paragraphe « mesure d'audience » apparaît
automatiquement dans les deux politiques de confidentialité, puisque les deux
sont pilotés par ce même paramètre. La bascule a été testée dans les deux sens.

> ⚠️ Cette clé doit rester **au-dessus de tout en-tête `[table]`** dans
> `params.toml`, sinon TOML l'imbrique dans la table précédente et le paramètre
> n'est jamais lu.

Alternative envisageable : le **palier gratuit d'Umami Cloud** (open source,
sans cookie, hébergé en UE). Il demanderait un partial supplémentaire, non écrit.
Je n'ai activé **aucun** des deux.
