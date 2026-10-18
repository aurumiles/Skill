---
name: sponsor-incontro
description: Aiuta a trovare e scrivere ai possibili sponsor per un incontro di pugilato di Davide, partendo dalla lista in "Social Pugilato e Costi.xlsx". Usala quando dice che ha un incontro in arrivo e deve trovare sponsor, o chiede "chi contatto per l'incontro", "aiutami con gli sponsor", "scrivi ai possibili sponsor".
---

# /sponsor-incontro

Un incontro da pro costa circa €3000 a Davide. Questa skill non trova gli sponsor al posto suo — la lista di chi contattare la fa lui, che conosce le persone — ma tiene i conti, gli ricorda a che punto è e gli prepara le bozze dei messaggi, cosa che rifare a mano ogni volta gli farebbe perdere tempo su un compito che si ripete a ogni incontro.

## 1. Guarda a che punto è

```bash
python3 script/sponsor_incontro.py list
```

Ti dà la lista con lo stato di ognuno (quota confermata, nota tipo "non vuole al momento", o "da chiedere") e il totale confermato contro l'obiettivo di €3000. Parti sempre da qui, non chiedere a Davide di ripeterti la lista se puoi leggerla.

## 2. Se la lista è cambiata, aggiornala

Quando Davide ti dà nomi nuovi, cifre confermate, o ti dice che qualcuno ha detto no, riscrivi l'intera lista con:

```bash
python3 script/sponsor_incontro.py set '[{"nome": "...", "quota": 100}, {"nome": "...", "quota": null, "nota": "da chiedere"}, ...]'
```

`set` sostituisce tutta la lista, non la somma a quella vecchia — quindi rimetti dentro anche le voci che non cambiano, prendendole da `list`. Chi non ha ancora una cifra confermata va con `"quota": null` e una `"nota"` breve (es. "da chiedere", "quota da confermare", o il motivo di un no).

Questo è lavoro meccanico (sommare, scrivere il foglio): lo fa lo script, non tu a mano dentro l'xlsx.

## 3. Il giudizio è tuo: chi contattare prima e cosa scrivere

Qui sta la parte che lo script non può fare:

- **Chi ha già detto di no o "non ora"** (dalle note) non richiamarlo con lo stesso messaggio — o lo salti, o gli scrivi qualcosa di diverso se è cambiato qualcosa (nuovo incontro, nuova visibilità).
- **Chi non ha ancora una cifra** è la priorità: sono soldi non ancora chiesti, non soldi rifiutati.
- Quando scrivi una bozza di messaggio, tienila breve e nel suo tono (`identity.md` — amichevole, diretto, non da comunicato aziendale), e adatta l'apertura al tipo di sponsor: un'attività locale (farmacia, ristorante, immobiliare) risponde a un discorso diverso da un amico o un conoscente che potrebbe sostenerlo a titolo personale.
- Se il totale confermato è già vicino o sopra l'obiettivo, dillo chiaramente invece di continuare a proporre nuovi contatti solo perché la lista non è esaurita.

## 4. Bozze, non invii

Regola del progetto, vale doppio qui: **prepari i messaggi, non li mandi.** Falli leggere a Davide prima. Se vuole, può copiarli lui o dirti di procedere — ma l'invio non parte da solo.

## Errori da non fare

| Errore | Perché è un errore |
|---|---|
| Chiedere a Davide di rifarti la lista a voce | È già nel file, leggila con lo script |
| Sommare o riscrivere l'xlsx a mano | È lavoro meccanico, per questo c'è lo script |
| Ricontattare chi ha già detto no con lo stesso messaggio | Brucia il rapporto per niente |
| Mandare un messaggio senza farlo leggere prima | Viola la regola "bozze sì, invii no" del progetto |
| Continuare a proporre sponsor quando l'obiettivo è già coperto | Fa perdere tempo a Davide e stanca i suoi contatti |
