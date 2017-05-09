# Redux & @ngrx

<img src="resources/reactivex.svg" class="plain" style="width: 25vw; display: block; margin: 0 auto">

---

## Pourquoi ?

<div class="block">
    <p class="block-header">Composant
    <div class="block-content">
        <p>Entrée de données
        <p>Affichage des données
        <p>Stockage de l'état du composant
    </div>
</div>

<div class="block">
    <p class="block-header">Service
    <div class="block-content">
        <p>Stockage de l'état de l'application
        <p>Logique métier
        <p>IO / Communications avec la source de données
    </div>
</div>

---

## Pourquoi ?

<div class="row">
    <div class="col-4">
        <div class="block">
            <p class="block-header">Composant 1
            <div class="block-content">
                <p>Input
                <p>Output
                <p>État
            </div>
        </div>
    </div>
    <div class="col-4">
        <div class="block">
            <p class="block-header">Composant 2
            <div class="block-content">
                <p>Input
                <p>Output
                <p>État
            </div>
        </div>
    </div>
    <div class="col-4">
        <div class="block">
            <p class="block-header">Composant 3
            <div class="block-content">
                <p>Input
                <p>Output
                <p>État
            </div>
        </div>
    </div>
</div>

<div class="row">
    <div class="col-4">
        <div class="block">
            <p class="block-header">Service 1
            <div class="block-content">
                <p>État
                <p>Logique
                <p>IO
            </div>
        </div>
    </div>
    <div class="col-4">
        <div class="block">
            <p class="block-header">Service 2
            <div class="block-content">
                <p>État
                <p>Logique
                <p>IO
            </div>
        </div>
    </div>
    <div class="col-4">
        <div class="block">
            <p class="block-header">Service 3
            <div class="block-content">
                <p>État
                <p>Logique
                <p>IO
            </div>
        </div>
    </div>
</div>

---

## Pattern Redux

<div class="row">
    <div class="col-4">
    </div>
    <div class="col-4">
        <div class="block" style="height: 6em;">
            <p class="block-header">Composants
            <div class="block-content">
            </div>
        </div>
    </div>
    <div class="col-4">
    </div>
</div>
<div class="row">
    <div class="col-4">
        <div class="block" style="height: 6em;">
            <p class="block-header">Reducers
            <div class="block-content">
                Fonctions pures (state, action) => newState
            </div>
        </div>
    </div>
    <div class="col-4">
        <div class="block" style="height: 6em;">
            <p class="block-header">Store
            <div class="block-content">
                (Unique)
            </div>
        </div>
    </div>
    <div class="col-4">
            <div class="block" style="height: 6em;">
            <p class="block-header">Créateurs d'actions
            <div class="block-content">
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-4">
    </div>
    <div class="col-4">
    </div>
    <div class="col-4">
    </div>
</div>

---

## Pattern Redux (2)

### L'état de l'application

* Unique : une seule hiérarchie qui contient l'état de toute l'application
* Immutable
  * on peut créer un état à t<sub>n+1</sub> à partir de l'état à t<sub>n</sub>
  * mais il est impossible de modifier un état existant

Exemple :

```typescript
{
    alpacaList: {
        list: Alpaca[],
        loading: boolean,
        loadingError: any
    },
    loggedUser: {
        login: string,
        name: string
    },
    router: RouterState
}
```

---

## Pattern Redux (3)

### Le store

* Stocke l'état de l'application
* Dispatche les actions
* Est observable, et permet d'obtenir un observable des enfants de l'état

### Les actions

* Structures simples, immutables
  * Type
  * Payload

### Les reducers

* Un reducer racine, composé généralement d'une hiérarchie de reducers
* Fonctions pures : (state, action) => newState
  * Ne dépend que de ses entrées
  * Ne modifie ni ses entrées, ni aucune autre variable autre
  * Renvoie toujours la même valeur si elle est appelée avec les mêmes entrées

---

## Installation

```
npm install --save @ngrx/core @ngrx/effects @ngrx/store @ngrx/store-devtools
```

---

## État de l'application

<div class="filename">alpaca-list.interface.ts</div>

```typescript
import { Alpaca } from './alpaca.interface';
export interface AlpacaList {
  readonly loading: boolean;
  readonly loadingError: any;
  readonly alpacas: Alpaca[];
}
```

<div class="filename">app-state.interface.ts</div>

```typescript
import { AlpacaList } from './alpaca/alpaca-list.interface';

export interface AppState {
  readonly alpacaList: AlpacaList;
}
```

---

## Créateur d'actions

<div class="filename">alpaca-list.store.ts</div>

