# Routage

<figure>
    <img src="resources/routing.jpg" style="width: 16vw">
    <figcaption>
        <div class="attribution">
            By austrini -
            <a rel="nofollow" class="external free" href="http://www.flickr.com/photos/fatguyinalittlecoat/2909850055/">http://www.flickr.com/photos/fatguyinalittlecoat/2909850055/</a>,
            <a href="http://creativecommons.org/licenses/by/2.0" title="Creative Commons Attribution 2.0">CC BY 2.0</a>,
            <a href="https://commons.wikimedia.org/w/index.php?curid=10298513">Link</a>
        </div>
    </figcaption>
</figure>

---

## Généralités (1)

### Pourquoi le routage côté client ?

* Parce qu'une application web est :
  * une application
  * <span class="highlight">**web**</span>.
* Et sur le web :
  * on utilise ça : <span class="highlight"><i class="fa fa-chrome"></i> <i class="fa fa-edge"></i> <i class="fa fa-firefox"></i> <i class="fa fa-safari"></i> <i class="fa fa-opera"></i></span>
  * qui disposent de ça : <span class="highlight"><i class="fa fa-arrow-left"></i> <i class="fa fa-arrow-right"></i></span>
  * et de ça : <span class="highlight"><i class="fa fa-refresh"></i></span>
  * et qui permettent de faire ça : <span class="highlight"><i class="fa fa-bookmark-o"></i></span>
  * et ça : <span class="highlight"><i class="fa fa-share-square-o"></i> <i class="fa fa-share-alt"></i></span>
* Tout ça grâce à l'URL

---

## Généralités (2)

### *Single-page applications* et URL

Pour une application client-serveur classique, la question ne se pose pas

Comment concilier une application *single-page*, qui ne se recharge pas, avec l'URL, qui par
définition recharge la page quand elle change ?

* les âges sombres : frameset, embeds swf: l'URL est négligée
* renaissance : l'indentifiant de fragment: `#`
  * `#!` pour être `robot-friendly`
* les temps modernes : HTML5 `history.pushState`
  * nécessite un navigateur suffisamment moderne (exit IE ≤ 9)
  * nécessite un support côté serveur

---

## Installation

```bash
npm install --save @angular/router
```

(Installé d'office avec angular-cli)

<div class="filename">app.module.ts</div>

```typescript
import { RouterModule } from '@angular/router';
import { appRoutes } from './app.routes';
@NgModule({
  /* ... */
  imports: [
    RouterModule.forRoot(appRoutes), /* ... */
  ],
})
export class AppModule { }
```
`app.component.html`

```html
<router-outlet></router-outlet>
```

---

## Définition des routes

<div class="filename">app.routes.ts</div>

<pre><code class="typescript" data-trim data-noescape>
import { Routes } from '@angular/router';

export const appRoutes: Routes = [
  { path: 'about', component: ContactComponent }<span class="callback">1</span>,
  { path: 'alpaca/:id', component: AlpacaDetailComponent }<span class="callback">2</span>,
  { path: 'à-propos', redirectTo: 'about' }<span class="callback">3</span>,
  { path: '', component: AlpacaListComponent, pathMatch: 'full' }<span class="callback">4</span>,
  { path: '**', component: PageNotFoundComponent }<span class="callback">5</span>,
];
</code></pre>

1. Route simple
2. Route avec paramètres
3. Redirection
4. Chemin complet
5. Route avec *wildcard*

---

## Liens vers une route

### Lien simple

```html
<a [routerLink]="['/about']">À propos</a>
```

### Par programmation

```typescript
import { Component } from '@angular/core';
import { Router } from '@angular/router';

@Component({ /* */ })
export class AlpacaDetailComponent implements OnInit, OnDestroy {
  constructor(
    private router: Router
  ) { }

  private home() {
    this.router.navigate(['/about']);
  }
}
```

---

## Route avec paramètres

```typescript
import { Component } from '@angular/core';
import { Router, ActivatedRoute } from '@angular/router';

@Component({ /* */ })
export class AlpacaDetailComponent implements OnInit, OnDestroy {
  private alpacaId: number;

  constructor(
    private router: Router,
    private route: ActivatedRoute
  ) {
    route.params.subscribe(params => this.alpacaId = +params.id)
  }

  private home() {
    this.router.navigate(['/about']);
  }
}
```

---

## Route avec paramètres

Allons récupérer les données sur le [service HTTP](/#/http-service).

```typescript
import { Component } from '@angular/core';
import { Router, ActivatedRoute } from '@angular/router';
import 'rxjs/add/operator/mergeMap';

@Component({ /* */ })
export class AlpacaDetailComponent implements OnInit, OnDestroy {
  private Alpaca: alpaca;

  constructor(
    private router: Router,
    private route: ActivatedRoute,
    private AlpacaService: alpacaService
  ) {
    route.params
      .map(params => +params.id)
      .mergeMap(id => this.alpacaService.read(id))
      .subscribe(alpaca => this.alpaca = alpaca);
  }
}
```

---

## Gardes (1)

Sert à protéger une route : empêcher d'y entrer (page d'admin), ou empêcher d'en sortir (êtes-vous
sûr de vouloir quitter sans sauvegarder ?)

