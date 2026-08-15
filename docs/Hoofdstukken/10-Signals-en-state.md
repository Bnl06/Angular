<span class="badge-niveau">Niveau: gevorderd</span>

# 10. Signals & state

Je zag `input()` en `output()` al signal-based werken (Hoofdstuk 5).
**Signals** zijn Angular's moderne manier om lokale state te beheren —
ze vervangen niet RxJS/Observables (die blijven nodig voor HTTP, zie
Hoofdstuk 7), maar zijn de aanbevolen aanpak voor eenvoudige,
synchrone state binnen een component.

## Waarom signals? Het probleem met gewone properties

```typescript
export class TaakLijst {
  taken: Taak[] = [];
  // ^ een GEWONE property. Angular's "change detection" moet bij ELKE
  //   mogelijke wijziging in de hele app checken of deze property
  //   veranderd is — dat kost performantie naarmate je app groeit.
}
```

```typescript
export class TaakLijst {
  taken = signal<Taak[]>([]);
  // ^ een SIGNAL: een "doos" rond een waarde die Angular precies kan
  //   volgen. Angular weet nu EXACT wanneer deze waarde verandert, en
  //   update enkel de stukjes template die er effectief van afhangen.
}
```

## De basis-API

```typescript
import { Component, signal, computed } from '@angular/core';

export class TaakLijst {
  taken = signal<Taak[]>([
    { id: 1, titel: 'Angular leren', status: 'bezig', prioriteit: 5 },
    { id: 2, titel: 'Python examen', status: 'open', prioriteit: 4 },
  ]);
  // ^ signal(initiëleWaarde) maakt een nieuwe signal aan.

  aantalTaken = computed(() => this.taken().length);
  // ^ computed() maakt een AFGELEIDE signal: herberekent automatisch
  //   zodra "taken" verandert. Je roept "taken()" aan (met haakjes) om
  //   de HUIDIGE waarde te lezen — dat geldt voor elke signal.

  openTaken = computed(() =>
    this.taken().filter(t => t.status === 'open')
  );
  // ^ nog een afgeleide signal: automatisch bijgewerkt, nooit "vergeten
  //   te synchroniseren" zoals je bij een handmatige tweede array wel
  //   zou kunnen.

  voegToe(titel: string): void {
    this.taken.update(huidigeTaken => [
      ...huidigeTaken,
      { id: Date.now(), titel, status: 'open', prioriteit: 3 },
    ]);
    // ^ update() geeft je de HUIDIGE waarde en verwacht de NIEUWE
    //   waarde terug. Gebruik dit i.p.v. .push() op de array zelf —
    //   signals detecteren enkel wijzigingen als de VERWIJZING
    //   (referentie) verandert, niet als je de inhoud stiekem muteert.
  }

  verwijder(id: number): void {
    this.taken.update(huidigeTaken =>
      huidigeTaken.filter(t => t.id !== id)
    );
  }

  vervang(nieuweTaken: Taak[]): void {
    this.taken.set(nieuweTaken);
    // ^ set() overschrijft de volledige waarde in één keer — handig
    //   bv. na een HTTP-call die de volledige lijst teruggeeft.
  }
}
```

```html
<!-- taak-lijst.html -->
<p>Aantal taken: {{ aantalTaken() }}</p>
<!-- ^ ook computed signals lees je met haakjes: aantalTaken() -->

@for (taak of taken(); track taak.id) {
  <li>{{ taak.titel }}</li>
}
<!-- ^ taken() geeft de actuele array terug voor @for om over te lopen -->
```

!!! warning "Signals altijd met haakjes lezen"
    `taken` is de signal zelf (een object/functie), `taken()` is de
    **waarde erin**. Vergeet je de haakjes in een template of in
    TypeScript-code, dan krijg je een TypeScript-foutmelding of
    onverwacht gedrag (je vergelijkt dan de signal-functie zelf i.p.v.
    de waarde). Dit is dezelfde regel als bij `input()` (Hoofdstuk 5).

## Observable → Signal: `toSignal()`

HTTP-data komt als Observable binnen (Hoofdstuk 7). Wil je dat in de
template gebruiken zoals een signal, zonder handmatig te subscriben?

```typescript
import { Component, inject } from '@angular/core';
import { toSignal } from '@angular/core/rxjs-interop';
import { TaakService } from '../../services/taak.service';

export class TaakLijst {
  private taakService = inject(TaakService);

  taken = toSignal(this.taakService.getTaken(), { initialValue: [] });
  // ^ "toSignal()" abonneert zich automatisch op de Observable, en
  //   ruimt dat abonnement ook automatisch op wanneer het component
  //   verdwijnt (geen memory leaks, iets wat je bij handmatig
  //   .subscribe() zelf moet regelen met bv. een Subscription).
  //   "initialValue" is nodig omdat de HTTP-data nog niet binnen is
  //   op het moment dat het component voor het eerst rendert.
}
```

```html
@for (taak of taken(); track taak.id) {
  <li>{{ taak.titel }}</li>
}
<!-- ^ geen @if (laden) nodig hier op deze simpele manier, al mis je dan
     wel expliciete laad/foutstatus — voor dat laatste blijft de
     .subscribe()-aanpak uit Hoofdstuk 7 vaak duidelijker. -->
```

!!! tip "Wat gebruik je wanneer?"
    - **`.subscribe()` in `ngOnInit`** — als je expliciete controle wil
      over laad-status, foutafhandeling, of side-effects (Hoofdstuk 7).
      Dit is ook wat je in je examenproject al ziet.
    - **`toSignal()`** — als je enkel de data in de template nodig hebt,
      en minder boilerplate wil.
    - **`signal()` / `computed()`** — voor lokale UI-state die niets met
      een backend te maken heeft (bv. "is een dropdown open?", "welke
      filter is actief?").

## Zelf proberen

1. Herbouw je `taak-lijst` uit Hoofdstuk 6 met `signal<Taak[]>` i.p.v.
   een gewone property.
2. Voeg een `computed()` toe die het aantal open taken telt.
3. Probeer `toSignal()` op je `TaakService.getTaken()`-Observable, en
   vergelijk het gevoel met de `.subscribe()`-aanpak uit Hoofdstuk 7.

Door naar **Hoofdstuk 11: Praktijkcase Takenplanner** — hier breng je
alles samen in één werkend project.
