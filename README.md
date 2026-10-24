# METIS — engineering build v6.0

Questa cartella contiene la build METIS riorganizzata per avvicinare il prodotto reale alla direzione visiva approvata e, soprattutto, per eliminare dati dimostrativi dal flusso principale.

## Cosa cambia rispetto alla base precedente

- METIS è il prodotto e l'identità visiva, non una copia grafica di Risparmia Oggi.
- Home, Opportunità, Piani intelligenti, Carburante, Spesa, Energia, Ricorrenti, Salute, Documenti, Profilo, Attività e Chiedi a METIS sono schermate reali della PWA.
- Non vengono mostrati prezzi o risparmi inventati come dati reali.
- I valori di risparmio sono distinti in realizzati e stimati.
- La ricerca carburante usa un endpoint pubblico derivato dagli open data MIMIT e, se la fonte non è raggiungibile, non sostituisce il dato con numeri fittizi.
- Open Prices viene interrogato per prodotto/codice a barre quando disponibile.
- Energia usa un calcolo locale della soglia di convenienza e rimanda al Portale Offerte ARERA per il confronto ufficiale.
- Salute rimanda alle Liste di Trasparenza AIFA senza inventare prezzi o disponibilità di farmacia.
- PDF/OCR sono caricati dinamicamente solo quando l'utente utilizza la funzione.
- L'AI locale WebLLM è opzionale e parte solo tramite azione esplicita.
- Il namespace localStorage è `metis_v4_`, quindi non riutilizza automaticamente i dati locali delle build precedenti.
- Il service worker è versionato a `metis-shell-v6` e pulisce le cache precedenti.

## Fonti usate dall'app

- MIMIT — Osservaprezzi carburanti / open data carburanti.
- ARERA — Portale Offerte.
- AIFA — Liste di Trasparenza.
- Open Food Facts / Open Prices — prodotti e prezzi pubblici quando presenti.

## Limite importante

Questa è una build ingegneristica avanzata e non una dichiarazione di certificazione commerciale. Prima di una pubblicazione reale servono ancora test su dispositivi fisici, verifica dei flussi di rete in produzione, audit delle dipendenze, test dei dataset e dei casi limite, verifica legale dei servizi/fonti utilizzati e una fase di QA completa.

## Audit v3 — 24 ottobre 2026
- JavaScript, HTML e service worker sottoposti a controllo sintattico.
- Verificati tutti gli ID DOM referenziati dal codice: nessun riferimento mancante.
- Storage locale separato dal precedente namespace.
- Fetch pubblici con timeout e cache-bypass per evitare dati stale.
- Carburante: fonte dichiarata MIMIT tramite Mappa Carburanti Italia; nessun prezzo sostitutivo se la fonte non risponde.
- Open Prices: risultati marcati come dati pubblicati nella banca dati aperta.
- Service worker: cache versionata e pulizia delle versioni precedenti; API esterne non vengono cache-ate.
- AI locale: attivazione solo su HTTPS e dopo verifica effettiva di WebGPU/adapter; fallback strutturato.
- Permessi browser: camera, microfono e geolocalizzazione negati tramite Permissions-Policy.
- Nota: il test fisico su iPhone resta necessario per convalidare Safari/iOS reale; Apple documenta Web Inspector per ispezionare pagine, service worker e Home Screen web apps su iOS.


## QA v4.1
- La sezione Spesa può ora cercare prodotti pubblici per testo tramite Open Food Facts e, quando disponibile, associare prezzi pubblicati da Open Prices.
- La priorità della ricerca parte dalle marche abituali salvate; se non produce risultati, METIS ripiega sulla ricerca generica.
- La ricerca non trasforma l'assenza di un prezzo pubblico in un prezzo stimato.
- La lettura documenti distingue PDF, testo e immagini; le immagini richiedono OCR esplicito.
- PDF oltre 20 MB o 30 pagine e immagini oltre 12 MB vengono rifiutati per evitare carichi eccessivi sul dispositivo.
- L'OCR termina sempre il worker anche in caso di errore.
- I prezzi Open Prices vengono filtrati per valori numerici positivi e ordinati.
- La navigazione Home include accessi diretti alle aree principali.
- Il metadato del motore distingue correttamente elaborazione locale, inferenza cloud (disattivata) e fonti pubbliche online.

## Verifica dipendenze
- WebLLM `0.2.86` è la release corrente rilevata al 24/10/2026.
- PDF.js `6.4.299` è la release corrente rilevata al 24/10/2026.
- Tesseract.js `5.1.1` resta fissato per compatibilità con l'integrazione attuale; esistono release successive, quindi va sottoposto a test separato prima di un eventuale upgrade.


## QA v6.0 — 24 ottobre 2026
- Corretto il namespace di storage per allinearlo alla build v4/v6.
- CSP aggiornata per consentire le librerie modello WebLLM servite da `raw.githubusercontent.com`; il codice continua a non usare inferenza cloud.
- Service worker: fallback a `index.html` solo per navigazioni, evitando di restituire HTML al posto di CSS/JS quando un asset manca offline.
- Piani intelligenti: il risparmio lordo non viene più troncato a zero; se il prezzo migliore inserito è in realtà più alto, il risultato può essere negativo.
- Spesa: la ricerca prova tutte le marche abituali salvate prima del fallback generico, invece di usare solo la prima.
- Aggiunta registrazione esplicita di un acquisto nell'Attività, senza trasformarlo automaticamente in risparmio realizzato.
- WebLLM 0.2.86 confermato come release corrente al momento dell'audit.


### v6 QA
La build aggiornata rafforza allowlist delle fonti, distinzione tra soglie matematiche e risparmi, AIFA Medicinali e cache PWA.
