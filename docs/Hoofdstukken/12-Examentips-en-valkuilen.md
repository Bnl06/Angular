<span class="badge-niveau">Niveau: herhaling</span>

# 12. Examentips & valkuilen

## De 10 meest gemaakte fouten (en hoe je ze herkent)

| # | Fout | Symptoom | Oplossing |
|---|---|---|---|
| 1 | Component niet in `imports: []` gezet | Tag verschijnt niet / compile-fout | Voeg de class toe aan `imports` van het component dat 'm gebruikt |
| 2 | `.subscribe()` vergeten | Niets gebeurt, geen foutmelding | Elke Observable moet gesubscribed worden om iets te doen |
| 3 | `track` vergeten in `@for` | Compile-fout | `@for (x of lijst; track x.id)` — altijd verplicht |
| 4 | Signal zonder haakjes lezen | TypeScript-fout of raar gedrag | `signal()`, `input()`, `computed()` altijd met `()` aanroepen |
| 5 | `provideHttpClient()` vergeten | "No provider for HttpClient" | Toevoegen in `app.config.ts` > providers |
| 6 | Route-volgorde fout (`:id` vóór een letterlijk pad) | Verkeerde pagina laadt | Specifieke paden altijd VÓÓR `:parameter`-paden |
| 7 | `ReactiveFormsModule` niet geïmporteerd | `formControlName` werkt niet | Toevoegen aan `imports` van dat component |
| 8 | State direct muteren i.p.v. vervangen | UI update niet | Gebruik `.filter()`/`.map()`/spread `{...}` i.p.v. `.push()`/direct wijzigen |
| 9 | Foutafhandeling vergeten bij HTTP | App crasht stil bij een fout | Voorzie altijd een `error`-callback in `.subscribe({ next, error })` |
| 10 | Route-parameter als getal gebruiken zonder conversie | Vergelijkingen kloppen niet | Route-params zijn altijd `string` — gebruik `Number(...)` |

!!! danger "Als je maar 1 ding onthoudt"
    **Lees je terminal en browserconsole altijd eerst.** Angular's
    foutmeldingen zijn meestal heel precies (bv. "NG0201: No provider
    for HttpClient"). 90% van de examenstress komt van paniekeren bij
    een fout i.p.v. hem gewoon te lezen.

## Snel debuggen — een vaste volgorde

1. **Terminal** — bouwt `ng serve` nog steeds? Rode tekst = compile-fout,
   meestal een ontbrekende import of typefout.
2. **Browserconsole** (F12) — runtime-fouten (bv. "Cannot read property
   'x' of undefined") verschijnen hier, niet in de terminal.
3. **Network-tab** — bij HTTP-problemen: zie je het request vertrekken?
   Welke status komt terug (404, 500, CORS-fout)?
4. **`console.log()`** — plaats tijdelijk in `.subscribe()` of `ngOnInit`
   om te zien of je code sowieso bereikt wordt en wat de data bevat.

## Checklist vlak voor het examen

- [ ] Ik snap het verschil tussen `{{ }}`, `[ ]`, `( )`, `[( )]`
- [ ] Ik kan een standalone component genereren en correct importeren
      in een ander component
- [ ] Ik kan `@if`/`@for`/`@switch` foutloos schrijven, inclusief `track`
- [ ] Ik kan `input()` en `output()` gebruiken tussen ouder en kind
- [ ] Ik snap waarom een service `providedIn: 'root'` een singleton is
- [ ] Ik kan een volledige CRUD-service schrijven met `HttpClient`
- [ ] Ik snap het verschil tussen een Observable en een Promise
- [ ] Ik kan routes correct ordenen, inclusief `:id` en een wildcard
- [ ] Ik kan een Reactive Form bouwen met validatie en foutmeldingen
- [ ] Ik kan een formulier vooraf invullen met `patchValue` voor een
      "bewerken"-scenario

## Tijdsbeheer tijdens het examen zelf

!!! tip "Werk in kleine, testbare stappen"
    Bouw en test **één ding tegelijk**: eerst de route werkt, dan de
    data verschijnt, dan pas de styling. Probeer nooit alles in één
    keer te schrijven en dan pas te testen — dan weet je bij een fout
    niet meer waar die vandaan komt, en dat kost tijd die je met een
    strakke deadline niet hebt.

!!! tip "Vastzitten? Terug naar de basis"
    Krijg je een onbegrijpelijke fout? Vraag jezelf in volgorde af:
    is het component geïmporteerd? Is de provider toegevoegd in
    `app.config.ts`? Klopt de naam van het veld exact tussen
    TypeScript en HTML (hoofdlettergevoelig)? De meeste examenfouten
    zijn kleine tikfouten, geen conceptuele misverstanden.

---

Dit was het volledige leerboek. Ga terug naar de
[startpagina](../index.md) om een hoofdstuk te herhalen, of begin nu je
eigen examenproject met deze basis stevig onder de knie.
