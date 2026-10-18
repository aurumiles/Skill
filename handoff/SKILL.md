---
name: handoff
description: Chiude una sessione di lavoro scrivendo su disco un file di passaggio (handoff) che la sessione successiva legge per ripartire esattamente da dove eravamo, al posto di /compact. Usala quando l'utente scrive /handoff, o quando dice frasi come "salvami il punto", "prepara la ripresa per domani", "chiudiamo qui", "riprendiamo domani".
---

# Handoff

Chiude una sessione di lavoro scrivendo su disco un file che la sessione dopo legge per ripartire esattamente da dove eravamo. Si usa **al posto di** `/compact`, mai insieme.

## Perché non `/compact`

`/compact` per scrivere il riassunto rilegge tutta la conversazione, e poi **sostituisce** la cronologia con quel riassunto. Il prefisso cambia, quindi la cache che era già stata costruita muore, e da lì in poi ogni turno la ricostruisce sopra un riassunto di quaranta turni. Non compra risparmio, compra continuità e la fa pagare.

Un file di handoff fa il contrario su tre fronti:

1. **Costa meno.** Il file sono qualche centinaio di token in scrittura e altrettanti in lettura, e la sessione nuova riparte da un prefisso piccolo e pulito.
2. **Sceglie l'utente cosa sopravvive.** `/compact` decide da solo e butta via la cosa più preziosa, cioè le strade già provate che non hanno funzionato. Nessun riassunto automatico le conserva.
3. **Resta su disco.** La terza sessione sullo stesso lavoro sa cosa hanno fatto la prima e la seconda. Un compact vive e muore dentro la sua sessione.

## Procedura

**1. Ricostruisci da solo, non chiedere quello che puoi vedere.** Cosa è stato toccato, cosa è stato provato, cosa ha fallito e perché, quali decisioni sono state prese, dove ci si è fermati: sono tutte cose che stanno nella conversazione. In un repo git aggiungi `git status --short` e `git log --oneline -5`.

**2. Chiedi solo i due buchi che non puoi dedurre.** Sono l'obiettivo (cosa si sta costruendo e **perché**, non cosa si è fatto oggi) e il prossimo passo voluto, che non è sempre il prossimo passo ovvio. Se mancano, chiedili **in una domanda sola**, con la tua ipotesi già dentro, così che basti rispondere "sì". Tutto il resto non si chiede: se una sezione è vuota si scrive `nessuno` e si va avanti, senza inventare problemi risolti che non ci sono stati.

**3. Decidi dove scriverlo.** In un repo di codice: `docs/handoff/{YYYY-MM-DD}-{slug}.md`. In un vault o una cartella di note (come questo stesso progetto, `il-tuo-os`): una cartella `handoffs/` dentro il progetto a cui il lavoro appartiene. Lo `{slug}` sono due o tre parole sul lavoro, non sulla data. Se esiste già un handoff con lo stesso slug **non sovrascriverlo**: il file nuovo incrementa un campo `seq` e mette il precedente in `prev`, così la catena si legge all'indietro.

**4. Scrivi il file** con questo frontmatter — `type: handoff`, `date`, `status`, `seq`, `prev`, `tags` — e queste sezioni, in questo ordine:

- **Obiettivo**: cosa si sta costruendo e perché, due o tre righe, la ragione inclusa.
- **A che punto siamo**: lo stato reale adesso. Cosa funziona, cosa è a metà, cosa non è iniziato.
- **Cosa abbiamo provato che NON ha funzionato**: la sezione più importante del file. Ogni strada scartata con il motivo. Serve a impedire che la sessione dopo la riprovi da capo.
- **Problemi incontrati e come li abbiamo risolti**: solo quelli che si ripresenterebbero. Sintomo, causa, correzione.
- **Decisioni prese**: cosa è stato scelto **e cosa è stato scartato**, con una riga di perché. Una decisione senza l'alternativa scartata si ridiscute da zero la settimana dopo.
- **File toccati**: percorsi, con una riga su cosa è cambiato in ognuno.
- **Dove vogliamo andare**: il prossimo passo, scritto all'imperativo. Poi quello dopo, se è già chiaro.
- **Da sapere prima di toccare qualcosa**: trappole, comandi che vanno lanciati in un certo modo, cose che sembrano rotte e non lo sono. Se non ce ne sono, `nessuna`.

**5. Chiudi stampando in chat la riga di ripresa**, pronta da incollare in una sessione nuova: `Leggi {percorso} e riprendi da "Dove vogliamo andare".` Più una riga per dire che a questo punto si può fare `/clear` e che la ripresa costerà solo quel file.

## Quando NON si usa

Una skill che si usa sempre è una skill che si usa a caso.

- Cambio di task senza continuità → `/clear`, non serve nessun handoff.
- Si è presa una strada sbagliata negli ultimi turni → `/rewind`, che torna a un punto che la cache conosce già.
- Si riprende fra poco e senza aggiornamenti di mezzo → `/rename` più `/resume`.
- Il lavoro continua un altro giorno o un'altra settimana → **questa skill**.

Questa skill non lancia `/clear` al posto dell'utente: lo suggerisce e basta.
