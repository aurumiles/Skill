---
name: query
description: Risponde a una domanda leggendo la memoria, e offre di rimettere dentro la risposta come pagina nuova. Usala quando l'utente chiede "cosa sappiamo di X", "fammi un'analisi di X", "confronta X e Y", o qualsiasi domanda che richieda di mettere insieme più cose che sa già.
---

# /query

Rispondi usando quello che c'è in memoria, e **fai in modo che la risposta resti**. È la differenza tra una memoria che cresce e una che viene solo consultata.

## Cosa fai

### 1. Cerca

Parti da `memoria/wiki/index.md` per orientarti, poi `grep` sulla wiki per le parole della domanda. Leggi le pagine che sembrano rilevanti, e da quelle segui i `[[collegamenti]]`: di solito la risposta buona sta a due passi da dove hai iniziato, non nella prima pagina che apri.

Guarda anche `context/`, che è il posto dove sta la verità sui fatti dell'azienda. Il grezzo aprilo solo se la domanda è "cosa diceva esattamente quel documento": per tutto il resto la pagina wiki basta, ed è per questo che l'hai scritta.

### 2. Rispondi

In prosa, corta, con i `[[collegamenti]]` alle pagine da cui viene ogni pezzo. Chi legge deve poter risalire a dove l'hai preso senza chiedertelo.

**Distingui quello che sai da quello che deduci.** Se metti insieme due pagine e ne esce una conclusione che in nessuna delle due c'è scritta, dillo: *"le due cose insieme suggeriscono X, ma non c'è scritto da nessuna parte."*

**Se la memoria non basta, dillo subito e in chiaro**, prima della risposta, non in fondo:

> Su questo ho poco: c'è solo [[pagina]] e parla d'altro. Ti rispondo con quello che c'è, ma non fidarti troppo.

Rispondere bene a una domanda su cui non si hanno dati è il modo più veloce di rendere inutile tutta la memoria, perché da lì in poi non si distingue più quello che sa da quello che immagina.

### 3. Offri di rimetterla dentro

Se la risposta è venuta bene, cioè se ha messo insieme roba che prima era sparsa, chiedi:

> La salvo come pagina? Così la prossima volta parto da qui invece di rifare il giro.

Se dice di sì: scrivi la pagina in `memoria/wiki/`, aggiungila a `index.md`, e collegala dalle pagine che hai usato. **Il collegamento all'indietro è la parte che conta**: una pagina nuova che nessuno linka è una pagina che non ritroverai.

Poi una riga in `memoria/wiki/log.md`.

Non chiederlo sempre. Se la risposta era una riga presa da una pagina sola, non c'è niente da salvare: sarebbe un doppione, e i doppioni sono il modo in cui la wiki smette di essere affidabile.

## Gli errori da non fare

| Errore | Perché è un errore |
|---|---|
| Rispondere di tua scienza senza aprire la memoria | È il motivo per cui la memoria esiste |
| Non dire che i dati erano pochi | Da lì in poi non si distingue più cosa sai da cosa immagini |
| Rispondere senza `[[collegamenti]]` | Nessuno può verificarti |
| Salvare ogni risposta | La wiki si riempie di doppioni e smette di servire |
| Salvare una pagina senza linkarla da nessuna parte | L'hai scritta per nessuno |
<!-- prova -->
