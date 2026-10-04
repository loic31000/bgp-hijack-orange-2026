# BGP Hijack visant Orange France (AS3215)

Investigation technique indépendante sur des annonces BGP anormales touchant de l'espace IPv4 attribué à **Orange France (AS3215)**, avec un focus principal sur `90.98.0.0/15` et `92.183.128.0/18`.

L'enquête retrace les changements d'AS path, les pivots d'infrastructure, l'état RPKI/IRR et la réponse d'Orange. Elle a été enrichie après revue et échanges techniques avec **Doug Madory (Kentik)**, puis recoupée avec plusieurs publications publiques de **The Spamhaus Project** sur le même ensemble d'annonces suspectes.

> **Rapport complet :** https://loic31000.github.io/bgp-hijack-orange-2026/

## Résumé

L'investigation documente notamment :

- l'annonce de `90.98.0.0/15` via **AS41128 (ORANGEFR-GRX-AS)** alors que ce chemin ne correspondait pas au routage attendu d'Orange ;
- un chemin observé passant notamment par **AS22541 (MEGALINK)** et **AS29802 (Hivelocity)** après un premier pivot impliquant **AS199524 (Gcore)** ;
- l'utilisation d'ASN peu ou plus utilisés associés à de grands opérateurs, combinée à des chemins de transit géographiquement incohérents ;
- l'absence de ROA couvrant certains préfixes étudiés au moment de l'enquête, laissant leur validation RPKI à l'état `UNKNOWN` ;
- des objets IRR/ALTDB étudiés comme vecteur possible de réinjection ;
- la reprise de contrôle par Orange au moyen d'annonces plus spécifiques, suivie du retrait de la route `90.98.0.0/15` via AS41128.

Le rapport v6 élargit aussi l'analyse à d'autres annonces suspectes corrélées au même schéma opérationnel.

## Doug Madory / Kentik

Doug Madory a apporté une **revue technique** de l'enquête et a publié une mise à jour sur la réponse d'Orange.

La chronologie retenue dans le rapport est la suivante :

- **20 avril 2026 — 15:01 UTC** : AS3215 commence à annoncer `90.98.0.0/16` et `90.99.0.0/16`, deux routes plus spécifiques que le `90.98.0.0/15` litigieux ;
- ces annonces plus spécifiques redonnent à Orange la préférence de routage pour cet espace ;
- **21 avril 2026 — 17:27 UTC** : la route `90.98.0.0/15` via AS41128 est retirée du DFZ ;
- le rapport reproduit également la chute de visibilité observée dans **Kentik BGP Route Viewer** autour de ce retrait.

➡️ [Voir la section Doug Madory / Kentik dans le rapport](https://loic31000.github.io/bgp-hijack-orange-2026/#update-madory)

Cette corroboration porte sur la **chronologie et le comportement de routage observé**. Elle ne doit pas être interprétée comme une validation automatique de chaque hypothèse d'attribution développée dans l'enquête.

## The Spamhaus Project

Spamhaus a publié plusieurs analyses indépendantes décrivant le même ensemble de routes anormales.

Leurs observations publiques recoupent plusieurs points centraux de l'enquête :

- `90.98.0.0/15` annoncé via **AS41128 → AS22541 → AS29802** ;
- déplacement apparent de l'infrastructure de **Chicago vers Dallas** ;
- incohérence géographique de certains AS de transit présents dans le chemin ;
- réutilisation d'ASN inactifs ou très peu utilisés appartenant à de grands opérateurs ;
- schéma consistant à annoncer un grand réseau peu utilisé depuis un ASN du même opérateur, puis à insérer de faux/intermédiaires de transit afin de rendre le chemin moins évident ;
- constat ultérieur qu'Orange avait annoncé des routes plus spécifiques prenant le dessus sur la route détournée, avant le retrait de cette dernière.

Références Spamhaus :

- [Large-scale routes involving Comcast, Charter, Orange and Gcore](https://www.linkedin.com/posts/the-spamhaus-project_weve-recently-observed-some-unusual-large-scale-activity-7449473128293896192-nq-j)
- [Update: Orange / AS41128 path moved to AS22541 + AS29802](https://www.linkedin.com/posts/the-spamhaus-project_look-over-the-past-48-hours-there-have-been-activity-7450561445601226752-5MvX)
- [Orange fights back against hijackers](https://www.linkedin.com/posts/the-spamhaus-project_earlier-this-week-orange-announced-new-routes-activity-7453008761964793856-qhMm)

Les publications Spamhaus constituent une **corroboration indépendante du routage et du modus operandi observés**. Elles ne valident pas nécessairement l'intégralité des conclusions ou attributions du rapport.

## Chronologie courte

| Date | Événement |
|---|---|
| Mai 2024 | Première opération documentée dans l'enquête sur `92.183.128.0/18` |
| 25 fév. 2026 | Première annonce documentée de `90.98.0.0/15` via AS41128 |
| 13 avr. 2026 | Pic d'annonces observé dans les données étudiées |
| 14–15 avr. 2026 | Pivot du chemin vers Hivelocity ; Spamhaus documente publiquement les routes suspectes |
| 20 avr. 2026 · 15:01 UTC | Orange annonce les /16 plus spécifiques |
| 21 avr. 2026 · 17:27 UTC | Retrait de la route `90.98.0.0/15` via AS41128 |
| 22–29 avr. 2026 | Investigation complémentaire : RPKI, IRR, infrastructure, corrélations et rapport v6 |

## Contenu du dépôt

- **[`index.html`](./index.html)** — rapport complet v6.0, chronologie, visualisations et sources ;
- **[`Tableau_ASN_GitHub.csv`](./Tableau_ASN_GitHub.csv)** — tableau de travail des ASN, infrastructures et observations ;
- **[`README.md`](./README.md)** — synthèse et points de corroboration externes.

## Méthodologie

L'enquête repose principalement sur de l'**OSINT réseau** et des sources publiques : données BGP, RIPEstat, RPKI, IRR, WHOIS, DNS, certificats, outils de visibilité de routage et publications techniques tierces.

Les constats sont datés et correspondent à l'état des sources au moment de l'investigation. Les éléments explicitement observés sont distingués autant que possible des **hypothèses, corrélations et conclusions d'attribution**.

---

**Auteur :** loic31000  
**Rapport public :** https://loic31000.github.io/bgp-hijack-orange-2026/
