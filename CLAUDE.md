# CLAUDE.md

Instructions pour l'assistant IA qui travaille sur ce repo. À lire avant toute modification.

## Contexte

**miam-tracker** : application de suivi alimentaire et de poids, développée à deux (Chaneze et Louise) en projet perso.
Multi-utilisateurs : les deux développeuses ont un compte **admin**, leurs **proches** ont un compte **user**.
Priorité : avancer vite avec un code simple, lisible et cohérent. Pas de sur-ingénierie.

**État actuel** : fin du cadrage. Stack choisie, schéma de base de données validé (à confirmer avec Louise). Projet pas encore initialisé. <!-- Mettre à jour au fil du projet -->

## Stack

### Retenu ✅

- **Architecture** : front React séparé + API séparée (pas de Next.js, pas de Supabase)
- **Langage** : JavaScript (modules ES : `import` / `export`), front et back
- **Type d'application** : PWA (web installable) pour commencer ; appli mobile plus tard avec Capacitor (même code React)
- **Front** : React + Vite
- **API** : Node.js + Express
- **Base de données** : PostgreSQL
- **ORM** : Prisma (le schéma de référence sera `schema.prisma`)
- **Structure** : un seul repo (monorepo) : `apps/web`, `apps/api`, `packages/shared` (code partagé, ex. schémas de validation)
- **Données nutritionnelles** : Open Food Facts (produits) + Ciqual de l'ANSES (aliments bruts)
- **Inscription des proches** : sur invitation d'une admin pour commencer
- **Validation des entrées** : Zod (pressenti)

### ⚠️ Encore à décider

Ces choix ne bloquent pas la mise en place du projet (monorepo, Docker, Express, Vite, Prisma). Mais tant qu'une ligne n'est pas tranchée :
- ne pas supposer de choix ;
- ne pas installer de dépendance liée à ce choix ;
- si une tâche en dépend, le signaler et demander.

Quand un choix est fait, le déplacer dans « Retenu ».

| Sujet | Options |
|-------|---------|
| Authentification | Better Auth / Passport.js / maison (JWT + refresh tokens, comme au stage) |
| Style | Tailwind (+ shadcn/ui) / MUI (comme au stage) |
| Hébergement | Front : Vercel ou Netlify ; API + BDD : Render, Railway ou autre |

## Commandes

<!-- À COMPLÉTER une fois le projet initialisé -->
```bash
# npm run dev
# npm run build
# npm run lint
# npm run format
```

## Structure du projet

<!-- À COMPLÉTER une fois le monorepo initialisé (apps/web, apps/api, packages/shared) -->

## Conventions de code

- JavaScript moderne, modules ES (`import` / `export`)
- Composants React fonctionnels
- Noms de fichiers en `kebab-case`, composants en `PascalCase`
- Code et noms de variables en anglais, textes de l'interface en français
- Commits au format Conventional Commits (`feat:`, `fix:`, `chore:`, `docs:`, `refactor:`)
- Branches : `feature/<nom>` ou `fix/<nom>` créées **depuis `dev`**, merge dans `dev` par PR relue par l'autre développeuse (CI verte obligatoire)
- `dev` = intégration / staging (branche par défaut) ; `main` = production, ne reçoit que des PR depuis `dev`. Ne jamais pousser directement sur `dev` ni sur `main`, ni créer une PR d'une feature vers `main`
- Correctif urgent en production : `fix/<nom>` depuis `main`, PR vers `main`, puis fusionner `main` dans `dev`
- Suivi des tâches sur Notion (pas dans le repo)
- <!-- À COMPLÉTER : conventions propres à Express et Prisma -->

## Règles importantes

1. **Petits changements** : une tâche = un périmètre limité. Ne pas refactorer du code hors du sujet demandé.
2. **Base de données** : toute modification de schéma passe par une nouvelle migration Prisma versionnée. Ne jamais modifier une migration existante. Demander confirmation avant de changer le schéma.
3. **Secrets** : aucune clé ou secret dans le code. Variables d'environnement uniquement, documentées dans `.env.example`.
4. **Dépendances** : demander avant d'ajouter une nouvelle librairie.
5. **Documentation** : ne pas créer de nouveaux fichiers `.md` ni modifier `README.md` / `CLAUDE.md` sans demande explicite.
6. **Vérification** : après une modification, s'assurer que le lint et le build passent.
7. **En cas de doute** sur le comportement attendu, poser la question plutôt que deviner.

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

- Calcul des besoins : métabolisme de base avec la formule de **Mifflin-St Jeor**, multiplié par le coefficient d'activité (1,2 / 1,375 / 1,55 / 1,725 / 1,9).
- Objectif calorique : privilégier une **perte lente et durable**. Règles vérifiées côté API :
  - IMC < 18,5 : objectif « perdre » refusé, avec un message bienveillant ;
  - IMC entre 18,5 et 25 : perte de 0,25 kg/semaine au maximum, avec un message neutre (le poids est déjà dans la zone de santé) ;
  - IMC > 25 : perte de 0,5 kg/semaine au maximum ;
  - dans tous les cas : déficit limité à 20 % de la dépense, et jamais sous **1200 kcal/jour pour une femme** ou **1500 kcal/jour pour un homme**.
