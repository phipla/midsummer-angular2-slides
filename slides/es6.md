# Introduction à ES6/ES2015

<img src="resources/es6.svg" class="plain" style="width: 18vw; display: block; margin: 0 auto">

---

## Historique

### ES5 Introduit en 2009
* `'use strict'`
* JSON
* getters, setters

### ES6 finalisé en juin 2015
* classes
* arrow functions (`=>`)
* for of
* générateurs
* Promises

### ES7 finalisé en juin 2016

### ES8
* async/await

---

## Ressources

* http://es6-features.org/
* http://node.green/
* https://developer.mozilla.org/en-US/docs/Web/JavaScript

---

## `let` et `const`

`let` déclare une variable dont le scope est le bloc local (contrairement à `var`, pour lequel le scope de la variable est la fonction) :

```javascript
function varTest() {
  var x = 1;
  if (true) {
    var x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 2
}
```

```javascript
function varTest() {
  let x = 1;
  if (true) {
    let x = 2;  // same variable!
    console.log(x);  // 2
  }
  console.log(x);  // 1
}
```

---

## `let` et `const`

`const` définit une référence constante. `const` ne rend toutefois pas l'objet référencé immutable.

```javascript
function varTest() {
  const x = { value: 2 };

  x = { value: 4 }; // Erreur: re-définit la référence !
  x.value = 4; // OK: ne re-définit pas la référence
}
```

En ES6, il est généralement d'usage de __ne plus utiliser `var` du tout__, mais d'utiliser `let`
ou `const`, selon le cas.

---

## Chaînes de caractères

Les chaînes de caractères peuvent, en plus des délimiteurs
<code style="font-size: 2em; position: relative; top: 0.3em; line-height: 0.5">'</code> et
<code style="font-size: 2em; position: relative; top: 0.3em; line-height: 0.5">"</code>,
utiliser le nouveau délimiteur
<code style="font-size: 2em; position: relative; top: 0.3em; line-height: 0.5">`</code>.

Ce dernier permet d'utiliser des chaînes multi-lignes, et permet également d'interpoler des expressions Javascripts arbitraires en utilisant `${}`

```javascript
let string1 = 'alpaca';
let string2 = "llama";

let string3 = `
I am an ${string1}
I am not a ${string2}

2 + 2 = ${2 + 2}
`
```

---

## Classes

### ES5

Les classes sont déclarées de la même manières que les fonctions. L'héritage est géré sous forme de chaîne de prototypes.

```javascript
function Animal(noise) {
  this.noise = noise;
}

Animal.prototype.speak = function speak() {
  return 'I say: ' + this.noise + '!';
};

function Cat() {
    Animal.call(this, 'meow');
}

Cat.prototype = Object.create(Animal.prototype);
Cat.prototype.constructor = Cat;
```

---

<!-- .slide: id="es6classes" -->

## Classes

### ES6

Le fonctionnement de base n'a pas changé, mais le mot-clé `class` rend l'intention et la syntaxe plus claire.

```javascript
class Animal {
  constructor(noise) {
    this.noise = noise;
  }

  speak() {
    return 'I say: ' + this.noise + '!';
  }
}

class Cat extends Animal {
  constructor() {
    super('meow');
  }
}
```

---

## Classes: getters et setters

Préfixer une méthode par get ou set en fait un getter ou un setter :

```javascript
class Duration {
  constructor() {
    this.inMinutes = 0;
  }

  set inHours(value) {
    this.inMinutes = 60 * value;
  }

  get inHours() {
    return this.inMinutes / 60;
  }
}

let duration = new Duration();
duration.inMinutes = 60;
console.log(duration.inHours); // 1
duration.inHours = 2;
console.log(duration.inMinutes); // 120
```

---

## Arrow functions

### ES5

```javascript
[ 1, 2, 3 ].map(function (a) {
    return 2 * a
});
```

### ES6

```javascript
[ 1, 2, 3 ].map(a => 2 * a);
[ 1, 2, 3 ].map((a) => 2 * a);
[ 1, 2, 3 ].map(a => { return 2 * a });
```

Retourner un objet littéral:

```javascript
[ 1, 2, 3 ].map(a => ({ value: a }));
```

Plusieurs paramètres d'entrée:

```javascript
[ 1, 2, 3 ].reduce((acc, val) => acc + val);
```

---

## Arrow functions: `this`

```javascript
class Multiplier {
  constructor(factor) {
    this.factor = factor;
  }

