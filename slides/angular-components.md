# COMPOSANTS

---

## Composants

Brique de base d'Angular 2

Encapsule :
* Template HTML
* CSS spécifiques au composant
* Comportement

Un composant est généralement défini par 4 fichiers

* Classe TypeScript
* HTML (peut être défini dans le TypeScript)
* CSS (peut être défini dans le TypeScript)
* Spécification (tests unitaires)

Par convention, ces fichiers sont généralement groupés dans un même dossier

---

## Composants

<div class="filename">app.component.ts</div>
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'msw-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
}
```

---

## Template HTML du composant

`app.component.html`
```html
<h1>
  {{title}}
</h1>
```

### À retenir

<i class="fa fa-hand-o-right"></i> `{{ ... }}` extrapole l'expression entre accolades

<i class="fa fa-hand-o-right"></i> Le scope des variables est la class du composant

---

## CSS du composant

Le CSS du composant est, par défaut, local au composant, et ne fuite pas sur les autres composants, parents ou enfants.

Le composant hérite toutefois du CSS global de l'application

Le pseudo-sélecteur `:host` permet de sélectionner l'élément HTML racine du composant

`:host(.active)` permet de sélectionner l'élément HTML seulement s'il a la classe "active"

---

## Saisie de données (1)

`app.component.html`

<pre><code class="html" data-trim data-noescape>
&lt;h1>{{title}}&lt;/h1>
&lt;input <mark>#name</mark>>
&lt;button <mark>(click)="addAlpaca(name)"</mark>>Nouvel alpaga&lt;/button>
</code></pre>

### À retenir

<i class="fa fa-hand-o-right"></i> Le `#` crée une variable accessible localement

<i class="fa fa-hand-o-right"></i> `(click)` exécute l'expression fournie lorsque l'évènement "click" a lieu

---

## Saisie de données (2)

<div class="filename">app.component.ts</div>
```typescript
class Alpaca {
  constructor(public name: string) {}
}

@Component({ /* .. */ })
export class AppComponent {
  title = 'AlpagaSoft';
  alpacas: Alpaca[] = [];
  addAlpaca(name: HTMLInputElement) {
    this.alpacas.push(new Alpaca(name.value));
    console.log(`Bienvenue, ${name.value}`);
  }
}
```

---

## Affichage des données

<pre><code class="html" data-trim data-noescape>
&lt;ul>
  &lt;li <mark>*ngFor="let alpaca of alpacas"</mark>>
    {{ alpaca.name }}
  &lt;/li>
&lt;/ul>
</code></pre>

### À retenir

<i class="fa fa-hand-o-right"></i> Les directives commençant par `*` sont des directives structurelles

<i class="fa fa-hand-o-right"></i> `*ngFor`, `*ngIf`, `*ngSwitch`

---

## (Parenthèse: toolkits d'UI!)

Par exemple bootstrap pour Angular

```bash
npm install ngx-bootstrap bootstrap@next --save
```

Mais il y a aussi

* Semantic UI
* Clarity
* Angular Material 2
* Kendo
* ...

Voir https://angular.io/resources/

---

## Communication entre composants

### Création d'un nouveau composant

```bash
ng generate component components/alpaca
```

```typescript
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'msw-alpaca',
  templateUrl: './alpaca.component.html',
  styleUrls: ['./alpaca.component.css']
})
export class AlpacaComponent implements OnInit {
  constructor() { }

  ngOnInit() {
  }
}
```

---

## Variables d'entrée: `@Input` (1)

<pre><code class="typescript" data-trim data-noescape>
import { Component, OnInit, <mark>Input</mark> } from '@angular/core';

@Component({
    selector: 'msw-alpaca',
    templateUrl: './alpaca-summary.component.html',
    styleUrls: ['./alpaca-summary.component.css']
})
export class AlpacaSummaryComponent implements OnInit {
    <mark>@Input() name: string;</mark>
    constructor() {}
    ngOnInit() {}
}
</code></pre>

```html
<div class="clearfix">
  <div class="content">
    <div class="name">{{ name }}</div>
  </div>
</div>
```

---

## Variables d'entrée: `@Input` (2)

