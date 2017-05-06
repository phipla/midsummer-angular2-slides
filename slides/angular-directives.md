# Directives

---

## NgStyle

```html
<div [style.background]="background: alpaca.notes ? '#f0f6ff' : '#fff'">
    <!-- ... -->
</div>
```

```html
<div [ngStyle]="{
    background: alpaca.notes ? '#f0f6ff' : '#fff'
}">
    <!-- ... -->
</div>
```

---

## NgClass

```html
<div [class.hasNotes]="alpaca.notes">
    <!-- ... -->
</div>
```

```html
<div [ngClass]="{
    hasNotes: alpaca.notes
}">
    <!-- ... -->
</div>
```

```html
<div [ngClass]="['hasNotes', 'needVaccine']">
    <!-- ... -->
</div>
```

---

## Directives structurelles

La notation `*` est en fait un raccourci. La syntaxe étendue utilise l'élément `<template>`
défini dans HTML5.

```html
<li *ngFor="let alpaca of alpacas">
{{ alpaca.name }}
</li>
```

```html
<template ngFor [ngForOf]="alpacas" let-alpaca>
    <li>
        {{ alpaca.name }}
    </li>
</template>
```

---

## NgFor

* index
* first
* last
* even
* odd

---

## NgIf

---

## NgSwitch

---

## Directives


