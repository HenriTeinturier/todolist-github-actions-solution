# TodoList GitHub Actions Solution

Ce repository est la solution complète de l'exercice de mise en place d'un workflow CI/CD avec GitHub Actions, basé sur le starter kit [todolist-github-action-starter](https://github.com/HenriTeinturier/todolist-github-action-starter).

## 🎯 Description

Cette solution implémente un workflow CI/CD complet pour une application Todo List React, démontrant les bonnes pratiques de :

- Intégration continue (CI)
- Déploiement continu (CD)
- Protection des branches
- Gestion des environnements
- Tests automatisés

## 🔄 Workflow Implémenté

### Flow de Développement

- Tests automatiques sur les PRs
- Build automatique
- Déploiement automatique vers l'environnement de développement
- Merge automatique vers develop
- Création automatique d'une PR vers main

### Flow de Production

- Déploiement automatique vers l'environnement de production après merge sur main
- Protection renforcée de la branche main

## ⚙️ Configuration

### Environnements GitHub

| Environnement | Variables                                     | Secrets        |
| ------------- | --------------------------------------------- | -------------- |
| `develop`     | `DEPLOY_URL: https://dev.todoapp.exemple.com` | `DEPLOY_TOKEN` |
| `production`  | `DEPLOY_URL: https://todoapp.exemple.com`     | `DEPLOY_TOKEN` |

### Protection des Branches

- `main` :
  - Pull requests obligatoires
  - Tests obligatoires
  - 1 reviewer minimum
- `develop` :
  - Pull requests obligatoires
  - Tests obligatoires

## 📋 Jobs Implémentés

- `unit-tests` : Tests unitaires (multi-version Node)
- `lint` : Vérification ESLint
- `build` : Build de l'application
- `deploy-dev` : Déploiement en développement
- `auto-merge-and-create-release-pr` : Gestion automatique des PRs
- `deploy-prod` : Déploiement en production

## 🖼️ Exemples de Workflow

### Workflow Develop

![Exemple de workflow sur develop](/assets/workflow_development.png)

### Workflow Main

![Exemple de workflow sur main](/assets/exemple_workflow_production.png)

## 🚀 Installation et Utilisation

```bash
# Cloner le repository
git clone https://github.com/votre-username/todolist-github-actions-solution.git

# Installer les dépendances
npm install

# Lancer l'application
npm run dev
```

## 📚 Documentation

Pour comprendre la mise en place de ce workflow, consultez :

- [Notice détaillée](notice.md)
- [Documentation GitHub Actions](https://docs.github.com/fr/actions)
- [Guide des Environnements GitHub](https://docs.github.com/fr/actions/deployment/targeting-different-environments/using-environments-for-deployment)

## 🔗 Liens Utiles

- [Repository de départ](https://github.com/HenriTeinturier/todolist-github-action-starter)
- [GitHub CLI dans les workflows](https://docs.github.com/fr/actions/writing-workflows/choosing-what-your-workflow-does/using-github-cli-in-workflows)

## 📄 Licence

MIT
