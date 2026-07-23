[English](README.md) · **Français**

> [!NOTE]
> **Réservé · futur foyer de Carrière** — développé dans le dépôt de base canonique [`libre-ai/libre-ai`](https://github.com/libre-ai/libre-ai) ([topologie multi-dépôts, ADR-0008](https://github.com/libre-ai/libre-ai/blob/main/docs/adr/0008-multi-repo-target-topology-and-brand.md)).
> Ce dépôt rouvrira comme dépôt produit réel lorsque le propriétaire l'activera, consommant la base comme dépendance versionnée. **La spécification est actuellement en cours de conception** — aucun code produit pour l'instant.

# Carrière

**Un assistant de recherche d'emploi souverain pour les cadres français.** Confrontez votre profil, vos compétences et vos attentes salariales à des données de marché réelles — taxonomies d'emplois, paysages d'employeurs, échelles salariales, signaux de recrutement — et obtenez des insights actionnables et transparents. Jamais de boîte noire. Jamais vos données monnayées.

Le problème canonique auquel il répond : _« Où sont les emplois pour lesquels je suis qualifié, qui recrute vraiment, et à quel prix ? »_ pour les **cadres** français — un marché qui voit ~300 k postes annuels, où le marché caché domine mais reste non mesuré, et où la fragmentation des données (France Travail, sites d'emploi, réseaux professionnels) rend la recherche intentionnelle difficile.

## Ce qui le distingue

- **Souverain par conception.** Pas de verrouillage propriétaire cloud, pas d'opacité algorithmique. Vous êtes propriétaire de votre historique de recherche et de vos alertes emploi ; les données circulent dans une infrastructure que vous pouvez auditer ou auto-héberger.
- **Construit sur des sources autorisées, pas du scraping.** Les offres d'emploi proviennent d'API officielles ([France Travail Offres d'emploi](https://www.francetravail.io/), taxonomie ROME 4.0) ; aucun reverse-engineering, aucune violation de conditions générales.
- **Des données de marché réelles.** Fourchettes salariales, fréquence d'embauche par secteur et employeur, mappages de titres de poste, appariement compétences-métiers — sourcées depuis les statistiques du travail officielles et les API publiées.
- **Recommandations explicables.** Quand un emploi correspond, vous voyez _pourquoi_ — quelles de vos compétences déclarées se sont alignées, quelles attentes d'expérience il satisfait ou manque, quelles fourchettes salariales sont typiques pour ce rôle. Aucune boîte noire de classement.
- **Conçu pour la culture professionnelle française.** Codes ROME, catégories de postes spécifiques cadres, normes de salaire brut, types de contrat (CDI/CDD/stage/contrat pro) et compréhension des mobilités géographiques — baked in, pas rajoutés après coup.

## État — au stade de l'idée

Carrière est en cours de conception comme spécification et ébauche de fonctionnalités ; aucun code produit n'existe pour l'instant. L'espace du problème est bien compris :

- France Travail propose une API officielle d'offres d'emploi (OAuth2, limites de re-requête 24 h, données structurées) ; la licence est spécifique et exige l'attribution.
- ROME 4.0 est la taxonomie canonique pour l'appariement emplois-compétences (granularité 6, 32, 80 ou 492 disponible).
- L'Apec (Association pour l'emploi des cadres) n'expose pas d'API publique ; les données d'emploi spécifiques cadres sont actuellement en import manuel ou via des intégrations partenaires uniquement.
- La transparence salariale est limitée : la directive UE 2023/970 (transparence des rémunérations) n'est pas encore transposée en droit français (décision du Conseil d'État, juin 2026 ; date d'application probable 2028). Aujourd'hui, les fourchettes salariales sont absentes de la plupart des annonces officielles et doivent être inférées.
- Le marché caché (réseaux informels, recrutement direct) est connu pour dominer mais n'est pas quantifiable — les affirmations des « 70 % d'emplois » sont non vérifiées et doivent être rejetées.
- La transparence salariale est inégale : la plupart des offres omettent une fourchette, si bien que les signaux de rémunération doivent être reconstruits à partir de références sourcées plutôt que lus dans l'annonce.

**Prochaines étapes** — le propriétaire définira :

- L'ensemble des fonctionnalités core (appariement profil, alertes emploi, guides de négociation salariale, outils de recherche d'employeur, préparation aux entretiens).
- La feuille de route d'intégration (France Travail, ROME, sources de données partenaires).
- L'expérience utilisateur (mobile-first ? desktop ? les deux ?).
- Les garanties de confidentialité et les politiques de rétention des données.

Cible de référence : **à définir par le propriétaire.** Carrière est au stade de l'idée ; sa référence de parité best-in-class n'a pas encore été choisie.

## Architecture — assemblée à partir de fondations partagées

Carrière est un produit assemblé à partir de briques versionnées indépendamment et définies dans le dépôt de base. Chaque brique est utilisable et testable seule ; le produit les compose via des packages publiés (la cible multi-dépôts de [l'ADR-0008](https://github.com/libre-ai/libre-ai/blob/main/docs/adr/0008-multi-repo-target-topology-and-brand.md)).

| Brique                                        | Rôle                                                | État                             |
| --------------------------------------------- | --------------------------------------------------- | -------------------------------- |
| **Plateforme web** (`@libre-ai/web-platform`) | Fondation SSR, récupération côté serveur            | Publié depuis la base            |
| **Tokens design system**                      | Couleurs, typographie, espacements, scales          | Publié depuis la base            |
| **Auth / identité**                           | Gestion sessions OAuth2, intégration France Travail | À concevoir, consommé de la base |
| **Contrats de données**                       | Schéma offres d'emploi, ROME, structures salaires   | À définir dans `contracts/` base |

L'hôte produit (ce dépôt, à l'activation) câblera ces briques dans une expérience utilisateur cohésive.

## Où se déroule le travail

Une fois activé, tout le développement actif se déroulera dans ce dépôt :

- `apps/carriere` — l'hôte produit (pages rendues serveur, interface de recherche d'emploi, alertes et recherches sauvegardées, profiling d'employeurs).
- `src/` — code applicatif, composants, intégrations avec l'API France Travail.
- `tests/` — suites de tests end-to-end et unitaires.

La spécification, les contrats de données et les briques partagées sont rédigés dans le dépôt de base, sous :

- [`docs/apps/carriere.md`](https://github.com/libre-ai/libre-ai/blob/main/docs/apps/carriere.md) — le cahier des charges produit complet (à rédiger).
- `crates/` — backends Rust pour appariement emplois, modélisation salariale, ou analytique du marché du travail (si applicable).
- `packages/` — packages TypeScript pour intégration API France Travail, gestion taxonomie ROME, etc.
- `contracts/` — schémas verrouillés pour offres d'emploi, profils utilisateurs, recherches sauvegardées.

Pour suivre l'avancement ou contribuer, ouvrez issues et pull requests dans [`libre-ai/libre-ai`](https://github.com/libre-ai/libre-ai). Ce dépôt est réservé et s'activera sur décision du propriétaire.

## Licence

EUPL-1.2.
