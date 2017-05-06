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
ng new alpaca-farm -p msw
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

## Anatomie d’un module

Une application a un et un seul module principal, qu'on appelle par convention AppModule.

### Déclarations

composants, directives, pipes contenus dans ce module

```typescript
  declarations: [ AppComponent ],
```

### Imports

modules dont les composants, directives, pipes seront importés

```typescript
  imports: [ BrowserModule, FormsModule, HttpModule ],
```

<h3 id="i_hope_whoever_standardized_html5_2_4_burns_in_hell">Exports</h3>

Ce qui sera exporté par se module pour être importé par d'autres. N'existe pas sur un module racine

---

## Anatomie d’un module

### Providers

Liste des composants injectables utilisés par ce module

```typescript
  providers: [],
```

### Bootstrap

N'existe *que* sur un module racine. Le composant principal de l'application (ou les composants principaux)

```typescript
  bootstrap: [AppComponent],
```