```typescript
import { Injectable } from '@angular/core';
import { Store } from '@ngrx/store';
import { Alpaca } from './alpaca.interface';
import { AppState } from '../app.state';
import { createAction } from '../action';

@Injectable()
export class AlpacaStore {
  public static CREATE = 'ALPACA_CREATE';
  public static UPDATE = 'ALPACA_UPDATE';

  constructor(private store: Store<AppState>) {}

  public create(alpaca: Alpaca) {
    this.store.dispatch(createAction(AlpacaStore.CREATE, { alpaca }));
  }
  public update(id: number, alpaca: Alpaca) {
    this.store.dispatch(createAction(AlpacaStore.UPDATE, { id, alpaca }));
  }
}
```

---

## Helpers pour observer le store

<div class="filename">alpaca-list.store.ts</div>

```typescript
@Injectable()
export class AlpacaStore {
  constructor(
    private store: Store<AppState>
  ) {}

  /* ... */

  public getAlpacas(): Observable<Alpaca[]> {
    return this.store.select(appState => appState.alpacaList.alpacas);
  }

  public getAlpaca(id: number): Observable<Alpaca> {
    return this.getAlpacas().map(alpacaList => alpacaList.find(alpaca => alpaca.id === id));
  }
}
```


---

<div class="filename">alpaca-list.reducer.ts</div>

```typescript
export function alpacaListReducer(
  state: AlpacaList, action: Action
): AlpacaList {
  state = state || createDefaultAlpacaList();
  switch (action.type) {
    case AlpacaStore.CREATE:
      return {
        ...state,
        alpacas: state.alpacas.concat(
            new Alpaca(action.payload.alpaca)
        )
      };
    case AlpacaStore.UPDATE:
      return {
        ...state,
        alpacas: state.alpacas.map(
            a => a.id === action.payload.id
                ? new Alpaca(action.payload.alpaca)
                : a
        )
      };
    default: return state;
  }
}
```

---

## Reducer (2)

<div class="filename">root.reducer.ts</div>

```typescript
import { alpacaListReducer } from './alpaca/index'

export const rootReducer = {
  alpacaList: alpacaListReducer
};

```

---

## Déclaration dans le module

<div class="filename">app.module.ts</div>

```typescript
@NgModule({
  declarations: [
  ],
  imports: [
    StoreModule.provideStore(rootReducer),
    StoreDevtoolsModule.instrumentOnlyWithExtension(),
  ],
  providers: [
    AlpacaService,
    AlpacaStore
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

---

## Effets

Dans cette architecture, les reducers sont des fonctions pures.

Exemples d'actions :

* Ajouter un objet au panier
* Supprimer un objet du panier
* Passer commande

La plupart de ces actions nécessitent des actions non-pures.

* Ajouter un objet au panier
* Objet ajouté au panier
* Erreur lors de l'ajout d'un objet au panier

---

## Effets

<div class="row">
    <div class="col-4">
    </div>
    <div class="col-4">
        <div class="block" style="height: 6em;">
            <p class="block-header">Composants
            <div class="block-content">
            </div>
        </div>
    </div>
    <div class="col-4">
    </div>
</div>
<div class="row">
    <div class="col-4">
        <div class="block" style="height: 6em;">
            <p class="block-header">Reducers
            <div class="block-content">
                Fonctions pures (state, action) => newState
            </div>
        </div>
    </div>
    <div class="col-4">
        <div class="block" style="height: 6em;">
            <p class="block-header">Store
            <div class="block-content">
                (Unique)
            </div>
        </div>
    </div>
    <div class="col-4">
            <div class="block" style="height: 6em;">
            <p class="block-header">Créateurs d'actions
            <div class="block-content">
            </div>
        </div>
    </div>
</div>
<div class="row">
    <div class="col-4">
    </div>
    <div class="col-4">
        <div class="block" style="height: 6em;">
            <p class="block-header">Effets
            <div class="block-content">
                Observable&lt;Action>
            </div>
        </div>
    </div>
    <div class="col-4">
    </div>
</div>

---

## Effets (1)

<div class="filename">alpaca-list.effecs.ts</div>

```typescript
import { Actions, Effect } from '@ngrx/effects';

@Injectable()
export class AlpacaListEffects {
  @Effect()
  retrieve$ = this.actions$
    .ofType(AlpacaStore.RETRIEVE)
    .mergeMap<Action, Action>(() => this.alpacaService.read()
      .map(alpacas =>
        createAction(AlpacaStore.RETRIEVE_SUCCESS, { alpacas }))
      .catch(error =>
        Observable.of(createAction( AlpacaStore.ERROR, { error })))
    );

  constructor(
    private actions$: Actions,
    private alpacaService: AlpacaService
  ) {}
}
```

---

## Effets (2)

<div class="filename">app.module.ts</div>

```typescript
@NgModule({
  /* ... */
  imports: [
    EffectsModule.run(AlpacaListEffects)
  ],
  providers: [
  ],
})
export class AppModule { }

```
