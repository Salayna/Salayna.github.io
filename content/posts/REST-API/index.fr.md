---
title: "Introduction aux API REST avec NodeJS"
date: 2021-03-03
description: Introduction aux API REST(avec un tutoriel avec NodeJS)
menu:
  sidebar:
    name: Introduction aux API REST
    identifier: api-rest-introduction
    weight: 10
---
# Qu'est ce qu'une API
Une API (Application Programming Interface) est un set de méthodes, de variables, et de protocoles permettent d'interfacer deux applications. On distingue plusieurs types d'API:  
- d'un côté on peut avoir une API qui servir à utilisé les fonctions d'une autre application, pour par exemple intégrer certains modules à d'autres application  
- de l'autre côté on peut avoir une API qui va servir des données à notre application comme les APIs en GraphQL ou Rest(ce que l'on va voir aujourd'hui)
## Le modèle REST
Le modèle **REST**(REpresentational State Transfer) est un ensemble de règles et de contraintes à respecter pour la création de services web. Ces services permettent manipulation de resources web et se repose lourdement sur le protocoles HTTP.
### Le protocole HTTP en bref
Le protocole HTTP est l'un des protocoles principale du World Wide Web(les fameux www). Le HTTP moderne se repose lui même sur le MIME(Multipurpose Internet Mail Extensions) qui définis les fichiers transférable à travers internet(comme son nom l'indique le MIME a été d'abord pensé pour l'envoie de différents types de fichiers par mail)
#### Les méthodes
Le protocole compte des méthodes qui vont définir les actions possible sur les ressources, parmis lesquels on peut compter:
- GET est la méthode qui permet de demander une ressource au serveur
- POST permet d'envoyer des données au serveur en vue d'une création de ressource
- PUT permet aussi d'envoyé des données au serveur en vue d'un ajout ou d'une modification totale de la ressource
- DELETE permet de supprimer la ressource visé
- PATCH permet d'effectuer une modification partiel de la ressource    

Les APIs REST vont se reposer sur les méthodes http pour gérer leurs requêtes.

# Implémentons
*ps: On utilisera du NodeJS pour ce tutoriel, le but de comprendre comment ça marche, vous pourrez trouver un repos github contenant le code du tutoriel à la fin de celui ci*

## Initialisation du projet
Pour commencer nous allons initialiser notre projet comme ceci:
- on va d'abord créer notre dossier (le nom importe peu)
- on s'assure que Node est installé avec la commande ``node -v`` si ce n'est pas le cas je vous renvoie vers le site officiel https://nodejs.org/en/download/
- Une fois Node installé on va effectuer la commande ``npm init``,on devrait avoir un prompt où remplir des informations trivial à la suite de quoi on aura à la racine de notre projet un fichier `package.json`.
- on va ensuite créer un fichier index.js aussi la racine du projet.

Avant d'aller plus loin on va d'abord expliquer quelques points vu au dessus:
### Qu'est-ce que NodeJS ? 
NodeJS est une plateforme logiciel qui permet de booster le Javascript qui, avant tout ça était surtout un langages utilisé pour faire de belles animations et rendre un peu plus dynamique les sites web. Le NodeJS permet au Javascript d'être utilisé côté serveur et de faire un bon nombre de choses assez incroyable (programmation asynchrone etc...)
### À quoi sert le fichier package.json ?
le fichier package.json sert de fichier de contrôle pour votre projet. Le nom, l'auteur, des script utile, les dépendances et pleins d'autres informations sont stocké à l'intérieur de ce fichier.  

### Ajout de dépendances
 On va avoir besoin de quelques dépendances pour ce projet.
 - Express: pour la création du serveur web et la gestion de requêtes
 - Nodemon: pour ne pas avoir à toujours redémarrer notre app pendant que l'on développe

 Pour installer ces dépendances on va utiliser la commande `npm install express`  
 Vous noterez que je n'ai pas mis nodemon dans la commande juste, c'est parce que je veux avoir nodemon que pendant le développement et pas en production, donc je vais l'installer en **dev dependency** comme ceci:  
 `npm install --save-dev nodemon`  
 Votre ficher `package.json` devrait maintenant avoir ces deux objets:  
 ```json
 "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "nodemon": "^2.0.7"
  }
 ```
 et votre dossier compte maintenant un fichier `package-lock.json` qui répertorie les dépendances de vos dépendances, qui sont contenue dans le dossier nodes_modules(Je vous déconseille fortement de toucher à ces deux entités, mais vous pouvez regarder si vous êtes de nature curieuse).
  
 maintenant que notre projet est en place, on peut commencer à coder.

 ## Création de notre serveur web et envoie de texte
  Dans le ficher index.js on va ajouter le code suivant:
  ```js
const express = require("express");
const app = express();

function sendStartingMessage(port) {
    console.log('app running on port '+port)
}
app.listen(3000, startingMessage(3000));
  ```
