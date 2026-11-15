# <!-- À COMPLÉTER : nom du projet -->

Application perso de suivi alimentaire et de poids, faite à deux pour notre propre usage.

> 🚧 Projet en cours de cadrage — la stack n'est pas encore choisie.

## Objectif

<!-- À COMPLÉTER : la vision en une phrase, ex. « Suivre ce qu'on mange et notre poids simplement, sans culpabiliser » -->

## Fonctionnalités

### MVP
- [ ] Authentification (nos deux comptes)
- [ ] Saisie du poids + historique en graphique
- [ ] Saisie des repas (recherche d'aliments via Open Food Facts)
- [ ] Tableau de bord du jour (calories / macros)
- [ ] <!-- À COMPLÉTER -->

### Plus tard
- Scan de code-barres
- <!-- À COMPLÉTER : idées piochées dans le benchmark -->

## Stack (en cours de décision)

**Option A, « rapide »** : Next.js (front + back) + Supabase (BDD, auth, RLS)

**Option B, « comme au taff »** : front React (Vite) + API Node (Express / Fastify / NestJS) + PostgreSQL + ORM (Prisma / Drizzle)

| Couche | Options | Décision |
|--------|---------|----------|
| Type d'application | PWA / appli native (Expo) | À décider |
| Front | Next.js / React + Vite / Expo | À décider |
| Back | Intégré à Next.js / API Node séparée | À décider |
| Base de données | Supabase / PostgreSQL + Prisma / PostgreSQL + Drizzle | À décider |
| Authentification | Supabase Auth / Auth.js / Better Auth / maison | À décider |
| Style | Tailwind (+ shadcn/ui) / NativeWind | À décider |
| Hébergement | Vercel / Render / Railway / Supabase | À décider |
| Données nutritionnelles | [Open Food Facts](https://world.openfoodfacts.org/data) | Retenu ✅ |
| Langage | TypeScript | Retenu ✅ |

Les raisons de chaque choix seront notées dans `docs/decisions.md`.

## Installation

<!-- À COMPLÉTER une fois la stack choisie -->

```bash
git clone <!-- URL du repo -->
cd <!-- nom du dossier -->
# commandes d'installation et de lancement
```

### Variables d'environnement

<!-- À COMPLÉTER : lister les variables de .env.example -->

⚠️ Ne jamais commiter les fichiers `.env` contenant des secrets.

## Organisation

- **Suivi des tâches** : GitHub Projects (une issue = une fonctionnalité)
- **Branches** : `main` est protégée ; on travaille sur `feature/<nom>` ou `fix/<nom>`
- **Merge** : via Pull Request, relue par l'autre avant merge
- **Commits** : [Conventional Commits](https://www.conventionalcommits.org) (`feat:`, `fix:`, `chore:`, `docs:`…)
- **Décisions techniques** : notées dans `docs/decisions.md`

## Équipe

- <!-- Prénom -->
- <!-- Prénom -->
