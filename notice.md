# Exercice : Mise en place d'un workflow CI/CD avec GitHub Actions

## Objectif

A partir d'une mini application, vous devez créer une chaîne d'intégration et de déploiement continu (CI/CD) en utilisant GitHub Actions avec des environnements de développement et de production.

## Développement application

- Créer un repository GitHub.
- Créer une mini application simple avec des tests et un linter (par exemple une todolist ou même une application encore plus simple) avec React/vite.
- Testez en local que les scripts de test et le linter passent.
- PS: les tests doivent se terminer à la fin de leur execution.

la branche main est notre branche principale.  
la branche develop est notre branche de développement.  
Pour développer votre application, vous devez créer des branches feature depuis develop et les merger dans develop.  
Puis quand develop est stable, vous pouvez le merger dans main.

## workflow CI/CD explication

Notre objectif est de mettre en place un workflow CI/CD complet pour notre application.

### Workflow de developpement

Lorsque nous effectuons une pull request de notre branche feature vers dévelop alors tout une série d'événements doit se produire:

- les tests sont lancés
- Puis un build (fictif) est lancé.
- Enfin un déploiement (fictif) sur l'environnement de développement est lancé

Si ces étapes sont un succès on veut que automatiquement:

- merger (valider) notre pull request.
- créer une pull request de develop vers main.

### Workflow de production

On doit reviewer puis valider la pull request de develop vers main manuellement ce qui doit ensuite entraîner:

- un déploiement (fictif) sur l'environnement de production

## workflow mise en place

### Création des environnements

Nous avons deux environnements à créer dans GitHub:

- `develop` pour le développement
- `production` pour la production

Ces environnements doivent contenir les variables et secrets nécessaires pour le deploiement.  
Ces variables sont des données fictives donc vous pouvez mettre ce que vous voulez dedans.  
Une variable `DEPLOY_URL`: <https://dev.todoapp.exemple.com> pour develop par exemple.  
Un secret `DEPLOY_TOKEN`: Une valeur factice pour l'exercice.

### Protection des branches

Nous devons protéger les branches main et develop.

- main et develop n'acceptent que les pr et pas les push directement.
- La validation de ces pr necessitera que les tests passent. (à activer plus tard quand le workflow sera en place)
- une pr sur main necessite au moins 1 reviewer.

## Workflow Github Actions

A partir de maintenant vous devez respecter les protections des branches et environnements.
Vous devez donc créer une branch feature depuis develop pour créer notre fichier de workflow.

Créer un fichier `.github/workflows/ci-cd.yml`.

### Event

Nous devons écouter les événements suivants:

- les pr sur develop et main.
- le merge manuel d'une pr sur main. (c'est un push)
- les pull request reviews de type submitted afin de lancer les tests quand une review est acceptée.

### Jobs

Implémenter les jobs suivants :

- `unit-tests` : exécute les tests unitaires
- `lint` : vérifie le code avec ESLint
- `build` : simule un build de l'application
- `deploy-dev` : simule un déploiement en développement (uniquement pour les PRs depuis les branches feature)
- `auto-merge-and-create-release-pr` : gère les PRs automatiquement (uniquement pour les PRs depuis les branches feature)
- `deploy-prod` : simule un déploiement en production (uniquement après merge sur main)

### Détails des jobs

#### unit-tests et lint

Ces deux jobs doivent s'effectuer en parallèle

##### `unit-tests`

- exécute les tests unitaires
- Doit utiliser une matrice pour executer des tests sur plusieurs version de node (18 et 22 par exemple).
- Doit u utiliser du cache pour les dépendances.
- Doit générer un résumé des tests dans le job summary (GITHUB_STEP_SUMMARY)

##### `lint`

- vérifie le code avec ESLint
- Doit également générer un résumé dans le job summary (GITHUB_STEP_SUMMARY)

#### build

Doit se lancer si les tests sont valides

- simule un build de l'application
- C'est un fake build donc vous pouvez juste faire des echo

#### deploy-dev

Simule un déploiement en développement (uniquement pour les PRs depuis les branches feature)

