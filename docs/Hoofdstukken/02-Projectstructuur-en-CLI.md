<span class="badge-niveau">Niveau: beginner</span>

# 2. Projectstructuur & CLI

## Wat elk bestand/map doet

```text
takenplanner/
├── src/
│   ├── app/
│   │   ├── app.ts          # Het hoofdcomponent (root) — startpunt van de UI
│   │   ├── app.html         # De template (HTML) van het hoofdcomponent
│   │   ├── app.css          # Styling van het hoofdcomponent
│   │   ├── app.config.ts    # Globale configuratie: providers (Http, Router...)
│   │   └── app.routes.ts    # De lijst van routes (URL's) van je app
│   ├── index.html           # De ENIGE echte HTML-pagina — Angular injecteert
│   │                         #   hier de hele app in via <app-root>
│   ├── main.ts               # Het echte startpunt: bootstrap van de app
│   └── styles.css            # Globale (app-brede) CSS
├── public/                   # Statische bestanden (afbeeldingen, favicon...)
├── angular.json               # Build-configuratie van de CLI
├── package.json               # Dependencies (welke npm-pakketten je gebruikt)
└── tsconfig.json              # TypeScript-compiler instellingen
```

!!! note "Waar begint alles?"
    `main.ts` roept `bootstrapApplication(App, appConfig)` aan. Dat laadt
    het `App`-component uit `app.ts`, dat op zijn beurt in `index.html`
    verschijnt via de tag `<app-root></app-root>`. Dit is de volledige
    opstartketen — je hoeft dit zelf zelden aan te passen.

## De CLI gebruiken om te genereren

De CLI genereert foutloze, consistente boilerplate. Gebruik dit altijd
in plaats van bestanden manueel aan te maken.

```bash
ng generate component tickets/taak-kaart
# afgekort: ng g c tickets/taak-kaart
# ^ maakt een nieuwe map "tickets/taak-kaart" met 3-4 bestanden:
#   taak-kaart.ts (de class), taak-kaart.html (template),
#   taak-kaart.css (stijl), en evt. een test-bestand.
#   Het pad "tickets/" bepaalt gewoon in welke map het terechtkomt —
#   dat is puur organisatie, geen technische vereiste.

ng generate service services/taak
# afgekort: ng g s services/taak
# ^ maakt taak.service.ts aan: een class met @Injectable erboven,
#   klaar om logica/data-ophaling in te zetten (zie Hoofdstuk 6).

ng generate interface models/taak
# afgekort: ng g i models/taak
# ^ maakt een leeg .ts-bestand met een interface-skelet.
#   Voor kleine projecten typen de meesten dit gewoon manueel (sneller).
```

!!! tip "Twijfel je aan een commando?"
    ```bash
    ng generate --help
    # ^ toont alle "schematics" (generator-types) die de CLI kent:
    #   component, service, interface, guard, pipe, directive, ...
    ```

## Hoe een standalone component eruitziet

```typescript
// taak-kaart.ts
import { Component } from '@angular/core';
// ^ "Component" is een decorator-functie uit Angular's core-package.

@Component({
  selector: 'app-taak-kaart',
  // ^ de HTML-tag waarmee je dit component ELDERS gebruikt:
  //   <app-taak-kaart></app-taak-kaart>

  imports: [],
  // ^ standalone-componenten declareren HIER expliciet wat ze nodig
  //   hebben (andere componenten, directives, pipes). Leeg als je
  //   (nog) niets extra's gebruikt.

  templateUrl: './taak-kaart.html',
  // ^ verwijst naar het aparte HTML-bestand met de template.

  styleUrl: './taak-kaart.css',
  // ^ verwijst naar de aparte CSS voor dit component (scoped: geldt
  //   enkel binnen dit component, lekt niet naar de rest van de app).
})
export class TaakKaart {
  // ^ de class-naam is standaard PascalCase, gebaseerd op de bestandsnaam.
  //   Dit is exact hetzelfde patroon als in jouw examenproject
  //   (bv. TicketListComponent).
}
```

!!! warning "selector-conflicten"
    Elke `selector` moet uniek zijn in je hele project. Krijg je een
    foutmelding over een dubbele selector? Check of je niet per ongeluk
    twee keer hetzelfde component gegenereerd hebt.

## npm scripts die je vaak gebruikt

```bash
npm start
# ^ meestal een alias voor "ng serve" — check package.json > scripts.

npm run build
# ^ bouwt een productieversie (geminificeerd, geoptimaliseerd) naar
#   de map "dist/". Gebruik je NIET tijdens gewoon ontwikkelen.

npm test
# ^ start de unit tests (Karma/Jasmine of Vitest, afhankelijk van setup).
```

## Zelf proberen

1. Genereer met de CLI een component `tickets/taak-lijst` in je
   `takenplanner`-project.
2. Genereer een service `services/taak`.
3. Open `app.routes.ts` (nog leeg op dit punt) en kijk alvast hoe die er
   in jouw examenproject uitziet (`app.routes.ts` in
   `wf_2526_ZT1_reg_opgave`) — dat komt terug in Hoofdstuk 8.

Door naar **Hoofdstuk 3: Componenten & templates**.
