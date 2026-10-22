# Cas d'étude : Assurance-Xpert 360 (CRM SaaS)

> Reformulation personnelle du scénario fourni en cours. Entreprise et application **fictives**.

## Contexte

**Prévoria Assurances** est une société de 1000 salariés spécialisée dans la gestion de contrats d'assurance santé pour des entreprises clientes. La Direction commerciale souhaite remplacer son ancien CRM interne par une solution SaaS, **Assurance-Xpert 360**, hébergée sur le cloud Azure et accessible depuis Internet.

L'application sera utilisée par environ **250 collaborateurs** (commerciaux, responsables commerciaux, marketing, service client, administrateurs fonctionnels). Elle devient le **principal outil de gestion de la relation client** : environ **80 % de l'activité commerciale** en dépendra.

## Fonctions de l'application

- Gérer prospects et clients, enregistrer les coordonnées des interlocuteurs ;
- conserver l'historique des échanges, gérer les opportunités commerciales ;
- stocker contrats et propositions commerciales ;
- produire des tableaux de bord ;
- envoyer automatiquement certains emails ;
- exporter les données vers Excel ;
- interfacer le CRM avec d'autres applications (ERP).

Certaines données concernent aussi les **salariés et assurés** des entreprises clientes.

## Données traitées

| Catégorie | Exemples |
|-----------|----------|
| Identité | Nom, prénom |
| Coordonnées | Email, téléphone |
| Informations professionnelles | Fonction, entreprise |
| Données commerciales | Contrats, chiffre d'affaires, propositions |
| Historique client | Appels, emails, réunions |
| Documents | Contrats au format PDF |
| Informations financières | Tarifs négociés |
| Données internes | Commentaires commerciaux |
| **Données personnelles** | Informations concernant certains assurés (**RGPD**) |

## Chaîne de sous-traitance

Le fournisseur recourt à plusieurs **sous-traitants** pour : l'envoi d'emails, les sauvegardes, le support utilisateur, et l'analyse comportementale de l'application.

## Points saillants du dossier (faits bruts)

**Gestion des accès**
- Authentification via **SSO Microsoft Entra ID**.
- MFA **obligatoire pour les administrateurs**, mais **pas systématique pour les commerciaux**.
- ~**15 comptes locaux** CloudSales subsistent pour des prestataires.
- Les responsables commerciaux peuvent **s'attribuer certains droits**.
- **Aucune revue trimestrielle** des droits. **23 anciens utilisateurs** ont encore un compte actif.
- **3 administrateurs** disposent du rôle **« Super Administrator »** (accès à toutes les données, création de comptes, modification des droits, suppression de données, export intégral de la base).

**API**
- Une **API relie l'application à l'ERP**, via une **clé API valable 365 jours**.
- La clé est **stockée en clair dans un fichier de configuration**, **partagée entre plusieurs serveurs**, **non renouvelée automatiquement**, et autorise **lecture + écriture**.
- Plusieurs milliers d'appels API par jour. **Aucune alerte** en cas d'usage anormal.

**Journalisation**
- Sont journalisés : connexions, certaines modifications, créations de comptes, certains exports.
- Logs conservés **30 jours**, **non envoyés au SIEM** de Prévoria Assurances.
- Le SOC surveille les postes, Entra ID et les serveurs internes, mais a **peu de visibilité sur le SaaS**.

**Sauvegardes & disponibilité**
- Fournisseur : **99,9 %** de disponibilité annuelle, sauvegardes quotidiennes.
- Le contrat ne précise pas clairement **RPO, RTO**, modalités de restauration, ni durée de conservation.
- **Aucun test de restauration** n'a jamais été réalisé.
- Une indisponibilité **> 8 h** perturberait fortement l'activité.

**Administration du fournisseur**
- Les équipes support peuvent **accéder aux environnements clients**.
- Accès annoncés comme « strictement contrôlés », mais **aucune information** sur le MFA des admins fournisseur, les habilitations, la journalisation de leurs interventions, ou la localisation des équipes.
