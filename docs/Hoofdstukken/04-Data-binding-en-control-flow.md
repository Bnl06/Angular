<span class="badge-niveau">Niveau: beginner → gevorderd</span>

# 4. Data binding & control flow

## De 4 soorten data binding

```html
<!-- 1. Interpolation: TypeScript-waarde -> HTML-tekst -->
<h2>{{ taak.titel }}</h2>
<!-- ^ toont de waarde van "taak.titel", ALTIJD als tekst -->

<!-- 2. Property binding: TypeScript-waarde -> HTML/component-eigenschap -->
<button [disabled]="taak.status === 'klaar'">Bewerken</button>
<!-- ^ [disabled] wordt dynamisch true/false, gebaseerd op de uitdrukking
     erna. De vierkante haken zeggen "dit is geen platte tekst, dit is
     een TypeScript-expressie die geëvalueerd wordt". -->

<!-- 3. Event binding: HTML-gebeurtenis -> TypeScript-methode -->
<button (click)="verwijderTaak(taak.id)">Verwijder</button>
<!-- ^ ronde haken (click) = luister naar het click-event, en roep dan
     de methode verwijderTaak() op met taak.id als argument. -->

<!-- 4. Two-way binding: gecombineerd, vooral bij formuliervelden -->
<input [(ngModel)]="taak.titel">
<!-- ^ [( )] ("banana in a box") = property binding + event binding
     ineen: de input toont taak.titel EN elke tik werkt taak.titel bij.
     Vereist de FormsModule (zie Hoofdstuk 9 voor het volledige
     formulier-verhaal via Reactive Forms). -->
```

!!! tip "Ezelsbruggetje voor de haakjes"
    - `{{ }}` — tekst tonen
    - `[ ]` — iets *instellen* (van TS naar HTML)
    - `( )` — iets *afluisteren* (van HTML naar TS)
    - `[( )]` — allebei tegelijk

## De nieuwe control-flow syntax (`@if`, `@for`, `@switch`)

Sinds Angular 17 gebruik je **geen** `*ngIf`/`*ngFor` meer (die bestaan
nog, maar zijn verouderd). De nieuwe syntax is ingebouwd in de template
engine zelf, sneller, en leesbaarder:

```html
@if (taken.length === 0) {
  <p>Geen taken gevonden.</p>
} @else {
  <p>Je hebt {{ taken.length }} taken.</p>
}
<!-- ^ @if / @else werkt zoals een gewone if/else, maar dan in de
     template. Geen sterretje, geen ng-template nodig. -->

<ul>
  @for (taak of taken; track taak.id) {
    <li>{{ taak.titel }} — {{ taak.status }}</li>
  }
  @empty {
    <li>Nog niets toegevoegd.</li>
  }
</ul>
<!-- ^ @for loopt over de array "taken". "track taak.id" is VERPLICHT:
     het vertelt Angular hoe het elk item uniek herkent, zodat het bij
     wijzigingen enkel de juiste <li> herrendert i.p.v. alles opnieuw
     op te bouwen (performantie). @empty toont iets als de lijst leeg is. -->

@switch (taak.status) {
  @case ('open') {
    <span class="badge open">Open</span>
  }
  @case ('bezig') {
    <span class="badge bezig">Bezig</span>
  }
  @default {
    <span class="badge klaar">Klaar</span>
  }
}
<!-- ^ @switch vervangt een reeks @if/@else if — netter bij 3+ opties,
     zoals hier de 3 mogelijke statussen van een taak. -->
```

!!! warning "'track' vergeten in @for"
    Zonder `track` geeft Angular een compile-fout — dit is bewust
    verplicht (in tegenstelling tot het oude `*ngFor`, waar `trackBy`
    optioneel was). Heb je geen uniek id? Gebruik dan `track $index` als
    noodoplossing, maar een echt id is altijd beter.

## Alles samen: een component dat een lijst toont

```typescript
// taak-lijst.ts
import { Component } from '@angular/core';

interface Taak {
  id: number;
  titel: string;
  status: 'open' | 'bezig' | 'klaar';
}

@Component({
  selector: 'app-taak-lijst',
  imports: [],
  templateUrl: './taak-lijst.html',
  styleUrl: './taak-lijst.css',
})
export class TaakLijst {
  taken: Taak[] = [
    { id: 1, titel: 'Angular leren', status: 'bezig' },
    { id: 2, titel: 'Python examen voorbereiden', status: 'open' },
  ];
  // ^ hardcoded testdata — in Hoofdstuk 7 vervang je dit door echte
  //   data uit een backend.

  verwijderTaak(id: number): void {
    this.taken = this.taken.filter(t => t.id !== id);
    // ^ filter() geeft een NIEUWE array terug zonder de taak met dit id.
    //   We herbewaren "taken" bewust met een nieuwe array (i.p.v.
    //   .splice() op de bestaande array) — dat maakt wijzigingen
    //   voorspelbaarder, en is straks ook hoe signals verwachten dat
    //   je met state omgaat (Hoofdstuk 10).
  }
}
```

```html
<!-- taak-lijst.html -->
<h2>Mijn taken</h2>

@if (taken.length === 0) {
  <p>Niets te doen — geniet ervan!</p>
} @else {
  <ul>
    @for (taak of taken; track taak.id) {
      <li>
        {{ taak.titel }}
        <button (click)="verwijderTaak(taak.id)">Verwijder</button>
      </li>
    }
  </ul>
}
```

## Zelf proberen

1. Bouw bovenstaand voorbeeld na in je `taak-lijst`-component.
2. Voeg een `@switch` toe die per taak een gekleurd label toont op basis
   van `status`.
3. Voeg een knop "Voeg testtaak toe" toe met `(click)` die een nieuwe
   taak aan de array toevoegt (`this.taken = [...this.taken, nieuweTaak]`)
   en kijk of `@for` automatisch de nieuwe rij toont.

Door naar **Hoofdstuk 5: Componentcommunicatie**.