Maintenant en effectuant la commande `node .` à la racine de notre projet on devrait avoir le message "app running on port 3000" dans le terminal. Ok, mais comment ça se fait ? Reprenons le code ligne par ligne.  
Dans les deux première lignes on va importer notre module avec la fonction `require("{le nom du module à importer}")`, dont on va créer un objet dans la variable `app`. On va ensuite créer un port d'écoute avec la fonction `listen()` qui prends en paramètre le port d'écoute. Vous noterez que `listen()` a aussi un paramètre assez particulier qui est la fonction `sendStartingMessage()`, c'est ce qu'on appelle un callback et ce callback ne va être effectuer que si le serveur est bien initialisé. Effectuons un tout petit changement à ce dernier.  
```js
const express = require("express");
const app = express();

app.listen(3000, ()=>{
    console.log('app running on port 3000');
});
```
Votre server fonctionne maintenant mais vous ne pouvez rien faire, on va donc créer notre premier endpoint juste au dessus de notre `app.listen()`:
```js
app.get('/', (req, res)=> {
    res.send("Welcome to this demo API")
});
```
Nous venons tout juste de rajouter un GET avec la fonction `app.get()` qui prends en paramètre la route de la ressource dans ce cas ci la racine '/' et une callback dont les arguments seront **req**: la requête, **res**: la réponse on va ensuite dire, dans ce callback, à notre serveur d'envoyé comme réponse "Welcome to this demo API";
Avant de tester cette requête on va juste rajouter un petit script qui va nous simplifier la vie dans le `package.json`. dans l'objet scripts ajoutez: 
``"dev:start": "nodemon ."``  
votre object scripts devrait maintenant ressembler à ça: 
```json
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "dev:start": "nodemon ."
  },
```
vous pouvez ensuite interrompre l'exécution de votre code et taper la commande ``npm run dev:start`` dans le terminal. maintenant à chaque fois que vous sauvegarderez vos modifications le code sera exécuté automatiquement.
Pour tester si notre GET fonctionne, rien de plus simple, on va juste aller sur notre navigateur et allez à l'adresse suivante: http://localhost:3000/
vous devriez avoir ce résultat.  
![response](images/api-message.png).

Avant d'aller plus loin créer des données factices pour notre API. Créons donc un tableau cars qui sera un tableau de voiture
```js
let cars = [];

let car1 = {
    id: 1,
    name: "Tesla Model S",
    proprio: "Mr Ren",
    date: 2017,
}

let car2 = {
    id: 2,
    name: "Lamborghini Urus",
    proprio: "Mr Pierre",
    date: 2019,
}

let car3 = {
    id: 3,
    name: "Ferrari Roma",
    proprio: "Mr Henry",
    date: 2016,
}

cars.push(car1, car2, car3);
```
## API CRUD
Dans la partie ci-dessous on va implémenter un CRUD (Create Read Update Delete) sur les voitures.

