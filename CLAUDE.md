# CLAUDE.md

Instructions pour l'assistant IA qui travaille sur ce repo. À lire avant toute modification.

## Contexte

**miam-tracker** : application de suivi alimentaire et de poids, développée à deux (Chaneze et une collègue) en projet perso.
Multi-utilisateurs : les deux développeuses ont un compte **admin**, leurs **proches** ont un compte **user**.
Priorité : avancer vite avec un code simple, lisible et cohérent. Pas de sur-ingénierie.

**État actuel** : phase de cadrage. Architecture choisie, plusieurs briques encore ouvertes. <!-- Mettre à jour au fil du projet -->

## Stack

### Retenu ✅

- **Architecture** : front React séparé + API Node.js séparée (pas de Next.js, pas de Supabase)
- **Langage** : TypeScript en mode `strict`, front et back
- **Base de données** : PostgreSQL, accédée via un ORM (voir ci-dessous)
- **Données nutritionnelles** : Open Food Facts
- **Validation des entrées** : Zod (pressenti)

### ⚠️ Encore à décider

Tant qu'une ligne ci-dessous n'est pas tranchée :
- ne pas supposer de choix et ne pas initialiser de projet ;
- ne pas installer de dépendances ;
- si une tâche dépend d'un choix non tranché, le signaler et demander.

Quand un choix est fait, le déplacer dans « Retenu » et noter la raison dans `docs/decisions.md`.

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

## Commandes

<!-- À COMPLÉTER une fois la stack choisie -->
```bash
# npm run dev
# npm run build
# npm run lint
# npm run format
```

## Structure du projet

<!-- À COMPLÉTER une fois la structure choisie (monorepo apps/web + apps/api + packages/shared envisagé) -->

## Conventions de code

- TypeScript strict, pas de `any` sauf justification en commentaire
- Composants React fonctionnels
- Noms de fichiers en `kebab-case`, composants en `PascalCase`
- Code et noms de variables en anglais, textes de l'interface en français
- Commits au format Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`, `refactor:`)
- Branches `feature/<nom>` ou `fix/<nom>`, merge par PR relue par l'autre développeuse ; `main` est protégée
- <!-- À COMPLÉTER : conventions propres au framework et à l'ORM choisis -->

## Règles importantes

1. **Petits changements** : une tâche = un périmètre limité. Ne pas refactorer du code hors du sujet demandé.
2. **Base de données** : toute modification de schéma passe par une nouvelle migration versionnée. Ne jamais modifier une migration existante. Demander confirmation avant de changer le schéma.
3. **Secrets** : aucune clé ou secret dans le code. Variables d'environnement uniquement, documentées dans `.env.example`.
4. **Dépendances** : demander avant d'ajouter une nouvelle librairie.
5. **Vérification** : après une modification, s'assurer que le lint et le build passent.
6. **En cas de doute** sur le comportement attendu, poser la question plutôt que deviner.

## Sécurité (côté API)

Toute la sécurité est appliquée **dans l'API** ; le front n'est jamais une barrière de confiance.

- **Rôles** : au minimum `admin` et `user`.
- **Sur chaque route** :
  - vérifier que l'utilisateur est authentifié ;
  - vérifier l'**appartenance** des données : un `user` ne lit et ne modifie que ses propres données (filtrer systématiquement par l'id de l'utilisateur connecté, jamais par un id fourni par le client) ;
  - vérifier le **rôle** pour les fonctions d'administration (réservées à `admin`).
- Ne jamais contourner ni « oublier » ces contrôles, même pour une route jugée anodine.
- **Authentification** : mots de passe hachés (argon2 ou bcrypt), sessions ou tokens sécurisés (cookies `httpOnly`, `secure`, `sameSite` si cookies), rate limiting sur la connexion.
- **Entrées** : valider toutes les entrées (body, params, query) avec un schéma (Zod pressenti).
- Ne jamais renvoyer de hash de mot de passe ni de données d'un autre utilisateur dans une réponse.

## Données personnelles (RGPD)

Le poids et l'alimentation de tierces personnes sont des données sensibles.

- Consentement explicite à l'inscription (avec lien vers la page de confidentialité).
- L'utilisateur peut supprimer son compte **et toutes ses données**.
- Une page de confidentialité courte explique ce qui est collecté et pourquoi.
- Ne collecter que le nécessaire ; ne pas ajouter de champ personnel sans le justifier.
- Ce que les admins peuvent voir des données des autres : <!-- À COMPLÉTER : à décider, par défaut aucun accès aux pesées et repas -->
- Ne jamais logger de données de santé ni de mots de passe.

## Règles métier

- Objectif calorique : ne jamais proposer ni accepter un objectif sous un seuil minimum raisonnable (<!-- À COMPLÉTER : seuil choisi -->) ; afficher un avertissement si la perte de poids visée est trop rapide.
- Ton de l'interface : encourageant et neutre, jamais culpabilisant (pas de rouge agressif ni de messages négatifs sur un dépassement).

## Modèle de données

<!-- À COMPLÉTER une fois le schéma défini. Entités prévues : users (avec rôle), profils et objectifs, pesées, repas, aliments (cache Open Food Facts) -->
