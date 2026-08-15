<span class="badge-niveau">Niveau: gevorderd</span>

# 9. Forms (Reactive Forms)

Angular heeft twee manieren om formulieren te bouwen: **Template-driven**
(simpel, met `[(ngModel)]`) en **Reactive Forms** (meer controle,
makkelijker te valideren en te testen). Voor een examen met formulieren
zoals "nieuwe taak toevoegen" of "taak bewerken" zijn **Reactive Forms**
de professionele standaard — dit hoofdstuk focust daarop.

## De basis: FormGroup en FormControl

```typescript
// taak-toevoegen.ts
import { Component, inject } from '@angular/core';
import { FormGroup, FormControl, Validators, ReactiveFormsModule } from '@angular/forms';
import { Router } from '@angular/router';
import { TaakService } from '../../services/taak.service';

@Component({
  selector: 'app-taak-toevoegen',
  imports: [ReactiveFormsModule],
  // ^ VERPLICHT: zonder deze import in "imports" herkent Angular de
  //   formcontrol-directives ([formGroup], formControlName...) niet
  //   in je template, en krijg je een compile-fout.
  templateUrl: './taak-toevoegen.html',
  styleUrl: './taak-toevoegen.css',
})
export class TaakToevoegen {
  private taakService = inject(TaakService);
  private router = inject(Router);

  taakForm = new FormGroup({
    // ^ FormGroup bundelt meerdere FormControls tot één geheel dat je
    //   samen kan valideren en versturen.
    titel: new FormControl('', {
      nonNullable: true,
      // ^ zonder deze optie mag de waarde ook "null" zijn (TypeScript-
      //   technisch); "nonNullable: true" houdt het type gewoon
      //   "string", wat verderop makkelijker werkt.
      validators: [Validators.required, Validators.minLength(3)],
      // ^ VALIDATORS: "required" = mag niet leeg zijn, "minLength(3)"
      //   = minstens 3 tekens. Angular checkt dit automatisch bij elke
      //   wijziging, jij hoeft dit niet zelf te controleren.
    }),
    prioriteit: new FormControl(3, {
      nonNullable: true,
      validators: [Validators.required, Validators.min(1), Validators.max(5)],
    }),
    status: new FormControl<'open' | 'bezig' | 'klaar'>('open', {
      nonNullable: true,
    }),
  });

  versturen(): void {
    if (this.taakForm.invalid) {
      // ^ "invalid" is true zodra ÉÉN van de validators hierboven faalt.
      this.taakForm.markAllAsTouched();
      // ^ toont foutmeldingen ook voor velden die de gebruiker nog
      //   niet heeft aangeraakt — anders blijft het formulier stil bij
      //   een klik op "versturen" zonder iets in te vullen.
      return;
    }

    this.taakService.voegToe(this.taakForm.getRawValue()).subscribe(() => {
      // ^ "getRawValue()" geeft alle waarden terug, ook van eventueel
      //   uitgeschakelde velden (in tegenstelling tot .value).
      this.router.navigate(['/taken']);
      // ^ terug naar het overzicht na succesvol toevoegen.
    });
  }
}
```

```html
<!-- taak-toevoegen.html -->
<form [formGroup]="taakForm" (ngSubmit)="versturen()">
  <!-- ^ [formGroup] koppelt dit <form>-element aan de TypeScript-
       FormGroup. (ngSubmit) vangt de submit-actie op (bv. Enter
       drukken of op de submit-knop klikken) EN voorkomt automatisch
       de standaard browser-page-reload die een <form> anders zou doen. -->

  <label>
    Titel
    <input formControlName="titel">
    <!-- ^ koppelt dit veld aan de "titel"-FormControl. Geen [(ngModel)]
         nodig — Reactive Forms regelt de binding zelf via formControlName. -->
  </label>

  @if (taakForm.controls.titel.invalid && taakForm.controls.titel.touched) {
    <p class="fout">Titel moet minstens 3 tekens bevatten.</p>
    <!-- ^ "touched" = de gebruiker heeft dit veld al aangeklikt/verlaten.
         Zo toon je geen foutmelding VOORDAT iemand iets ingevuld heeft. -->
  }

  <label>
    Prioriteit (1-5)
    <input type="number" formControlName="prioriteit">
  </label>

  <label>
    Status
    <select formControlName="status">
      <option value="open">Open</option>
      <option value="bezig">Bezig</option>
      <option value="klaar">Klaar</option>
    </select>
  </label>

  <button type="submit" [disabled]="taakForm.invalid">
    Toevoegen
  </button>
</form>
```

!!! tip "Waarom Reactive Forms i.p.v. ngModel voor een examen?"
    Reactive Forms geven je gebundelde validatie (`taakForm.invalid`),
    makkelijk uit te lezen status (`.touched`, `.dirty`), en typechecked
    waarden via TypeScript. Bij een groter formulier (zoals "nieuw
    ticket" met categorie, status, prioriteit, titel, omschrijving) is
    dit veel overzichtelijker dan losse `[(ngModel)]`-bindings.

## Een formulier vooraf invullen (bewerken)

```typescript
// taak-bewerken.ts
ngOnInit(): void {
  this.taakService.getTaakById(Number(this.id())).subscribe(taak => {
    this.taakForm.patchValue(taak);
    // ^ "patchValue()" vult de FormGroup met bestaande waarden, zonder
    //   dat je elk veld apart moet toewijzen. Gebruik "patchValue" (niet
    //   "setValue") als je niet ALLE velden van het object hebt — zo
    //   krijg je geen foutmelding voor ontbrekende velden.
  });
}
```

## Zelf proberen

1. Bouw `taak-toevoegen` met een `FormGroup` voor titel, prioriteit en
   status, inclusief validatie en foutmeldingen.
2. Zorg dat de submit-knop uitgeschakeld is zolang het formulier
   ongeldig is.
3. Bouw daarna `taak-bewerken`: haal de bestaande taak op via het
   route-id (Hoofdstuk 8), vul het formulier met `patchValue`, en
   verstuur de wijziging via `TaakService.bijwerken()`.

Door naar **Hoofdstuk 10: Signals & state**.
