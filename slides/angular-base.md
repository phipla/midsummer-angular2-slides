## Angular

---

## Présentation du projet

---

## Mise en place

### angular-cli

Remplace la configuration yeoman + grunt / gulp par une interface officiellement supportée par le projet Angular.

### Création du projet

Installation de ng-cli

```bash
npm install -g @angular/cli
```

Création du projet

```bash
ng new alpaca-farm
```

---

## Modules

```typescript
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FormsModule } from '@angular/forms';
import { HttpModule } from '@angular/http';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [
    AppComponent
  ],
  imports: [
    BrowserModule,
    FormsModule,
    HttpModule
  ],
  providers: [],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

## Modules

Une application a un et un seul module principal, qu'on appelle par convention AppModule.

---

## Composants

`app.component.ts` 
```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  title = 'app works!';
}
```
`app.component.html` 
```html
<h1>
  {{title}}
</h1>
```

---

## Saisie de données

`app.component.html` 

```html
<h1>{{title}}</h1>
<input #name>
<button (click)="addAlpaca(name)">Nouvel alpaga</button>
```

`app.component.ts` 

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
    console.log(`Bienvenue, ${name}`);
  }
}
```

---

## Affichage des données

```html
<ul>
  <li *ngFor="let alpaca of alpacas">
    {{ alpaca.name }}
  </li>
</ul>
```

