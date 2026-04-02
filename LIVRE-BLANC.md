# KARUKIA — Livre Blanc

**Méthodologie de développement assistée par IA : 11 dimensions d'audit via le protocole MCP.**

*Version 3.1 — Document en français. English version: [WHITEPAPER.md](./WHITEPAPER.md)*

```
██╗  ██╗ █████╗ ██████╗ ██╗   ██╗██╗  ██╗    ██╗ █████╗
██║ ██╔╝██╔══██╗██╔══██╗██║   ██║██║ ██╔╝    ██║██╔══██╗
█████╔╝ ███████║██████╔╝██║   ██║█████╔╝     ██║███████║
██╔═██╗ ██╔══██║██╔══██╗██║   ██║██╔═██╗     ██║██╔══██║
██║  ██╗██║  ██║██║  ██║╚██████╔╝██║  ██╗    ██║██║  ██║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝    ╚═╝╚═╝  ╚═╝
   Méthodologie IA pour les domaines réglementés · Made in Guadeloupe 🇬🇵
```

---

## 1. Le problème

Les assistants IA (Claude, GPT, Copilot) sont puissants mais génériques. Quand on demande un audit de sécurité, l'IA improvise : elle connaît les concepts OWASP mais ne suit pas une méthodologie structurée. Le résultat varie d'une session à l'autre, sans exhaustivité garantie.

**Conséquences concrètes :**

- Des vulnérabilités passent entre les mailles parce que l'IA « oublie » de vérifier certains points
- Aucune traçabilité : impossible de prouver qu'un contrôle a été effectué
- Pas de reproductibilité : deux audits du même code donnent des résultats différents
- Zéro conformité : les frameworks (ISO 27001, SOC 2, HDS) exigent des preuves formelles
- Aucune visibilité sur la qualité du code TypeScript, l'architecture, les performances, ou la dette technique

## 2. La solution KARUKIA

KARUKIA transforme le problème en injectant une **méthodologie complète** directement dans le contexte de l'IA, à la demande.

### Comment ça marche

KARUKIA est un serveur MCP (Model Context Protocol) — le standard ouvert d'Anthropic pour connecter des outils à une IA. Quand l'utilisateur demande un audit, le serveur MCP retourne un **prompt monolithique** contenant :

1. **L'identité du persona** — nom, expertise, style de communication
2. **Le workflow** — étapes obligatoires, dans l'ordre, avec gates de validation
3. **Les checklists** — tous les points de contrôle pertinents, inline dans le prompt
4. **Les templates** — format de sortie attendu (tables, scores, verdicts)
5. **Les garde-fous** — règles non-négociables (ex. : « chaque finding doit citer fichier:ligne »)

L'IA reçoit ce prompt et **devient** le spécialiste pour la durée de la session. Elle ne peut pas « oublier » un contrôle — il est écrit dans son contexte.

### Le protocole MCP

