---
name: setup
description: Riempie questa cartella la prima volta. Usala quando l'utente scrive /setup, quando apre questa cartella per la prima volta, o quando identity.md e i file in context/ sono ancora vuoti.
---

# /setup

Scrivi `identity.md` e i quattro file di `context/` partendo da materiale che parla dell'utente, non da un questionario.

**La regola che decide se questo setup funziona: non chiedere quello che puoi leggere.** Chi ha appena scaricato una cartella non ha voglia di rispondere a sei domande su di sé. Ha voglia di vedere qualcosa che funziona. Le domande sono il tappabuchi, non il percorso.

**La regola che lo rende affidabile: non inventare niente.** Se il materiale non dice una cosa, quella cosa o la chiedi o la lasci in bianco con scritto che manca. Un file pieno di roba plausibile e falsa è molto peggio di un file mezzo vuoto, perché l'utente non ha modo di accorgersene e tu ci lavorerai sopra per mesi.

## Cosa produci

| File | Cosa ci va |
|---|---|
| `identity.md` | chi è, cosa fa, come parla, e come NON vuole che gli si scriva |
| `context/azienda.md` | cosa vende, a chi, com'è fatta, come si chiamano le cose da loro |
| `context/persone.md` | chi nomina in una settimana normale: nome, ruolo, cosa serve sapere |
| `context/tool.md` | dove vivono i dati: fogli, gestionale, mail, drive |
| `context/obiettivi.md` | cosa deve succedere nei prossimi sei mesi, e cosa no |

Più due cose alla fine: le righe in `.claude/segreti.txt` e la proposta di una prima skill di lavoro.

Non toccare `CLAUDE.md`: è l'indice, è uguale per tutti e funziona già. Non toccare `context/regime-fiscale.md`: quello lo scrive l'utente con il commercialista, e tu non puoi.

---

## 0. Accendi la rete di sicurezza, in silenzio

Prima di qualsiasi domanda, controlla se la cartella è un repo git:

```bash
git rev-parse --git-dir 2>/dev/null || (git init -q && git add -A && git commit -q -m "il template, prima di toccarlo")
```

Serve perché due dei tre hook senza git non fanno niente: il commit automatico non ha dove committare, e il blocco sui segreti non ha un commit da bloccare. Una cartella appena scompattata non è un repo, quindi finché non lo fai la rete di sicurezza c'è ma è staccata.

**Non annunciarlo e non spiegarlo.** È una riga di comando, non un passaggio del setup. Se qualcosa va storto (git non installato), vai avanti lo stesso e dillo alla fine, in una riga: senza git non si torna indietro di un passo, ma tutto il resto funziona.

---

## 1. Chiedi il materiale, non le risposte

Apri così, senza preamboli e senza spiegare cosa stai per fare:

> Dammi qualcosa che parla di te. Il sito, il tuo profilo LinkedIn, l'export di una chat di lavoro, le ultime mail che hai mandato. Anche uno solo, e partiamo.
>
> Se ce l'hai, l'export di una chat vale più di tutto il resto: il sito dice come vuoi apparire, la chat dice come scrivi davvero.

Poi **fermati e aspetta**. Non fare le sei domande adesso.

Cosa fare con quello che arriva:

| Ti dà | Tu fai |
|---|---|
| Un URL di un sito | `WebFetch`. La prima volta chiede il permesso, è normale |
| Un URL LinkedIn | Provalo, ma quasi sempre torna una pagina di login: LinkedIn blocca. Quando succede non insistere, chiedi di incollare il testo del profilo |
| Un file (export, PDF, mail) | `Read`. Se è grosso, leggilo a pezzi, non a campione |
| Testo incollato | Usalo così com'è |
| "Non ho niente" | Vai al percorso a mano, qui sotto |

Se un file è enorme (un export di chat di mesi), leggi gli ultimi 200-300 messaggi. Serve come scrive adesso, non come scriveva l'anno scorso.

---

## 2. Leggi e scrivi le bozze

Da dove esce di solito ogni casella:

| Casella | Dove sta |
|---|---|
| chi è e cosa fa | sito, LinkedIn |
| l'azienda e come si chiamano le cose | sito |
| le persone | export di chat, firme delle mail |
| i tool | export di chat, e cosa vedi già collegato |
| **come parla** | **export di chat, ed è la fonte migliore che esista** |
| gli obiettivi | da nessuna parte. Questa gliela devi chiedere |

**Su come parla, sii concreto.** Non scrivere "tono diretto e professionale": non serve a niente e non lo distingue da nessun altro. Guarda e scrivi quello che vedi: lunghezza media delle frasi, se dà del tu, se apre con un saluto o entra subito, le parole che ripete, le parole che non usa mai, se usa gli elenchi puntati o scrive di seguito, se chiude con una domanda. Cita due o tre frasi sue vere dentro `identity.md`, tra virgolette. Valgono più di un paragrafo di aggettivi.

**Sui nomi propri, non tirare a indovinare i ruoli.** Se dal materiale vedi che nomina Marco venti volte ma non capisci cosa fa, scrivilo: `Marco: compare spesso, ruolo da confermare`. Poi lo chiedi in blocco alla fine.

Scrivi i file adesso, con quello che hai. Nei buchi metti una riga marcata così, **sempre su una riga sua**, mai in coda a una frase:

```
> DA CONFERMARE: chi è il secondo dentista. Il sito dice che siete in due, nella chat compare solo Andrea.
```

