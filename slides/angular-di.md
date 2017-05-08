# Injection de dépendances

---

## `@Injectable` (1)

### Définition d'un composant injectable

```typescript
import { Injectable } from '@angular/core';

@Injectable()
export class AlpacaService {
  constructor() { }
}
```

### Déclaration d'un composant injectable dans un module

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from './model/alpaca.service';

@NgModule({
  declarations: [ /* ... */ ],
  imports: [ /* ... */ ],
  providers: [
    <mark>AlpacaService,</mark>
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
</code></pre>

---

## `@Injectable` (2)

### Injection

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from '../../model/alpaca.service';

@Component({ /* ... */ })
export class AlpacaListComponent implements OnInit {
  constructor(
    <mark>private alpacaService: AlpacaService</mark>
  ) {
  }
}
</code></pre>

---

## Injection de primitives (1)

Chaque injectable dispose :

* d'une clé qui permet de le référencer
* d'une valeur

Lorsqu'on injecte une classe :

* la clé est la classe
* la valeur est un singleton, instancié par Angular

---

## Injection de primitives (2)

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from './model/alpaca.service';

@NgModule({
  /* ... */
  providers: [
    <mark>AlpacaService</mark>
  ],
  /* ... */
})
export class AppModule { }
</code></pre>

* * *

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from './model/alpaca.service';

@NgModule({
  /* ... */
  providers: [
    <mark>{ provide: AlpacaService, useClass: AlpacaService }</mark>
  ],
  /* ... */
})
export class AppModule { }
</code></pre>

---

## Injection de primitives (3)

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from '../../model/alpaca.service';

@Component({ /* ... */ })
export class AlpacaListComponent implements OnInit {
  constructor(
    private alpacaService: AlpacaService
  ) {
  }
}
</code></pre>

* * *

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from '../../model/alpaca.service';

@Component({ /* ... */ })
export class AlpacaListComponent implements OnInit {
  constructor(
    <mark>@Inject(AlpacaService)</mark> private alpacaService: AlpacaService
  ) {
  }
}
</code></pre>

---

## Injection de primitives (4)

<pre><code class="typescript" data-trim data-noescape>
@NgModule({
  /* ... */
  providers: [
    { provide: 'CONFIG', useValue: { MAX_FARM_SIZE: 100 } }
  ],
  /* ... */
})
export class AppModule { }
</code></pre>

* * *

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from '../../model/alpaca.service';

@Component({ /* ... */ })
export class AlpacaListComponent implements OnInit {
  constructor(
    @Inject('CONFIG') private config
  ) {
  }
}
</code></pre>

---

## Éviter les collisions de clé: `OpaqueToken`

<pre><code class="typescript" data-trim data-noescape>
import OpaqueToken from '@angular/core';

export const ALPACA_CONFIG_TOKEN = new OpaqueToken('alpaca-config');

export const alpacaConfig = {
    MAX_FARM_SIZE: 100
}
</code></pre>

---

## Utilisation d'`OpaqueToken`

<pre><code class="typescript" data-trim data-noescape>
import { alpacaConfig, ALPACA_CONFIG_TOKEN } from './alpaca-config';

@NgModule({
  /* ... */
  providers: [
    <mark>{ provide: ALPACA_CONFIG_TOKEN, useValue: alpacaConfig }</mark>
  ],
  /* ... */
})
export class AppModule { }
</code></pre>

* * *

<pre><code class="typescript" data-trim data-noescape>
import { ALPACA_CONFIG_TOKEN } from './alpaca-config';

@Component({ /* ... */ })
export class AlpacaListComponent implements OnInit {
  constructor(
    <mark>@Inject(ALPACA_CONFIG_TOKEN)</mark> private alpacaConfig
  ) {
  }
}
</code></pre>

---

## Injection par factory

On a vu: useClass, useValue

<pre><code class="typescript" data-trim data-noescape>
import { Http } from '@angular/http';

export let alpacaServiceFactory =
    (http: Http) => new AlpacaService(http);
</code></pre>

* * *

<pre><code class="typescript" data-trim data-noescape>
import { AlpacaService } from './model/alpaca.service';

@NgModule({
  /* ... */
  providers: [
    <mark>{ provide: AlpacaService, useFactory: alpacaServiceFactory }</mark>
  ],
  /* ... */
})
export class AppModule { }
</code></pre>
