# Formulaires

<figure>
    <img src="resources/forms.jpeg" style="width: 28vw">
    <figcaption>
        <div class="attribution">
            By <a href="//commons.wikimedia.org/wiki/User:Mattes" title="User:Mattes">User:Mattes</a> -
            <span class="int-own-work" lang="en">Own work</span>, Public Domain,
            <a href="https://commons.wikimedia.org/w/index.php?curid=597107">Link</a>
        </div>
    </figcaption>
</figure>

---

## Installation

```bash
npm install --save @angular/forms
```

(Installé d'office avec angular-cli)

* * *

<div class="filename">app.module.ts</div>
```typescript
import { FormsModule, ReactiveFormsModule } from '@angular/forms';

@NgModule({
  /* ... */
  imports: [
    FormsModule, ReactiveFormsModule
  ],
})
export class AppModule { }
```

---

## Formulaires basés sur le template (1)

Utilise les directives ngForm et ngModel

<pre><code class="html" data-trim data-noescape>
&lt;form <mark>#f="ngForm"</mark> (ngSubmit)="save(f.value)">
    &lt;label for="inputName">
        Nom
    &lt;/label>
    &lt;input id="inputName" name="name" <mark>ngModel</mark>>
    &lt;label for="inputNotes">
        Description
    &lt;/label>
    &lt;textarea id="inputNotes" name="notes" <mark>ngModel</mark>>&lt;/textarea>
    &lt;button type="submit">
        Enregistrer
    &lt;/button>
&lt;/form>
</code></pre>

<i class="fa fa-hand-o-right"></i> La directive "ngForm" est ajoutée automatiquement à tous les
formulaires

<i class="fa fa-hand-o-right"></i> La variable locale "#f" prendrait par défaut la valeur de
l'élément `<form>` lui-même. Ici, on lui dit de plutôt prendre la valeur de la directive `ngForm`.

---

## Formulaires basés sur le template (2)

```typescript
import { Component } from '@angular/core';

@Component({ /* */ })
export class AlpacaDetailComponent {
  private Alpaca: alpaca;

  constructor(
    private AlpacaService: alpacaService
  ) {
  }

  private save(value: any) {
    this.alpacaService.update(this.alpaca.id, new Alpaca(value));
  }
}
```

---

## Éléments du formulaire

### Éléments

* `FormControl` : contrôle du formulaire (`input`, `select`, etc)
* `FormGroup` : groupe de contrôle (le formulaire lui-même, ou des sous-groupes)

### Directives

La directive `ngModel` crée un nouveau `FormControl` qu'elle associe au `FormGroup` courant

La directive `ngModelGroup` crée un nouveau `FormGroup`

---

## Binding au modèle (1)

<pre><code class="html" data-trim data-noescape>
&lt;form #f="ngForm" (ngSubmit)="save(f.value)">
    &lt;label for="inputName">
        Nom
    &lt;/label>
    &lt;input id="inputName" name="name" <mark>[ngModel]="alpaca.name"</mark>>
    &lt;label for="inputNotes">
        Description
    &lt;/label>
    &lt;textarea id="inputNotes" name="notes" <mark>[ngModel]="alpaca.notes"</mark>>&lt;/textarea>
    &lt;button type="submit">
        Enregistrer
    &lt;/button>
&lt;/form>
</code></pre>

---

## Binding au modèle (2)

<pre><code class="html" data-trim data-noescape>
&lt;form #f="ngForm" (ngSubmit)="save(f.value)">
    &lt;label for="inputName">
        Nom
    &lt;/label>
    &lt;input id="inputName" name="name" <mark>[(ngModel)]="alpaca.name"</mark>>
    &lt;label for="inputNotes">
        Description
    &lt;/label>
    &lt;textarea id="inputNotes" name="notes" <mark>[(ngModel)]="alpaca.notes"</mark>>&lt;/textarea>
    &lt;button type="submit">
        Enregistrer
    &lt;/button>
&lt;/form>
</code></pre>

---

## Validation

Utiliser la validation native HTML5

<pre><code class="html" data-trim data-noescape>
&lt;form #f="ngForm" (ngSubmit)="save(f.value)">
    &lt;label for="inputName">
        Nom
    &lt;/label>
    &lt;input id="inputName" name="name" ngModel <mark>required maxlength="50"</mark>>
    &lt;label for="inputNotes">
        Description
    &lt;/label>
    &lt;textarea id="inputNotes" name="notes" ngModel <mark>maxlength="1000"</mark>>&lt;/textarea>
    &lt;button type="submit">
        Enregistrer
    &lt;/button>
&lt;/form>
</code></pre>

---

## Formulaires réactifs

Définir le formulaire dans le template est :

* simple est rapide,
* potentiellement limité,
* difficilement testable.

Pour pouvoir utiliser des validateurs plus complexes, il est possible de gérer la logique dans le
code avec des formulaires "réactifs".

---

## Définition des contrôles

<pre><code class="html" data-trim data-noescape>
&lt;form (ngSubmit)="save()">
    &lt;label for="inputName">
        Nom
    &lt;/label>
    &lt;input id="inputName" name="name" <mark>[formControl]="name"</mark>>
    &lt;label for="inputNotes">
        Description
    &lt;/label>
    &lt;textarea id="inputNotes" name="notes" <mark>[formControl]="notes"</mark>>&lt;/textarea>
    &lt;button type="submit">
        Enregistrer
    &lt;/button>
&lt;/form>
</code></pre>

<pre><code class="typescript" data-trim data-noescape>
import { FormControl } from '@angular/forms';

@Component({ /* */ })
export class AlpacaDetailComponent {
  name = new FormControl();
  notes = new FormControl();
}
</code></pre>

---

## Groupement dans un FormGroup

<pre><code class="html" data-trim data-noescape>
&lt;form (ngSubmit)="save()" <mark>[formGroup]="alpacaForm"</mark>>
    &lt;!-- ... -->
&lt;/form>
</code></pre>

<pre><code class="typescript" data-trim data-noescape>
import { FormControl, FormGroup } from '@angular/forms';

@Component({ /* */ })
export class AlpacaDetailComponent {
  name = new FormControl();
  notes = new FormControl();
  alpacaForm = new FormGroup({ name: this.name, notes: this.notes })
}
</code></pre>

---

## Utilisation de FormBuilder (1)

<pre><code class="typescript" data-trim data-noescape>
import { FormControl, FormGroup, FormBuilder } from '@angular/forms';

@Component({ /* */ })
export class AlpacaDetailComponent {
  alpacaForm: FormGroup;

  constructor(fb: FormBuilder) {
    this.alpacaForm = fb.group({
      name: [ alpaca.name ],
      notes: [ alpaca.notes ]
    })
  }
}
</code></pre>

---

## Utilisation de FormBuilder (2)

<pre><code class="html" data-trim data-noescape>
&lt;form (ngSubmit)="save()">
    &lt;label for="inputName">
        Nom
    &lt;/label>
    &lt;input id="inputName" name="name"
        <mark>[formControl]="alpacaForm.name"</mark>>
    &lt;label for="inputNotes">
        Description
    &lt;/label>
    &lt;textarea id="inputNotes" name="notes"
        <mark>[formControl]="alpacaForm.notes"</mark>>&lt;/textarea>
    &lt;button type="submit">
        Enregistrer
    &lt;/button>
&lt;/form>
</code></pre>

---

## Validateurs (1)

<pre><code class="typescript" data-trim data-noescape>
import { FormControl, FormGroup, FormBuilder, Validators } from '@angular/forms';

@Component({ /* */ })
export class AlpacaDetailComponent {
  alpacaForm: FormGroup;

  constructor(fb: FormBuilder) {
    this.alpacaForm = fb.group({
      name: ['', [ Validators.required, Validators.maxLength(50) ]],
      notes: ['', Validators.maxLength(1000) ]
    })
  }
}
</code></pre>

---

## Validateurs (2)

<pre><code class="html" data-trim data-noescape>
&lt;form (ngSubmit)="save()">
    &lt;div <mark>[class.invalid]="
        !alpacaForm?.controls?.name.valid
        && alpacaForm?.controls?.name.touched"</mark>>
        &lt;label for="inputName">
            Nom
        &lt;/label>
        &lt;input id="inputName" name="name"
            [formControl]="alpacaForm.name">
    &lt;/div>
    &lt;div <mark>[class.invalid]="
        !alpacaForm?.controls?.inputNotes.valid
        && alpacaForm?.controls?.inputNotes.touched"</mark>>
        &lt;label for="inputNotes">
            Description
        &lt;/label>
        &lt;textarea id="inputNotes" name="notes"
            [formControl]="alpacaForm.notes">&lt;/textarea>
    &lt;/div>
&lt;/form>
</code></pre>

---

## Validateurs (2)

* valid / invalid : le contrôle est valide
* dirty / pristine : l'utilisateur a interagi avec le contrôle (saisi du texte etc.)
* touched / untouched : le contrôle a déjà perdu le focus

errors: map entre l'identifiant de l'erreur et un paramètre qui dépend de l'erreur

---

## Validateurs personnalisés

Un validateur implémente soit l'interface ValidatorFn, soit l'interface Validator

```typescript
export declare type ValidationErrors = { [key: string]: any; };

export interface ValidatorFn {
    (c: AbstractControl): ValidationErrors | null;
}

export interface Validator {
    validate(c: AbstractControl): ValidationErrors | null;
    registerOnValidatorChange?(fn: () => void): void;
}
```

```typescript
export class Validators {
  static required(c: FormControl): StringMap<string, boolean> {
    return isBlank(c.value) || c.value == ""
      ? {"required": true}
      : null;
  }
}
```

---

## Suivi des changements

### Exemple: composant de recherche

`search.component.html`

```html
<input
  type="search"
  class="form-control"
  [formControl]="textInput"
  >
```

---

## Suivi des changements (2)

```typescript
import { Component, OnInit, EventEmitter, Output, Input } from '@angular/core';
import { FormControl } from '@angular/forms';
import 'rxjs/add/operator/debounceTime';

@Component({
  selector: 'msw-search',
  templateUrl: './search.component.html',
  styleUrls: ['./search.component.css']
})
export class SearchComponent implements OnInit {
  @Output() textEntered = new EventEmitter();
  textInput = new FormControl();

  constructor() { }
  ngOnInit() {
    this.textInput.valueChanges
      .debounceTime(500)
      .subscribe(this.textEntered);
  }
}
```
