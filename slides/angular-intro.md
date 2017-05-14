# Angular ?

<img src="resources/angular-what.svg" class="plain" style="width: 25vw; display: block; margin: 0 auto">

---

## Angular ?

Framework complet pour la réalisation d'applications web, développé principalement par Google.

Fonctionnalité de base : étendre le HTML en permettant de définir ses propres composants (ou de redéfinir les composants existants).

### jQuery

```html
<div class="my-calendar"></div>
<script>
    var calendarDate = new Date();
    $('.my-calendar').myCalendar({
        date: calendarDate
    });
</script>
```

### Angular

```html
<my-calendar [options]="calendarDate"></my-calendar>
```

---

## Angular ?

Pas de manipulation du DOM, mais des templates déclaratifs.

### Framework complet

* HTML - Javascript binding
* Requêtes HTTP
* Routage (*single-page apps*)
* Injection de dépendances
* Animations
* Intégration framework de test
* Internationalisation

### Tooling moderne

TypeScript, Webpack

---

## Versions

| Version (AngularJS) | Version (Angular) | Date    |
|---------------------|-------------------|---------|
| v1.0.0              |                   | 06 2012 |
| v1.1.0              |                   | 08 2012 |
| v1.2.0              |                   | 08 2013 |
| v1.3.0              |                   | 10 2014 |
| v1.4.0              |                   | 05 2015 |
|                     | v2.0.0-alpha      | 06 2015 |
| v1.5.0              |                   | 02 2016 |
|                     | v2.0.0            | 09 2016 |
| v1.6.0              |                   | 12 2016 |
|                     | v4.0.0            | 03 2017 |
|                     | v5.0.0 (?)        | 09 2017 |
|                     | v6.0.0 (?)        | 03 2018 |
|                     | v7.0.0 (?)        | 09 2018 |
