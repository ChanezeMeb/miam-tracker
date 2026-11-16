# miam-tracker

Application de suivi alimentaire et de poids, pensée pour nous deux et pour nos proches.

> 🚧 Projet en cours de cadrage : l'architecture générale est choisie, plusieurs briques restent à trancher (voir « Encore à décider »).

## Objectif

<!-- À COMPLÉTER : la vision en une phrase, ex. « Suivre ce qu'on mange et son poids simplement, sans culpabiliser » -->

Inspiration : ce qu'on aime dans MyFitnessPal, Yazio, Foodvisor, Lifesum, Lose It!, Cronometer… (benchmark à faire).

## Utilisateurs et rôles

- **admin** : nous deux. Gestion des comptes (ou des invitations) et des fonctions d'administration.
- **user** : nos proches. Chacun crée son compte et ne voit que ses propres données.

## Fonctionnalités

### MVP (à valider)
- [ ] Inscription, connexion et rôles (`admin` / `user`)
- [ ] Saisie du poids + historique en graphique
- [ ] Saisie des repas avec recherche d'aliments via Open Food Facts
- [ ] Tableau de bord du jour (calories / macros)
- [ ] Espace admin minimal (gestion des comptes ou des invitations)
- [ ] Données personnelles : consentement à l'inscription, suppression du compte et des données, page de confidentialité

### Plus tard
- Scan de code-barres
- <!-- À COMPLÉTER : idées issues du benchmark -->

## Stack

**Architecture retenue (« comme au taff »)** : front React séparé + API Node.js séparée + PostgreSQL + ORM, le tout en TypeScript strict.

| Couche | Décision |
|--------|----------|
| Architecture | Front séparé + API Node.js séparée ✅ |
| Langage | TypeScript (mode `strict`) ✅ |
| Base de données | PostgreSQL ✅ |
| Données nutritionnelles | [Open Food Facts](https://world.openfoodfacts.org/data) ✅ |
| Validation des entrées | Zod (pressenti) |

### Encore à décider

| Sujet | Options |
|-------|---------|
| Type d'application | PWA (web installable) / appli native avec Expo |
| Front | React + Vite (si PWA) / Expo (si natif) |
| Framework API | Express / Fastify / NestJS (s'aligner si possible sur la stack du stage) |
| ORM | Prisma / Drizzle |
| Authentification | Better Auth / Auth.js / maison (JWT + refresh tokens) |
| Style | Tailwind (+ shadcn/ui) / NativeWind (si Expo) |
| Hébergement | Front : Vercel ou Netlify ; API + BDD : Render, Railway ou autre |
| Structure du repo | Monorepo `apps/web` + `apps/api` (+ `packages/shared` pour les types) recommandé |
| Inscription des proches | Libre / sur invitation par une admin (plus simple à sécuriser) |

Les raisons de chaque choix seront notées dans `docs/decisions.md`.

## Sécurité et données personnelles

- Contrôle d'accès **côté API**, sur chaque route : vérification du rôle et de l'appartenance des données.
- Mots de passe hachés (argon2 ou bcrypt), sessions ou tokens sécurisés, rate limiting sur la connexion, validation de toutes les entrées.
- Le poids et l'alimentation sont des données sensibles (RGPD) : consentement explicite, suppression du compte et des données à la demande, page de confidentialité, collecte limitée au nécessaire.
- Ce que les admins peuvent voir des données des autres utilisateurs : <!-- À COMPLÉTER -->

## Installation

<!-- À COMPLÉTER une fois les choix restants faits -->

```bash
git clone <!-- URL du repo -->
cd miam-tracker
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

- Chaneze
- <!-- Prénom de la collègue -->
