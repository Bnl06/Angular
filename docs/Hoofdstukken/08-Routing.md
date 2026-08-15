<span class="badge-niveau">Niveau: gevorderd</span>

# 8. Routing

Routing laat je tussen "pagina's" navigeren **zonder** dat de browser de
hele app opnieuw laadt — enkel het relevante deel van het scherm ververst.

## Routes definiëren

```typescript
// app.routes.ts
import { Routes } from '@angular/router';
import { TaakLijst } from './tickets/taak-lijst/taak-lijst';
import { TaakDetail } from './tickets/taak-detail/taak-detail';
import { TaakToevoegen } from './tickets/taak-toevoegen/taak-toevoegen';
import { NietGevonden } from './niet-gevonden/niet-gevonden';

export const routes: Routes = [
  { path: 'taken', component: TaakLijst },
  // ^ localhost:4200/taken toont het TaakLijst-component.

  { path: 'taken/nieuw', component: TaakToevoegen },
  // ^ LET OP DE VOLGORDE: dit moet VÓÓR 'taken/:id' staan, anders
  //   probeert Angular "nieuw" te interpreteren als een taak-id
  //   (zie waarschuwing verderop).

  { path: 'taken/:id', component: TaakDetail },
  // ^ ":id" is een ROUTE-PARAMETER: een variabel stuk van de URL.
  //   /taken/1, /taken/42, /taken/hallo werken allemaal — Angular
  //   geeft gewoon door wat er staat, valideren doe je zelf.

  { path: '', redirectTo: '/taken', pathMatch: 'full' },
  // ^ de root-URL (leeg pad) stuurt automatisch door naar /taken.
  //   "pathMatch: 'full'" betekent: enkel als het ENTIERE pad leeg is
  //   (dus niet bv. bij /taken/iets).

  { path: '**', component: NietGevonden },
  // ^ "**" = wildcard, vangt ALLE routes op die hierboven niet matchten.
  //   MOET altijd de LAATSTE regel zijn — Angular matcht van boven naar
  //   onder en stopt bij de eerste match.
];
```

!!! warning "Volgorde van routes is cruciaal"
    Angular matcht routes **in volgorde, van boven naar beneden**, en
    stopt bij de eerste match. Zet je `'taken/:id'` vóór
    `'taken/nieuw'`, dan interpreteert Angular "nieuw" als een id en
    komt je `TaakToevoegen`-pagina nooit aan bod. Dit is exact hoe je
    eigen examenproject `app.routes.ts` is opgebouwd — bekijk die
    volgorde bewust.

## De router-outlet: waar routes verschijnen

```html
<!-- app.html -->
<nav>
  <a routerLink="/taken">Taken</a>
  <!-- ^ "routerLink" i.p.v. "href": navigeert ZONDER de pagina te
       herladen (dat is het hele punt van een SPA). -->
</nav>

<router-outlet></router-outlet>
<!-- ^ HIER injecteert Angular het component dat bij de huidige URL
     hoort. Vergeet je dit, dan blijft je scherm leeg ongeacht welke
     route actief is. -->
```

## Route-parameters uitlezen (de moderne manier)

```typescript
// app.config.ts — vereiste provider-optie
import { provideRouter, withComponentInputBinding } from '@angular/router';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes, withComponentInputBinding()),
    // ^ "withComponentInputBinding()" activeert de MODERNE manier om
    //   route-parameters te ontvangen: rechtstreeks als component-input
    //   (zie hieronder), i.p.v. via ActivatedRoute.
  ],
};
```

```typescript
// taak-detail.ts
import { Component, input, inject, OnInit } from '@angular/core';
import { TaakService } from '../../services/taak.service';

@Component({
  selector: 'app-taak-detail',
  imports: [],
  templateUrl: './taak-detail.html',
  styleUrl: './taak-detail.css',
})
export class TaakDetail implements OnInit {
  id = input.required<string>();
  // ^ dankzij withComponentInputBinding() koppelt Angular de route-
  //   parameter ":id" AUTOMATISCH aan een input met dezelfde naam "id".
  //   Let op: dit komt altijd binnen als STRING, ook al lijkt het een
  //   getal — converteer expliciet met Number(this.id()) als je ermee
  //   moet rekenen.

  private taakService = inject(TaakService);
  taak?: Taak;

  ngOnInit(): void {
    this.taakService.getTaakById(Number(this.id())).subscribe(taak => {
      this.taak = taak;
    });
  }
}
```

!!! note "Alternatief: ActivatedRoute (oudere, maar nog veel gebruikte stijl)"
    ```typescript
    import { ActivatedRoute } from '@angular/router';

    private route = inject(ActivatedRoute);

    ngOnInit(): void {
      const id = this.route.snapshot.paramMap.get('id');
      // ^ "snapshot" leest de parameter ÉÉN keer bij het laden. Als de
      //   gebruiker binnen hetzelfde component naar een ander id
      //   navigeert (zonder het component te herladen), mis je dan een
      //   update — gebruik dan route.paramMap (een Observable) i.p.v.
      //   snapshot. Voor een examen-scope volstaat snapshot meestal.
    }
    ```
    Beide manieren zijn geldig; kies er één en gebruik hem consistent.

## Programmatisch navigeren (bv. na een succesvolle submit)

```typescript
import { Router } from '@angular/router';

private router = inject(Router);

opGelukt(): void {
  this.router.navigate(['/taken']);
  // ^ stuurt de gebruiker naar /taken, bv. nadat een formulier
  //   succesvol verstuurd is (zie Hoofdstuk 9).
}
```

## Zelf proberen

1. Bouw de routes hierboven na: `/taken`, `/taken/nieuw`, `/taken/:id`,
   plus een wildcard-route naar een "niet gevonden"-component.
2. Voeg navigatielinks toe met `routerLink` in `app.html`.
3. Laat `taak-detail` het juiste taak-id tonen via `input()` +
   `withComponentInputBinding()`.
4. Test bewust een onbestaande URL (bv. `/blabla`) — verschijnt je
   wildcard-pagina?

Door naar **Hoofdstuk 9: Forms**.
