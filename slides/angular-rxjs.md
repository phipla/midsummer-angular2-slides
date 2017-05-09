# ReactiveX

<img src="resources/reactivex.svg" class="plain" style="width: 25vw; display: block; margin: 0 auto">

---

## ReactiveX (1)
### Qu'est-ce que c'est ?

http://reactivex.io/

* Microsoft Labs
* Open-source
* Implémentation de la pattern Observable pour Java, JavaScript, C#, Scala, Clojure, C++, Lua, Ruby, Python, Go, Groovy, Kotlin, Swift, PHP ...

### Pattern Observable

En cours de standardisation dans ES7

Canal de communication continu, sur lequel plusieurs valeurs peuvent être émises au cours du temps.

---

## Ressources

* http://rxmarbles.com/
* http://reactivex.io/rxjs/
* https://www.learnrxjs.io

---

## ReactiveX (2)

Cycle de vie d'un observable:

1. créé,
2. valeurs émises,
3. clôturé.

Sur un observable peuvent s'appliquer des opérateurs fonctionnels :

<rx-marbles key="map"></rx-marbles>

---

## ReactiveX (2)

<rx-marbles key="map"></rx-marbles>

```typescript
import 'rxjs/Rx';
import { Observable } from 'rxjs/Observable';

let opMap = Observable.from([1, 2, 3])
    .map(x => 10 * x)
    .subscribe(
        a => console.log(`Emitted: ${a}`),
        a => console.log(`Error: ${a}`),
        () => console.log('Closed')
    )
```

---

## ReactiveX (3)

<rx-marbles key="reduce"></rx-marbles>

```typescript
let opReduce = Observable.from([1, 2, 3, 4, 5])
    .reduce((x, y) => x + y)
    .subscribe(
        a => console.log(`Emitted: ${a}`),
        a => console.log(`Error: ${a}`),
        () => console.log('Closed')
    )
```

---

## ReactiveX (4)

<rx-marbles key="filter"></rx-marbles>

```typescript
let opFilter = Observable.from([2, 30, 22, 5, 60, 1])
    .filter(x => x > 10)
    .subscribe(
        a => console.log(`Emitted: ${a}`),
        a => console.log(`Error: ${a}`),
        () => console.log('Closed')
    )
```

---

## ReactiveX (5)

L'aspect "temps" rentre également en compte :

<rx-marbles key="debounce"></rx-marbles>

---

## ReactiveX (6)

Il est également possible de combiner des Observables :

<rx-marbles key="merge"></rx-marbles>

---

## ReactiveX (7)

Il est également possible de combiner des Observables :

<rx-marbles key="combineLatest"></rx-marbles>

---

## `mergeMap`

<img src="resources/mergeMap.png" class="plain" style="width: 25vw; float: right; margin-left: 2em">

Un Observable qui émet des nombres

Une fonction "map" qui prend un nombre en entrée, et qui retourne un Observable

Ledit Observable ré-émet 3 fois le nombre * 10

* * *

```typescript
let opMergeMap = new Observable(observer => {
    setTimeout(() => observer.next(1), 1000)
    setTimeout(() => observer.next(3), 2000)
    setTimeout(() => observer.next(5), 2500)
    setTimeout(() => observer.complete(), 3000)
})

let mapFunc = (i: number)
    => Observable.interval(250).take(3).map(() => 10 * i)
opMergeMap.mergeMap(mapFunc).subscribe(/* ... */)
```

---

## `switchMap`

<img src="resources/switchMap.png" class="plain" style="width: 25vw; float: right; margin-left: 2em">

Un Observable qui émet des nombres

Une fonction "map" qui prend un nombre en entrée, et qui retourne un Observable

Ledit Observable ré-émet 3 fois le nombre * 10

* * *

```typescript
let opSwitchMap = new Observable(observer => {
    setTimeout(() => observer.next(1), 1000)
    setTimeout(() => observer.next(3), 2000)
    setTimeout(() => observer.next(5), 2500)
    setTimeout(() => observer.complete(), 3000)
})

opMergeMap.switchMap(mapFunc).subscribe(/* ... */)
```

---

## Souscrire à un observable

```typescript
let subscription = observable.subscribe(observer);
let subscription = observable.subscribe(onNext, onError, onCompleted);
```

### Observer

```typescript
interface Observer<T> {
  closed?: boolean;
  next: (value: T) => void;
  error: (err: any) => void;
  complete: () => void;
}
```

### Annuler une souscription

```typescript
subscription.unsubscribe();
```

### Gestion des erreurs

```typescript
observable.catch(handler)
```

---

## Créer un observable

```typescript
const myObserver = {
    next(a) { console.log(`Emitted: ${a}`) },
    error(a) { console.log(`Error: ${a}`) },
    complete() { console.log('Complete') },
}
```

### Observable.of

```typescript
s = Observable.of(2, 4, 5).subscribe(myObserver);
```

### Observable.from

Génère un observable à partir d'un tableau, d'une promesse, ou d'un autre observable

```typescript
s = Observable.from([2, 4, 5]).subscribe(myObserver);
s = Observable.from(Promise.resolve(4)).subscribe(myObserver);
```

---

## Créer un observable (2)

### `new Observable`

```typescript
(new Observable(observer => {
    setTimeout(() => observer.next(10), 1000)
    setTimeout(() => observer.next(20), 2000)
    setTimeout(() => observer.complete(), 3000)
})).subscribe(myObserver);
```

### Autres

```typescript
Observable.throw(error);
Observable.never();
Observable.range(start, count);
Observable.timer(initialDelay, period);
```

---

## Observables froids / chauds

### Observable chaud

Émet des valeurs, indépendamment du fait qu'il ait ou non des souscriptions

<div style="text-align: center">
    <i class="fa fa-4x fa-tv"></i>
</div>

### Observable froid

Émet ses valeurs pour chaque souscription

<div style="text-align: center">
    <i class="fa fa-4x fa-youtube"></i>
</div>

---

## Exemples

### Exemple d'observable froid

```typescript
let coldObservable = (new Observable(observer => {
    setTimeout(() => observer.next(10), 1000)
    setTimeout(() => observer.next(20), 2000)
    setTimeout(() => observer.next(30), 3000)
    setTimeout(() => observer.complete(), 4000)
}));

coldObservable.subscribe(console.log)
coldObservable.subscribe(console.log)
```

### Transformer un observable froid en observable chaud

```typescript
let connectableObservable = coldObservable.publish();
connectableObservable.subscribe(a => console.log(`1: ${a}`))
setTimeout(
    () => connectableObservable.subscribe(a => console.log(`2: ${a}`)),
    2500);
connectableObservable.connect();
```

---

## Sujets

### Subject

À la fois un `Observable` et un `Observer`

### BehaviorSubject

