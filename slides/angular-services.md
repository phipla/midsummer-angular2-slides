# Services

---

## Architecture des données (1)

<div class="block nostyle">
    <p class="block-header fragment" data-fragment-index="6">Composant
    <div class="block-content">
        <p class="fragment" data-fragment-index="1">Éléments d'interface utilisateur
        <p class="fragment" data-fragment-index="2">Pages d'interface utilisateur
        <p class="fragment" data-fragment-index="3">Logique métier
        <p class="fragment" data-fragment-index="4">Stockage de l'état de l'application
        <p class="fragment" data-fragment-index="5">IO / Communications avec la source de données
    </div>
</div>

---

## Architecture des données (2)

<figure>
    <img src="resources/spaghetti.jpeg" style="width: 35vw">
    <figcaption>
        (Une illustration)
        <div class="attribution">
            By <a href="//commons.wikimedia.org/wiki/User:Dr.Conati" title="User:Dr.Conati">Dr.Conati</a> -
            <span class="int-own-work" lang="en" xml:lang="en">Own work</span>,
            Public Domain,
            <a href="https://commons.wikimedia.org/w/index.php?curid=4360376">Link</a>
        </div>
    </figcaption>
</figure>

---

## Architecture des données (3)

<div class="block">
    <p class="block-header">Composant de présentation
    <div class="block-content">
        <p>Éléments d'interface utilisateur
    </div>
</div>

<div class="block">
    <p class="block-header">Composant intelligent
    <div class="block-content">
        <p>Pages d'interface utilisateur
        <p>Logique métier
        <p>Stockage de l'état de l'application
        <p>IO / Communications avec la source de données
    </div>
</div>

---

## Architecture des données (4)

<div class="block">
    <p class="block-header">Composant de présentation
    <div class="block-content">
        <p>Éléments d'interface utilisateur
    </div>
</div>

<div class="block">
    <p class="block-header">Composant intelligent
    <div class="block-content">
        <p>Pages d'interface utilisateur
    </div>
</div>

<div class="block">
    <p class="block-header">Service
    <div class="block-content">
        <p>Logique métier
        <p>Stockage de l'état de l'application
        <p>IO / Communications avec la source de données
    </div>
</div>

---

## Création d'un service

```bash
$ ng generate service model/alpaca
WARNING Service is generated but not provided, it must be provided to be used
```

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class AlpacaService {
  constructor() { }
}
```

```typescript
import { AlpacaService } from './model/alpaca.service';

@NgModule({
  declarations: [ /* ... */ ],
  imports: [ /* ... */ ],
  providers: [
    AlpacaService,
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

## Service

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class AlpacaService {
  alpacas: Alpaca[] = [];

  constructor() { }

  getAlpacas() {
    return this.alpacas;
  }

  addAlpaca(alpaca: Alpaca) {
    this.alpacas.push(alpaca);
  }

  updateAlpaca(index: number, newAlpaca: Alpaca) {
    this.alpacas[index] = newAlpaca;
  }
}
```
