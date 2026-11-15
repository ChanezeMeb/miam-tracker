# CLAUDE.md

Instructions pour l'assistant IA qui travaille sur ce repo. À lire avant toute modification.

## Contexte

Application de suivi alimentaire et de poids, développée à deux pour un usage personnel (2 utilisatrices, pas de public).
Priorité : avancer vite avec un code simple, lisible et cohérent. Pas de sur-ingénierie.

**État actuel** : phase de cadrage. **La stack n'est pas encore choisie.** <!-- Mettre à jour au fil du projet -->

## ⚠️ Stack — en cours de décision

Tant qu'une ligne ci-dessous est marquée « À décider » :
- ne pas supposer de choix et ne pas initialiser de projet ;
- ne pas installer de dépendances ;
- si une tâche dépend d'un choix non tranché, le signaler et demander.

Quand un choix est fait, remplacer « À décider » par l'option retenue et supprimer les autres.

### Combinaisons envisagées

- **Option A, « rapide »** : Next.js (front + back) + Supabase (BDD, auth, RLS)
- **Option B, « comme au taff »** : front React (Vite) + API Node séparée + PostgreSQL + ORM

### Choix couche par couche

| Couche | Options | Décision |
|--------|---------|----------|
| Type d'application | PWA (web installable) / appli native avec Expo (React Native) | À décider |
| Front | Next.js (App Router) / React + Vite / Expo (si natif) | À décider |
| Back | Intégré à Next.js (Server Actions, route handlers) / API Node : Express, Fastify ou NestJS | À décider |
| Base de données | Supabase (PostgreSQL managé) / PostgreSQL + Prisma / PostgreSQL + Drizzle | À décider |
| Authentification | Supabase Auth / Auth.js / Better Auth / auth maison (JWT) | À décider |
| Style | Tailwind CSS (+ shadcn/ui) / NativeWind (si Expo) | À décider |
| Hébergement | Vercel (front ou Next.js) / Render ou Railway (API Node) / Supabase (BDD) | À décider |
| Données nutritionnelles | Open Food Facts | Retenu ✅ |
| Langage | TypeScript en mode `strict` | Retenu ✅ |

## Commandes

<!-- À COMPLÉTER une fois la stack choisie -->
```bash
# npm run dev
# npm run build
# npm run lint
# npm run format
```

## Structure du projet

<!-- À COMPLÉTER une fois la stack choisie (un seul projet pour l'option A, monorepo apps/web + apps/api pour l'option B) -->

## Conventions de code

- TypeScript strict, pas de `any` sauf justification en commentaire
- Composants React fonctionnels
- Noms de fichiers en `kebab-case`, composants en `PascalCase`
- Code et noms de variables en anglais, textes de l'interface en français
- Commits au format Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`, `refactor:`)
- <!-- À COMPLÉTER : conventions propres à la stack choisie -->

## Règles importantes

1. **Petits changements** : une tâche = un périmètre limité. Ne pas refactorer du code hors du sujet demandé.
2. **Base de données** : toute modification de schéma passe par une nouvelle migration versionnée. Ne jamais modifier une migration existante. Demander confirmation avant de changer le schéma.
3. **Sécurité** : chaque utilisatrice n'accède qu'à ses propres données. Selon la stack, via des politiques RLS (Supabase) ou des vérifications systématiques dans l'API. Ne jamais contourner ce contrôle.
4. **Secrets** : aucune clé ou secret dans le code. Variables d'environnement uniquement, documentées dans `.env.example`.
5. **Dépendances** : demander avant d'ajouter une nouvelle librairie.
6. **Vérification** : après une modification, s'assurer que le lint et le build passent.
7. **En cas de doute** sur le comportement attendu, poser la question plutôt que deviner.

## Règles métier

- Objectif calorique : ne jamais proposer ni accepter un objectif sous un seuil minimum raisonnable (<!-- À COMPLÉTER : seuil choisi -->) ; afficher un avertissement si la perte de poids visée est trop rapide.
- Ton de l'interface : encourageant et neutre, jamais culpabilisant (pas de rouge agressif ni de messages négatifs sur un dépassement).

## Modèle de données

<!-- À COMPLÉTER une fois le schéma défini (tables, relations principales) -->