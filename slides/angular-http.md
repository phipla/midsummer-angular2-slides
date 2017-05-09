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

## Requêtes

```typescript
  public readAll(): Observable<Alpaca[]> {
    return this.http.get(this.url)
      .map(response => response.json().map(data = new Alpaca(data)));
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