### Read
Le Read passe donc par la méthode GET du protocole HTTP.On va d'abord donc commencer par le Read étant la fonction la plus simple, on fera trois lecture une qui permettra de récupérer toutes les voitures et deux qui permettront de récupérer une voiture en fonction de ses attributs.
Pour le premier Read on va rajouter un endpoint sur la route '/cars'. et ce endpoint renverra juste le contenue de notre tableau cars.
```js
app.get('api/cars', (req, res)=> {
    res.send(cars);
})
```
Assez simple comme première fonction *notez que je fais ici abstraction de tout ce qui est base de données dans un exemple d'API en production(surement la prochaine leçon) le code sera beaucoup plus fourni*. Pour la prochaine fonction on récupérera une voiture en fonction de certaines informations sur la voiture en paramètre en utilisant le paramètre req de la callback de la fonction get. Mais il y a deux manière de récupérer le paramètre soit à travers `req.query` ou à travers `req.params` le choix de l'un des deux va changer comment coder la fonction. On va donc voir les deux.
#### req.params
**req.params** va s'occuper de parser les paramètre au niveau de la route et pas de l'URL. La différence entre la route et l'URL est assez subtile mais disons que l'URL est l'entité en entière par exemple le "https://localhost:3000/cars" là où la route est plus une définission de chemin par exemple `api/cars/`. Pour utilisé req.params on va d'abord dire au router de notre api qu'on attend une valeur au niveau de la route comme ceci: 
```js
app.get('/api/cars/:id', (req, res)=>{
    const id  = req.params.id
    const car = cars[id-1]
    res.send(car);
});
```
Comme vous pouvez la route a une forme particulière avec son `:id`, ce `:id` va permettre de dire à l'api qu'ici on attend  une variable et c'est cette variable qu'on va récupérer avec le `req.params.id`.
l'URL aura donc cette tête: `http://localhost:3000/api/cars/2`
#### req.query
Pour le **req.query** la requête va totalement changer de tête puisque les paramètre vont être parser après un `?` (un query en anglais) et on a pas besoin de spécifier au router de l'api avant. la route aura donc cette tête:
```js
app.get('/api/car', (req, res)=>{
 const id = req.query.id;
 const date = req.query.date;
 const car = cars.find(element => (element.id === id) && (element.date === date));
 res.send(car); 
});
```
notez que vous pouvez mettre autant paramètre que vous voulez et que donc, vous devez réfléchir en amont à ce que vous voulez récupérer sachant que si les paramètres ne sont pas pris en compte dans le code c'est comme s'ils n'existent pas.
L'URL pour cette route ressemblera à ça: `http://localhost:3000/api/car?id=2&date=2019`.

Nous avons donc mis en place nos routes `GET` mais cela ne s'arrête pas là ils nous reste `POST`(pour le CREATE), `PATCH`(pour le UPDATE), `DELETE`(pour le DELETE). Passons donc au POST.

### CREATE

pour cette fonctionnalité on passe par la méthode POST du protocole HTTP, mais ce sera un peu spécial cette fois ci. Dans le cas de la méthode POST on va passer les données dans le corps de la requête, le body, et pour se faire on va devoir le parser, c'est là qu'intervient le middleware `json` intégrer au à express que l'on va intégrer comme ceci:
```js
app.use(express.json())
```
Ce middleware va donc s'occuper de parser les données que l'API va recevoir. Maintenant il faut que l'on créé la route qui va s'occuper de recevoir ces données donc on va créer une route post cette fois ci:
```js
app.post('/api/cars', (req, res)=> {
 const body = req.body;
 cars.push(body);
 res.send(body);
});
```
Tester cette requête va s'avérer plus compliqué car cette fois ci on doit envoyé des données.On peut le faire de deux manières avec CURL, ou un client comme [POSTMAN](https://www.postman.com/downloads/), ce que je vous recommande et aussi ce que j'utilise.
Donc dans le corps de notre requête sur POSTMAN on va y mettre un objet car.
![POSTREQ](images/postman-test.png).
Avec ceci notre CREATE est fait, il nous reste donc le UPDATE et le DELETE.

### UPDATE
Le UPDATE (PATCH en HTTP) en HTTP est un mélange du GET et du POST car on va devoir d'abord chercher la ressource avec son id et ensuite lui dire les champs à éditer, dans l'exemple on va juste le faire avec le proprio de la voiture, on va d'abord créer la route qui convient
```js
app.patch('/api/car/:id', (req, res)=>{
    const id = req.params.id
    let proprio = req.body.proprio;
    cars[id-1].proprio = proprio;
    res.send(cars[id-1]);
});
```
![patch](images/postman-patch.png)
avec cette méthode on arrive donc à modifier une données maintenant il ne reste plus qu'à pouvoir en supprimer.

