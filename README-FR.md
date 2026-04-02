# KARUKIA MCP

```
██╗  ██╗ █████╗ ██████╗ ██╗   ██╗██╗  ██╗    ██╗ █████╗
██║ ██╔╝██╔══██╗██╔══██╗██║   ██║██║ ██╔╝    ██║██╔══██╗
█████╔╝ ███████║██████╔╝██║   ██║█████╔╝     ██║███████║
██╔═██╗ ██╔══██║██╔══██╗██║   ██║██╔═██╗     ██║██╔══██║
██║  ██╗██║  ██║██║  ██║╚██████╔╝██║  ██╗    ██║██║  ██║
╚═╝  ╚═╝╚═╝  ╚═╝╚═╝  ╚═╝ ╚═════╝ ╚═╝  ╚═╝    ╚═╝╚═╝  ╚═╝
   Méthodologie IA pour les domaines réglementés · Made in Guadeloupe 🇬🇵
```

**La méthodologie complète de développement assistée par IA, livrée via MCP.**

**Dernière version : v3.1.0** — 27 outils, 20 skills, 1800+ points de contrôle sur 11 dimensions d'audit. Support multi-IA.

Conçu et testé pour **Claude Code** et **OpenAI Codex**. Compatible avec tout client MCP.

*English version: [README.md](./README.md)*

---

