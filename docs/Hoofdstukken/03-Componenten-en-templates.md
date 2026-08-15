<span class="badge-niveau">Niveau: beginner</span>

# 3. Componenten & templates

Een **component** = TypeScript-class (logica) + HTML-template (weergave)
+ CSS (styling), altijd met dat drietal samen.

## Statische data tonen

```typescript
// taak-lijst.ts
import { Component } from '@angular/core';

@Component({
  selector: 'app-taak-lijst',
  imports: [],
  templateUrl: './taak-lijst.html',
  styleUrl: './taak-lijst.css',
})
export class TaakLijst {
  titel = 'Mijn taken';
  // ^ een gewone class-property. Geen speciale decorator nodig om iets
  //   in de template te kunnen tonen — alle public properties zijn
  //   automatisch beschikbaar in de bijhorende .html.

  aantalTaken = 4;
}
```

```html
<!-- taak-lijst.html -->
<h2>{{ titel }}</h2>
<!-- ^ dubbele accolades = "interpolation": toont de waarde van de
     property "titel" als tekst. Dit is de meest gebruikte binding. -->

<p>Je hebt {{ aantalTaken }} taken.</p>
<!-- ^ je kan ook uitdrukkingen gebruiken, bv. {{ aantalTaken * 2 }} -->
```

!!! note "Waarom apart .ts en .html?"
    Je kán ook `template: '...'` inline in de `@Component`-decorator
    zetten (handig voor héél kleine componenten), maar gescheiden
    bestanden houden logica en weergave overzichtelijk — vooral bij
    grotere componenten zoals je examenproject.

## Een component ergens gebruiken

Om `<app-taak-lijst>` te mogen gebruiken in bv. `app.html`, moet je hem
importeren in het component waarin je hem gebruikt:

```typescript
// app.ts
import { Component } from '@angular/core';
import { TaakLijst } from './taak-lijst/taak-lijst';
// ^ importeer de class van het kindcomponent.

@Component({
  selector: 'app-root',
  imports: [TaakLijst],
  // ^ VERPLICHT bij standalone components: zonder dit in de "imports"
  //   array, herkent Angular de tag <app-taak-lijst> niet in app.html
  //   en krijg je een compile-fout.
  templateUrl: './app.html',
  styleUrl: './app.css',
})
export class App {}
```

```html
<!-- app.html -->
<app-taak-lijst></app-taak-lijst>
<!-- ^ dit rendert het hele TaakLijst-component hier. -->
```

!!! warning "Vergeten component te importeren"
    De #1 beginnersfout: je gebruikt `<app-taak-kaart>` in een template,
    maar vergeet `TaakKaart` toe te voegen aan de `imports: []` array
    van het component waarin je het gebruikt. Angular toont dan gewoon
    niets (of een foutmelding in de console) — check dit altijd eerst
    als een component niet verschijnt.

## Attribute vs. property in de template

```html
<button class="primary" [disabled]="true">Verstuur</button>
<!--     ^^^^^^^^^^^^^^   ^^^^^^^^^^^^^^^^
     gewoon HTML-attribuut    Angular property binding (zie Hoofdstuk 4) -->
```

Onthoud voorlopig: alles zonder vierkante haken `[ ]` is gewone statische
HTML. Zodra je een dynamische waarde uit je TypeScript-class wil tonen,
kom je in Hoofdstuk 4 terecht (data binding).

## CSS is standaard "scoped"

```css
/* taak-kaart.css */
p {
  color: gray;
}
/* ^ deze regel geldt ENKEL binnen taak-kaart.html, niet globaal in de
     hele app. Angular voegt automatisch unieke attributen toe aan de
     HTML-elementen om dit af te dwingen (view encapsulation). */
```

Wil je toch iets globaal stylen (bv. een lettertype voor de hele app)?
Zet dat in `src/styles.css`, niet in een component-specifieke CSS.

## Zelf proberen

1. Maak in je `takenplanner`-project een component `taak-kaart` met een
   hardcoded titel, status en prioriteit in de template.
2. Gebruik dat component in `app.html` (vergeet de import niet!).
3. Verander de kleur van de tekst via de component-specifieke CSS en
   controleer dat dit geen invloed heeft op andere componenten.

Door naar **Hoofdstuk 4: Data binding & control flow** — hier wordt het
pas echt dynamisch.
