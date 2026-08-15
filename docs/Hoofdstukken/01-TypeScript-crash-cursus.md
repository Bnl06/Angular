<span class="badge-niveau">Niveau: absolute beginner</span>

# 1. TypeScript crash-cursus

Angular is volledig in **TypeScript** geschreven, en dat verwacht het ook
van jou. TypeScript is JavaScript **+ types**. Je hoeft geen expert te
zijn, maar deze basis moet vast zitten voor je verder gaat.

!!! note "Al JavaScript-ervaring?"
    Scan dit hoofdstuk snel door — het belangrijkste zijn de **interfaces**
    en **types**, want die kom je overal tegen in Angular-modellen.

## Variabelen en types

```typescript
let naam: string = "Youness";
// ^ "naam" mag enkel tekst bevatten. Probeer je er een getal aan toe te
//   kennen, dan geeft TypeScript meteen een foutmelding — vóór je runt.

let leeftijd: number = 24;
// ^ getallen (zowel gehele als kommagetallen) zijn altijd "number".

let actief: boolean = true;
// ^ true of false, niets anders.

let hobbies: string[] = ["3D-printen", "coderen"];
// ^ een array van strings. De [] zegt "lijst van".

const MAX_TICKETS = 100;
// ^ "const" = mag niet meer veranderen na declaratie.
//   Gebruik altijd const, tenzij je de waarde echt moet wijzigen (dan let).
```

!!! tip "let vs const"
    Gebruik standaard `const`. Gebruik enkel `let` als je de variabele
    later effectief herbewaardeert (bv. een teller die oploopt).

## Interfaces — de vorm van je data beschrijven

```typescript
interface Taak {
  id: number;
  // ^ elke taak heeft een uniek numeriek id.
  titel: string;
  // ^ verplicht veld: de titel van de taak.
  status: 'open' | 'bezig' | 'klaar';
  // ^ een "union type": status mag ENKEL één van deze 3 exacte
  //   tekstwaarden zijn. Typ je "Open" met hoofdletter? Foutmelding.
  prioriteit: number;
  // ^ bv. 1 t.e.m. 5.
  omschrijving?: string;
  // ^ het "?" betekent: dit veld is OPTIONEEL, mag ontbreken.
}
```

Een `interface` beschrijft enkel de **vorm** van data — het genereert geen
code, het is puur een contract voor de compiler. Dit is exact hoe je
examenproject `ticket.model.ts` is opgebouwd (met `Ticket` in plaats van
`Taak`).

```typescript
const mijnTaak: Taak = {
  id: 1,
  titel: "Angular leren",
  status: 'bezig',
  prioriteit: 5,
};
// ^ dit object voldoet aan de "Taak"-vorm: alle verplichte velden zijn
//   ingevuld, "omschrijving" mag ontbreken want dat is optioneel.
```

## Functies met types

```typescript
function verhoogPrioriteit(taak: Taak, stap: number): Taak {
  // ^ deze functie verwacht een Taak en een number, en GEEFT een Taak terug
  //   (dat "Taak" na de haakjes is het return-type).
  return { ...taak, prioriteit: taak.prioriteit + stap };
  // ^ "..." (spread operator) kopieert alle velden van taak, en
  //   overschrijft enkel "prioriteit" met de nieuwe waarde.
  //   Zo verander je nooit het originele object (immutability).
}
```

## Classes

```typescript
class TakenTeller {
  // ^ een class is een blauwdruk om objecten mee te maken.
  private aantal: number = 0;
  // ^ "private" = enkel bruikbaar binnen deze class zelf.

  constructor(startWaarde: number = 0) {
    // ^ de constructor loopt automatisch bij het aanmaken van een
    //   nieuw object via "new TakenTeller(...)".
    this.aantal = startWaarde;
  }

  verhoog(): void {
    // ^ "void" = deze methode geeft niets terug.
    this.aantal++;
  }

  get totaal(): number {
    // ^ een "getter": je kan dit oproepen als een eigenschap
    //   (teller.totaal), niet als een functie (teller.totaal()).
    return this.aantal;
  }
}
```

!!! tip "Angular componenten zijn gewoon classes"
    Elk Angular-component is een TypeScript `class` met een `@Component`
    decorator erboven. Als classes hierboven logisch aanvoelen, is de stap
    naar componenten in Hoofdstuk 3 klein.

## Async/await (nodig voor HTTP-calls later)

```typescript
async function haalTakenOp(): Promise<Taak[]> {
  // ^ "async" betekent: deze functie geeft een Promise terug — een
  //   belofte van een waarde die er ASYNCHROON (later) aankomt.
  const response = await fetch('/api/taken');
  // ^ "await" wacht op het resultaat zonder de rest van het programma
  //   te blokkeren. Dit zie je in Angular terug bij Observables
  //   (Hoofdstuk 7), al werkt Angular daar iets anders mee.
  return response.json();
}
```

## Zelf proberen

Maak een bestand `oefening.ts` (los van je Angular-project, gewoon om te
testen) en schrijf:

1. Een `interface Boek` met `titel`, `auteur`, en een `gelezen: boolean`.
2. Een functie `markeerAlsGelezen(boek: Boek): Boek` die een kopie
   teruggeeft met `gelezen: true`.
3. Test dat de types werken door expres een fout veld in te vullen — zie
   je de foutmelding van TypeScript?

Klaar? Door naar **Hoofdstuk 2: Projectstructuur & CLI**.
