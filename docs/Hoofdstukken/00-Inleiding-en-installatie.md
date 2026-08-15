<span class="badge-niveau">Niveau: absolute beginner</span>

# 0. Inleiding & installatie

## Wat is Angular eigenlijk?

Angular is een **framework** (geen bibliotheek zoals React) om
single-page applicaties (SPA's) te bouwen met TypeScript. "Framework"
betekent: Angular bepaalt in grote mate hoe je project georganiseerd is —
je krijgt structuur, maar minder vrijheid dan bij een bibliotheek.

Een Angular-app bestaat uit:

| Bouwsteen | Wat het doet |
|---|---|
| **Componenten** | Een stuk UI + de logica erachter (bv. een ticketkaartje) |
| **Templates** | De HTML die bij een component hoort |
| **Services** | Herbruikbare logica/data, los van de UI (bv. data ophalen) |
| **Routing** | Navigatie tussen "pagina's" zonder de browser te herladen |
| **Modules / standalone** | Hoe je alles samen organiseert (zie verderop) |

!!! note "Standalone components — het huidige Angular"
    Vroeger (Angular 2–16) moest je componenten registreren in een
    `NgModule`. Sinds Angular 17+ is **standalone de standaard**: elk
    component regelt zelf zijn eigen imports. Dit hele leerboek gebruikt
    de standalone-stijl, omdat dat ook is wat je examenproject gebruikt.

## Wat je nodig hebt

- **Node.js** (LTS-versie, bv. 20 of 22) — Angular draait bovenop Node's
  tooling (npm, de CLI, de build-tools).
- **npm** — komt automatisch mee met Node.js.
- Een code-editor, bij voorkeur **VS Code** (goede Angular-ondersteuning
  via extensies).

## Installatie stap voor stap

```bash
node -v
# ^ toont je Node-versie. Verwacht iets als v20.x of v22.x.
#   Geen Node? Download het via nodejs.org (kies de LTS-versie).

npm -v
# ^ toont je npm-versie, komt automatisch mee met Node.

npm install -g @angular/cli
# ^ installeert de Angular CLI globaal op je systeem.
#   "CLI" = Command Line Interface: het programma waarmee je
#   projecten aanmaakt, componenten genereert, en de app start.

ng version
# ^ bevestigt dat de CLI correct geïnstalleerd is en toont
#   welke Angular-versie de CLI standaard zal gebruiken.
```

!!! tip "'ng' is jouw beste vriend"
    Elk `ng`-commando genereert nette, foutloze boilerplate-code voor je.
    Typ zelf zo min mogelijk boilerplate — laat de CLI het werk doen. Dit
    scheelt je typfouten én tijd, wat met een deadline van 2 weken telt.

## Je eerste project aanmaken

```bash
ng new takenplanner
# ^ maakt een nieuw Angular-project met de naam "takenplanner".
#   De CLI vraagt je een paar dingen: stylesheet-formaat (kies CSS,
#   tenzij je SCSS gewend bent), en of je SSR (server-side rendering)
#   wil. Kies "No" op SSR als je dat niet nodig hebt — houdt het simpel.

cd takenplanner
# ^ ga de projectmap in. Alle volgende commando's voer je hier uit.

ng serve
# ^ start een lokale ontwikkelserver (standaard op http://localhost:4200).
#   De app herlaadt automatisch (hot reload) telkens je een bestand opslaat.
```

Open nu `http://localhost:4200` in je browser. Zie je de Angular
welkomstpagina? Dan werkt alles.

!!! warning "Poort al in gebruik?"
    Als je meerdere Angular-projecten tegelijk draait (zoals je eigen
    examenproject ernaast), botsen ze op poort 4200. Start de tweede met:
    ```bash
    ng serve --port 4300
    # ^ dwingt Angular om een andere poort te gebruiken.
    ```

## Zelf proberen

1. Maak het `takenplanner`-project aan zoals hierboven.
2. Open de map in VS Code (`code .` in de terminal, als je die shortcut
   geïnstalleerd hebt).
3. Verander in `src/app/app.html` de tekst en sla op — kijk of de browser
   automatisch ververst.

Zodra dit werkt, ga je verder naar **Hoofdstuk 1: TypeScript crash-cursus**.
