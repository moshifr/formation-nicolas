# Etapes

- Installation de docker (VM permettant d'installer et d'utiliser les outils serveur / développement) rapidement
- Installation Symfony (back)
- Installation VueJS (front)

### INSTALLATION DOCKER ###
Prérequis : 
- Installation de docker desktop https://www.docker.com/products/docker-desktop/
- Installation de docker-compose https://www.ionos.fr/digitalguide/serveur/configuration/docker-compose-sous-windows/
J'ai créé un fichier docker-compose.yml qui permet de configurer une instance docker (dans notre cas PHP / nginx / MySQL)
  
- Lancer 
- 
```
  docker-compose up -d
```

Pour entrer dans la VM en ligne de commande :
```shell
docker-compose exec php bash
```

### Installation SYMFONY ###
- https://symfony.com/doc/current/setup.html
- Dans le projet j'ai déjà installé symfony-cli
```
symfony new projet-nicolas --api --dir=src/ --no-git
```
- Dans notre cas on veut une api (--api) mais si on voulait un projet web complet on aurait pu mettre --webapp 
- Configuration du fichier .env 
- Remplacer :  
- DATABASE_URL="mysql://app:app@database:3306/ultima?serverVersion=8&charset=utf8"
  
- http://localhost
- Créer votre premier controller : 

```shell
cd src/
composer require symfony/maker-bundle --dev
bin/console make:controller PremierController
```

### Installation VueJS ###

```shell
composer require symfony/webpack-encore-bundle
npm install --dev vue vue-loader vue-template-compiler
```
- Rajouter cette ligne dans le fichier webpack.config.js après la ligne 70 :

```shell
.enableVueLoader()
; 
```
- Modifier le fichier src/assets/app.js

```js 
import './styles/app.css';

import Vue from 'vue';

import Exemple from '../js/components/Exemple'

/**
* Create a fresh Vue Application instance
*/
new Vue({
  el: '#app',
  components: {Exemple}
});
```
- Créer votre premier composant vue :
- assets/js/components/Exemple.vue
```shell
<template>
  <div>
    <p>This is an example of a new components in VueJs</p>
  </div>
</template>

<script>
export default {
  name: "exemple"
}
</script>

<style scoped>

</style>
```

- Compiler :

```shell
## Pour mettre en production : 
npm run build

## Pour travailler en local : 
npm run watch  
```

### Exercice ###

Créer un gestionnaire de client, sans login mot de passe dans un premier temps, via une API 
et un front JS

Avoir : 
- Listing des clients
- Edition de client
- Suppression de client
- Export CSV

Etapes : 

- Créer les entités
- Créer les routes pour l'API 
- Utiliser fetch() et await 
- Les afficher avec un v-for
- Supprimer les clients 
- Export CSV avec une url personnalisée 
- Rajouter des filtres éventuels 