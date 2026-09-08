# TODO — contenu et informations à fournir

Tout ce qui suit nécessite une information que moi seul peux fournir. Rien n'a été
inventé : chaque champ manquant apparaît soit comme un marqueur jaune
« À COMPLÉTER » sur le site, soit est purement omis des données structurées.

---

## ✅ Plus rien ne bloque la mise en ligne

**Adresse professionnelle et adresse e-mail sont renseignées** dans
`data/legal.yaml`. Elles apparaissent désormais partout automatiquement :
mentions légales, politique de confidentialité et données structurées JSON-LD.
Les marqueurs jaunes « À COMPLÉTER » correspondants ont disparu du site.

Le site est donc **publiable en l'état**, sous réserve du point 10 (relecture
juridique), qui est recommandé mais pas bloquant.

> ### L'adresse e-mail publiée
>
> `ant.soetewey@gmail.com` est publiée comme contact légal et RGPD. C'est
> parfaitement valable juridiquement, et le passage de l'adresse UCLouvain à une
> adresse personnelle règle les deux risques précédents : plus de dépendance à un
> règlement d'usage universitaire, plus de boîte qui disparaît avec le contrat.
>
> Reste une question de perception, non bloquante : une adresse sur ton propre
> domaine (`contact@datanalyze.be`) fait plus professionnel sur un site
> commercial qu'un Gmail. Un seul champ à changer dans `data/legal.yaml`, le jour
> où tu configures une boîte sur le domaine.

---

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

### 3. Profil Google Business (visibilité locale)

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

---

## 🟡 Améliorations, non bloquantes

### 6. Image de partage sur les réseaux sociaux

Aujourd'hui, l'image Open Graph est un **recadrage automatique 1200×630 de ton
portrait**, avec détection de contenu (`Smart`) : depuis le passage au nouveau
portrait en paysage, le cadrage est correct et le visage bien placé. Une image
dessinée (nom, titre commercial, URL) resterait plus efficace lorsqu'un lien est
partagé sur LinkedIn, mais ce n'est plus un point faible.

Pour la remplacer : dépose un fichier 1200×630 dans `assets/img/` et ajoute
`image: "img/ton-fichier.png"` dans le front matter de la page concernée, ou
modifie la valeur par défaut dans `layouts/partials/head.html`.

### 7. Conditions générales — délibérément non rédigées

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

### 8. Témoignages, logos clients et grille tarifaire

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

### 9. Activer la mesure d'audience (si tu en ressens le besoin)

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

### 10. Textes juridiques à faire relire

Les mentions légales et la politique de confidentialité sont des **modèles
génériques** adaptés à une activité d'indépendant en Belgique, pas un conseil
juridique. Une relecture par un professionnel du droit est recommandée avant la
mise en ligne définitive, surtout pour la partie sous-traitance RGPD (tu traites
des données de santé pour tes clients hospitaliers).

### 11. Points de contenu à vérifier

- **easystat.be** est toujours cité comme activité de cours particuliers, sur la
  page « À propos ». À confirmer.
- Le compte **Twitter/X** `@statsandr` a été retiré des liens (le réseau ne fait
  plus partie des canaux utiles pour ce type d'activité). Dis-moi si tu veux le
  remettre : `config/_default/menus.*.toml`, table `[[social]]`.
- La **liste des publications** citées sur « À propos » est volontairement une
  sélection de revues, sans titres ni dates, pour éviter qu'elle ne se périme.
  Le site académique reste la référence.
