---
name: caroselli-canva
description: SKILL CAROSELLI CANVA — Crea un carosello Instagram per @aurumiles dentro Canva (create-design / copy-design di #8), stile deciso con Davide il 24-25 set 2026: font Anton/Roboto/OpenSauce, oro su nero, una foto vera di Davide solo in copertina. Usala SOLO quando Davide chiede esplicitamente «in Canva» o di clonare/adattare un carosello già fatto in Canva. Per i caroselli nello stile delle lezioni (rosso su nero, Oswald, fatti qui da Claude) usa la skill `caroselli-claude`.
---

# Caroselli Canva (ex carosello-aurum)

> **Dal 13 ott 2026** Davide preferisce lo stile delle slide della Lezione 1 e vuole replicarlo per tutti i caroselli: quelli si fanno con la skill `caroselli-claude` (nessun Canva). Questa skill resta per i caroselli in Canva.

Sostituisce il vecchio metodo (template Canva copiato a mano, font Anton su foto+fascia scura). Quello resta buono come stile di riferimento per le **copertine dei reel** ([[copertine-reel]]), non più per i caroselli. Sostituisce anche la skill globale `ig-carousel` (Higgsfield/Nano Banana): quella genera immagini AI e testo bruciato dentro, non è più il nostro flusso.

**Prima di aprire questa skill**, il contenuto (testi, studi, tono) si prepara come descritto in [[caroselli]] (memoria/wiki): 5-6 domande a Davide, studi veri verificati, voce in "tu" con Davide sempre impersonale (mai "io"). Questa skill parte da lì e si occupa solo della **produzione grafica**.

## Perché questo metodo e non quello vecchio

`edit-design` (lo strumento usato per le copertine dei reel) è chirurgico: modifica un elemento che esiste già, non può mai scegliere un font o un layout per qualcosa di nuovo. `create-design` è diverso: gli si manda un **brief scritto** e **il motore AI di Canva stesso** (lo stesso di "Magic Design") decide font, layout, icone, gerarchia — non io a mano. Il risultato è un design vero, con una grafica diversa per ogni slide (mirino, numeri giganti, checklist, cerchi numerati), non un collage. Il prezzo: **arriva sempre con difetti da controllare** — per questo la Fase 4 qui sotto non è opzionale.

## Fase 1 — Il brief

Un unico brief testuale (non un outline strutturato — quello vale solo per `design_type: presentation`, che qui non usiamo) con:

- **Format**: `"Instagram Post"` (dà 4:5, 1080x1350 — confermato in questo progetto).
- **Tono**: registro **cavalleresco/romano moderato**, deciso con Davide 25 set 2026 dopo una ricerca — gladiatore/spartano è il cliché più saturo del settore fitness/combattimento (Gladiator Gym Gear, Spartan Race...), il cavaliere è meno abusato ed è coerente con quanto già scritto in [[cammino-guerriero]] (Musashi/Bushido = codice individuale, non disciplina di massa di una legione). **Non ancora fissato al 100%** — Davide ha detto che gli piacciono entrambi i registri e vuole trovare "il giusto mix" col tempo: chiedi conferma se non è passato molto tempo dall'ultima volta, altrimenti riproponi lo stesso mix (Anton/Roboto/OpenSauce, oro su nero, nessun elmo/scudo/gladio esplicito, solo dettagli discreti come numeri romani).
- **Colori**: sfondo nero/antracite, testo bianco, **un solo accento oro chiaro** (zona `#c29342`–`#d8af54`, varia leggermente slide per slide perché generato dall'AI — non è un bug, non serve uniformarlo).
- **Foto**: **SOLO la copertina ha una foto** (deciso da Davide 24 set: le altre slide restano grafiche/tipografiche, niente foto — così non serve gestire tante immagini diverse). Nel brief scrivi "SOLO la copertina ha una foto... le altre 7 pagine sono grafiche/tipografiche, senza foto".
- **Struttura**: elenca ogni slide come `PAGINA N: ...` con etichetta, titolo, corpo, e note in corsivo dove c'è uno studio citato. L'ultima pagina è la chiusura con la CTA "salvataggio" (icona + due righe, vedi Fase 4).
- **Segnale di scorrimento**: chiedi un indicatore di pagina "1/8"…"8/8" + freccia su tutte le pagine tranne l'ultima (te lo dà già così, poi in Fase 4 si converte in numeri romani).
- **Logo/handle**: "AURUM MILES" piccolo, discreto, su ogni pagina.

Chiama `create-design` con questo brief, `user_intent` breve, nessun `outline` (siamo su `Instagram Post`, non `Presentation`).

## Fase 2 — Aspettare la generazione

`create-design` è asincrono: torna un `job_id` + `continuation_token` e uno `status: pending`.
1. Aspetta `polling_policy.wait_seconds` (di solito 10-15s) prima del primo poll.
2. Chiama `get-create-design-async-job` con `job_id` + l'ultimo `continuation_token`.
3. Ripeti finché `status: completed` — di solito 4-6 giri, **non accorciare l'attesa** (la stessa risposta torna identica, non accelera nulla). Usa `Bash` con `sleep` per aspettare, non chiamate ravvicinate.
4. Il risultato dà `design.id` e `design.url` — quello è il carosello da controllare e correggere.

## Fase 3 — Controllo qualità (sempre, senza eccezioni)

Prima di mostrare qualsiasi cosa a Davide, leggi **tutte** le pagine (`read-design`, `design_content` senza `page_indices` per vedere tutto il testo in un colpo) e controlla:

### a) L'ordine fisico delle pagine
Il generatore a volte crea le pagine in un ordine diverso da quello scritto sopra ogni slide ("N/8"). Confronta il testo di ogni pagina con l'etichetta che porta; se una slide è fuori posto, riordina con:
```
merge-designs, type: modify_existing_design, operations: [{type: move_pages, from_page_numbers: [N], to_after_page_number: M}]
```
**Un'operazione per chiamata** — il tool rifiuta batch multipli.

### b) Slide mancanti
Il generatore a volte salta una pagina del brief (è già successo: 7 pagine invece di 8, mancava la chiusura). Se manca:
1. `copy-design` della pagina più simile per struttura (es. l'ultima "correzione" per una chiusura, non una pagina con un numero-fantasma tipo "01" tema-specifico).
2. Riscrivi i testi con `replace_text` sui locator esistenti (**mai `add_text` per titoli** — perde il font scelto dal generatore, vedi nota tecnica sotto).
3. Cancella gli elementi decorativi che non servono più (es. la fila di icone checklist se diventa una CTA) con `delete_element`.
4. Inserisci la CTA finale (icona + due righe, vedi sotto).
5. Reinserisci la pagina al posto giusto con `merge-designs` → `insert_pages`.

### c) Testo "fantasma" nello sfondo
**Bug ricorrente di questo generatore**: il testo originale (prima di qualsiasi `replace_text`) a volte resta "cotto" dentro l'immagine di sfondo della pagina, non solo nell'elemento di testo modificabile — sostituire il testo non lo fa sparire dal render. Si vede come una scritta sbiadita dietro il titolo o il corpo.
**Fix**: `insert_shape` con un rettangolo pieno (path `M0,0 H<w> V<h> H0 Z`), colore vicino al nero della pagina (`#0e0e0e` o `#131110` vanno bene), posizionato sopra tutta la zona sporca — **poi sempre** `layer_element` con `position: "back"` sullo stesso locator, così la toppa sta sopra lo sfondo raster ma sotto titolo/corpo/badge (che restano elementi separati, visibili). Se il fantasma continua a vedersi più in basso (es. vicino a una lista), allarga la toppa con `resize_element` finché non resta traccia — controllalo sempre col render prima di considerarla chiusa.

### d) Ordine di sovrapposizione (z-order) in generale
Ogni elemento nuovo (`insert_shape`, `add_text`) si inserisce **sempre in cima** (davanti a tutto). Se aggiungi qualcosa che deve stare *dietro* al testo esistente (come le toppe sopra), serve sempre `layer_element: back` dopo. Se invece un elemento esistente (logo, titolo, numero pagina) è finito dietro a qualcosa che hai aggiunto tu, usa `layer_element: front` su di lui.

### e) Numeri di pagina in romano
Il generatore fa "1/8", "2/8"… Sostituisci con "I / VIII", "II / VIII"… fino a "VIII / VIII" sull'ultima. Il testo romano è più lungo: quasi sempre va **allargato il box** (`resize_element`, +100/150px di width) e **riposizionato a sinistra** (`position_element`) perché non sconfini nella freccina di fianco — controlla sempre left dell'icona-freccia sulla stessa pagina e lascia il box del numero flush prima di lei. **L'ultima slide non ha la freccia** (non c'è più nulla da scorrere): lascia solo "VIII / VIII" senza toccare altro.

### f) Font coerente
Il generatore assegna font diversi a elementi diversi. **Non posso scegliere un font per un elemento nuovo o già esistente** — nessuna operazione lo permette (`format_text` cambia solo size/peso/colore/corsivo, mai la famiglia). L'unico modo: Davide seleziona il testo in Canva (link dell'`edit_url`) e sceglie il font a mano, **una volta per elemento**; da lì lo riciclo su altri elementi con `replace_text`, che preserva sempre il font dell'elemento di destinazione. Se un carosello nuovo esce con font incoerenti tra slide, è normale — chiedi a Davide di fare lo stesso giro rapido che ha già fatto (titoli in un font, corpo in un altro, etichette piccole in un terzo), poi correggi tu dimensioni/spaziatura.

### g) Foto di copertina
Se Davide manda una foto sua: `create-upload-url` (non `upload-asset-from-url`, quello vuole un URL pubblico) → una `POST` diretta del file all'URL monouso ricevuto → `mediaId` → `update_fill` sul rect-foto della copertina. Il ritaglio automatico di Canva di solito è già buono su foto verticali o quadrate; controllalo col render prima di aggiustare a mano con `crop_media`. Le altre slide restano senza foto (vedi Fase 1).

## Fase 4 — La CTA finale (slide di chiusura)

Standard fissato con Davide (22-25 set 2026), da riprodurre uguale ogni volta:
- Icona a segnalibro (`insert_shape`, path `M0,0 H34 V44 L17,30 L0,44 Z`, colore oro) **sulla stessa riga** della prima riga di CTA, non sopra.
- **Riga 1** (l'azione, es. "SALVALO PRIMA DEL PROSSIMO SPARRING."): maiuscola, grassetto, NON corsivo, colore oro.
- **Riga 2** (il di più, es. "Mandalo a chi deve salire la prossima volta."): minuscola tranne l'iniziale, corsivo, stesso oro — un gradino più leggero. **Attenzione al testo**: non restringere troppo il target (es. non "chi sale per la prima volta" se il messaggio deve valere per chiunque torni sul ring, non solo i principianti — dipende dal contenuto specifico, chiedi a Davide se il primo abbozzo sembra troppo stretto).
- Blocco **staccato con un vero spazio verticale** dal paragrafo sopra (~150px), non a ridosso — altrimenti sembra la continuazione della stessa frase.
- Due righe = **due elementi di testo distinti** (non un `\n` dentro uno solo), perché ognuna porta una formattazione diversa.

## Fase 5 — Consegna

1. `export-design`, formato `png`, `width: 1080, height: 1350`, senza `pages` (tutte insieme) una volta che la Fase 3 è chiusa.
2. Scarica ogni PNG e salvali in `Caroselli elaborati/AAMMGG_Titolo/1.png … 8.png` (stessa cartella del testo, `slide_`, `caption_`, `note_` — vedi [[caroselli]] e [[produzione-contenuti]]).
3. Manda un contact sheet o le 8 immagini a Davide **prima** di considerarlo definitivo — è sempre un giro di correzioni, non dare per approvato al primo export.
4. **Controllo pre-pubblicazione**: conta i PNG esportati (N) e nel messaggio di consegna ricorda esplicitamente a Davide di controllare che tutte le N slide siano in coda su Instagram prima di premere pubblica (es. "Sono N slide, 1.png...N.png — controlla che ci siano tutte prima di pubblicare"). È già successo (carosello #8) che una slide restasse fuori per dimenticanza in caricamento, senza che nessuno se ne accorgesse prima della pubblicazione — non ha inciso sulla resa quella volta, ma è un controllo a costo zero.
5. Solo dopo il suo ok: log in `memoria/wiki/log.md`, aggiorna lo stato in [[caroselli]].

## Standard di layout — fissato da Davide il 2 ott 2026 su #12 (vale per ogni carosello da ora in poi)

Dopo il clone-di-#8 (vedi sotto), Davide ha corretto a mano su Canva una serie di dettagli che tornano identici su ogni slide "a header" (etichetta + numerazione romana + freccia in alto, tipo "Tecnica N"). Queste sono le misure/criteri fissi da applicare **da subito**, non da riscoprire slide per slide:

- **Numerazione romana allineata all'etichetta**: il `top` del numero romano deve coincidere (o quasi) col `top` dell'etichetta accanto ("Tecnica 2", "Perché", ecc.), non stare più in alto o più in basso per conto suo. Stessa riga, stessa altezza visiva.
- **Freccia**: font/dimensione `~32-36px`, colore sempre `#c29342` (oro standard, vedi sotto). La freccia sta subito dopo il numero romano con un piccolo distacco pulito (~15-20px), mai a ridosso.
- **Divisore orizzontale sotto l'header**: quando c'è (slide con riga sotto l'etichetta), farlo quasi a tutta larghezza (ordine di `560-930px`), non un tratto corto — taglia pulito sotto l'intero blocco etichetta+numerazione, non solo sotto l'etichetta.
- **Corpo testo in bold**: il corpo testo (non il titolo, non le citazioni) va in `font_weight: bold`, non `normal`/`medium` — resa più piena e leggibile su sfondo nero. Paragrafi brevi e consecutivi (es. "frase 1:" + "frase 2.") si possono accorpare in un unico elemento di testo invece di tenerli separati, se logicamente sono una frase sola spezzata in due righe.
- **Bianco mai puro**: `#ffffff` va sostituito con `#f1f1f1` su testo bianco e logo "Aurum Miles" — leggermente meno duro contro il nero.
- **CTA finale (icona+2 righe)**: l'icona di salvataggio e la prima riga di testo ("SALVALO...") condividono lo **stesso `top`** (stesso rigo, non icona-più-in-alto-testo-più-in-basso), con ~25-30px di distacco orizzontale tra il bordo destro dell'icona e l'inizio del testo.

### Bug confermato: `resize_element` su icone a fill-immagine
Non risolvibile lato nostro: ridimensionare via API un'icona con fill-immagine (frecce, badge) la ritaglia invece di scalarla (vedi nota in [[caroselli]]). **Quando serve una freccia/icona più piccola, non usare `resize_element`**: o la si lascia alla dimensione nativa e si lavora sulla posizione, o — se Davide la sistema a mano in Canva — si nota che lì il ridimensionamento funziona bene (l'editor vero scala il contenuto correttamente, è solo l'operazione API che ha il bug). Segno concreto che qualcosa si è rotto così: un elemento vicino a un testo con `textAlign: end` che sembra "capovolto" — probabile frammento di icona rotta sovrapposto, non un bug nel testo.

## Note tecniche sparse (da non riscoprire ogni volta)

- `merge-designs`: un'operazione per chiamata, sempre.
- `resize-design` (quello del vecchio metodo con template Canva) è a uso limitato/trial — non serve più con questo flusso, `create-design` genera già al formato giusto.
- Se serve conservare una versione precedente come "punto a cui tornare" prima di sperimentare uno stile nuovo, dillo esplicitamente e tieni traccia del `design.id` vecchio nel log — non extrapola da solo quale sia "la versione buona" tra tentativi diversi.