<pre><code class="html" data-trim data-noescape>
&lt;div *ngFor="let alpaca of alpacas">
    &lt;msw-alpaca
        <mark>[name]</mark>="alpaca.name"
        >&lt;/msw-alpaca>
    &lt;/li>
&lt;/div>
</code></pre>

<pre><code class="html" data-trim data-noescape>
    &lt;msw-alpaca
        <mark>name</mark>="Roméo"
        >&lt;/msw-alpaca>
    &lt;/li>
</code></pre>

### À retenir

<i class="fa fa-hand-o-right"></i> Une variable annotée avec `@Input` dans le contrôleur est
initialisée avec la valeur de l'attribut du même nom

<i class="fa fa-hand-o-right"></i> L'attribut est évalué si il est mis entre `[crochets]`, sinon
il est considéré comme une simple chaîne de caractères

---

## Sorties: `@Output` (1)


```bash
ng generate component components/text-input
```

<pre><code class="typescript" data-trim data-noescape>
import { Component, OnInit, Output, EventEmitter } from '@angular/core';

@Component({ /* ... */ })
export class TextInputComponent {
  <mark>@Output() textEntered = new EventEmitter();</mark>

  constructor() {}
  enter(input: HTMLInputElement) {
    <mark>this.textEntered.emit(input.value);</mark>
    input.value = '';
  }
}
</code></pre>

```html
<input type="text" #textInput (keyup.enter)="enter(textInput)">
<button (click)="enter(textInput)">Saisie</button>
```

---

## Sorties: `@Output` (2)

On a déjà vu comment utiliser ce composant:

```html
<msw-text-input
  (textEntered)="addAlpaca($event)"
  ></msw-text-input>
```

### À retenir

<i class="fa fa-hand-o-right"></i> `(textEntered)` exécute l'expression fournie lorsque l'évènement
"textEntered" a lieu

<i class="fa fa-hand-o-right"></i> La valeur de l'évènement est dans la variable `$event`

---

## Entrées-sorties (1)

<pre><code class="typescript" data-trim data-noescape>
import { Component, OnInit, Input, <mark>Output, EventEmitter</mark> } from '@angular/core';

@Component({
    selector: 'msw-alpaca',
    templateUrl: './alpaca-summary.component.html',
    styleUrls: ['./alpaca-summary.component.css']
})
export class AlpacaSummaryComponent {
    @Input() name: string;
    <mark>@Output() textEntered = new EventEmitter();</mark>
    constructor() {}
    <mark>enter(input: HTMLInputElement) { this.textEntered.emit(input.value); }</mark>
}
</code></pre>

<pre><code class="html" data-trim data-noescape>
&lt;div class="clearfix">
  &lt;div class="content">
    &lt;div class="name">{{ name }}&lt;/div>
    <mark>&lt;input type="text" #textInput (keyup.enter)="enter(textInput)"></mark>
  &lt;/div>
&lt;/div>
</code></pre>

---

## Entrées-sorties (2)

<pre><code class="html" data-trim data-noescape>
    &lt;msw-alpaca
        <mark>[(name)]</mark>="alpaca.name"
        >&lt;/msw-alpaca>
    &lt;/li>
</code></pre>

Équivaut à

<pre><code class="html" data-trim data-noescape>
    &lt;msw-alpaca
        [name]="alpaca.name"
        (nameChange)="alpaca.name=$event"
        >&lt;/msw-alpaca>
    &lt;/li>
</code></pre>

<img style="float: right; width: 10vw" class="plain" src="resources/bananas-in-a-box.svg">

### À retenir

<i class="fa fa-hand-o-right"></i> L'opérateur "banane-dans-une-boîte" `[()]` est un raccourci
permettant de réaliser un binding bi-directionnel.

---

## Entrées-sorties sur composants natifs

`[]` et `()` marchent également pour les paramètres et les évènements de éléments HTML natifs
comme `<input>`.

Il est donc possible de faire comme ceci:

```html
<input [value]="test" (input)="test=$event.target.value" />
```

Mais il est égalment possible d'utiliser la directive `ngModel`:

```html
<input [(ngModel)]="test" />
```

