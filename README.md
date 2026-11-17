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


## Fase 1: dati reali
- La dashboard legge la previsione oraria da Open-Meteo (modello Copernicus CAMS, griglia ~11 km) e il vento da Open-Meteo. Se il servizio non risponde, mostra una schermata di errore con "Riprova" e non valori di riserva.
- Le misure giornaliere delle centraline ARPA Puglia (Taranto e Statte) sono lette in modo opzionale: se il servizio non risponde, i marker restano grigi.
- Lo storico è nascosto finché non sarà collegato ai dati validati ARPA (Fase 2, con funzione Netlify e cache).
- Per vedere l'interfaccia con valori simulati: aggiungere `?sim=1` all'indirizzo.

## Misure delle centraline (aggiornamento)
Le misure di Taranto/Statte ora arrivano dal servizio pubblico EEA (European Air Quality Index, file JSON orari per centralina, CORS aperto, nessuna chiave): niente backend necessario. Solo valori misurati (modelled=0), non validati, ritardo di alcune ore, cache 15 min. ARPA diretta (cloud.arpa.puglia.it) non è più usata perché non risponde in modo affidabile. La centralina Orsini (rete ADI) non è nel feed EEA e resta grigia.

## Storico e statistiche (Taranto e Statte)
Pagina reale, senza dati simulati: serie giornaliere degli ultimi giorni (medie delle misure orarie, solo valori misurati, almeno 12 ore per giorno), picco orario, confronto tra centraline, e statistiche dell'anno in corso calcolate dall'EEA (`stats/IT/<codice>.json`: giorni per livello, mese per mese, giorno peggiore). Dati non validati, non adatti a verificare i limiti di legge. Altre località: "non ancora disponibile". Storico validato ARPA previsto in seguito.

## Chiamate a Open-Meteo
Il servizio gratuito (solo uso non commerciale, CC BY 4.0) limita le chiamate per indirizzo IP e per applicazione: meno di 10.000 al giorno, 5.000 all'ora, 600 al minuto. Le richieste partono dal telefono di ogni utente (nessun proxy). L'app le riduce con una cache in memoria e in localStorage (modello e vento: 30 minuti; vento sui 12 impianti: 60 minuti), non aggiorna in background quando la pagina è nascosta, e non usa mai dati più vecchi del TTL. Un proxy Netlify con cache è un'opzione futura, ma concentrerebbe tutte le richieste sugli IP di Netlify e quindi non serve finché si resta sulle chiamate dirette.
Se l'app diventasse commerciale (abbonamenti o pubblicità) serve il piano a pagamento di Open-Meteo.
