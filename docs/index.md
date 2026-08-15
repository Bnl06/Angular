---
title: Angular Leerboek
description: Angular vanaf 0 leren voor het herexamen Web Frameworks.
hide:
  - navigation
  - toc
---

# Angular Leerboek — vanaf 0 naar examenklaar

!!! abstract "Waarvoor dit leerboek dient"

    Dit is jouw persoonlijke leerboek om **Angular volledig vanaf 0** te leren
    voor je herexamen Web Frameworks. Elke hoofdstuk bouwt voort op het
    vorige, elk codevoorbeeld heeft **uitleg per regel** (`# zo lees je dit`),
    en elk hoofdstuk sluit af met tips en een korte oefening.

    De voorbeelden gebruiken een eigen oefenproject — een **Takenplanner** —
    zodat je de concepten eerst zelf begrijpt en toepast, en ze daarna kan
    hergebruiken in je eigen examenproject.

<div class="grid cards" markdown>

-   :material-numeric-0-box:{ .lg .middle } __Inleiding & installatie__

    ---

    Node.js, Angular CLI, je eerste project aanmaken en draaien.

    [:octicons-arrow-right-24: Start hier](Hoofdstukken/00-Inleiding-en-installatie.md)

-   :material-language-typescript:{ .lg .middle } __TypeScript crash-cursus__

    ---

    Types, interfaces, classes — de basis die Angular overal gebruikt.

    [:octicons-arrow-right-24: Naar hoofdstuk 1](Hoofdstukken/01-TypeScript-crash-cursus.md)

-   :material-folder-cog:{ .lg .middle } __Projectstructuur & CLI__

    ---

    Wat elk bestand doet, en hoe je de CLI gebruikt om te genereren.

    [:octicons-arrow-right-24: Naar hoofdstuk 2](Hoofdstukken/02-Projectstructuur-en-CLI.md)

-   :material-view-grid-plus:{ .lg .middle } __Componenten & templates__

    ---

    Je eerste component bouwen, standalone-stijl.

    [:octicons-arrow-right-24: Naar hoofdstuk 3](Hoofdstukken/03-Componenten-en-templates.md)

-   :material-sync:{ .lg .middle } __Data binding & control flow__

    ---

    `{{ }}`, `[property]`, `(event)`, en de nieuwe `@if`/`@for`/`@switch`.

    [:octicons-arrow-right-24: Naar hoofdstuk 4](Hoofdstukken/04-Data-binding-en-control-flow.md)

-   :material-swap-horizontal:{ .lg .middle } __Componentcommunicatie__

    ---

    Data doorgeven met `input()` en `output()`.

    [:octicons-arrow-right-24: Naar hoofdstuk 5](Hoofdstukken/05-Componentcommunicatie.md)

-   :material-cog-transfer:{ .lg .middle } __Services & Dependency Injection__

    ---

    Logica en state centraliseren, en waarom dat zo belangrijk is.

    [:octicons-arrow-right-24: Naar hoofdstuk 6](Hoofdstukken/06-Services-en-DI.md)

-   :material-web:{ .lg .middle } __HTTP & Observables__

    ---

    Data ophalen van een backend met `HttpClient` en RxJS.

    [:octicons-arrow-right-24: Naar hoofdstuk 7](Hoofdstukken/07-HTTP-en-observables.md)

-   :material-routes:{ .lg .middle } __Routing__

    ---

    Navigeren tussen pagina's, route-parameters, wildcard routes.

    [:octicons-arrow-right-24: Naar hoofdstuk 8](Hoofdstukken/08-Routing.md)

-   :material-form-select:{ .lg .middle } __Forms__

    ---

    Formulieren bouwen en valideren met Reactive Forms.

    [:octicons-arrow-right-24: Naar hoofdstuk 9](Hoofdstukken/09-Forms.md)

-   :material-signal-variant:{ .lg .middle } __Signals & state__

    ---

    De moderne manier van state beheren in Angular.

    [:octicons-arrow-right-24: Naar hoofdstuk 10](Hoofdstukken/10-Signals-en-state.md)

-   :material-clipboard-check:{ .lg .middle } __Praktijkcase: Takenplanner__

    ---

    Alles samenbrengen in één volledig werkend voorbeeldproject.

    [:octicons-arrow-right-24: Naar hoofdstuk 11](Hoofdstukken/11-Praktijkcase-Takenplanner.md)

-   :material-alert-decagram:{ .lg .middle } __Examentips & valkuilen__

    ---

    Veelgemaakte fouten, een checklist, en hoe je snel debugt.

    [:octicons-arrow-right-24: Naar hoofdstuk 12](Hoofdstukken/12-Examentips-en-valkuilen.md)

</div>

## Hoe dit leerboek te gebruiken

!!! tip "Lees niet enkel — typ mee"

    Angular leer je niet door te lezen, maar door zelf code te typen (niet
    kopiëren!) en te zien wat er gebeurt. Elk hoofdstuk heeft een sectie
    **"Zelf proberen"** — sla die niet over.

| Hoofdstuk | Tijdsindicatie | Focus |
|---|---|---|
| 0 – 2 | ~half dag | Opzet, TypeScript-basis, projectstructuur |
| 3 – 5 | ~1 dag | Componenten, templates, data binding |
| 6 – 7 | ~1 dag | Services, DI, HTTP/Observables |
| 8 – 9 | ~1 dag | Routing en formulieren |
| 10 – 11 | ~1 dag | Signals + alles samenbrengen in de praktijkcase |
| 12 | ~2 uur | Herhaling, valkuilen, laatste check |

**Legende bij codevoorbeelden:** overal waar het nuttig is, staat er een
commentaar in de vorm `// uitleg` of `# uitleg` direct bij de regel code die
het betreft. Lees die uitleg altijd mee — dat is de kern van dit boek.
