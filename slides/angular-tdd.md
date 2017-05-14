# Test-driven development

<figure>
    <img src="resources/fixture.jpg" style="width: 25vw">
    <figcaption>
        <div class="attribution">
            By Gamestarturn (Own work) [<a href="http://creativecommons.org/licenses/by-sa/4.0">CC BY-SA 4.0</a>], <a href="https://commons.wikimedia.org/wiki/File%3AKES-400A_KEM-400AAA_Test_fixture.JPG">via Wikimedia Commons</a>
        </div>
    </figcaption>
</figure>

---

## TDD ?

### Objectifs

* Éviter les bugs
* Qualité du design

### Méthodologie

* Orienté test
* Test d'abord, code ensuite

### Types de test

* Test unitaire
* Test d'intégration
* Test end-to-end

---

## Tooling

<img style="float: left; width: 40vw;" class="plain" src="resources/karma.svg">
<img style="float: right; width: 18vw" class="plain" src="resources/jasmine.svg">
<img style="margin: 0 auto; display: block; width: 12vw" class="plain" src="resources/protractor.svg">

---

## Tester un Pipe

(ou n'importe quelle classe simple)

```typescript
describe('CapitalizeFirstPipe', () => {
  it('create an instance', () => {
    const pipe = new CapitalizeFirstPipe();
    expect(pipe).toBeTruthy();
  });

  it('capitalizes a string', () => {
    const pipe = new CapitalizeFirstPipe();
    expect(pipe.transform('test')).toBe('Test');
  });

  it('returns an empty string when called with a null', () => {
    const pipe = new CapitalizeFirstPipe();
    expect(pipe.transform(null)).toBe('');
  });
});
```

---

## Tester un composant (1)

### Tester des inputs

```typescript
describe('AlpacaComponent', () => {
  let component: AlpacaComponent;
  let fixture: ComponentFixture<AlpacaComponent>;

  beforeEach(async(() => {
    TestBed.configureTestingModule({
      declarations: [ AlpacaComponent, CapitalizeFirstPipe, HighlightDirective ]
    })
    .compileComponents();
  }));

  beforeEach(() => {
    fixture = TestBed.createComponent(AlpacaComponent);
    component = fixture.componentInstance;
    component.alpacaId = 12;
    component.alpaca = new Alpaca('test name');
    fixture.detectChanges();
  });
});
```

---

## Tester un composant (2)

### Tester des inputs (suite)

```typescript
describe('AlpacaComponent', () => {
  /* ... */
  it('should display the alpaca\'s name', () => {
    expect(fixture.nativeElement.querySelector('.alpaca').innerHTML).toContain('Test name');
    component.alpaca = new Alpaca('roberto');
    fixture.detectChanges();
    expect(fixture.nativeElement.querySelector('.alpaca').innerHTML).toContain('Roberto');
  });
});
```

---

## Tester un composant (3)

### Tester des outputs

```typescript
import { async, fakeAsync, tick, ComponentFixture, TestBed } from '@angular/core/testing';
import 'rxjs/add/operator/toPromise';

import { TextInputComponent } from './text-input.component';

describe('TextInputComponent', () => {
  /* ... */
  it('enter should emit a value', fakeAsync(() => {
    const mockHtmlInputElement = <HTMLInputElement>{
      value: "Test"
    };
    let textEntered: any
    component.textEntered.subscribe((data:any) => textEntered = data);
    component.enter(mockHtmlInputElement);
    tick();
    expect(textEntered).toBe('Test')
  }));
});
```

---

## Tester un service

### Mock de dépendances et autres services

```typescript
        { provide: AlpacaService, useValue: {
          getAlpacas() { return [] }
        }}
```

---

## Mocker le backend HTTP (1)

```typescript
import { TestBed, inject, fakeAsync, tick } from '@angular/core/testing';
import { AlpacaRepositoryService } from './alpaca-repository.service';
import { HttpModule, Http, XHRBackend, Connection, ConnectionBackend, Response, ResponseOptions } from '@angular/http';
import { MockBackend, MockConnection } from '@angular/http/testing';

describe('AlpacaRepositoryService', () => {
  beforeEach(() => {
    TestBed.configureTestingModule({
      imports: [
        HttpModule
      ],
      providers: [
        { provide: XHRBackend, useClass: MockBackend },
        AlpacaRepositoryService
      ]
    });
  });
});
```

---

## Mocker le backend HTTP (2)

```typescript
  it('getAll should query a localhost url', inject([
    AlpacaRepositoryService,
    XHRBackend
  ], fakeAsync((
    service: AlpacaRepositoryService,
    mockBackend: MockBackend
  ) => {
    mockBackend.connections.subscribe((c: MockConnection) => {
      expect(c.request.url).toBe('http://localhost:3000/alpacas');
      let response = new ResponseOptions({
        body: [ { name: 'Roberto'} ]
      });
      c.mockRespond(new Response(response));
    });
    let alpacaList: any;
    service.readAll().subscribe((data:any) => alpacaList = data)
    tick();
    expect(alpacaList.length).toBe(1);
    expect(alpacaList[0].name).toBe('Roberto');
  })));
```

