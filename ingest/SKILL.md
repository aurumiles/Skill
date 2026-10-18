---
name: ingest
description: Trasforma quello che c'è in memoria/grezzo/ in pagine consultabili dentro memoria/wiki/. Usala quando l'utente dice che ha messo qualcosa nel grezzo, quando droppa un PDF o una trascrizione, o quando dice "ingesta questo", "leggi questo", "mettilo nella memoria".
---

# /ingest

Prendi una cosa grezza e falla diventare sapere consultabile. È l'operazione che fa crescere la memoria invece di farla accumulare.

**La regola che non si tocca: il grezzo non si modifica mai.** Non lo riscrivi, non lo rinomini, non lo riordini, non lo cancelli nemmeno quando è brutto o doppio. Quando fra sei mesi una pagina della wiki sembrerà sbagliata, l'unico modo per verificarla sarà tornare alla fonte. Se hai toccato la fonte, non c'è più niente a cui tornare.

## Cosa fai, in ordine

### 1. Trova la roba

Guarda `memoria/grezzo/`. Se l'utente ha indicato un file, quello. Se non ha indicato niente, prendi quello che non risulta ancora in `memoria/wiki/index.md`.

Se in `grezzo/` non c'è niente, dillo e fermati: *"Il grezzo è vuoto. Buttaci dentro qualcosa e rilanciami."*

### 2. Leggila

| Tipo | Come |
|---|---|
| `.md`, `.txt`, `.csv`, `.json` | `Read` |
| `.pdf` | `Read` legge i PDF direttamente |
| Un URL | `WebFetch` |
| Audio, video | serve una trascrizione: chiedi all'utente di metterla nel grezzo, non provare a indovinare il contenuto dal nome del file |

Se un formato non lo sai aprire, **dillo e vai avanti con gli altri**. Non fingere di averlo letto e non riassumere un file dal titolo.

### 3. Scrivi le pagine

Una fonte diventa **una o più pagine dentro `memoria/wiki/`**, non un riassunto unico. Una pagina per cosa di cui si parla: un cliente, un concetto, una decisione, un processo.

Come deve essere una pagina:

- **corta**: se supera una schermata, sono due pagine
- **autonoma**: si capisce senza aver letto la fonte
- **collegata**: nomina le altre pagine con `[[nome-file]]` dentro le frasi, non in una lista in fondo
- **con la fonte in cima**: da quale file del grezzo viene, così è verificabile

**Il nome del file è la regola che tiene su tutto.** Una pagina si chiama come il suo argomento, minuscolo e con i trattini: il cliente Rossi Srl sta in `rossi-srl.md`, e ovunque lo nomini scrivi `[[rossi-srl]]`. Il link è sempre il nome del file senza `.md`. Così seguire un collegamento vuol dire aprire quel file, senza doverlo indovinare, ed è quello che ti permette di navigare la memoria saltando da una pagina all'altra. Se il link e il nome del file non combaciano, il salto si rompe.

Questa qui sotto è la pagina `rossi-srl.md`:

```markdown
---
fonte: memoria/grezzo/call-rossi-2026-03-14.md
data: 2026-03-14
---

Rossi Srl paga a 28 giorni ed è sempre stata puntuale. Nella call di marzo
[[marco]] ha chiesto di spostare la fatturazione a fine mese perché il loro
ufficio acquisti chiude il ciclo il 30. Riguarda anche [[fatturazione]].
```

Se una pagina su quel tema esiste già, **aggiornala invece di crearne una seconda**. Due pagine sullo stesso cliente sono il modo in cui una wiki smette di servire.

**Se la fonte nuova contraddice quello che c'era, non cancellare il vecchio: scrivi che è cambiato e da quando.** "A marzo pagava a 28 giorni, da luglio è passato a 60" vale più di riscrivere "60" e basta, perché il cambiamento è a sua volta un'informazione: la prossima volta che quel cliente sfora saprai se è un'anomalia o la sua nuova normalità. Segnalare le contraddizioni è metà del motivo per cui la memoria serve.

### 4. Aggiorna l'indice e il log

Sempre, e nella stessa passata.

`memoria/wiki/index.md`: una riga per pagina, con l'aggancio. Una pagina che non è nell'indice non la ritrova nessuno.

`memoria/wiki/log.md`: una voce in cima.

```markdown
## [2026-03-14 16:20] ingest | Trascrizione call Rossi del 14 marzo
Tre pagine: [[rossi-srl]] (nuova), [[fatturazione]] e [[marco]] (aggiornate).
```

Il formato della prima riga, `## [data] tipo | titolo`, non è un vezzo: tiene il log leggibile con un comando solo (`grep "^## \[" memoria/wiki/log.md`), quando saranno cento voci.

### 5. Di' cosa hai fatto

Tre righe. Cosa hai letto, quante pagine hai toccato, e **una cosa che hai imparato e prima non c'era**. L'ultima è quella che fa capire all'utente che è servito a qualcosa.

## Gli errori da non fare

| Errore | Perché è un errore |
|---|---|
| Modificare o rinominare un file del grezzo | Perdi l'unica cosa verificabile che hai |
| Una pagina unica con tutto dentro | Non si riusa, non si collega, non si aggiorna |
| Pagine senza `[[collegamenti]]` | Diventano una cartella di file, non una memoria |
| Un link che non combacia col nome del file | Seguirlo diventa indovinare, e la navigazione si rompe |
| Riscrivere un dato vecchio sopra al nuovo senza dire che è cambiato | Perdi il cambiamento, che era l'informazione |
| Riassumere un file che non sei riuscito ad aprire | Sembra vero ed è inventato |
| Scrivere le pagine e dimenticare `index.md` | Le hai scritte per niente |
| Aggiungere una seconda pagina su un tema che ce l'ha già | Da qui in poi nessuna delle due è affidabile |