- Doit avoir des regles de concurrence.
- Doit se déclencher si la PR est depuis une branche feature et si le build est validé
- On doit récupérer les variables d'environnement DEPLOY_URL et DEPLOY_TOKEN depuis l'environnement develop et les utiliser de façon fictive
- on fait donc des echos dans le job car c'est un deploiement fictif

#### auto-merge-and-create-release-pr

De façon automatique merge les pull requests vers develop (si validé) et crée une PR de develop vers main

- Ce workflow se declenche si deploy-dev est un succès (donc les tests et le build sont également validé)
- Il doit valider automatiquement la pr de feature vers develop
- il doit également créer une pr de develop vers main

Pour ça on doit utiliser le github cli.
Cela permet d'interagir avec github depuis le runner github actions via des commandes CLI.
Github CLI est automatiquement installé dans les runners github actions.

- [GitHub CLI dans les workflows](https://docs.github.com/fr/actions/writing-workflows/choosing-what-your-workflow-does/using-github-cli-in-workflows)
- [Manuel de GitHub CLI](https://cli.github.com/manual/)
- [gh pr merge](https://cli.github.com/manual/gh_pr_merge)
- [gh pr create](https://cli.github.com/manual/gh_pr_create)

Pour le job `auto-merge-and-create-release-pr`, vous devrez :

1. Ajouter les permissions nécessaires :

   ```yaml
   permissions:
     # On veut que le workflow puisse merger les PR
     pull-requests: write
     # On veut que le workflow puisse créer des PR
     contents: write
   ```

2. Utiliser ces commandes :

   ```yaml
   # Merge la PR si elle est validée
   - run: gh pr merge --auto --merge "${{ github.event.pull_request.number }}"
   # Crée une PR de develop vers main
   - run: gh pr create --base main --head develop --title "Release to production" --body "Automated PR from develop to main"
   ```

3. exemple complet:

   ```yaml
   run: gh pr merge --auto --merge "${{ github.event.pull_request.number }}"
   # nécéssite d'utiliser le token github et le mettre dans la variable d'environnement GH_TOKEN
   env:
     GH_TOKEN: ${{ secrets.GITHUB_TOKEN }}
   ```

Pour pouvoir créer une PR dans le workflow, il faut que dans les settings du repository, dans les actions, il soit coché "allow GITHUB to create and approve pull request"
settings > actions > general > allow GITHUB to create and approve pull request

![Allow Github Action to create and approve pull requests](./assets/Allow_action_pr.png)

#### deploy-prod

Simule un déploiement en production (uniquement après merge sur main)

- Doit avoir des regles de concurrence
- Doit se déclencher si c'est un merge sur main (validation manuelle d'une pr c'est un push)
- C'est un fake donc juste des echo avec les variables et secrets de l'environnement production à afficher
- on peut ajouter des informations dans le job summary (GITHUB_STEP_SUMMARY)

## A savoir / Aide

on peut utiliser des contextes github pour récupérer des informations sur l'événement qui a déclenché le workflow.

le nom de l'événement: `github.event_name`  
github.event_name == push | pull_request

le nom de la branche: `github.ref`  
github.ref == refs/heads/main | refs/heads/develop

Le numéro de la PR: `github.event.pull_request.number`  
github.event.pull_request.number

## test de l'exercice

Création d'une feature:

- créer une branche feature/test et y faire un commit
- créer une PR depuis feature/test vers develop

Le workflow doit se déclencher automatiqueemnt suite à la création de la PR:

- les tests sont lancés
- s'ils réussisent le fake build est lancé
- s'il réussit le deploiement fictif sur develop est lancé
- enfin la pr est mergé automatiquement sur develop
- et une novuelle pr est crée de develop vers main

On review et merge cette PR et alors le déploiement en production doit se déclencher

## résultats attendus

Exemple de workflow sur develop:

![Exemple de workflow sur develop](./assets/workflow_develop.png)

Exemple de workflow sur main:

![Exemple de workflow sur main](./assets/workflow_main.png)

Exemple de summary:

![Exemple de summary](./assets/summary.png)