### DELETE
Le DELETE suis le même procéder que le PATCH sauf qu'ici on veut pas éditer une donnée mais la supprimer, on va donc s'y prendre comme ceci:
```js
app.delete('/api/car/:id', (req, res)=>{
    const id = req.params.id;
    cars.splice(id-1, 1);
    res.send(cars);
});
```
![delete](images/postman-delete.png)
Et normalement vous devriez avoir votre CRUD d'opérationnel, Mais une API reste ne se résume pas qu'à ça, car on doit aussi savoir si notre opération est un succès ou un échec et c'est les là que les code de réponses entre jeux.

### Status Codes
Les Status Codes sont classées de la manière suivante:
- 2XX correspond aux réussite des requêtes
- 4xx correspond aux echec mais côté client
- 5xx correspond aux erreur côté 

vous trouverez une liste de des [code](https://developer.mozilla.org/fr/docs/Web/HTTP/Status) ici.


On, va donc rajouter les codes de réussites et ensuite on ira gérer les erreurs. Les codes de succès sont assez simple à rajouter il suffit de le mettre en préfixe du send, de cette manière `res.status({status code}).send({notre message})`.
Pour le cas du create ce sera un code 201 donc la réponse ressemblera à ça `res.status(201).send({message: "Car added to the db", entity: car})` et pour le reste on utilisera du 200 *notez que pour le delete on peut mettre du 204 à condition que la requête soit vide*. 

On peut ensuite passé à la gestion d'erreurs dans le cas des get il faut vérifier que la ressources puisse être trouvé. On va donc effectuer les vérification et si ce n'est pas le cas on enverra code d'erreur 404(le fameux) avec le message "resource not found" ensuite il faut que l'on vérifie si les données envoyés sont bonnes, si notre champs proprio est bien un texte et pas nombre, dans le cas échéant on mettra une erreur 400 suivi du message (Bad request). 

On se retrouve donc avec le code ci dessous:
```js
const express = require("express");
const app = express();
app.use(express.json())

let cars = [];

let car1 = {
    id: 1,
    name: "Tesla Model S",
    proprio: "Mr Ren",
    date: 2017,
}

let car2 = {
    id: 2,
    name: "Lamborghini Urus",
    proprio: "Mr Pierre",
    date: 2019,
}

let car3 = {
    id: 3,
    name: "Ferrari Roma",
    proprio: "Mr Henry",
    date: 2016,
}

cars.push(car1, car2, car3);

app.get('/', (req, res)=> {
    res.send("Welcome to this demo API")
});

app.get('/api/cars', (req, res)=> {
    res.send(cars);
});

app.get('/api/cars/:id', (req, res)=>{
    const id  = req.params.id
    if(!cars[id-1]){
        res.status(404).send("ressources not found");
        return;
    }
    res.status(200).send(car);
});

app.post('/api/cars', (req, res)=> {
 const body = req.body;
 if(!body.id ||!body.name || !body.proprio || !body.date) {
     res.status(400).send("Bad Request");
     return
 }
 
 cars.push(body);
 res.status(201).send({
     message: "Entity added to the db",
     entity:body
 })
});

app.patch('/api/car/:id', (req, res)=>{
    const id = req.params.id
    if(!cars[id-1]){
        res.status(404).send("ressources not found");
    } 
    let proprio = req.body.proprio;
    if(typeof proprio !== 'string' || proprio instanceof String === false) {
        res.status(400).send("Bad Request");
        return;
    };
    cars[id-1].proprio = proprio;
    res.status(200).send(cars[id-1]);
});

app.delete('/api/car/:id', (req, res)=>{
    const id = req.params.id;
    if(!cars[id-1]) {
        res.status(404).send("Ressource not found");
        return;
    }
    cars.splice(id-1, 1);
    res.status(200).send(cars);
});

app.listen(3000, ()=>{
    console.log('app running on port 3000');
});
```
Voilà vous avez votre premier API REST

Les prochains cours de cette série seront dédiés à la création d'API avec une base données en NoSQL(comme mongoDB) et une en SQL(comme PostgreSQL) avec et sans ORM(j'expliquerai ce qu'est un ORM dans un prochain cours).

N'hésitez pas à me faire des retours concernant mes cours en commentaire juste en dessous ! À bientôt !

Le répo du projet: [API REST example](https://github.com/Salayna/api-rest-example)
