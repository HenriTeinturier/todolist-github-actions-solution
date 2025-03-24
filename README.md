# TodoList GitHub Actions Starter

Un kit de démarrage d'application Todo List React conçu pour apprendre les workflows CI/CD avec GitHub Actions.

## Fonctionnalités

- Ajouter de nouvelles tâches
- Marquer les tâches comme complétées
- Modifier les tâches existantes
- Supprimer les tâches

## Stack Technique

- React 19
- TypeScript
- Vite
- Vitest
- React Testing Library
- CSS3

## Tests

Exécuter les tests en mode watch :

```bash
npm test
# ou
yarn test
```

Exécuter les tests pour le CI (exécution unique) :

```bash
npm run test:ci
# ou
yarn run test:ci
```

Générer un rapport de couverture de tests :

```bash
npm run test:coverage
# ou
yarn test:coverage
```

## Structure du Projet

```
src/
  ├── components/
  │   ├── TodoList.tsx
  │   ├── TodoItem.tsx
  │   ├── TodoItemEdit.tsx
  │   └── __tests__/
  │       └── TodoList.test.tsx
  ├── App.tsx
  ├── App.css
  └── main.tsx
```

## Objectifs d'Apprentissage

Ce repository de démarrage est conçu pour vous aider à apprendre :

- L'implémentation de GitHub Actions
- La mise en place de pipelines CI/CD
- Les stratégies de protection des branches
- La configuration des environnements
- Les tests automatisés en CI

## Prochaines Étapes

Consultez le repository solution [todolist-github-actions-solution](https://github.com/HenriTeinturier/todolist-github-actions-solution) pour voir l'implémentation complète avec les workflows GitHub Actions.