> **Vous utilisez KARUKIA pour votre équipe ou votre entreprise ?** Une licence commerciale est requise.
> Voir [Licence commerciale](#licence-commerciale) ci-dessous ou contactez **contact@karukia.com**

---

## Qu'est-ce que KARUKIA ?

KARUKIA est une méthodologie de développement structurée, construite autour de personas IA spécialisés. Chaque persona (Neo pour la sécurité, Jeffrey pour l'architecture, Viper pour le pentest, Opo pour la qualité…) embarque son propre workflow, ses garde-fous et sa base de connaissances.

Quand tu appelles un outil KARUKIA, le serveur MCP retourne un prompt complet -- identité du persona, workflow standardisé, suivi de couverture, checklists, templates -- qui transforme ton assistant IA en ce spécialiste pour la session.

```
Toi : « Audite la sécurité de mon projet »
  -> l'IA appelle l'outil neo
  -> le serveur MCP retourne :
       GUARD (obligations non-négociables)
     + Persona Neo (identité, style, expertise)
     + WORKFLOW (processus standardisé en 6 étapes)
     + COVERAGE (charger le manifeste précédent, prioriser les fichiers non scannés)
     + Checklists (445 contrôles sécurité inline)
     + AGENTS (exploration multi-agents parallèle)
  -> l'IA devient Neo, suit la méthodologie, produit des findings structurés
  -> Manifeste de couverture écrit : 67% scanné -- la prochaine session reprend là où celle-ci s'est arrêtée
```

---

## Les 11 dimensions d'audit

```
SÉCURITÉ      → Neo (445 pts)       « Mon code est-il sécurisé ? »
QUALITÉ       → Certix (369 pts)    « Mon app est-elle bien construite ? »
OFFENSIF      → Viper (245+ tests)  « Comment un attaquant entrerait-il ? »
DUE DILIGENCE → deep_review (12 ax) « Ce codebase est-il prêt pour un investissement ? »
TS            → ts_quality (118)    « Mon TypeScript est-il propre ? »
CSS           → css_quality (55)    « Mon design system est-il maintenable ? »
ARCHI         → archi (70)          « Mon architecture est-elle saine ? »
TESTS         → test_coverage (68)  « Est-ce que je teste les bons trucs ? »
PERF          → perf (90)           « Où sont les goulots d'étranglement ? »
DETTE         → debt (55)           « Qu'est-ce qui nous ralentit ? »
HDS/ISO       → audit_expert (200+) « Suis-je prêt pour la certification ? »
SCAN          → karukia_scan        « Lancer les 11 dimensions en parallèle »
```

---

## Démarrage rapide

**Prérequis :** [Node.js](https://nodejs.org/) 22 ou supérieur.

### Étape 1 — Ajouter KARUKIA à ton projet

Crée ou édite `.mcp.json` à la racine de ton projet :

```json
{
  "mcpServers": {
    "karukia": {
      "command": "npx",
      "args": ["karukia-mcp"]
    }
  }
}
```

> **Note :** Si le fichier existe déjà avec d'autres serveurs MCP, ajoute simplement la clé `"karukia"` dans l'objet `"mcpServers"` existant.

### Étape 2 — Redémarre ton client IA

Redémarre Claude Code (`/quit` puis relance) ou ton IDE. Les 27 outils KARUKIA sont désormais disponibles.

> Au premier lancement, `npx` télécharge le package automatiquement (~175 Ko). Les lancements suivants utilisent la version en cache.

### Étape 3 — Configure ton projet

Dis à ton IA :

> « karukia install »

KARUKIA scanne ton projet, détecte ton stack et génère les fichiers de configuration (scope sécurité, CLAUDE.md, structure mémoire).

### Étape 4 — Commence à travailler

Décris simplement ce que tu veux en langage naturel, toujours via l'orchestrateur `auto` :

> « karukia auto: ajoute l'authentification utilisateur »
> « karukia auto: audite la sécurité »
> « karukia auto: lance un pentest »

`auto` est le point d'entrée recommandé pour tout — il analyse ta demande et route vers les bons spécialistes automatiquement (Jeffrey → Neo → Opo, ou Neo seul, ou Viper, etc.).

> **Conseil :** Il te suffit de deux commandes : `karukia install` (une fois) puis `karukia auto` + ta demande (toujours). `auto` s'occupe du reste. Pour un usage avancé, les skills peuvent aussi être appelés directement : « karukia neo », « karukia viper ». Tape « karukia start » à tout moment pour un guide complet.

### Où placer la config

| Client | Fichier | Portée |
|--------|---------|--------|
| **Claude Code CLI** | `.mcp.json` à la racine du projet | Ce projet uniquement |
| **Claude Code CLI** | `~/.claude.json` (dossier home) | Tous tes projets |
| **Claude Desktop** | `claude_desktop_config.json` | Global |
| **Cursor** | `.cursor/mcp.json` à la racine | Ce projet uniquement |
| **Windsurf** | Panneau MCP settings | Global |

---

## Installation globale (optionnel)

Si tu veux KARUKIA disponible dans tous tes projets sans ajouter `.mcp.json` à chaque fois :

```bash
npm install -g karukia-mcp
```

Puis ajoute à ta config IA globale (`~/.claude.json` pour Claude Code) :

```json
{
  "mcpServers": {
    "karukia": {
      "command": "karukia-mcp"
    }
  }
}
```

---

## 27 Outils

### Essentiels (commence ici)

| Outil | Description |
|-------|-------------|
| `install` | **[PREMIÈRE ÉTAPE]** Configure KARUKIA pour ton projet — à lancer une seule fois |
| `auto` | **[OUTIL PRINCIPAL]** Décris ce que tu veux — KARUKIA route vers les bons skills |
| `start` | Guide de démarrage — explique tous les skills à 3 niveaux progressifs |

### Skills core (Personas IA)

Chaque skill retourne un prompt complet qui transforme ton IA en spécialiste.

| Outil | Persona | Ce qu'il fait |
|-------|---------|---------------|
| `neo` | Auditeur Sécurité | Audit défensif contre 6 frameworks (OWASP, HDS, ISO 27001, SOC 2, PCI-DSS, HIPAA) |
| `viper` | Brigade Pentest | Tests offensifs avec 16 agents, scoring CVSS v4, mapping MITRE ATT&CK |
| `jeffrey` | Architecte Full-Stack | Implémentation de features avec TDD et validation sécurité |
| `opo` | Validateur Qualité | Qualité web contre les 369 règles Certix |
| `audit_certix` | Auditeur Qualité | Audit Certix complet avec 5 profils |
| `ebios_rm_audit` | Analyste Risques | Méthodologie EBIOS Risk Manager (ANSSI) — analyse formelle |
| `security_hardening` | Planificateur Durcissement | Chantiers d'amélioration sécurité |
| `doc_refactor` | Auditeur Doc | Audit de précision documentation vs code réel |
| `deep_review` | Expert Due Diligence | Évaluation technique 12 axes : code, archi, scalabilité, coûts, sécurité, résilience, tests, DX, perf frontend, réglementaire, IA, maintenabilité |

### Skills dimensionnels (nouveau en v3.0)

| Outil | Points | Ce qu'il fait |
|-------|--------|---------------|
| `ts_quality` | 118 | TypeScript — type safety, config stricte, patterns async |
| `css_quality` | 55 | CSS/Design System — maintenabilité, accessibilité, métriques |
| `archi` | 70 | Architecture — structure modules, couplage, layering |
| `test_coverage` | 68 | Tests — inventaire frontend/backend, qualité échantillons |
| `perf` | 90 | Performance — frontend, backend, build/bundle |
| `debt` | 55 | Dette technique — code mort, dépendances, code smells |
| `karukia_scan` | 1800+ | **Scan global** — les 11 dimensions en parallèle |
| `audit_expert_hds` | 200+ | Expert HDS 2.0/ISO 27001 — 8 domaines, préparation certification |
| `change_report` | — | Rapport de changements ISO 27001 A.8.32 |

### Utilitaires

| Outil | Description |
|-------|-------------|
| `list_checklists` | Parcourir les 22 checklists par catégorie |
| `suggest_checklists` | Décrire ton projet — obtenir un plan d'audit priorisé |
| `generate_report` | Compiler les résultats d'audit en rapport Markdown scoré |

### Mémoire & Config

| Outil | Description |
|-------|-------------|
| `init_memory` | Initialiser la structure mémoire KARUKIA dans un projet |
| `get_session_template` | Obtenir des templates de session pré-remplis pour chaque skill |
| `get_config_template` | Obtenir des templates de configuration (scope sécurité, CLAUDE.md) |

---

## 22 Checklists

### Sécurité défensive (Neo) — 6 checklists, 445 contrôles

| Checklist | Points | Périmètre |
|-----------|--------|-----------|
| **OWASP Security Baseline** | 62 | Toute application web |
| **HDS 2.0** | 52 | Données de santé, France |
| **ISO 27001:2022** | 93 | SMSI entreprise |
| **SOC 2 Type II** | 74 | SaaS, marché US |
| **PCI-DSS v4.0** | 97 | Traitement de paiements |
| **HIPAA** | 67 | Données de santé, US |

### Qualité web (Certix) — 5 profils, 369 règles

369 règles réparties en 5 profils : DEV (développement), UX (expérience utilisateur), CONT (contenu), OPS (opérations), JUR (juridique/conformité).

Certix est le référentiel qualité web propre à KARUKIA Solutions, conçu pour les applications web modernes.

### Sécurité offensive (Viper) — 4 checklists, 245+ tests

| Checklist | Tests | Périmètre |
|-----------|-------|-----------|
| **OWASP WSTG v5** | 100 | Pentest web |
| **Cloud Platform** | 80+ | Firebase, GCP, AWS, Azure |
| **Healthcare** | 50+ | PHI, chiffrement, données médicales |
| **Scénarios d'attaque** | 15+ | Templates PTES, MITRE ATT&CK |

### Qualité dimensionnelle (nouveau en v3.0) — 7 checklists, 656+ points

| Checklist | Points | Périmètre |
|-----------|--------|-----------|
| **TypeScript Quality** | 118 | Type safety, config stricte, patterns |
| **CSS / Design System** | 55 | Maintenabilité, accessibilité, métriques |
| **Architecture** | 70 | Structure modules, couplage, layering |
| **Test Coverage** | 68 | Inventaire frontend/backend, qualité |
| **Performance** | 90 | Frontend, backend, build/bundle |
| **Technical Debt** | 55 | Code mort, dépendances, code smells |
| **Expert HDS/ISO 27001** | 200+ | Préparation certification — 8 domaines |

---

## Support multi-IA

KARUKIA est conçu et testé pour **Claude Code** et **OpenAI Codex**. Il est compatible avec tout client MCP (Cursor, Windsurf, Copilot, etc.), mais ceux-ci n'ont pas été testés avec le paramètre `client_id`.

Tous les outils de skill acceptent un paramètre optionnel `client_id` : `"claude"` (défaut), `"codex"`, ou `"generic"`. L'intégralité du prompt s'adapte :

| Ce qui s'adapte | Claude (`client_id: "claude"`) | Codex (`client_id: "codex"`) | Generic |
|---|---|---|---|
| Orchestration sous-agents | Task API avec hints de modèle | Instructions en langage naturel | Langage naturel |
| Fichier config généré | `CLAUDE.md` | `CODEX-PROJECT.md` | `AI-CONFIG.md` |
| Références de modèle | Opus / Sonnet | Noms génériques | Noms génériques |
| Racine mémoire | `karukia/` | `karukia/` | `karukia/` |

Premier serveur MCP méthodologique avec une véritable abstraction multi-plateforme IA. Un seul package npm, une seule entrée `.mcp.json`, la méthodologie complète quel que soit le client IA.

---

## Suivi de couverture itératif (Nouveau en v3.1)

Chaque skill d'audit trace les fichiers analysés entre les sessions. Aucun fichier de votre codebase n'est oublié.

**Fonctionnement :**
1. **Scan 1** -- KARUKIA analyse votre codebase, couvre ~40% des fichiers en périmètre. Écrit un manifeste de couverture dans `karukia/memory/coverage/{skill}-latest.json`.
2. **Scan 2** -- Lit le manifeste précédent, saute les fichiers déjà analysés, couvre les ~40% suivants. Cumulé : 80%.
3. **Scan 3** -- Traite les 20% restants. Statut : **COMPLET**.

Après chaque scan, le manifeste enregistre exactement quels fichiers ont été analysés, lesquels ont été sautés, et quels findings ont été découverts -- avec le décompte par sévérité. Quand des fichiers sont modifiés après un scan complet, seuls les fichiers modifiés sont ré-analysés.

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

Les périmètres de couverture sont résolus depuis la config projet (`karukia/config/coverage-scopes.json`, généré par `install`) ou depuis les globs par défaut du skill. Un audit TypeScript scanne les fichiers `**/*.ts`, un audit CSS scanne `**/*.css` et `**/*.scss`, etc.

---

## Workflow standardisé en 6 étapes (Nouveau en v3.1)

Chaque skill d'audit suit le même workflow structuré :

```
Étape 0   : PRÉPARATION         -- Créer la session, charger les références
Étape 0.5 : CHARGEMENT COUVERTURE -- Lire le manifeste précédent, prioriser les fichiers non scannés
Étape 1   : EXPLORATION          -- Scan multi-agents en parallèle (chaque agent couvre un périmètre)
Étape 2   : ANALYSE              -- Synthétiser les découvertes, identifier les actions requises
Étape 3   : EXÉCUTION            -- Exécuter le plan, mettre à jour la progression après chaque action
Étape 4   : VALIDATION           -- Lint, build, test. Corriger TOUS les problèmes avant clôture
Étape 4.5 : ÉCRITURE COUVERTURE  -- Écrire le manifeste de couverture pour la prochaine session
Étape 5   : CLÔTURE              -- Finaliser la session, mettre à jour trackers et base de connaissances
```

Points clés du workflow :
- **Règle des 2 actions** : Après chaque 2 opérations de lecture, les findings DOIVENT être écrits dans `findings.md`. Le contexte n'est jamais perdu, même si la session est interrompue.
- **Protocole des 3 tentatives** : Diagnostiquer et corriger -- approche alternative -- repenser les hypothèses -- escalader à l'utilisateur. Pas de retries aveugles.
- **Persistance des connaissances** : Les leçons apprises et les patterns réutilisables sont sauvegardés dans `karukia/memory/knowledge/` entre les sessions. La méthodologie s'améliore au fil du temps.

---

## Exemples d'utilisation

### Audit sécurité complet

> « Audite la sécurité de mon projet »

Ton IA appelle `neo` — devient l'auditeur Neo — suit la méthodologie — produit des findings structurés avec sévérité, références fichier:ligne et étapes de remédiation.

### Construire une feature avec garde-fous

> « karukia jeffrey: implémente l'authentification utilisateur »

Ton IA appelle `jeffrey` — devient l'architecte Jeffrey — implémente avec TDD, puis enchaîne avec Neo pour la validation sécurité (boucle de rejet : si Neo rejette, Jeffrey corrige, max 3 itérations).

### Pentester son app

> « karukia viper »

Ton IA appelle `viper` — déploie la méthodologie Brigade avec 16 agents spécialisés sur les phases Reco, Analyse de surface et Exploitation.

### Due diligence technique (Nouveau en v3.1)

> « karukia deep_review »

Ton IA appelle `deep_review` -- déploie une brigade de 6 agents parallèles -- chacun couvre 2 des 12 axes (qualité code, architecture, scalabilité, coûts, sécurité, résilience, tests, DX, perf frontend, conformité réglementaire, patterns IA, maintenabilité). Produit un scorecard avec notes A+ à F par axe, un score global sur 120, et un plan d'action priorisé. À utiliser avant un investissement, une prise de poste CTO, ou un refactor majeur.

### Tout orchestrer

> « karukia auto: ajoute un bouton logout et audite la sécurité »

Ton IA appelle `auto` — analyse la demande — route vers les bons skills — gère la chaîne.

---

## Pour les secteurs réglementés

KARUKIA est conçu pour les équipes qui développent dans des environnements soumis à certification : HDS 2.0, ISO 27001, SOC 2, PCI-DSS, HIPAA, RGPD.

**KARUKIA ne remplace pas l'auditeur humain. Il prépare le dossier** : findings tracés, mappés par framework, avec référence fichier:ligne. Quand l'auditeur arrive, les preuves existent déjà.

Voir le [Livre Blanc](./LIVRE-BLANC.md) pour une analyse détaillée.

---

## Documentation

- [Livre Blanc (Français)](./LIVRE-BLANC.md) — Document technique détaillé : architecture, méthodologie, cas d'usage
- [Whitepaper (English)](./WHITEPAPER.md) — Technical deep-dive: architecture, methodology, use cases

---

## À propos

KARUKIA est développé par **[KARUK IA Solutions](https://karukia.com)**, studio SaaS B2B spécialisé dans les domaines réglementés (santé, finance, pharma), basé en Guadeloupe. 🇬🇵

KARUKIA est né du développement d'un SaaS de santé en cours de préparation pour les certifications HDS 2.0 et ISO 27001. La méthodologie a été construite pour répondre à une vraie question : qu'est-ce que la certification exige concrètement, dès le premier jour de développement ? Les checklists reflètent ce qu'un auditeur réel demande, point par point — pas de la théorie.

Le projet est construit autour de trois principes :
1. **Séparation des responsabilités** — Sécurité, qualité et implémentation sont des disciplines distinctes, portées par des personas IA séparés.
2. **Points de contrôle formels plutôt qu'intuition** — 1800+ points documentés valent mieux qu'un "je pense que c'est bon".
3. **Défense en profondeur** — Audit défensif d'abord, validation qualité ensuite, test offensif en dernier.

> *Made in Guadeloupe — L'IA ne remplace pas l'expert, elle le libère.*

---

## Licence commerciale

KARUKIA MCP est distribué sous [Business Source License 1.1](./LICENSE) (BUSL-1.1).

### Usage gratuit (pas de licence nécessaire)

- Projets personnels et développeurs individuels
- Établissements d'enseignement, étudiants, recherche
- Organisations à but non lucratif

### Licence commerciale requise

Si votre entreprise ou ESN utilise KARUKIA en production ou le déploie pour ses équipes de développeurs, une licence commerciale est requise.

| Formule | Prix HT/an | Équipe |
|---------|------------|--------|
| **Starter** | 5 000 EUR | Jusqu'à 10 développeurs |
| **Business** | 12 000 EUR | Jusqu'à 50 développeurs |
| **Enterprise** | 20 000 EUR | Développeurs illimités + support prioritaire |

Toutes les formules incluent : accès complet aux 27 outils, 20 skills, 1800+ points de contrôle sur 11 dimensions d'audit, et toutes les mises à jour pendant la durée de la licence. Licence annuelle, renouvelable.

**Contact :** contact@karukia.com

> Un seul audit de sécurité externe coûte 10-15k EUR. KARUKIA donne à toute votre équipe la méthodologie pour auditer en continu — pour moins que le prix d'un seul audit.

### Conversion de licence

Le 2 avril 2036, le Logiciel sera automatiquement converti sous [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0).