- Ton de l'interface : encourageant et neutre, jamais culpabilisant (pas de rouge agressif ni de messages négatifs sur un dépassement).

## Modèle de données

Schéma validé. Une fois le projet initialisé, la source de vérité sera `apps/api/prisma/schema.prisma`.

Principes :
- Les tables ne sont créées qu'au moment où la feature qui en a besoin est développée.
- Toutes les données personnelles ont un `user_id` et sont supprimées avec le compte (`onDelete: Cascade`).
- Unités fixes en base : kg, cm, g, kcal, minutes.

Validé :

**Comptes (MVP)**
- **`users`** (connexion) : email unique, mot de passe haché, prénom, nom (facultatif), pseudo, rôle (`admin` / `user`), date d'acceptation du consentement RGPD, `disabled_at` (compte désactivé par une admin).
- **`user_profiles`** (une par user, données de santé, séparées pour ne jamais apparaître dans les listes admin) : date de naissance (pas l'âge), sexe, taille, niveau d'activité, préférence « ajouter les calories du sport au budget du jour », et **un seul objectif courant** (type perdre / maintenir / prendre, poids cible, rythme, calories et macros visées).
- **`invitations`** : inscription sur invitation d'une admin pour commencer (pourra devenir libre plus tard). Code d'invitation stocké haché, date d'expiration, date d'utilisation.

**Poids et mensurations (MVP)**
- **`weight_entries`** : une pesée par jour et par user (une nouvelle saisie remplace l'ancienne), poids en kg, taux de masse grasse facultatif, note facultative.
- **`body_measurements`** : une ligne par séance de mesures, toutes facultatives : tour de taille, hanches, poitrine, bras, cuisse (cm).

**Aliments et repas (MVP)**
- **`foods`** : sources Open Food Facts, Ciqual (ANSES, aliments bruts) et aliments créés par les users. Les aliments créés sont **visibles et utilisables par tous**. Valeurs nutritionnelles pour 100 g, code-barres facultatif, portion type.
- **`food_log_entries`** : 4 repas fixes (petit-déjeuner, déjeuner, dîner, collation), quantité en g, calories et macros **copiées au moment de la saisie**.

**IA (V3)**
- **`user_permissions`** : une permission par fonction IA (ex. génération de séances, batch cooking), accordée par une admin, avec quota mensuel.
- **`ai_requests`** : journal des appels IA (user, fonction, coût, date) pour le suivi du budget. Ne jamais y stocker le contenu des prompts.

**Contenus partagés (aliments, recettes, exercices, séances types)**
- Champ `status` commun pour la modération.
- Aliments, recettes et séances types : visibles par tous dès leur création, bouton « signaler », une admin peut corriger ou masquer.
- Exercices : visibles d'abord par leur seul auteur, partagés à tous après validation par une admin (risque de blessure).
- À la suppression d'un compte, ses contenus partagés sont conservés et anonymisés (auteur mis à `null`) ; le préciser dans la page de confidentialité.

**Sport (V2)**
- Catalogues importés au départ (MET : Compendium of Physical Activities ; exercices : wger ou free-exercise-db), puis enrichis par les users.
- `activities` + `activity_intensities` (variantes d'une activité, ex. course à 8 ou 12 km/h, chacune avec son MET) ; `exercises`.
- `workout_templates` (séances types, partageables) → `workout_blocks` (bloc simple ou circuit avec nombre de tours et repos entre les tours) → `workout_block_items` (un exercice ou une activité ; des répétitions ou une durée ; charge ; repos).
- `workout_sessions` + `workout_session_sets` : ce qui a été réellement fait, série par série ou tour par tour.
- Calories dépensées calculées avec le MET (`MET × poids × durée en heures`), puis figées dans la séance.

**Recettes (V2)**
- `recipes`, `recipe_ingredients` (liés à `foods` pour le calcul nutritionnel), `recipe_steps`, `utensils` + `recipe_utensils`.
- Recettes partagées visibles par tous les comptes. Anti-doublon à l'import depuis une URL.
- `recipe_favorites` et `tags` + `recipe_tags` en V2 ; notes (`recipe_ratings`) plus tard.
- Photos stockées par l'appli, compressées à l'envoi (ex. WebP 1200 px avec sharp), taille et type de fichier limités.

**Menus et courses (V3)**
- `meal_plans`, `meal_plan_items`, `shopping_lists`, `shopping_list_items`, `batch_cooking_steps`.
- **Foyers** : `households` + `household_members`. Un menu ou une liste de courses appartient à une personne **ou** à un foyer ; on y accède si c'est le sien ou si on est membre du foyer.
