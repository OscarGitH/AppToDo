# Application de ToDo

## Objectif du TP
L’objectif des TP de la semaine 4 en client riche est de se (re)familiariser avec les formulaires et la gestion des données avec Vue.js. Le but est de créer une application web de ToDo.

## Fonctionnalités
L'application permet de :
- Ajouter une tâche via un formulaire
- Supprimer une tâche via un bouton supprimer
- Afficher la liste des tâches
- Filtrer les tâches par état (à faire, en cours, terminé)
- Filtrer les tâches par priorité (haute, moyenne, basse)
- Filtrer les tâches par date de début
- Filtrer les tâches par date de fin

## Informations des tâches
Chaque tâche contient les informations suivantes :
- Description
- Date de début
- Date de fin
- État (à faire, en cours, terminé)
- Priorité (haute, moyenne, basse)

## Design
Le design de l'application utilise le framework CSS Bootstrap. Vous pouvez utiliser un autre framework CSS de votre choix si vous le souhaitez.

## Stockage des données
Les données sont stockées dans le local storage du navigateur de l’utilisateur. Pour plus d'informations, vous pouvez consulter la documentation de l'API Web Storage : [Web Storage API](https://developer.mozilla.org/fr/docs/Web/API/Web_Storage_API).

## Compatibilité
L'application est compatible avec les navigateurs les plus connus et est responsive pour un affichage sur ordinateur et plateforme mobile. Aucune page ne se recharge lors de l'ajout ou de la modification de tâches. 

## Installation et utilisation

### Prérequis
- Node.js
- npm (Node Package Manager) ou yarn

### Installation
1. Clonez le dépôt :
    ```bash
    git clone https://github.com/OscarGitH/AppToDo.git
    cd AppToDo/to_do_app
    ```

2. Installez les dépendances :
    ```bash
    npm install
    # ou
    yarn install
    ```

### Utilisation
Pour lancer l'application en mode développement :
```bash
npm run serve
# ou
yarn serve
```
Accédez à l'application via http://localhost:8081