```bash
ng generate guard guards/logged-in
```

<pre><code class="typescript" data-trim data-noescape>
import { Injectable } from '@angular/core';
import { CanActivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs/Observable';

@Injectable()
export class LoggedInGuard implements CanActivate {
  <mark>constructor(private authService: AuthService) {}</mark>

  canActivate(
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable&lt;boolean> | Promise&lt;boolean> | boolean {
    <mark>return this.authService.isLoggedIn();</mark>
  }
}
</code></pre>

---

## Gardes (2)

<div class="filename">app.routes.ts</div>
<pre><code class="typescript" data-trim data-noescape>
import { Routes } from '@angular/router';

import { LoggedInGuard } from './guards/logged-in.guard';

export const appRoutes: Routes = [
  /* ... */
  {
    path: 'profile',
    component: ProfileComponent,
    <mark>canActivate: LoggedInGuard</mark>
  }
];
</code></pre>

---

## Gardes (3)

Il existe aussi canDeactivate, qui s'utilise de la même manière :

<pre><code class="typescript" data-trim data-noescape>
import { Injectable } from '@angular/core';
import { CanDeactivate, ActivatedRouteSnapshot, RouterStateSnapshot } from '@angular/router';
import { Observable } from 'rxjs/Observable';

@Injectable()
export class SavedGuard
implements <mark>CanDeactivate&lt;AlpacaDetailComponent></mark> {
  canDectivate(
    component: AlpacaDetailComponent,
    next: ActivatedRouteSnapshot,
    state: RouterStateSnapshot
  ): Observable&lt;boolean> | Promise&lt;boolean> | boolean {
    <mark>return component.areFormsSaved()</mark>
  }
}
</code></pre>

---

## Routes enfant (1)

```typescript
export const appRoutes: Routes = [
  {
    path: 'alpaca/:id',
    component: AlpacaDetailComponent,
    children: [
      { path: 'detail-1', component: AlpacaDetail1Component },
      { path: 'detail-2', component: AlpacaDetail2Component },
    ]
  },
];
```

Rajouter dans le composant `AlpacaDetailComponent`

```html
<a href="#" routerLink="detail-1">Composant 1</a>
<a href="#" routerLink="detail-2">Composant 2</a>

<router-outlet></router-outlet>
```

---

## Routes enfant (2)

Si on se trouve sur la route `/alpaca/23`

<pre><code class="html" data-trim data-noescape>
&lt;a href="#" routerLink="detail-1">Composant 1&lt;/a>
&lt;!-- => /alpaca/23/detail-1 -->

&lt;a href="#" routerLink="<mark>./</mark>detail-1">Composant 1&lt;/a>
&lt;!-- => /alpaca/23/detail-1 -->

&lt;a href="#" routerLink="<mark>/</mark>detail-1">Composant 1&lt;/a>
&lt;!-- => /detail-1 -->

&lt;a href="#" routerLink="<mark>../</mark>4/detail-1">Composant 1&lt;/a>
&lt;!-- => /alpaca/4/detail-1 -->
</code></pre>
