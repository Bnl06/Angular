<span class="badge-niveau">Niveau: gevorderd</span>

# 7. HTTP & Observables

## HttpClient inschakelen

```typescript
// app.config.ts
import { ApplicationConfig } from '@angular/core';
import { provideHttpClient } from '@angular/common/http';
import { provideRouter } from '@angular/router';
import { routes } from './app.routes';

export const appConfig: ApplicationConfig = {
  providers: [
    provideRouter(routes),
    provideHttpClient(),
    // ^ VERPLICHT: zonder deze regel bestaat HttpClient niet in je app,
    //   en krijg je bij het injecteren een runtime-fout ("No provider
    //   for HttpClient"). Dit is een klassieke, snel te vergeten stap.
  ],
};
```

## Een service die data ophaalt

```typescript
// taak.service.ts
import { Injectable, inject } from '@angular/core';
import { HttpClient } from '@angular/common/http';
import { Observable } from 'rxjs';

interface Taak {
  id: number;
  titel: string;
  status: 'open' | 'bezig' | 'klaar';
}

@Injectable({ providedIn: 'root' })
export class TaakService {
  private http = inject(HttpClient);
  private baseUrl = 'http://localhost:3000';
  // ^ het adres van je backend — check dit tegen wat jouw
  //   backend-project (Express of NestJS) daadwerkelijk gebruikt.

  getTaken(): Observable<Taak[]> {
    return this.http.get<Taak[]>(`${this.baseUrl}/taken`);
    // ^ http.get() geeft GEEN data terug, maar een OBSERVABLE — een
    //   "belofte van data die er nog aankomt". Er gebeurt NIETS tot
    //   iemand zich erop abonneert (zie verderop .subscribe()).
    //   <Taak[]> vertelt TypeScript welk type data je verwacht.
  }

  getTaakById(id: number): Observable<Taak> {
    return this.http.get<Taak>(`${this.baseUrl}/taken/${id}`);
  }

  voegToe(taak: Omit<Taak, 'id'>): Observable<Taak> {
    // ^ "Omit<Taak, 'id'>" = het type Taak, MINUS het veld "id" —
    //   handig omdat de backend het id zelf toekent bij het aanmaken.
    return this.http.post<Taak>(`${this.baseUrl}/taken`, taak);
  }

  bijwerken(id: number, taak: Partial<Omit<Taak, 'id'>>): Observable<Taak> {
    // ^ "Partial<...>" = alle velden worden OPTIONEEL — handig omdat je
    //   bij een update niet noodzakelijk alle velden meestuurt.
    return this.http.put<Taak>(`${this.baseUrl}/taken/${id}`, taak);
  }

  verwijder(id: number): Observable<void> {
    return this.http.delete<void>(`${this.baseUrl}/taken/${id}`);
  }
}
```

!!! tip "Dit is bijna letterlijk je examenproject"
    Vergelijk dit met `ticket.service.ts` in je examenproject: exact
    dezelfde opbouw (get/post/put/delete, Observable-returntypes). Als
    dit hoofdstuk logisch aanvoelt, begrijp je je eigen servicebestand.

## Observable ≠ Promise — het grote verschil

```typescript
// Promise (bv. fetch) — start METEEN wanneer je de functie aanroept:
async function haalOp() {
  const data = await fetch('/taken');
  // ^ het HTTP-request is hier al vertrokken, ongeacht of iemand
  //   ooit naar het resultaat kijkt.
}

// Observable (Angular's HttpClient) — start pas bij subscribe():
this.taakService.getTaken();
// ^ dit alleen doet HELEMAAL NIETS. Er is nog geen HTTP-request
//   vertrokken. Een Observable is "lui" (lazy).

this.taakService.getTaken().subscribe(taken => {
  // ^ PAS HIER vertrekt het HTTP-request, en de callback-functie
  //   loopt zodra het antwoord binnenkomt.
  console.log(taken);
});
```

!!! warning "De #1 examenfout: .subscribe() vergeten"
    Roep je enkel `this.taakService.getTaken();` aan zonder
    `.subscribe()`, dan gebeurt er visueel he-le-maal niets — geen
    foutmelding, gewoon stilte. Zie je geen data verschijnen? Check
    eerst of je `.subscribe()` (of een async pipe) gebruikt.

## Data ophalen en tonen in een component

```typescript
// taak-lijst.ts
import { Component, inject, OnInit } from '@angular/core';
import { TaakService } from '../services/taak.service';

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
export class TaakLijst implements OnInit {
  // ^ "implements OnInit" is een TypeScript-contract: het dwingt je om
  //   een ngOnInit()-methode te schrijven (zie onder).
  private taakService = inject(TaakService);

  taken: Taak[] = [];
  laden = true;
  foutmelding = '';

  ngOnInit(): void {
    // ^ ngOnInit() is een LIFECYCLE HOOK: Angular roept dit automatisch
    //   aan zodra het component klaar is met initialiseren. Dit is de
    //   juiste plek om data op te halen — NIET de constructor (die is
    //   voor DI, niet voor logica met neveneffecten zoals HTTP-calls).
    this.taakService.getTaken().subscribe({
      next: (data) => {
        // ^ loopt bij een succesvol antwoord.
        this.taken = data;
        this.laden = false;
      },
      error: (err) => {
        // ^ loopt bij een mislukt request (netwerkfout, 404, 500...).
        //   ALTIJD voorzien — anders crasht je app stilletjes bij een
        //   fout, of blijft "laden" eeuwig op true staan.
        this.foutmelding = 'Kon taken niet ophalen.';
        this.laden = false;
      },
    });
  }
}
```

```html
<!-- taak-lijst.html -->
@if (laden) {
  <p>Bezig met laden...</p>
} @else if (foutmelding) {
  <p class="fout">{{ foutmelding }}</p>
} @else {
  <ul>
    @for (taak of taken; track taak.id) {
      <li>{{ taak.titel }}</li>
    }
  </ul>
}
```

## Zelf proberen

1. Zet een simpele backend op (of gebruik `json-server` met een klein
   `db.json` bestand met een `taken`-array) op poort 3000.
2. Bouw `TaakService` met alle CRUD-methodes hierboven.
3. Toon de opgehaalde taken in `taak-lijst`, met laad- en foutstatus.
4. Test bewust een foutscenario: zet de backend tijdelijk uit en
   controleer of je foutmelding verschijnt in plaats van een stille app.

Door naar **Hoofdstuk 8: Routing**.
