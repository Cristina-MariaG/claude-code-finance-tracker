# Claude Code — Aide-mémoire

Notes personnelles sur les commandes et concepts appris avec Claude Code.

## Tokens — la base à comprendre

Tout ce que Claude "lit" ou "écrit" est converti en **tokens** (fragments de mots). Chaque token coûte de l'argent et occupe de la place dans la **fenêtre de contexte** (200k tokens max pour Opus 4.7). Plus le contexte est rempli, plus les réponses coûtent cher et ralentissent. L'objectif : garder le contexte propre et pertinent.

## Commandes essentielles

### `/context`
Affiche l'utilisation actuelle de la fenêtre de contexte, répartie par catégorie :
- **System prompt** — les instructions internes de Claude Code
- **System tools** — les outils disponibles (Read, Edit, Bash…)
- **Memory files** — fichiers CLAUDE.md chargés
- **Skills**, **Custom agents**, **MCP tools**
- **Messages** — la conversation en cours
- **Free space** — ce qu'il reste
- **Autocompact buffer** — réserve pour la compaction automatique

Utile pour voir ce qui "mange" le contexte.

### `/clear`
Efface complètement la conversation et redémarre à zéro. À utiliser quand on change de sujet ou que le contexte est pollué par d'anciennes tâches. **Simple et radical.**

### `/compact`
Comprime la conversation en un résumé pour libérer du contexte **sans tout perdre**. À préférer à `/clear` quand on veut garder la mémoire des décisions récentes mais libérer de l'espace.

### `/cost` et `/usage`
- `/cost` — affiche le coût de la session courante (tokens consommés × prix du modèle).
- `/usage` — montre la consommation sur l'abonnement (limites, quotas restants).

Pratique pour surveiller la dépense en temps réel.

### `/stats`
Statistiques globales de la session (nombre de messages, tokens envoyés/reçus, durée…).

### `/model`
Change le modèle actif : Opus (puissant, cher), Sonnet (équilibré), Haiku (rapide, léger). Choisir selon la tâche : Haiku pour du texte simple, Opus pour du raisonnement complexe.

### `/init`
Génère automatiquement un fichier `CLAUDE.md` à la racine du projet en analysant le code. Point de départ idéal pour toute nouvelle codebase.

## CLAUDE.md — garder court = économiser

Le fichier `CLAUDE.md` est **chargé automatiquement à chaque conversation**. Tout ce qu'il contient est facturé en tokens à chaque message.

**Règle d'or :** garder CLAUDE.md **court et dense**. Y mettre uniquement ce que Claude ne peut pas deviner en lisant le code :
- commandes du projet (`npm run dev`, etc.)
- conventions non évidentes
- invariants importants (ex : "les montants sont des nombres, pas des strings")

**Ne PAS y mettre :** de la doc générale, l'historique des changements, des explications de code que Claude peut lire lui-même.

Un CLAUDE.md de 500 mots bien écrits >>> un CLAUDE.md de 5000 mots verbeux.

## Mémoire persistante

Différent de CLAUDE.md : Claude dispose d'un système de **mémoire sur disque** (dans `~/.claude/projects/…/memory/`) pour retenir des infos d'une conversation à l'autre.

Quatre types :
- **user** — qui je suis, mon rôle, mes préférences
- **feedback** — corrections que j'ai données ("ne fais pas X", "continue à faire Y")
- **project** — contexte du projet non visible dans le code (deadlines, décisions…)
- **reference** — pointeurs externes (dashboards, tickets Linear…)

Dire explicitement *"souviens-toi que…"* pour forcer une sauvegarde. Dire *"oublie que…"* pour effacer.

## MCP — Model Context Protocol

Protocole qui permet à Claude de se **connecter à des services externes** : Gmail, Google Drive, Calendar, bases de données, APIs internes… Chaque serveur MCP ajoute des outils utilisables pendant la conversation.

Gérer avec `/mcp`. **Attention :** chaque outil MCP actif consomme des tokens dans le contexte, même inutilisé. N'activer que ce dont on a besoin.

## Habitudes pour réduire les coûts

1. `/clear` ou `/compact` entre deux tâches différentes.
2. Garder `CLAUDE.md` court.
3. Vérifier `/context` quand ça devient lent.
4. Descendre en `/model haiku` pour les tâches simples.
5. Surveiller avec `/cost` régulièrement.
6. Désactiver les serveurs MCP inutilisés.