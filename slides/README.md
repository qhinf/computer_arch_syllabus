# slides - Computerarchitectuur

Deze slides zijn gemaakt in [Reveal.js](https://revealjs.com/), met TLDraw extensie voor tekeningen op de slides. Zo gebruik je ze:

1. Installeer [Node.js](https://nodejs.org/en), mocht je dat nog niet hebben
2. (Optioneel) Installeer [pnpm](https://pnpm.io/), zodat dependencies wat efficienter op je schijf bewaard worden
3. `pnpm install` of `npm install`
4. `pnpm serve` of `npm serve`
5. Open <http://127.0.0.1:8000/> in je browser
   - Navigeer door de presentatie met de pijltjestoetsen of de spatiebalk
   - Toets <kbd>S</kbd> om de speaker view te openen
   - Druk op <kbd>D</kbd> of dubbelklik om op de slides te tekenen
     - Tekeningen worden standaard opgeslagen in localstorage, maar zijn ook te downloaden vanuit het hamburger menu
     - <kbd>Esc</kbd> sluit de tekenmodus
   - Gebruik <kbd>?</kbd> om alle sneltoetsen te zien

## Structuur

- `src/bijeenkomst_*.html` - HTML wrapper voor elke bijeenkomst
- `src/bijeenkomst_*.md` - Markdown content met de slides
- `src/assets/bijeenkomst_*` - Afbeeldingen en andere bestanden per bijeenkomst
- `src/index.ts` - Reveal.js configuratie
- `src/index.css` - Custom styling

## Slides maken

Gebruik Markdown voor de content:

- `***` voor horizontale slide scheidingen
- `---` voor verticale slide scheidingen (sub-slides)
- `Notes:` voor speaker notes