Come si scrivono questi file:

- **Niente `# Titolo` in cima**: il file si chiama già `persone.md`, scriverci sopra "Persone" è una riga sprecata su cinque file.
- **Prosa corta, righe che si leggono.** Niente tabelle di aggettivi, niente sezioni vuote lasciate per simmetria. Se di una persona sai solo nome e ruolo, la riga è una sola.
- **Niente link markdown fra i file.** Nominali e basta: "il dettaglio sta in `persone.md`".

---

## 3. Chiedi solo i buchi

Adesso, e solo adesso, fai le domande. **Solo su quello che nel materiale non c'era**, tutte insieme, numerate.

Una la devi fare sempre, perché nel materiale non c'è mai:

> **Cosa deve succedere nei prossimi sei mesi? E cosa invece non vuoi che succeda?**

La seconda metà è quella che serve di più: è come farai a dirgli che una cosa non vale la pena, invece di aiutarlo a farla bene. Se risponde solo alla prima, richiedi la seconda una volta.

Se i buchi sono più di quattro, hai letto male il materiale. Rileggilo prima di chiedere.

### Il percorso a mano, se non ha materiale

Sei domande, una alla volta, aspettando la risposta. **Nessuna domanda astratta**: "descrivi il tuo stile di lavoro" non riceve mai una risposta utile da chi non ha mai scritto un system prompt.

1. **Chi sei e cosa fai?** Due righe, come lo diresti a un amico al bar, non come sta sul sito.
2. **L'azienda: cosa vende, a chi, e come si chiamano le cose da voi?** I tuoi sono clienti o pazienti, i lavori sono progetti o commesse.
3. **Chi nomini più spesso in una settimana?** Nome e cosa fa, bastano tre o quattro.
4. **Dove vivono i tuoi dati?** Fogli, gestionale, mail, drive: solo i nomi, collegarli lo facciamo dopo.
5. **Cosa deve succedere nei prossimi sei mesi?** E cosa invece non vuoi che succeda.
6. **Come vuoi che ti scriva, e soprattutto cosa ti fa dire "no, non così"?**

La 3 e la 6 sono quelle che portano più valore per secondo speso: i nomi propri e i divieti di tono sono esattamente ciò che separa un output che sembra scritto da un collega da uno che sembra scritto da un'AI. Se risponde corto alla 6, aiutalo con un esempio: *"tipo: niente 'spero che questa email ti trovi bene', niente elenchi puntati, non chiamarmi dottore."*

---

## 4. Rileggi e fatti correggere

Scritti i file, **fagli vedere `identity.md`** (solo quello, non tutti e cinque) e chiedi:

> Questo ti somiglia? Cosa cambieresti?

Un giro solo di correzione, non tre. Applica quello che dice e vai avanti. Se dice che va bene, non insistere.

Questo è il momento in cui la cosa sembra viva invece che compilata. Non saltarlo.

---

## 5. La domanda sulla privacy

> C'è qualcosa che non deve uscire mai da qui? I nomi dei tuoi clienti, un progetto riservato, il tuo IBAN.

La risposta non finisce in prosa: **la scrivi in `.claude/segreti.txt`**, una riga per cosa, sotto il commento che dice "le tue". Da quel momento l'hook `niente-esce.sh` blocca ogni commit che le contiene.

Se risponde "i nomi dei miei clienti", chiedi quali e scrivili uno per riga. "I nomi dei miei clienti" come riga in un file di pattern non blocca niente, e sarebbe peggio di non averlo scritto perché lui si crede protetto.

Poi diglielo in una riga: *"Fatto. Ho messo quei nomi in `.claude/segreti.txt`, e da adesso se provo a committarli il commit si blocca da solo. Quel file non finisce su GitHub."*

---

## 6. Proponi la prima skill di lavoro

Guarda `context/obiettivi.md` e proponi **una** skill, con le sue parole:

> Hai detto che vuoi chiudere il mese senza rincorrere le fatture. Ne facciamo una skill?

Una sola, la più vicina a quello che ha scritto. Il criterio per sceglierla: **c'è un punto di giudizio che si ripete?** Se non c'è giudizio è uno script, se il giudizio non si ripete è un prompt. Solo in mezzo vale la pena.

Se dice di sì, costruiscila. Se dice dopo, chiudi e basta.

---

## Come finisce

Tre righe, non un riepilogo:

> Fatto. Ho scritto cinque file: `identity.md` e i quattro in `context/`. Aprili quando vuoi, sono tuoi e si correggono a mano.
>
> Da adesso leggo `identity.md` a ogni conversazione. Se una risposta non ti suona, il file da correggere è quello.

## Gli errori da non fare

| Errore | Perché è un errore |
|---|---|
| Fare le sei domande subito | È il questionario che stiamo evitando. Prima il materiale |
| Riempire i buchi con roba plausibile | Nessuno se ne accorge, e ci lavori sopra per mesi |
| "Tono professionale e diretto" | Vale per chiunque, quindi non vale per nessuno. Frasi sue, tra virgolette |
| Scrivere i file e non farli rileggere | Salti il momento in cui diventa suo |
| Mettere "i nomi dei clienti" in `segreti.txt` | Non è un pattern, non blocca niente, e lui si crede coperto |
| Riempire `regime-fiscale.md` | Le regole fiscali non le sai. Se le deduci le sbagli, e con sicurezza |
| Scrivere in `CLAUDE.md` | È l'indice, è già a posto |