  multiply(arr) {
    return arr.map(function (a) {
        // /!\ Danger ! this n'est pas défini ici !
        return this.factor * a;
    })
  }
}
```


```javascript
class Multiplier {
  constructor(factor) {
    this.factor = factor;
  }

  multiply(arr) {
    return arr.map(a => this.factor * a);
  }
}
```

---

## Fonctions: reste des paramètres

```javascript
function f(a, b, ...allTheRest) {
  console.log(a, b, allTheRest);
}

f(); // undefined, undefined, []
f(1, 2); // 1, 2, []
f(1, 2, 3, 4) // 1, 2, [3, 4]
```

---

## Collections: `Set`

Un "Set" est un ensemble non ordonné de valeurs uniques.

```javascript
let obj = {};
let set = new Set([1, obj]);

set.add('Alpaca');

set.has(1); // true
set.has(2); // false
set.has('Alpaca'); // true
set.has('Llama'); // false
set.has(obj); // true
set.has({}); // false (obj !== {})
```

---

## Collections: `Map`

Une "Map" est un dictionnaire, c'est-à-dire une carte de clés / valeurs.

```javascript
let obj = {};
let map = new Map();

map.set('animal', 'Alpaca');
map.set(obj, 'Object')
map.set(1, 'One')

map.has(1); // true
map.get(1); // 'One'
map.get(obj); // 'Object'
map.get({obj}); // undefined
map.get(1); // 'One'
map.get('1'); // undefined
```

---

## Collections: `WeakMap`

Une "WeakMap" est un dictionnaire dont les références sont "faibles".
Ses clés ne peuvent être que des objets, ses valeurs peuvent être n'importe quoi.

```javascript
let obj1 = {};
let obj2 = {};
let obj3 = {};
let map = new WeakMap();
map.set(obj1, 'Alpaca');
map.set(obj2, 'Llama');

map.get(obj1); // 'Alpaca'
map.get(obj2); // 'Llama'
```

Il n'est pas possible d'énumérer les valeurs contenues dans une `WeakMap`, ou même de les compter avec .length. Lorsqu'une des clés de la `WeakMap` est nettoyée par le ramasse-miette, la référence est supprimée de la `WeakMap`.

---

## Opérateur `of`

```javascript
let iterable = [10, 20, 30];

for (let value of iterable) {
  value += 1;
  console.log(value);
}
// Résultat: 11, 21, 31
```

À ne pas confondre avec l'opérateur `in` qui itère sur les propriétés énumérables d'un objet.

```javascript
var iterable = [10, 20, 30];

for (var value in iterable) {
  console.log(value);
}
// Résultat: 0, 1, 2
```

Fonctionne aussi sur les autres types d'énumérables: `string`, `Map`, `Set`, etc.

---

## Promesses (`Promise`)

Une promesse est un objet qui représente un résultat futur.

Une promesse a une méthode `.then()` qui permet de spécifier une fonction qui sera exécutée en cas de succès.

Une promesse a une méthode `.catch()` qui permet de spécifier une fonction qui sera exécutée en cas d'erreur.

```javascript
const promise = new Promise((resolve, reject) => $.ajax({
  url: 'https://midsummerweb.com',
  success: resolve,
  error: reject
}))

promise.then(data => console.log(data.substr(0, 15)))
```

---

## Enchaînement de promesses

Les méthodes `.then()` et `.catch()` retournent elles-mêmes des `Promises`.

La valeur de résolution de ces `Promise`s est déduite de la valeur de retour des fonctions exécutées (on peut donc les chaîner).

Si les fonctions exécutées retournent une `Promise`, c'est la valeur de résolution de cette `Promise`. Si les fonctions exécutées retournent une autre valeur, c'est la valeur retournée.

```javascript
const promise = new Promise((resolve, reject) => $.ajax({
  url: 'https://midsummerweb.com',
  success: resolve,
  error: reject
}))

promise
  .then(data => data.substr(0, 15))
  .then(console.log)
```

---

## Générateurs

Les générateurs sont des fonctions qui ne retournent pas immédiatement une valeur, mais retournent un objet de type `Generator` qui permet d'itérer sur les valeurs intermédiaires envoyées par cette fonction.

```javascript
function *animalActions(animal) {
  yield `The ${animal} eats`;
  yield `The ${animal} drinks`;
  yield `The ${animal} sleeps`;
}

