# miam-tracker

Application de suivi alimentaire et de poids, pensée pour nous deux et pour nos proches.

> 🚧 Fin du cadrage : la stack et le schéma de base de données sont choisis, le projet n'est pas encore initialisé.

## Objectif

<!-- À COMPLÉTER : la vision en une phrase, ex. « Suivre ce qu'on mange et son poids simplement, sans culpabiliser » -->

Inspiration : ce qu'on aime dans MyFitnessPal, Yazio, Foodvisor, Lifesum, Lose It!, Cronometer… (benchmark à faire).

## Utilisateurs et rôles

- **admin** : nous deux. Gestion des comptes (ou des invitations) et des fonctions d'administration.
- **user** : nos proches. Chacun crée son compte et ne voit que ses propres données.

## Fonctionnalités

### MVP (à valider)
- [ ] Inscription, connexion et rôles (`admin` / `user`)
- [ ] Profil et objectif
- [ ] Saisie du poids et des mensurations + historique en graphique
- [ ] Saisie des repas avec recherche d'aliments via Open Food Facts
- [ ] Tableau de bord du jour (calories / macros)
- [ ] Espace admin minimal (gestion des comptes ou des invitations)
- [ ] Données personnelles : consentement à l'inscription, suppression du compte et des données, page de confidentialité

### V2
- Sport : catalogue d'activités, séances réalisées, séances types créées à la main
- Recettes : saisie manuelle et import depuis une URL
- Scan de code-barres

### V3
- Menus de la semaine, liste de courses, batch cooking
- Génération par IA (séances, ordre de préparation), réservée aux comptes qui ont la permission

### Plus tard
- Appli mobile avec Capacitor (réutilise le code de la PWA)
- Panier drive
- <!-- À COMPLÉTER : idées issues du benchmark -->

## Stack

**Architecture retenue (« comme au taff »)** : front React séparé + API Express séparée + PostgreSQL + Prisma, en JavaScript.

| Couche | Décision |
|--------|----------|
| Architecture | Front séparé + API Node.js séparée ✅ |
| Langage | JavaScript ✅ |
| Type d'application | PWA (web installable) pour commencer ; appli mobile plus tard avec Capacitor ✅ |
| Front | React + Vite ✅ |
| API | Express ✅ |
| Base de données | PostgreSQL ✅ |
| ORM | Prisma ✅ |
| Structure du repo | Monorepo : `apps/web` + `apps/api` + `packages/shared` ✅ |
| Données nutritionnelles | [Open Food Facts](https://world.openfoodfacts.org/data) (produits) + [Ciqual](https://ciqual.anses.fr) (aliments bruts) ✅ |
| Inscription des proches | Sur invitation d'une admin (pourra devenir libre plus tard) ✅ |
| Validation des entrées | Zod (pressenti) |

### Encore à décider

| Sujet | Options |
|-------|---------|
| Authentification | Better Auth / Passport.js / maison (JWT + refresh tokens, comme au stage) |
| Style | Tailwind (+ shadcn/ui) / MUI (comme au stage) |
| Hébergement | Front : Vercel ou Netlify ; API + BDD : Render, Railway ou autre |

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

- **Suivi des tâches** : Notion
- **Branches** :

  ```
  feature/xxx ──PR──► dev ──PR de release──► main
  (travail)          (staging : on teste)   (production)
  ```

  - `feature/<nom>` et `fix/<nom>` partent de `dev` et y reviennent par Pull Request
  - `dev` (branche par défaut) : intégration et staging. PR obligatoire, relue par l'autre, CI verte, pas de push direct
  - `main` : production. Ne reçoit que des PR depuis `dev` (releases), CI verte, pas de push direct
  - Correctif urgent en production : `fix/<nom>` depuis `main`, PR vers `main`, puis fusion de `main` dans `dev`
- **CI** : GitHub Actions (lint, tests, build) sur chaque PR, vers `dev` comme vers `main`
- **Commits** : [Conventional Commits](https://www.conventionalcommits.org) (`feat:`, `fix:`, `chore:`, `docs:`…)
## Équipe

- Chaneze
- Louise
