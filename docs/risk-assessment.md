# Analyse de risque ISO/IEC 27005:2022

**Cas pratique : Assurance-Xpert 360 (CRM SaaS) · Société Prévoria Assurances**

Module : Governance, Risk & Compliance (GRC) · Réalisé par : Haitam Oualif

## Sommaire

1. [Contexte et objectif de l'analyse](#1-contexte-et-objectif-de-lanalyse)
2. [Périmètre et parties prenantes](#2-périmètre-et-parties-prenantes)
3. [Critères d'appréciation (échelles)](#3-critères-dappréciation-échelles)
4. [Inventaire et valorisation des biens](#4-inventaire-et-valorisation-des-biens)
5. [Menaces et vulnérabilités (par zone)](#5-menaces-et-vulnérabilités-par-zone)
6. [Registre des risques (scénarios)](#6-registre-des-risques-scénarios)
7. [Cotation et évaluation des risques](#7-cotation-et-évaluation-des-risques)
8. [Traitement des risques et risques résiduels](#8-traitement-des-risques-et-risques-résiduels)
9. [Acceptation et surveillance](#9-acceptation-et-surveillance)
10. [Note de synthèse (Direction)](#10-note-de-synthèse-direction)
11. [Limites et choix méthodologiques](#11-limites-et-choix-méthodologiques)

- [Annexe : Justification des cotations et des risques résiduels](#annexe--justification-des-cotations-et-des-risques-résiduels)

---

## 1. Contexte et objectif de l'analyse

La société Prévoria Assurances (1000 salariés, gestion de contrats d'assurance santé) remplace son ancien CRM interne par la solution SaaS Assurance-Xpert 360, hébergée sur le cloud Azure et accessible depuis Internet. L'application sera utilisée par environ 250 collaborateurs (commerciaux, marketing, service client, administrateurs) et traite des données sensibles, dont des données personnelles d'assurés relevant du RGPD. Elle devient le principal outil de la relation client : environ 80 % de l'activité commerciale en dépendra.

**Objectif de l'analyse :** apprécier les risques de sécurité liés à l'adoption de ce SaaS afin de (1) répondre aux exigences d'un SMSI, (2) assurer la conformité RGPD, et (3) éclairer la Direction sur les conditions d'un déploiement maîtrisé. La démarche suit le processus de l'ISO/IEC 27005:2022 : établissement du contexte, appréciation du risque (identification, analyse, évaluation), traitement et acceptation, avec communication et surveillance en continu.

## 2. Périmètre et parties prenantes

**Inclus :** l'application SaaS et ses données, le flux API vers l'ERP, la gestion des identités (Microsoft Entra ID et comptes locaux CloudSales), la journalisation, les sauvegardes et la disponibilité, ainsi que les accès du fournisseur et de ses sous-traitants.

**Exclus :** la sécurité physique des centres de données Azure et l'infrastructure sous-jacente du fournisseur, qui relèvent d'un modèle de **responsabilité partagée** (le client reste néanmoins responsable des accès, des données et de la configuration).

**Parties prenantes / propriétaires de risque (risk owners) :**

| Partie prenante | Responsabilité |
| ----------------- | ---------------- |
| RSSI / DSI | Biens techniques : API, IAM, journalisation, sauvegardes |
| Direction commerciale | Processus métier : 250 utilisateurs, 80 % de l'activité |
| DPO | Données personnelles des assurés (RGPD) |

## 3. Critères d'appréciation (échelles)

Conformément à l'ISO 27005, les critères sont définis et documentés **avant** l'appréciation. Formule retenue : **Risque = Probabilité × Impact**, sur une matrice 5×5 (valeurs de 1 à 25).

### 3.1. Échelle de gravité / impact (1 à 5)

| N | Libellé | Repère (financier · légal-RGPD · réputation · opérationnel) |
| --- | --------- | ------------------------------------------------------------- |
| 1 | Négligeable | Gêne interne, aucune donnée sensible, pas d'interruption notable |
| 2 | Mineur | Interruption courte (< 1 h) ou incident sans donnée personnelle |
| 3 | Important | Interruption de quelques heures ou fuite de données commerciales internes ; réputation entamée |
| 4 | Majeur | Fuite de données personnelles d'assurés (obligations RGPD) ou indisponibilité > 8 h |
| 5 | Critique | Compromission massive de la base + sanction RGPD lourde + réputation durablement atteinte, ou arrêt prolongé menaçant l'activité (80 %) |

### 3.2. Échelle de probabilité (1 à 5)

| N | Libellé | Repère (motivation · capacité · facilité d'exploitation · contrôles existants) |
| --- | --------- | --------------------------------------------------------------------------------- |
| 1 | Très improbable | Conditions exceptionnelles, aucun signe |
| 2 | Improbable | Possible mais peu motivé ou difficile |
| 3 | Possible | Vulnérabilité réelle, exploitable sur l'année |
| 4 | Probable | Faille connue, exposée sur Internet, attaquants motivés |
| 5 | Quasi-certain | Exploitation triviale ou signaux déjà présents |

### 3.3. Critère d'acceptation du risque

| Niveau | Plage (P×I) | Décision |
| -------- | ------------- | ---------- |
| Faible | 1 – 4 | Acceptable |
| Modéré | 5 – 9 | Acceptable sous surveillance |
| Élevé | 10 – 14 | À traiter |
| Critique | 15 – 25 | Inacceptable (traitement prioritaire) |

## 4. Inventaire et valorisation des biens

Valorisation par analyse d'impact (**BIA**) : impact si le bien est indisponible (D), divulgué (C) ou altéré (I), noté de 1 à 5. La valeur retenue est le **maximum**.

### 4.1. Biens primaires (données et processus métier)

| ID | Bien | D | I | C | Val. | Justification |
| ---- | ------ | --- | --- | --- | ------ | --------------- |
| D1 | Base prospects / clients | 4 | 4 | 5 | **5** | Cœur du CRM ; secret commercial + données personnelles |
| D2 | Données personnelles des assurés | 3 | 4 | 5 | **5** | RGPD : une fuite est une violation à notifier + sanction |
| D3 | Contrats / propositions / tarifs négociés | 3 | 4 | 5 | **5** | Confidentialité concurrentielle forte |
| P1 | Processus relation client / activité commerciale | 5 | 3 | 3 | **5** | 80 % de l'activité en dépend ; > 8 h d'arrêt = perturbation forte |

### 4.2. Biens supports

| ID | Bien | D | I | C | Val. | Justification |
| ---- | ------ | --- | --- | --- | ------ | --------------- |
| S1 | Application SaaS Assurance-Xpert 360 | 5 | 4 | 4 | **5** | Support de P1 ; multi-tenant, exposée Internet |
| S2 | API / iPaaS vers l'ERP (clé API) | 3 | 5 | 4 | **5** | Droits lecture + écriture sur l'ERP → intégrité critique |
| S3 | IAM : Entra ID SSO + 15 comptes locaux | 4 | 4 | 5 | **5** | Clé de voûte des accès ; compromission = accès à tout |
| S4 | Journaux / logs | 2 | 4 | 3 | **4** | Preuve / traçabilité ; indispensable en cas d'incident |
| S5 | Sauvegardes | 4 | 3 | 3 | **4** | Dernier rempart de disponibilité ; contient une copie des données |
| S6 | Fournisseur + sous-traitants | 4 | 3 | 4 | **4** | Dépendance tierce ; accès aux environnements clients |

## 5. Menaces et vulnérabilités (par zone)

| Zone | Vulnérabilités | Menaces / sources |
| ------ | ---------------- | ------------------- |
| Accès / IAM | MFA non imposé aux commerciaux ; 23 comptes dormants ; aucune revue trimestrielle ; droits auto-attribués ; 15 comptes locaux ; 3 Super Administrators (export intégral) | Phishing & usurpation (cybercriminel) ; abus de privilège (interne/prestataire) |
| API ↔ ERP | Clé API valable 365 j, en clair dans un fichier de conf, partagée entre serveurs, droits lecture + écriture, aucune alerte | Vol de secret → exfiltration et altération (attaquant avec accès serveur) |
| Journalisation | Logs conservés 30 j, non envoyés au SIEM, SOC sans visibilité sur le SaaS | Incident non détecté / investigation impossible (aggravant) |
| Sauvegardes & disponibilité | RPO/RTO non contractualisés, modalités floues, aucun test de restauration, dépendance 80 %, rupture à 8 h | Panne fournisseur ; corruption / ransomware côté SaaS |
| Fournisseur & sous-traitants | Accès support sans visibilité (MFA ? habilitations ? logs ? localisation ?) ; 4 sous-traitants | Accès tiers illégitime ; fuite via la chaîne de sous-traitance |
| Transit / architecture | TLS 1.1 (protocole obsolète et déprécié) sur l'accès Internet | Interception / downgrade des données en transit |

## 6. Registre des risques (scénarios)

Chaque scénario combine : **Bien + Source + Menace + Vulnérabilité exploitée → événement redouté → conséquence** (DICP : Disponibilité, Intégrité, Confidentialité, Preuve).

| # | Bien | Source | Menace | Vulnérabilité exploitée | Événement redouté | Conséquence |
| --- | ------ | -------- | -------- | ------------------------- | ------------------- | ------------- |
| R1 | Base clients | Cybercriminel ext. | Phishing / usurpation | MFA absent + comptes dormants + pas de revue | Connexion frauduleuse puis export de données clients | C (RGPD), réputation |
| R2 | Base complète | Interne / compte compromis | Abus de privilège | Rôle Super Admin (accès total + export intégral) | Export massif de toute la base | C (+ I) : fuite totale |
| R3 | IAM | Interne / prestataire | Accès illégitime persistant | Comptes locaux + dormants + droits auto-attribués | Accès maintenu ou élevé sans légitimité | C / I |
| R4 | Flux API / ERP | Attaquant (accès serveur) | Vol de clé API | Clé 365 j en clair, R/W, pas d'alerte | Exfiltration et altération des données ERP | C + I |
| R5 | Journaux | Facteur aggravant | Défaut de détection | Logs 30 j, hors SIEM, SOC aveugle sur le SaaS | Incident non détecté / non investigable | P + aggrave tous |
| R6 | Dispo. CRM | Panne / incident | Interruption de service | RPO/RTO flous, 0 test de restauration, dépendance 80 % | Indisponibilité > 8 h | D |
| R7 | Données + sauvegardes | Ransomware / corruption | Perte / restauration impossible | Sauvegardes jamais testées, modalités floues | Perte durable de données | D + I |
| R8 | Environnement SaaS | Fournisseur / support | Accès tiers non maîtrisé | Aucune visibilité MFA/habilitations/logs/localisation | Accès ou fuite via le fournisseur | C |
| R9 | Données | Sous-traitants | Fuite via chaîne d'appro. | 4 sous-traitants, sous-traitance non maîtrisée | Fuite ou traitement non conforme | C + conformité |
| R10 | Données assurés | Conformité | Non-conformité RGPD | Données perso + sous-traitance + transferts non maîtrisés | Violation de données / manquement RGPD | C + légal |
| R11 | Données en transit | Attaquant réseau | Interception / downgrade | TLS 1.1 obsolète sur l'accès Internet | Interception de données en transit | C / I |

## 7. Cotation et évaluation des risques

| # | Risque | P | I | R | Niveau | Justification |
| --- | -------- | --- | --- | --- | -------- | --------------- |
| R1 | Compromission compte commercial | 4 | 5 | **20** | 🔴 Critique | Phishing trivial + MFA absent + comptes dormants ; export Excel → fuite large |
| R10 | Non-conformité RGPD | 4 | 4 | **16** | 🔴 Critique | Sous-traitance + données assurés → manquement probable ; sanction |
| R2 | Abus rôle Super Admin | 3 | 5 | **15** | 🔴 Critique | MFA admin abaisse l'externe, mais menace interne + export intégral |
| R4 | Vol de la clé API | 3 | 5 | **15** | 🔴 Critique | Accès serveur requis, mais clé en clair R/W + aucune alerte → C + I |
| R7 | Perte de données / restauration KO | 3 | 5 | **15** | 🔴 Critique | Ransomware plausible + sauvegardes jamais testées → perte durable |
| R3 | Accès illégitime persistant | 4 | 3 | **12** | 🟠 Élevé | Dormants + comptes locaux + droits auto → très exploitable ; portée variable |
| R5 | Défaut de détection | 4 | 3 | **12** | 🟠 Élevé | État actuel : un incident SaaS passe inaperçu ; gonfle l'impact des autres |
| R6 | Indisponibilité > 8 h | 3 | 4 | **12** | 🟠 Élevé | 99,9 % ≈ 8,7 h/an possible + aucun PRA testé ; 80 % de l'activité |
| R8 | Accès fournisseur non maîtrisé | 3 | 4 | **12** | 🟠 Élevé | Support accède sans visibilité ; accès potentiel aux données assurés |
| R9 | Fuite via sous-traitants | 3 | 4 | **12** | 🟠 Élevé | 4 sous-traitants traitant des données perso → fuite / non-conformité |
| R11 | Interception via TLS 1.1 | 3 | 4 | **12** | 🟠 Élevé | Protocole déprécié + MITM possible ; données perso en transit |

**Priorisation (classement décroissant).** Au Point de décision 1, l'appréciation est jugée satisfaisante : aucun risque n'est acceptable en l'état (5 critiques, 6 élevés), tous font l'objet d'un traitement.

| Rang | # | R | Décision |
| ------ | --- | --- | ---------- |
| 1 | R1 | 20 | Traitement prioritaire |
| 2 | R10 | 16 | Traitement prioritaire |
| 3 | R2 / R4 / R7 | 15 | Traitement prioritaire |
| 6 | R3 / R5 / R6 / R8 / R9 / R11 | 12 | À traiter |

## 8. Traitement des risques et risques résiduels

Pour chaque risque : option (**Modifier / Éviter / Partager / Conserver**), mesures concrètes (type de contrôle), puis risque résiduel ré-estimé après traitement.

| # | Option | Mesures (type de contrôle) | P→ | I→ | R résiduel |
| --- | -------- | ---------------------------- | ---- | ---- | ------------ |
| R1 | Modifier | MFA obligatoire pour tous (préventif) ; désactivation comptes dormants + revue trimestrielle (détectif) ; surveillance et limitation des exports via DLP (détectif) | 2 | 4 | **8 (Modéré)** |
| R10 | Modifier + Partager | Registre des traitements + AIPD (directif) ; clauses de sous-traitance art. 28 / DPA + encadrement des transferts (administratif) | 2 | 3 | **6 (Modéré)** |
| R2 | Modifier | Réduire le nombre de Super Admins + comptes à privilèges dédiés (PAM/PIM, just-in-time) (préventif) ; séparation des tâches + journalisation renforcée (détectif) | 2 | 4 | **8 (Modéré)** |
| R4 | Modifier | Clé dans un coffre à secrets (Key Vault) + rotation courte (préventif) ; 1 identité par service + scope lecture seule (préventif) ; alerte SIEM (détectif) | 2 | 4 | **8 (Modéré)** |
| R7 | Modifier | Tests de restauration périodiques (récupération) ; sauvegardes immuables/hors-ligne + RPO/RTO vérifiés (correctif) | 2 | 3 | **6 (Modéré)** |
| R3 | Modifier | Suppression comptes dormants + fédération/suppression comptes locaux (préventif) ; workflow d'approbation des droits + revue trimestrielle (détectif) | 2 | 3 | **6 (Modéré)** |
| R5 | Modifier | Connexion des logs SaaS au SIEM + rétention > 30 j + règles d'alerte au SOC (détectif) | 2 | 3 | **6 (Modéré)** |
| R6 | Modifier + Partager | Contractualiser RPO/RTO + SLA (partage) ; PRA testé + plan de continuité ISO 22301 (récupération) | 2 | 3 | **6 (Modéré)** |
| R8 | Modifier + Partager | Exiger par contrat la visibilité (MFA, habilitations, logs, localisation) + droit d'audit (administratif) ; accès support validé et tracé (détectif) | 2 | 3 | **6 (Modéré)** |
| R9 | Partager + Modifier | DPA art. 28 avec chaque sous-traitant + liste des sous-traitants ultérieurs (administratif) ; clauses sécurité + audit (directif) | 2 | 3 | **6 (Modéré)** |
| R11 | Modifier | Forcer TLS 1.2+, désactiver TLS 1.0/1.1 + HSTS (préventif, *quick win*) | 1 | 4 | **4 (Faible)** |

## 9. Acceptation et surveillance

**Acceptation (Point de décision 2) :** après traitement, tous les risques repassent sous le seuil critique : dix en niveau Modéré (acceptables sous surveillance) et R11 en niveau Faible. Les risques résiduels sont acceptés formellement par les propriétaires de risque (RSSI pour le technique, Direction commerciale pour le métier, DPO pour le RGPD).

**Communication :** plan de traitement validé en comité de sécurité ; le fournisseur est sollicité pour les engagements contractuels (R6, R8, R9).

**Surveillance et revue :** une fois les journaux raccordés au SIEM (R5), l'analyse est revue à chaque changement majeur et au moins une fois par an, conformément à la logique d'amélioration continue (**PDCA**).

## 10. Note de synthèse (Direction)

**Objet :** Analyse de risque ISO 27005 : adoption du CRM SaaS Assurance-Xpert 360.

**Constat général :** l'application est saine dans son principe (SSO Entra ID, WAF, chiffrement), mais plusieurs faiblesses structurelles l'exposent : authentification incomplète, gestion des accès non maîtrisée, secret d'API durable et sur-privilégié, absence de supervision (logs non exploités), continuité non éprouvée (sauvegardes jamais testées) et relation fournisseur peu encadrée (contrat et RGPD).

**Les 5 risques les plus critiques :**

1. **Compromission d'un compte commercial (20/25) :** sans MFA généralisé, un phishing peut exposer les données clients et assurés.
2. **Non-conformité RGPD (16/25) :** données d'assurés et sous-traitance non encadrées → risque de sanction.
3. **Abus du rôle Super Administrator (15/25) :** trois comptes peuvent exporter l'intégralité de la base.
4. **Vol de la clé API (15/25) :** clé permanente, en clair, avec droits d'écriture sur l'ERP → atteinte à l'intégrité.
5. **Restauration non garantie (15/25) :** sauvegardes jamais testées → perte de données possible.

**Décisions attendues :** le déploiement est recommandé **sous conditions**, en deux temps.

- **Mesures rapides (quelques semaines) :** généraliser le MFA, nettoyer les comptes dormants et locaux, forcer TLS 1.2+, raccorder les journaux au SIEM.
- **Chantiers de fond (quelques mois) :** sécuriser et faire tourner les secrets d'API, tester les restaurations et contractualiser RPO/RTO, encadrer les comptes à privilèges (PAM), formaliser le cadre RGPD et les clauses de sécurité avec le fournisseur et ses sous-traitants.

Après traitement, l'ensemble des risques repasse à un niveau tolérable. **Recommandation :** valider ces chantiers et obtenir les engagements contractuels du fournisseur avant la bascule complète.

---

## 11. Limites et choix méthodologiques

Cette section explicite les choix retenus et les limites assumées de l'analyse, condition d'une démarche rigoureuse et base de discussion lors d'une revue.

**Approche par les biens vs approche par événement.** L'ISO/IEC 27005:2022 reconnaît deux angles d'identification du risque : *asset-based* (on part des biens) et *event-based* (on part des événements redoutés / scénarios stratégiques, dans l'esprit d'EBIOS Risk Manager). J'ai retenu l'approche **par les biens** parce que le périmètre est centré sur une application unique et bien délimitée, pour laquelle l'inventaire des biens est accessible et stable. Une approche par événement serait plus pertinente pour une vision stratégique multi-périmètres (ex. chaîne de valeur complète de l'assureur).

**Matrice multiplicative P×I : limite connue.** Le produit `P × I` écrase l'information : un couple `P=1 / I=5` (événement rare mais catastrophique) et `P=5 / I=1` (fréquent mais bénin) donnent le même score (5) alors que leur traitement diffère. J'assume ce choix car (1) il reste lisible pour la Direction, et (2) l'impact est ici plafonné par le contexte métier (données RGPD + dépendance à 80 %), ce qui limite les cas à faible impact. En cas de besoin, la matrice serait complétée par une **règle de priorité à l'impact** (tout I=5 traité en priorité quel que soit P) pour corriger l'effet d'aplatissement.

**Risque inhérent vs risque courant.** L'échelle de probabilité intègre « les contrôles existants ». Les cotations du §7 sont donc des **risques courants** (nets des contrôles déjà en place : SSO, WAF, chiffrement, MFA admin), et non des risques inhérents bruts. Le §8 réduit encore P et I via les contrôles additionnels pour obtenir le **risque résiduel**. Cette distinction évite de compter deux fois un même contrôle.

**R5 : facteur aggravant plutôt que scénario autonome.** Le défaut de détection (logs non raccordés au SIEM) n'a pas d'« événement redouté » propre : il n'engendre pas de préjudice seul, mais **allonge la durée et aggrave l'impact** des autres scénarios. Je l'ai néanmoins coté (12, Élevé) pour le rendre visible dans la priorisation et justifier le quick win SIEM ; dans une lecture stricte, il relèverait d'un registre de « faiblesses transverses » distinct du registre des risques.

**Résiduels calibrés à P=2.** La plupart des probabilités résiduelles convergent vers 2 : c'est le reflet du fait que les mesures préventives retenues (MFA, PAM, coffre à secrets, nettoyage des comptes) ramènent des vulnérabilités « exploitables » à « improbables » sans pouvoir atteindre 1 (le risque zéro n'existe pas, et la menace interne/tiers demeure). Les valeurs ne sont pas mécaniques : R11 descend à P=1 car la mesure **supprime** la vulnérabilité (désactivation de TLS 1.1), là où les autres ne font que la **réduire**.

**Données d'entrée.** L'analyse s'appuie sur les éléments déclaratifs du dossier (SLA annoncé, « accès strictement contrôlés » du fournisseur). Plusieurs cotations (R6, R8, R9) sont donc conditionnées à une **vérification contractuelle** : elles seraient révisées à réception des engagements réels du fournisseur (droit d'audit, preuves de MFA, localisation des équipes).

---

## Annexe : Justification des cotations et des risques résiduels

**Principes :** la probabilité (P) se justifie par les 4 facteurs (motivation, capacité, facilité d'exploitation, contrôles existants) ; l'impact (I) par l'échelle croisée avec le critère DICP touché et le contexte métier. Pour les résiduels : une mesure **préventive réduit P**, une mesure qui **réduit la portée** (moindre privilège, segmentation, restauration fiable) **réduit I**, une mesure **détective réduit surtout la durée d'exposition**.

| # | Pourquoi ce P | Pourquoi cet I | Pourquoi le résiduel baisse |
| --- | --------------- | ---------------- | ------------------------------ |
| R1 | Phishing trivial et motivé, MFA absent + comptes dormants (P=4) | C cassée + données assurés (RGPD) + export possible (I=5) | MFA (préventif) fait chuter P ; DLP/revue limite la portée |
| R10 | Sous-traitance + données perso déjà non encadrées (P=4) | Sanction RGPD + réputation (I=4) | Registre/AIPD + clauses art. 28 : conformité démontrée |
| R2 | MFA admin abaisse l'externe, mais menace interne réelle (P=3) | Export intégral de la base (I=5) | PAM/PIM + séparation des tâches réduisent P et la portée |
| R4 | Nécessite un accès serveur d'abord (P=3) | R/W sur l'ERP → exfiltration et altération (I=5) | Coffre + rotation (P) ; scope lecture seule (I) |
| R7 | Ransomware / corruption plausible (P=3) | Sauvegardes jamais testées → perte durable (I=5) | Tests de restauration + sauvegardes immuables fiabilisent la reprise |
| R3 | Dormants + comptes locaux = très exploitable (P=4) | Portée d'un compte variable (I=3) | Nettoyage + workflow d'approbation (préventif) → P chute |
| R5 | État actuel : un incident SaaS passe inaperçu (P=4) | Perte de traçabilité, aggrave les autres (I=3) | SIEM + alertes : réduit surtout la durée d'un incident |
| R6 | 99,9 % ≈ 8,7 h/an, aucun PRA testé (P=3) | > 8 h perturbe 80 % de l'activité (I=4) | SLA contractuel + PRA testé |
| R8 | Support fournisseur accède sans visibilité (P=3) | Accès potentiel aux données assurés (I=4) | Exigences contractuelles + droit d'audit + traçabilité |
| R9 | 4 sous-traitants traitant des données perso (P=3) | Fuite / traitement non conforme (I=4) | DPA art. 28 + audits |
| R11 | TLS 1.1 déprécié, MITM possible (P=3) | Données perso en transit (I=4) | Forcer TLS 1.2+ élimine la vulnérabilité → P→1 |

> **Phrase de synthèse :** « Les mesures préventives réduisent la probabilité, les mesures de portée réduisent l'impact, et la supervision réduit le temps d'exposition ; c'est pourquoi les risques résiduels sont tolérables sans être nuls : le risque zéro n'existe pas. »
