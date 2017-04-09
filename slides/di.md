## Injection de dépendances (Dependency Injection)

### Pourquoi faire ?

```typescript
class Alpaca {
    public name: string;
    public vaccinationDates: Date[];
}

/** Gère la communication avec le Webservice AlpacaNet */
class AlpacaRepository {
    constructor(
        private apiUrl: string,
        private retryDelayMs: number
    ) {}

    getAlpacaList() : Promise<Alpaca[]> {
        // Fait des appels HTTP au Webservice ici
    }
}
```

---

## Injection de dépendances

<pre><code class="typescript" data-noescape>
class AlpacaVaccinationCheckerService {
    private alpacaRepository: AlpacaRepository;

<div class="fragment">    constructor(apiUrl: string, retryDelayMs: number) {
        this.alpacaRepository = 
            new AlpacaRepository(apiUrl, retryDelayMs);
    }</div>
    getAlpacasNotVaccinatedAfter(limitDate: Date) : Promise&lt; Alpaca[]> {
        return this.alpacaRepository.getAlpacaList()
            .then(list => list.filter(
                alpaca => alpaca.vaccinationDates
                    .filter(d => d > limitDate)
                    .length !== 0
            ))
    }
}
</code></pre>

---

## Injection de dépendances

```typescript
class AlpacaVaccinationCheckerService {
    constructor(
        private alpacaRepository: AlpacaRepository;
    ) {}

    getAlpacasNotVaccinatedAfter(limitDate: Date) : Promise&lt; Alpaca[]> {
        return this.alpacaRepository.getAlpacaList()
            .then(list => list.filter(
                alpaca => alpaca.vaccinationDates
                    .filter(d => d > limitDate)
                    .length !== 0
            ))
    }
}
```

Charge au code appelant de fournir l'implémentation d'`AlpacaRepository`

---

## Injection de dépendances

Implémentation naïve d'un conteneur DI

```typescript
class ServiceContainer {
    private container: Map<any, any> = new Map();

    private register(key: any, value: any) {
        this.container.set(key, value);
    }

    private get<T>(key: any) : T {
        return <T>(this.container.get(key));
    }
}

const serviceContainer = new ServiceContainer();

// Dans la configuration de l'application
serviceContainer.register(
    AlpacaRepository, 
    new AlpacaRepository('https://alpacaweb.info', 300)
);
```

---

## Injection de dépendances

Le container centralise toutes les dépendances.

```typescript
class AlpacaVaccinationCheckerService {
    alpacaRepository: AlpacaRepostory = serviceContainer.get(AlpacaRepository),

    constructor() {}

    getAlpacasNotVaccinatedAfter(limitDate: Date) : Promise&lt; Alpaca[]> {
        return this.alpacaRepository.getAlpacaList()
            .then(list => list.filter(
                alpaca => alpaca.vaccinationDates
                    .filter(d => d > limitDate)
                    .length !== 0
            ))
    }
}
```

---

## Injection de dépendances

Le framework s'occupe d'injecter automatiquement les dépendances en se basant sur le type des paramètres du constructeur.

```typescript
class AlpacaVaccinationCheckerService {
    // Le framework injecte l'AlpacaRepository
    constructor(
        private alpacaRepository: AlpacaRepository 
    ) {}

    getAlpacasNotVaccinatedAfter(limitDate: Date) : Promise&lt; Alpaca[]> {
        return this.alpacaRepository.getAlpacaList()
            .then(list => list.filter(
                alpaca => alpaca.vaccinationDates
                    .filter(d => d > limitDate)
                    .length !== 0
            ))
    }
}
```
