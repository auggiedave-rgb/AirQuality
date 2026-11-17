# AQ Watcher

Web app (PWA installabile) per monitorare la qualità dell'aria in Italia, con attenzione ai siti industriali (ex ILVA di Taranto in primis).

**Versione 0.1 – prototipo grafico: tutti i dati sono simulati.**

## Provarla in locale
Apri `AQWatcher.html` nel browser (serve internet per la mappa). L'installazione sulla home funziona solo quando è pubblicata su https (Netlify).

## Struttura
- `AQWatcher.html` – l'app (HTML, CSS e JS in un unico file)
- `manifest.webmanifest`, `sw.js`, `icons/` – installazione come app
- `netlify.toml` – configurazione Netlify (la radice del sito apre `AQWatcher.html`)
- `netlify/functions/` – qui andrà il backend (fase 2)

## Pubblicazione
1. Crea un repo GitHub e carica il contenuto di questa cartella.
2. Su Netlify: *Add new site → Import from Git →* scegli il repo. Nessun comando di build, cartella di pubblicazione `.`.
3. Apri l'indirizzo Netlify dal telefono con Chrome → menu → *Installa app*.

## Roadmap
- Fase 1: Open-Meteo (previsione inquinanti e vento) + API ARPA Puglia (storico Taranto)
- Fase 2: funzioni Netlify per WAQI ed EEA (misure delle centraline, chiavi nascoste)
- Fase 3: rifinitura per il pubblico