let alpacaActions = animalActions('alpaca');
console.log(alpacaActions.next().value); // 'The alpaca eats'
console.log(alpacaActions.next().value); // 'The alpaca drinks'
console.log(alpacaActions.next().value); // 'The alpaca sleeps'
```

Les générateurs sont *itérables* au même titre que des `Arrays`

```javascript
for (let action of alpacaActions()) {
  console.log(action) ;
}
```

---

## Raccourcis pour la notation des objets

ES6 Introduit plusieurs raccourcis permettant de simplifier l'écriture d'objets littéraux:

```javascript
const animal = 'Alpaca';
const alpacaList = [ 'Samantha', 'Bob', 'Monica', 'Ted' ];

const obj = {
  alpacaList, // Équivaut à écrire: alpacaList: alpacaList

  // Permet d'utiliser le résultat d'une expression comme clé
  [ 'is' + animal ]: true,

  // Équivaut à getAlpaca: function getAlpaca(index) { ... }
  getAlpaca(index) {
    return this.alpacaList[index];
  }
}
```

---

## Affectation par décomposition (destructuring)

```javascript
let [a, b] = [1, 2]; // a = 1; b = 2;
let [a, , b] = [1, 2, 3]; // a = 1; b = 3;

let [a, b, ...c] = [1, 2, 3, 4, 5]; // a = 1; b = 2; c = [3, 4, 5];
let {a, b} = {a:1, b:2}; // a = 1; b = 2;
```

Supporte les valeurs par défaut

```javascript
[a = 5, b = 7] = [1]; // a = 1; b = 7
```

---

## Affectation par décomposition (destructuring)

Permet d'échanger deux variables (ou plus)

```javascript
let a = 1;
let b = 3;

[a, b] = [b, a]; // a = 3; b = 1;
```

Ne fonctionne pas (encore) avec es objets

```javascript
let a = { a: 5, b: 2 };
let b = { ...a }; // Impossible... pour l'instant (mais possible avec ES7 et/ou Typescript)
let c = { ...a, b: 3 }; // Idem
```

---

## Modules

<!-- .slide: id="es6modules" -->

Remplace et standardise les formats de module CommonJS / node.js / AMD / UMD

Mots-clés `export` et `import`

```javascript
//--- module1 ---
let a = 1, b = 2, test = 3;
export { a, b, test as c };

//--- module2 ---
import { a as valeurA, b as valeurB, c as valeurC } from 'module1';
```

Il est également possible d'utiliser un export par défaut

```javascript
//--- module1 ---
let a = 1;
export default a;

//--- module2 ---
import valeurA from 'module1';
```

---

<!-- .slide: data-background="#ffffff" data-background-image="bower_components/reveal.js-plugins/chalkboard/img/whiteboard.png"-->

## TD ES6 - préparation

1\. Créer un dossier ES6, contenant des répertoires src/ et lib/

2\. Créer un projet

```bash
npm init -y
npm install --save-dev babel-cli babel-preset-env
```

3\. Créer un fichier .babelrc

```json
{
  "presets": ["env"]
}
```

4\. Éditer package.json et mettre dans scripts

```json
{
    "scripts": {
        "build": "babel src -d lib",
        "start": "node lib/index.js"
    }
}
```

---

<!-- .slide: data-background="#ffffff" data-background-image="bower_components/reveal.js-plugins/chalkboard/img/whiteboard.png"-->

## TD ES6 - buts

1. Créer **et exporter** une classe `Animal` dans `animal.js`
  * Un animal a une méthode `walk` et une propriété `height`, et un getter `name`
  * La méthode `walk` renvoie une Promise, qui se résoud après `height * 1000` ms, avec la chaîne `animal a fini de marcher`, animal étant remplacé par animal.name
2. Créer, et exporter, dans un fichier `llama.js`, une classe `Llama` qui étend `Animal`, qui redéfinit `name`
3. Créer, et exporter, dans un fichier `alpaca.js`, une classe `Alpaca` qui étend `Animal`, qui redéfinit `name`, et qui redéfinit `walk` pour que la promise retournée se résolve après `height * 500` ms
4. Importer les deux classes dans `index.js`
5. Initialiser un tableau de `Llama`s et un d'`Alpaca`s à partir d'un tableau d'objets JavaScript génériques en utilisant .map().
6. Faire la course ! Utiliser Promise.race pour afficher l'animal le plus rapide.

## TD ES6 - corrigés

<div class="xx-large">https://git.io/v9MpJ</div>
