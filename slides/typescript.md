# TypeScript

<div class="typescript-logo"></div>

---

## Introduction

https://www.typescriptlang.org/

* Sur-ensemble de Javascript / ES6 permettant le typage statique des variables
* Se compile en Javascript
* Open-Source (licence Apache)
* Développé par Microsoft
* Date de 2012, version actuelle 2.2 (22 février 2017)
* Auto-complétion intelligente de type "Intellisense" supportée par de nombreux IDE (Visual Studio, Visual Studio code, plugins pour Eclipse, Atom...)

---

## Ressources

* https://www.typescriptlang.org/docs/tutorial.html

---

## Préparation

Installation de ts-node

```bash
npm install -g typescript ts-node
```

---

## Type des variables

### Types de base

Typescript supporte tous les types de variables natifs ES6: `boolean`, `number`, `string`:

```typescript
let alpacaCount: number
alpacaCounter = 4; // OK
alpacaCounter = '4'; // Erreur !
alpacaCounter = false; // Erreur !
```

### Enum

```typescript
enum Color {Red, Green, Blue};
let c: Color = Color.Green; // OK
let c = 124; // Erreur
```

### Coercion de types

```typescript
let value1: any = 4;
let value2: number;
value2 = <number>value1;
```

---

## Types composés

### Tableaux

```typescript
let alpacaNames: string[] = [];
alapacaNames.push('Monica'); // OK
alapacaNames.push(51); // Erreur !
```

### Tuples

```typescript
let alpacaWithAge: [ string, number ];
alpacaWithAge = ['Monica', 5]; // OK
alpacaWithAge = [1, 2, 3]; // Erreur !

alpacaWithAge[0].substr(1); // OK
alpacaWithAge[1].substr(1); // number n'a pas de membre 'substr' !
```

### Dictionnaires

```typescript
let alpacaMap: { [key: string]: number };

alpacaMap = { 'Monica': 5, 'Roberto': 3 }; // OK
alpacaMap = { 'Monica': 5, 'Roberto': 'Trois' }; // Erreur
```

---

## Types divers

### `any`

N'importe quel type :

```typescript
let quImporte: any;
quImporte = 4;
quImporte = 'Clé à molette';
quImporte = window.document;
```

### `void`

Rien. Utilisé généralement comme type de retour pour les fonctions qui ne retournent rien :

```typescript
function alerteMoi(): void {
    alert("Attention, derrière toi !!!!");
}
```

---

## Types divers

### `null` et `undefined`