Le [Model Context Protocol](https://modelcontextprotocol.io/) est un standard ouvert qui permet à n'importe quel client IA de se connecter à des serveurs d'outils. KARUKIA est conçu et testé pour **Claude Code** et **OpenAI Codex**. Il est compatible avec tout client MCP (Cursor, Windsurf, Copilot, etc.).

L'installation se fait en une ligne :

```bash
npx karukia-mcp
```

Aucun compte, aucune clé API, aucune donnée envoyée à l'extérieur. Le serveur tourne en local sur la machine du développeur.

---

## 3. Architecture

### Transport local (par défaut)

```
Développeur <-> Client IA <-> [stdio] <-> KARUKIA MCP Server (local)
                    |                         |
                    |                         ├── 20 skills (prompt builders)
                    |                         ├── Blocs partagés :
                    |                         │     GUARD + WORKFLOW + COVERAGE + AGENTS
                    |                         ├── Abstraction plateforme (client_id) :
                    |                         │     Claude / Codex / Generic
                    |                         ├── 22 checklists (1800+ points)
                    |                         └── Système de mémoire :
                    |                               sessions/ + coverage/ + knowledge/ + trackers/
                    |
               Claude Code
               OpenAI Codex
               Tout client MCP
```

Le serveur communique via **stdio** (entrée/sortie standard). Aucun port réseau ouvert, aucun appel HTTP. Tout reste sur la machine locale du développeur.

### Ce que le serveur retourne

Chaque tool MCP retourne un **prompt monolithique** — un seul bloc de texte contenant tout ce dont l'IA a besoin. Pas de chaînes d'appels, pas de RAG, pas de base vectorielle. Juste un prompt bien construit.

Exemple simplifié pour `neo` (audit sécurité) :

```
[GUARD — obligations non-négociables]
+
[Persona Neo — identité, style, expertise]
+
[WORKFLOW — processus standardisé en 6 étapes]
+
[COVERAGE — chargement du manifeste précédent, priorisation des fichiers non-scannés]
+
[Checklist OWASP — 62 contrôles inline]
+
[Checklist ISO 27001 — 93 contrôles inline]
+
[AGENTS — stratégie d'exploration multi-agents parallèle]
+
[Template de sortie — format table + verdict]
```

Les blocs WORKFLOW et COVERAGE sont nouveaux en v3.1 — ils transforment chaque audit d'un scan ponctuel en un **processus itératif et cumulatif** où aucun fichier du codebase n'est oublié.

---

## 4. Les 11 dimensions d'audit

### Sécurité défensive (Neo)

Vérification point par point de chaque contrôle applicable. Chaque finding cite le fichier et la ligne. Chaque verdict est soit CONFORME, soit NON-CONFORME avec preuve.

| Framework | Points | Périmètre |
|-----------|--------|-----------|
| OWASP Security Baseline | 62 | Toute application web |
| HDS 2.0 | 52 | Données de santé (France) |
| ISO 27001:2022 | 93 | SMSI entreprise |
| SOC 2 Type II | 74 | SaaS (marché US) |
| PCI-DSS v4.0 | 97 | Traitement de paiements |
| HIPAA | 67 | Données de santé (US) |

Neo est systématiquement appelé après Jeffrey (implémentation). Si Neo rejette, Jeffrey corrige — boucle maximum 3 itérations.

### Qualité web (Certix)

369 règles Certix en 5 profils (DEV, UX, CONT, OPS, JUR). Deux modes : validation ciblée avant merge (`opo`) ou audit exhaustif (`audit_certix`). Certix est le référentiel qualité web propre à KARUKIA Solutions, conçu pour les applications web modernes.

### Sécurité offensive (Viper)

Méthodologie BRIGADE avec 16 agents spécialisés. Scoring CVSS v4.0, mapping MITRE ATT&CK, narratifs d'attaque réalistes. Couverture web, cloud, et santé.

### TypeScript Quality

118 points de contrôle en 7 catégories : type safety, config stricte, génériques, patterns async, modules, erreurs, métriques. Notation A à D.

### CSS / Design System

55 points de contrôle pour la maintenabilité, l'accessibilité, et la cohérence du design system.

### Architecture

70 points de contrôle pour la structure des modules, le couplage/complexité, et le respect du layering.

### Couverture de tests

68 points de contrôle pour l'inventaire frontend/backend et la qualité des tests par échantillonnage.

### Performance

90 points de contrôle couvrant le frontend, le backend, et l'analyse du build/bundle.

### Dette technique

55 points de contrôle pour le code mort, la santé des dépendances, et les code smells.

### Expert HDS / ISO 27001

200+ points de contrôle répartis en 8 domaines pour préparer la certification : cryptographie, audit trail, contrôle d'accès, classification des données, multi-tenant, résilience, gestion des vulnérabilités, réseau.

### Scan global (`karukia_scan`)

Méta-orchestrateur qui lance les 11 dimensions en parallèle — 1800+ points au total. Produit un scorecard unifié, déduplique les findings, et priorise les corrections.

---

## 5. L'orchestrateur (Auto)

L'outil `auto` est le point d'entrée principal. L'utilisateur décrit sa demande en langage naturel, et l'orchestrateur :

1. **Analyse** la requête (type, scope, complexité)
2. **Route** vers la bonne chaîne de skills
3. **Gère la boucle de rejet** (si Neo rejette, Jeffrey corrige, max 3 itérations)
4. **Consolide** le rapport final

### Table de routage

| Type de demande | Chaîne de skills |
|----------------|-----------------|
| Feature frontend | Jeffrey → Neo → Opo |
| Feature backend | Jeffrey → Neo |
| Correction de bug | Jeffrey → Neo |
| Audit sécurité | Neo |
| Pentest | Viper |
| Audit complet | karukia_scan |
| Qualité TypeScript | ts_quality |
| Architecture | archi |
| Performance | perf |
| Analyse de risques | ebios_rm_audit |
| Préparation certification | audit_expert_hds |
| Due diligence / revue de code | deep_review |

### Utilisation

```
« karukia: ajoute l'authentification utilisateur »
    → Jeffrey implémente → Neo valide la sécurité → Opo vérifie la qualité

« karukia: audite la sécurité de mon projet »
    → Neo audite point par point → rapport structuré

« karukia: lance un pentest »
    → Viper déploie 16 agents → CVSS scoring → narratifs d'attaque
```

---

## 6. Système de mémoire

KARUKIA maintient une mémoire structurée entre les sessions :

```
karukia/
├── config/
│   ├── security-scope.md         — Frameworks et contraintes actives
│   └── coverage-scopes.json      — Globs de couverture par skill (généré par install)
├── memory/
│   ├── sessions/                 — Une session par tâche
│   ├── coverage/                 — Manifestes de couverture (un par skill)
│   ├── knowledge/                — Leçons apprises et patterns réutilisables
│   └── references/               — Plans de durcissement
└── trackers/
    └── KARUKIA-TRACKER.json      — Tracker unifié (toutes les dimensions)
```

Le `KARUKIA-TRACKER.json` trace les chantiers d'amélioration sur toutes les dimensions : sécurité, qualité, TypeScript, CSS, architecture, tests, performance et dette. Chaque skill lit et met à jour sa section automatiquement.

### Communication inter-skills

Les skills communiquent via `context.json` dans le répertoire de session. Quand Jeffrey implémente une feature, il écrit un contexte que Neo lit pour la validation sécurité. Quand Neo rejette, Jeffrey lit la raison du rejet et corrige. Cela permet la chaîne de validation : **Jeffrey -> Neo -> Opo** avec boucle de rejet automatique (max 3 itérations).

---

## 7. Suivi de couverture (Nouveau en v3.1)

Les audits IA traditionnels sont ponctuels : l'IA analyse ce qui tient dans sa fenêtre de contexte et s'arrête. Si votre codebase contient 300 fichiers et que l'IA en a analysé 80, vous n'avez aucun moyen de savoir quels 220 ont été oubliés -- ni de reprendre là où elle s'est arrêtée.

KARUKIA v3.1 résout ce problème avec un **suivi de couverture persistant** entre les sessions.

### Fonctionnement

Chaque skill d'audit écrit un manifeste de couverture à la fin de chaque scan :

```
karukia/memory/coverage/{skill}-latest.json
```

Le manifeste enregistre :
- Les fichiers analysés (avec horodatage)
- Les fichiers restants (périmètre non couvert)
- Le pourcentage de couverture cumulé
- Le nombre de findings par sévérité (critique, haute, moyenne, basse)
- Le contexte de branche git

### Scan itératif

```
Scan 1 : Analyse ~40% des fichiers en périmètre  -> manifeste : 40% couvert
Scan 2 : Lit le manifeste, saute les fichiers déjà analysés -> manifeste : 80% couvert
Scan 3 : Traite les fichiers restants            -> manifeste : 100% COMPLET
```

Après un scan complet, KARUKIA détecte les fichiers modifiés depuis le dernier passage (via git) et ne ré-analyse que ceux-là -- zéro effort gaspillé.

### Résolution du périmètre

Le périmètre de couverture est résolu dans cet ordre :
1. **Configuration projet** (`karukia/config/coverage-scopes.json`) -- générée par `install`, contient des globs adaptés au projet
2. **Valeurs par défaut du skill** -- globs génériques (ex. : `**/*.ts` pour un audit TypeScript, `**/*.css` pour CSS)

### Ce que cela signifie pour les équipes

Une équipe de développement qui lance `karukia neo` chaque semaine atteint 100% de couverture du codebase en 2-3 sessions. Le manifeste de couverture fournit une **preuve auditable** de ce qui a été vérifié et quand -- exactement ce que les frameworks de conformité exigent.

```
--- COUVERTURE neo ---
Périmètre total : 120 fichiers
Ce scan         : 48 fichiers analysés
Cumulé          : 96 / 120 (80%)
Statut          : PARTIEL

Restant -- le prochain scan commence par :
  - src/auth/session.ts
  - src/api/handlers/patient.ts
  - ... (24 autres)
---
```

---

## 8. Workflow standardisé en 6 étapes (Nouveau en v3.1)

Tous les skills d'audit suivent le même workflow structuré. Ce n'est pas un détail d'implémentation interne -- le workflow est injecté dans le prompt et appliqué par GUARD.

```
Étape 0   : PRÉPARATION         — Créer la session, charger les références et patterns
Étape 0.5 : CHARGEMENT COUVERTURE — Lire le manifeste précédent, prioriser les fichiers non scannés
Étape 1   : EXPLORATION          — Scan multi-agents en parallèle
Étape 2   : ANALYSE              — Synthèse des découvertes, planification des actions
Étape 3   : EXÉCUTION            — Exécuter le plan, mettre à jour la progression après chaque action
Étape 4   : VALIDATION           — Lint, build, test. Corriger TOUS les problèmes avant clôture
Étape 4.5 : ÉCRITURE COUVERTURE  — Écrire le manifeste de couverture pour la prochaine session
Étape 5   : CLÔTURE              — Finaliser la session, mettre à jour trackers et base de connaissances
```

### Exploration multi-agents

À l'étape 1, les skills déploient des agents d'exploration en parallèle, chacun couvrant un périmètre spécifique :

```
Agent 1 : Explore src/api/       ─┐
Agent 2 : Explore src/auth/       ├── En parallèle
Agent 3 : Explore src/services/  ─┘
          Consolidation des résultats
```

Chaque agent retourne la liste des `files_analyzed` dans son rapport. Le contexte principal consolide toutes les listes pour le suivi de couverture.

### Règle des 2 actions

Après chaque 2 opérations de lecture/recherche, les findings DOIVENT être écrits dans `findings.md`. Cela garantit que même si une session est interrompue (limite de contexte, timeout, abandon utilisateur), le travail effectué est préservé.

### Protocole des 3 tentatives

```
Tentative 1 : Diagnostiquer et corriger (cause racine)
Tentative 2 : Approche alternative (JAMAIS répéter la même action)
Tentative 3 : Repenser globalement (remettre en question les hypothèses)
Après 3     : Escalader à l'utilisateur
```

### Persistance des connaissances

À la fin de chaque session, le skill sauvegarde les apprentissages :
- `karukia/memory/knowledge/lessons.md` -- Erreurs évitées, pièges découverts
- `karukia/memory/knowledge/patterns.md` -- Patterns de code réutilisables (utilisés 2+ fois)

La méthodologie apprend de votre codebase au fil du temps. Le deuxième audit est plus intelligent que le premier.

---

## 9. Deep Review -- Due Diligence Technique 12 Axes (Nouveau en v3.1)

L'outil `deep_review` est une revue technique de niveau Staff Engineer sur l'ensemble d'un codebase. Il a été conçu pour trois scénarios :

1. **Due diligence pré-investissement** -- Un investisseur veut savoir si le code vaut la mise
2. **Prise de poste CTO** -- Un nouveau directeur technique a besoin d'une vision complète en une session
3. **Planification d'un refactor majeur** -- L'équipe doit savoir où sont les vrais problèmes

### Les 12 axes

Deep Review déploie une brigade de 6 agents parallèles, chacun couvrant 2 axes :

| Agent | Axes | Ce qu'il évalue |
|-------|------|-----------------|
| Agent 1 | Qualité code + Patterns IA | Fonctions >100 lignes, copier-coller, casts `any`, sur-ingénierie, code mort généré par IA |
| Agent 2 | Architecture + Maintenabilité | Séparation des responsabilités, dépendances circulaires, patterns cohérents, magic numbers, bus factor |
| Agent 3 | Scalabilité + Coûts | Hot spots, cold starts, O(n) sur les tenants, modélisation coût-par-tenant |
| Agent 4 | Sécurité + Réglementaire | Chemins non authentifiés vers les données, secrets côté frontend, flux RGPD, politiques de rétention |
| Agent 5 | Résilience + Tests | Circuit breakers, dégradation gracieuse, qualité réelle des tests (pas juste le %) |
| Agent 6 | DX + Perf Frontend | Temps d'onboarding, scripts npm, taille du bundle, lazy loading, re-renders |

### Scorecard

Chaque axe est noté sur 10 avec une lettre (A+ à F). Le score global est sur 120.

```
| # | Axe               | Score /10 | Note | Red Flags | Points forts |
|---|-------------------|-----------|------|-----------|--------------|
| 1 | Qualité code      | 7         | B    | 3         | 5            |
| 2 | Architecture      | 8         | A-   | 1         | 4            |
| ...                                                                  |

Score global : 84/120
Note globale : C

Top 10 Red Flags + Top 10 Points forts + Plan d'action (P0/P1/P2)
```

Le verdict final répond à une question : **"Ce code ressemble-t-il au travail d'un expert humain, ou à du code généré par IA ?"**

---

## 10. Support Multi-IA (Nouveau en v3.1)

KARUKIA est le premier serveur MCP méthodologique avec une véritable abstraction multi-plateforme IA. Les 27 outils acceptent un paramètre optionnel `client_id` qui adapte l'intégralité du prompt.

### Plateformes supportées

| Plateforme | `client_id` | Statut |
|------------|-------------|--------|
| Claude Code | `"claude"` (défaut) | Conçu pour, entièrement testé |
| OpenAI Codex | `"codex"` | Conçu pour, entièrement testé |
| Tout client MCP | `"generic"` | Compatible, non testé individuellement |

### Ce qui s'adapte

| Aspect | Claude | Codex | Generic |
|--------|--------|-------|---------|
| Orchestration sous-agents | Task API avec hints Sonnet/Opus | Instructions en langage naturel | Langage naturel |
| Fichier config généré par `install` | `CLAUDE.md` | `CODEX-PROJECT.md` | `AI-CONFIG.md` |
| Références modèle dans les prompts | Opus (principal), Sonnet (exploration) | Noms de modèle génériques | Noms de modèle génériques |
| Chemin des settings | `.claude/settings.json` | N/A | N/A |

L'abstraction de plateforme n'est pas un simple wrapper -- elle change la façon dont les agents sont dispatchés, comment les modèles sont référencés, et comment les fichiers sont organisés. La méthodologie reste identique ; la livraison s'adapte au client.

---

## 11. Pour les secteurs réglementés

### Le vrai coût d'une certification

Obtenir une certification HDS, ISO 27001 ou SOC 2 est coûteux — non pas parce que l'auditeur humain est incompétent, mais parce que **la documentation est absente le jour J**. Les preuves sont dispersées dans des Jira, des e-mails, des commits sans structure. Les correctifs ont été appliqués en urgence, sans traçabilité.

**KARUKIA ne remplace pas l'auditeur humain. Il prépare le dossier.**

### Ce que KARUKIA produit pour votre certification

| Besoin de l'auditeur | Ce que KARUKIA génère |
|----------------------|-----------------------|
| Preuve de contrôle | Findings tracés avec référence fichier:ligne |
| Cartographie des risques | Rapport EBIOS RM (5 ateliers ANSSI) |
| Politique de sécurité technique | `security-scope.md` généré par `install` |
| Conformité par framework | Rapports Neo (HDS, ISO, SOC 2, PCI-DSS…) |
| Audit niveau expert | `audit_expert_hds` — 8 domaines, 200+ points |
| Rapport de changements | `change_report` — ISO 27001 A.8.32 |
| Pentest documenté | Rapport Viper avec scoring CVSS v4 + MITRE ATT&CK |

### Pourquoi KARUKIA est différent des autres outils

| Critère | Outils d'audit génériques | KARUKIA v3.1 |
|---------|--------------------------|---------|
| Couverture frameworks | Un framework à la fois | 6 frameworks + EBIOS RM en un seul outil |
| Dimensions couvertes | 1 (sécurité) | 12 (sécurité à due diligence) |
| Construction de preuves | Snapshots manuels | Mémoire structurée inter-sessions |
| Suivi de couverture | Aucun | Manifestes persistants, itératif jusqu'à 100% |
| Cycle complet | Audit seul | Code -> Sécurité -> Qualité -> Pentest |
| Due diligence technique | Manuelle | `deep_review` : 12 axes, 6 agents, scorecard A+ à F |
| Origine | Théorique | Construit en préparant une vraie certification HDS/ISO 27001 |
| Qualité web | Absente | 369 règles Certix (référentiel KARUKIA Solutions) |

### Construit sur le terrain

KARUKIA est né du développement d'un SaaS de santé en cours de préparation pour les certifications HDS 2.0 et ISO 27001. La méthodologie a été construite pour répondre à une vraie question : qu'est-ce que la certification exige concrètement, dès le premier jour de développement ? Les checklists reflètent ce qu'un auditeur réel demande, point par point — pas de la théorie.

---

## 12. Cas d'usage

### Startup SaaS — mise en conformité SOC 2

1. `karukia install` — détecte le stack (React + Node.js + PostgreSQL)
2. `karukia neo` avec frameworks SOC 2 + OWASP — audit complet
3. Les findings deviennent des chantiers de durcissement
4. `karukia jeffrey` implémente les corrections → Neo revalide
5. Rapport exportable pour l'auditeur SOC 2

### Application de santé — certification HDS

1. `karukia install` — détecte les données de santé, active HDS 2.0
2. `karukia audit_expert_hds` — audit expert 8 domaines
3. `karukia viper` — pentest offensif spécifique santé
4. `karukia ebios_rm_audit` — analyse de risques formelle ANSSI
5. Documentation complète pour le dossier de certification

### Due diligence technique — pré-investissement ou prise de poste CTO

1. `karukia deep_review` -- revue complète 12 axes en une session
2. Scorecard avec notes A+ à F par axe, score global sur 120
3. Top 10 red flags + Top 10 points forts + plan d'action priorisé (P0/P1/P2)
4. Verdict : "ce code ressemble-t-il au travail d'un expert ou à du code généré par IA ?"

### Équipe de développement — qualité continue

1. `karukia install` sur le projet partagé
2. Les développeurs utilisent `karukia: [tâche]` au quotidien
3. Chaque feature passe par Jeffrey → Neo → Opo automatiquement
4. `karukia_scan` lancé périodiquement pour un bilan global sur les 11 dimensions

---

## 13. Déploiement

### Local — Gratuit pour usage personnel (disponible maintenant)

```bash
npx karukia-mcp
```

Chaque développeur installe KARUKIA en local via `npx`. Zéro serveur, zéro infrastructure, zéro coût.

---

## 14. Comparaison

| Critère | IA générique | KARUKIA v3.1 |
|---------|-------------|---------|
| Méthodologie | Improvisée | 1800+ contrôles documentés |
| Dimensions couvertes | 1 (ce qu'on demande) | 12 (sécurité à due diligence) |
| Reproductibilité | Variable | Déterministe (mêmes checklists) |
| Traçabilité | Aucune | Findings avec fichier:ligne |
| Suivi de couverture | Aucun -- impossible de savoir ce qui a été scanné | Manifestes persistants, itératif jusqu'à 100% |
| Continuité inter-sessions | Repart de zéro à chaque fois | Couverture + connaissances conservées |
| Support multi-IA | Une seule plateforme | Claude, Codex, tout client MCP |
| Due diligence | Manuelle | Revue automatisée 12 axes avec scoring |
| Conformité | Impossible à prouver | Rapports par framework |
| Coût | — | Gratuit pour usage personnel (licence commerciale pour les équipes) |
| Données envoyées | Selon le provider | Aucune (100 % local) |
| Installation | — | `npx karukia-mcp` |

---

## 15. À propos & Licence

KARUKIA est développé par **[KARUK IA Solutions](https://karukia.com)**, studio SaaS B2B spécialisé dans les domaines réglementés (santé, finance, pharma), basé en Guadeloupe. 🇬🇵

KARUKIA est né du développement d'un SaaS de santé en cours de préparation pour les certifications HDS 2.0 et ISO 27001. La méthodologie a été construite pour répondre à une vraie question : qu'est-ce que la certification exige concrètement, dès le premier jour de développement ?

> *Made in Guadeloupe — L'IA ne remplace pas l'expert, elle le libère.*

KARUKIA MCP est **gratuit pour usage personnel et éducatif**. Aucun compte requis.

Pour toute utilisation commerciale, contactez **KARUKIA Solutions** — contact@karukia.com

**npm :** [karukia-mcp](https://www.npmjs.com/package/karukia-mcp)

**GitHub :** [github.com/getkarukia/KARUKIA](https://github.com/getkarukia/KARUKIA)
