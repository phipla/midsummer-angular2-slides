# HTTP

---

## Installation

```bash
npm install --save @angular/http
```

(Installé d'office avec angular-cli)

* * *

```typescript
import { HttpModule } from '@angular/http';

@NgModule({
    declarations: [ /* ... */ ],
    imports: [
        /* ... */
        HttpModule,
        /* ... */
    ],
    providers: [ /* ... */ ],
    bootstrap: [ AppComponent ]
})
export class AppModule { }
```

---

```bash
npm install --save json-server
```

<div class="filename">package.json</div>

```json
{
    "scripts": {
        "start": "json-server --watch db/db.json & ng serve"
    }
}
```

<div class="filename">db/db.json</div>

```json
{
  "alpacas": [
    {
      "id": 1,
      "name": "Roberto",
      "notes": ""
    },
    {
      "id": 2,
      "name": "Monica",
      "notes": ""
    }
  ]
}
```

---

## Requêtes

```typescript
import { Injectable } from '@angular/core';
import { Http } from '@angular/http';
import 'rxjs/add/operator/map';
import { Observable } from 'rxjs/Observable';
import { Alpaca } from './alpaca.interface';

@Injectable()
export class AlpacaService {
  private readonly url = 'http://localhost:3000/alpacas';

  constructor(
    private http: Http
  ) {
  }

  /* */
}
```

---

<!-- .slide: id="http-service" -->

```typescript
  public readAll(): Observable<Alpaca[]> {
    return this.http.get(this.url)
      .map(response => response.json().map(data => new Alpaca(data)));
  }

  public read(id: number): Observable<Alpaca> {
    return this.http.get(`${this.url}/${id}`)
      .map(response => new Alpaca(response.json()));
  }

  public create(alpaca: Alpaca): Observable<Alpaca>  {
    return this.http.post(this.url, alpaca)
      .map(response => new Alpaca(response.json()));
  }

  public update(id: number, alpaca: Alpaca): Observable<Alpaca> {
    return this.http.put(`${this.url}/${id}`, alpaca)
      .map(response => new Alpaca(response.json()));
  }

  public delete(id: number) {
    return this.http.delete(`${this.url}/${id}`)
      .map(response => new Alpaca(response.json()));
  }
```

---

## Methodes HTTP

```typescript
class Http {
    request(url: string|Request, options?: RequestOptionsArgs)
        : Observable<Response>
    get(url: string, options?: RequestOptionsArgs)
        : Observable<Response>
    post(url: string, body: any, options?: RequestOptionsArgs)
        : Observable<Response>
    put(url: string, body: any, options?: RequestOptionsArgs)
        : Observable<Response>
    delete(url: string, options?: RequestOptionsArgs)
        : Observable<Response>
    patch(url: string, body: any, options?: RequestOptionsArgs)
        : Observable<Response>
    head(url: string, options?: RequestOptionsArgs)
        : Observable<Response>
    options(url: string, options?: RequestOptionsArgs)
        : Observable<Response>
}
```
