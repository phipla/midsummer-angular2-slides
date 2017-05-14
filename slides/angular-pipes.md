# PIPES

<div class="pipe-logo"></div>

---

## À quoi ça sert ?

<div class="filename">alpaca.component.html</div>

```html
<div class="clearfix">
  <div class="content">
    <div class="name">{{ name }}</div>
  </div>
</div>
```

<pre><code class="html" data-trim data-noescape>
&lt;div class="clearfix">
  &lt;div class="content">
    &lt;div class="name">{{ name <mark>| capitalizeFirst</mark> }}&lt;/div>
  &lt;/div>
&lt;/div>
</code></pre>

---

## Génération d'un pipe

```bash
ng generate pipe pipes/capitalize-first
```

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'capitalizeFirst'
})
export class CapitalizeFirstPipe implements PipeTransform {
  transform(value: string): string {
    return value[0].toUpperCase() + value.substr(1);
  }
}
```

---

## Pipe avec paramètres

```bash
ng generate pipe pipes/zero-padding
```

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'zeroPadding'
})
export class ZeroPaddingPipe implements PipeTransform {
  transform(value: number, digits: number): string {
    return ("0".repeat(digits) + value).substr(-digits);
  }
}
```

<pre><code class="html" data-trim data-noescape>
&lt;div>
    {{ 13 <mark>| zeroPadding:6</mark> }}
&lt;/div>
</code></pre>

Résultat :

<pre><code class="html" data-trim data-noescape>
    000013
</code></pre>

---

## Pipe pur / impur (1)

```typescript
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'count'
})
export class CountPipe implements PipeTransform {
  transform(value: Array<any>): number {
    return value.length;
  }
}
```

```html
<button (click)="test.push(test.length)">Add</button>
{{ test | json }}
{{ test | count }}
```
```typescript````
test = [ 0, 1 ];
```

<i class="fa fa-warning warning"></i> Count ne change pas !

---

## Pipe pur / impur (2)

* Un pipe pur n'est exécuté que quand la référence de valeur d'entrée (ou ses paramètres) changent
* La référence vers un `Object` (ou `Array`) ne change pas quand son contenu change !

### Pipe impur

<pre><code class="typescript" data-trim data-noescape>
import { Pipe, PipeTransform } from '@angular/core';

@Pipe({
  name: 'count',
  <mark>pure: false</mark>
})
export class CountPipe implements PipeTransform {
  transform(value: Array<any>): number {
    return value.length;
  }
}
</code></pre>

* Un pipe impur est ré-évalué à chaque cycle
* Fonctionne, mais bien moins bonnes performances

---

## Pipes par défaut d'Angular

Entre autres:

* DatePipe
* UpperCasePipe
* LowerCasePipe
* CurrencyPipe
* PercentPipe
* AsyncPipe (transforme une Promise ou un Observable en sa valeur)
* JsonPipe (utile pour le debug)