Correspondent aux types équivalents en Javascript. Par défaut, il est possible d'assigner le type `null` à n'importe quelle variable (sauf si l'option strictNullChecks est activée).

```typescript
let a: string;
a = null; // OK, sauf si strictNullChecks est activée
```

### `never`

Un type particuler qui représente une valeur qui ne peut jamais être assignée. S'utilise comme type de retour pour une fonction qui ne peut pas retourner: envoie un exception, boucle infinie, etc.

```typescript
function error(message: string): never {
    throw new Error(message);
}
```

---

## Alias de types

Permettent de nommer un type

```typescript
type AlpacaName = string;
type AlpacaAge = number;
type AlpacaWithAge = [ AlpacaName, AlpacaAge ];

let alpacaWithAge: AlpacaWithAge = ['Monica', 5];
```

---

## Inférence de type

Un type n'a pas a être spécifié explicitement s'il peut être automatiquement déduit :

```typescript
let a /* : string */ = 'Hello!';
a = 2; // Erreur !
```

Y compris dans des cas plus complexes :

```typescript
let f = function (a /* : number */ = 2) /* : number */ {
    return a + 3;
}

let a /* : number */ = f(3);
a = 'Texte'; // Erreur !

```

---

## Classes

Les classes Typescript sont basées sur <a href="#/es6classes">les classes ES6</a>.

Elles peuvent avoir des membres privés, protégés et publics.

L'accès par défaut est public.

```typescript
class Alpaca {
    private name: string;
    private age?: number;

    constructor(name: string, age?: number /* Paramètre optionnel */) {
        this.name = name;
        this.age = age;
    }

    public sayMyName(): string {
        return `Hello, my name is ${this.name}`;
    }
}
```

Toutes les fonctionnalités des classes ES6 (extends, accesseurs...) sont accessibles sur les classes TypeScript.

---

## Propriétés en lecture seule (`readonly`)

Une propriété peut être marquée comme `readonly`. Elle ne peut alors être modifiée que lors de sa déclaration, ou dans le constructeur.

```typescript
class Alpaca {
    readonly name: string;
    readonly age: number = 8;

    constructor(name: string) {
        this.name = name;
    }
}

const al = new Alpaca('Albert');
al.name = 'Roberto'; // Erreur !
al.age = 3; // Erreur !
```

---

## Initialisation des propriétés dans le constructeur

```typescript
class Alpaca {
    private name: string;
    private age?: number;

    constructor(name: string, age?: number) {
        this.name = name;
        this.age = age;
    }
}
```

Il est possible de simplifier l'écriture de cette classe de la manière suivante

```typescript
class Alpaca {
    constructor(private name: string, private age?: number) {}
}
```

---

## Classes

### Classes abstraites

```typescript
abstract class Animal {
    private name: string;
    private age?: number;
    abstract makeSound(): void;
}
```

### Propriétés statiques

```typescript
class Alpaca extends Animal {
    static species = 'Vicugna pacos';
    makeSound() {
        console.log('Hummm');
    }
}

console.log(Alpaca.species);
```

---

## Classes

### Type de retour "this"

```typescript
class Animal() {
    name: string;
    age: number;
    setName(value: string) {
        this.name = value;
        return this;
    }
}
var doge = (new Animal()).setName('Medor');

class Alpaca extends Animal {
    wool: number;
    setWool(value: number) {
        this.wool = value;
        return this;
    }
}
var alpaca = (new Alpaca()).setName('Roberto').setWool(5); // Erreur !
```

---

## Classes

### Type de retour "this"

```typescript
class Animal() {
    name: string;
    age: number;
    setName(value: string): this {
        this.name = value;
        return this;
    }
}

class Alpaca extends Animal {
    wool: number;
    setWool(value: number): this {
        this.wool = value;
        return this;
    }
}
var alpaca = (new Alpaca()).setName('Roberto').setWool(5); // OK
```

---

## Interfaces

Une interface ne génère pas de code Javascript: elle sert uniquement d'indication au compilateur Typescript:

```typescript
interface Shearable {
    shear(): number
}

class Alpaca implements Shearable {
    woolKg: number = 10;

    shear() {
        const shornWool = this.woolKg;
        this.woolKg = 0;
        return shornWool;
    }
}

let shearable: Shearable;
shearable = new Alpaca();
console.log(shearable.shear()); // 10
```

---

## Interfaces: duck typing

Implémenter l'interface explicitement n'est pas nécessaire si le contrat de l'interface est implémenté.

```typescript
interface Shearable {
    shear(): number
}

class Alpaca /* implements Shearable */ {
    woolKg: number = 10;

    shear() {
        const shornWool = this.woolKg;
        this.woolKg = 0;
        return shornWool;
    }
}

let shearable: Shearable;
shearable = new Alpaca(); // Pas de problème !
console.log(shearable.shear()); // 10
```

---

## Fonctions

Comment écrire le type d'une fonction ?

```typescript
type Adder = (x: number, y: number) => number;
type AdderAlt = { (x: number, y: number): number };

let adder: Adder = (a: number, b: number) => a + b;
```

Un constructeur n'est toujours qu'une fonction :

```typescript
class Alpaca {
    constructor(private name: string, private age: number) {}
}

type AlpacaFactory = new(name: string, age: number) => Alpaca;
type AlpacaFactoryAlt = { new(name: string, age: number): Alpaca };

const alpacaFactory: AlpacaFactory = Alpaca;
```

---

## Fonctions

Paramètres optionnels ou par défaut

```typescript
type NameBuilder = (fistName: string, lastName?: string) => string;

function buildName1(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}

const buildName2 = (firstName: string, lastName = 'McBob')
    => firstName + " " + lastName;

let b1: NameBuilder = buildName1;
let b2: NameBuilder = buildName2;
```

---

## Génériques

### Pourquoi les génériques ?

Prenons une fonction naïve permettant de trouve l'index d'un élément dans un tableau :

```typescript
function findInArray(array: number[], value: number): number {
    for (let i = 0; i < array.length; i++) {
        if (array[i] === value) return i;
    }
}

findInArray([1, 4, 56], 4); // 1
findInArray(['Sheep', 'Goat', 'Alpaca'], 'Alpaca'); // Erreur
```

---

## Génériques

### Une fonction par type ?

```typescript
function findInNumberArray(array: number[], value: number): number {
    for (let i = 0; i < array.length; i++) {
        if (array[i] === value) return i;
    }
}

function findInStringArray(array: string[], value: string): number {
    for (let i = 0; i < array.length; i++) {
        if (array[i] === value) return i;
    }
}
// function findInBooleanArray, etc etc

findInNumberArray([1, 4, 56], 4); // 1
findInStringArray(['Sheep', 'Goat', 'Alpaca'], 'Alpaca'); // 2
```

---

## Génériques

### Avec des génériques

```typescript
function findInArray<T>(array: T[], value: T): number {
    for (let i = 0; i < array.length; i++) {
        if (array[i] === value) return i;
    }
}

findInArray([1, 4, 56], 4); // 1
findInArray(['Sheep', 'Goat', 'Alpaca'], 'Alpaca'); // 2
findInArray(['Sheep', 'Goat', 'Alpaca'], 3); // Erreur
```

---

## Génériques

### Contraintes

```typescript
interface Shearable {
    shear(): number
}

class Alpaca implements Shearable() { /* ... */ }
class Sheep implements Shearable() { /* ... */ }
class AngoraRabbit implements Shearable() { /* ... */ }

function shear<T extends Shearable>(shearable: T) {
    const woolShorn: number = shearable.shear(); // Cette méthode existe forcément
    console.log(woolShorn);
    return woolShorn;
}
```

---

## Génériques

### Classes

Les classes également peuvent être génériques:

```typescript
class AnimalPen<T extends Animal, S extends T[]> {
    animals: S;

    addAnimal(animal: T): this {
        this.animals.push(animal);
        return this;
    }
}

let pen = new AnimalPen<Alpaca, Alpaca[]>(); // OK
let pen2 = new AnimalPen<Alpaca, Animal[]>(); // Erreur: Animal[] n'étend pas Alpaca[]
```

---

## Types avancés

### Types d'intersection

```typescript
function extend<T, U>(first: T, second: U): T & U {
    let result = <T & U>{};
    Object.assign(result, first, second);
    return result;
}

class Alpaca {
    constructor( public name: string, public age: number ) {}
}
let roberto = new Alpaca('Roberto', 3)

class VaccinationLog {
    constructor( public vaccinationDates: Date[] ) {}
}
let robertosVaccinationLog = new VaccinationLog([
    new Date('2015-06-12'),
    new Date('2017-01-01')
]);

let vaccinatedRoberto = extend(roberto, robertosVaccinationLog);
```

---

## Types avancés

### Types d'union

```typescript
class Alpaca {
    constructor( public name: string, public woolInKg: number ) {}
}

class Human {
    constructor(
        public name: string, public hasDrivingLicence: boolean
    ) {}
}

function sayMyName(me: Alpaca | Human) {
    console.log(me.name); // OK
    console.log(me.woolInKg); // Error !
}
```

---

## Décorateurs

Fonctionnalité expérimentale d'ES6 et de Typescript, nécessite l'option `experimentalDecorators`

4 types de décorateur :

* Décorateur de classe
* Décorateur d'accesseur
* Décorateur de propriété
* Décorateur de méthode

Les décorateurs sont des fonctions qui sont appelées, lors de l'exécution du code, quand la classe est initialisée. Ils peuvent modifier la classe / propriété etc, ou exécuter des fonctions annexes.

---

## Décorateur de classe

```typescript
type AnimalProperties = { [key: string]: string };
let animalRegistry: Map<Function, AnimalProperties> = new Map();

function Register(properties: AnimalProperties) {
    return (animalConstructor: Function) => {
        animalRegistry.set(animalConstructor, properties)
    }
}

function getAnimalProperties(animal: Function) {
    return animalRegistry.get(animal);
}

@Register({
    name: 'Alpaca',
    description: 'A furry animal from the andes'
})
class Alpaca { /* ... */ }

getAnimalProperties(Alpaca); // { name: 'Alpaca', description: '...' }
```

---

<!-- .slide: data-background="#ffffff" data-background-image="bower_components/reveal.js-plugins/chalkboard/img/whiteboard.png"-->

## TD TypeScript - préparation

1\. Créer un dossier ts

```bash
npm init -y
npm install --save typescript
```

2\. Initialiser le fichier tsconfig.json

```json
{
    "compilerOptions": {
        "target": "es6",
        "module": "commonjs",
        "declaration": false,
        "strictNullChecks": true,
        "noImplicitAny": true,
        "noLib": false,
        "outDir": "./lib"
    },
    "include": [ "src/**/*" ],
    "exclude": [ "node_modules" ]
}
```

---

<!-- .slide: data-background="#ffffff" data-background-image="bower_components/reveal.js-plugins/chalkboard/img/whiteboard.png"-->


## TD TypeScript - préparation - 2


3\. Éditer package.json et mettre dans scripts

```json
{
    "scripts": {
        "build": "tsc",
        "start": "node lib/index.js"
    }
}
```

---

<!-- .slide: data-background="#ffffff" data-background-image="bower_components/reveal.js-plugins/chalkboard/img/whiteboard.png"-->

## TD TypeScript - préparation - 3

<div class="filename">webservice.ts</div>

```typescript
const data = [
  { name: 'Roberto', height: 0.67, age: 2 },
  { name: 'Monica', height: 0.56, age: 1 },
  { name: 'Bobby', height: 0.9, age: 5 },
  { name: 'Marcel', height: 0.87, age: 1.2 },
  { name: 'Albert', height: 0.89, age: 10 },
];
let rnd = () => Math.random() * 1000;

export const get = (i: number) =>
  new Promise(r => setTimeout(() => r(data[i]), rnd()));
export const getAll = () =>
  new Promise(r => setTimeout(() => r(data), rnd()));
export const add = (item: any) =>
  new Promise(r => setTimeout(() => {data.push(item); r(item)}, rnd()));
export const set = (i: number, item: any) =>
  new Promise(r => setTimeout(() => r(data[i] = item), rnd()));
export const erase = (i: number) =>
  new Promise(r => setTimeout(() => r(data.splice(i, 1)[0]), rnd()));
```

---

<!-- .slide: data-background="#ffffff" data-background-image="bower_components/reveal.js-plugins/chalkboard/img/whiteboard.png"-->

## TD TypeScript - buts

1. Importer les classes du TD précédent et les convertir en Typescript
2. Créer une classe Typescript générique repository.ts
    * qui contient un field `data` de type `Array<T>`
    * qui prend, comme paramètre du constructeur, une fonction `(input: any) => T`
    * qui dispose d'une méthode `get(position: number)` qui :
        * récupère les données à partir du webservice mais uniquement si nécessaire
        * retourne une `Promise<T>`, à partir des données retournées par le webService
    * qui dispose des méthodes suivantes, qui modifient les données à la fois dans le webService, et dans le field data
        * `add(item: T)`
        * `set(i:number, item: T)`
        * `erase(i:number)`
    * toutes les trois retournent une `Promise<T>`

---

<!-- .slide: data-background="#ffffff" data-background-image="bower_components/reveal.js-plugins/chalkboard/img/whiteboard.png"-->

## TD TypeScript - corrigés

<div class="xx-large">https://git.io/v9MpJ</div>
