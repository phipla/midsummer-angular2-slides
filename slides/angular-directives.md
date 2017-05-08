# Directives

---

## NgStyle

```html
<div [style.background]="background: alpaca.notes ? '#f0f6ff' : '#fff'">
    <!-- ... -->
</div>
```

```html
<div [ngStyle]="{
    background: alpaca.notes ? '#f0f6ff' : '#fff'
}">
    <!-- ... -->
</div>
```

---

## NgClass

```html
<div [class.hasNotes]="alpaca.notes">
    <!-- ... -->
</div>
```

```html
<div [ngClass]="{
    hasNotes: alpaca.notes
}">
    <!-- ... -->
</div>
```

```html
<div [ngClass]="['hasNotes', 'needVaccine']">
    <!-- ... -->
</div>
```

---

## Directives structurelles

La notation `*` est en fait un raccourci. La syntaxe étendue utilise l'élément `<template>`
défini dans HTML5.

```html
<li *ngFor="let alpaca of alpacas">
{{ alpaca.name }}
</li>
```

```html
<template ngFor [ngForOf]="alpacas" let-alpaca>
    <li>
        {{ alpaca.name }}
    </li>
</template>
```

---

## NgFor

<pre><code class="html" data-trim data-noescape>
&lt;ul>
  &lt;li
    *ngFor="
        let alpaca of alpacas<span class="callback">1</span>;
        let i = index<span class="callback">2</span>;
        let isEven = even<span class="callback">2</span>"
    [style.background]="isEven ? '#f0f6ff' : '#fff'"
    >
    {{ alpaca.name }}
  &lt;/li>
&lt;/ul>
</code></pre>

1. `let ... of`
2. Autres `let`

Variables pré-définies dans la boucle `ngFor`:

index, first, last, even, odd

---

## NgFor, trackBy

---

## NgIf

Affiche l'élément dans le DOM uniquement si la condition retourne vrai (ou une quelconque valeur <i>truish</i>)

<pre><code class="html" data-trim data-noescape>
&lt;li *ngFor="let alpaca of alpacas">
    &lt;div class="name">{{ alpaca.name }}&lt;/div>
    &lt;div
        class="warning"
        <mark>*ngIf="alpaca.needsVaccination()"</mark>>
        Cet alpaga doit être vacciné !
    &lt;/div>
&lt;/li>
</code></pre>

---

## NgSwitch

Équivalent de `switch ... case`, dans un template:

<pre><code class="html" data-trim data-noescape>
&lt;li *ngFor="let alpaca of alpacas">
    &lt;div class="name">{{ alpaca.name }}&lt;/div>
    &lt;div <mark>[ngSwitch]="alpaca.ageGroup"</mark>>
        &lt;div <mark>*ngSwitchCase="CRIA"</mark>>Cria (bébé, moins de 6 mois)&lt;/div>
        &lt;div <mark>*ngSwitchCase="JUVENILE"</mark>>Juvénile (6 à 12 mois)&lt;/div>
        &lt;div <mark>*ngSwitchCase="YEARLING"</mark>>Jeune (13 à 24 mois)&lt;/div>
        &lt;div <mark>*ngSwitchCase="ADULT"</mark>>Adulte (24 mois ou plus)&lt;/div>
    &lt;/div>
&lt;/li>
</code></pre>

---

## Créer une nouvelle directive

```bash
ng generate directive directives/highlight
```

<pre><code class="typescript" data-trim data-noescape>
import { Directive } from '@angular/core';

@Directive({
  <mark>selector: '[mswHighlight]'</mark>
})
export class HighlightDirective {
  constructor() { }
}
</code></pre>

<i class="fa fa-warning warning"></i> Au même titre que les composants, la directive doit être
rajoutée au module. `angular-cli` fait ça pour nous.

Le sélecteur de directive est un sélecteur CSS (le sélecteur de composant était également un
sélecteur CSS !)

```html
<div mswHighlight></div>
```

---

## Directive attribut

<pre><code class="typescript" data-trim data-noescape>
import { Directive, ElementRef } from '@angular/core';

@Directive({
  selector: '[mswHighlight]'
})
export class HighlightDirective {
  constructor(
    <mark>el: ElementRef</mark>
  ) {
    el.nativeElement.style.backgroundColor = '#f0f6ff';
  }
}
</code></pre>

ElementRef injecte une référence vers l'élément sur lequel s'applique la directive.

À noter qu'ElementRef fonctionnerait aussi avec un composant

<i class="fa fa-warning warning"></i> Utiliser ElementRef sur un composant est généralement une
mauvaise pratique.

---

## Paramètres de directive (1)

<pre><code class="typescript" data-trim data-noescape>
import { Directive, ElementRef, Input } from '@angular/core';

@Directive({
  selector: '[mswHighlight]'
})
export class HighlightDirective {
  <mark>@Input color: string;</mark>

  constructor(
    <mark>el: ElementRef</mark>
  ) {
    el.nativeElement.style.backgroundColor = this.color;
  }
}
</code></pre>

Utilisation de la directive

```html
<div mswHighlight color="yellow"></div>
<div mswHighlight [color]="defaultColor"></div>
```

---

## Paramètres de directive (2)

<pre><code class="typescript" data-trim data-noescape>
import { Directive, ElementRef, Input } from '@angular/core';

@Directive({
  selector: '[mswHighlight]'
})
export class HighlightDirective {
  <mark>@Input mswHighlight: string;</mark>
  // Ou bien @Input('mswHighlight') color: string

  constructor(
    <mark>el: ElementRef</mark>
  ) {
    el.nativeElement.style.backgroundColor = this.mswHighlight;
  }
}
</code></pre>

Utilisation de la directive

```html
<div mswHighlight="yellow"></div>
<div [mswHighlight]="defaultColor"></div>
```

---

## Souscrire à des évènements: @HostListener

<pre><code class="typescript" data-trim data-noescape>
import { Directive, ElementRef, Input, HostListener }
from '@angular/core';

@Directive({
  selector: '[mswHighlight]'
})
export class HighlightDirective {
  @Input('mswHighlight') color: string
  constructor(private el: ElementRef) {}

  <mark>@HostListener('mouseenter')</mark> onMouseEnter() {
    this.el.nativeElement.style.backgroundColor = this.color;
  }
  <mark>@HostListener('mouseleave')</mark> onMouseLeave() {
    this.el.nativeElement.style.backgroundColor = null;
  }
}
</code></pre>

