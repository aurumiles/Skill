---
name: caroselli-claude
description: SKILL CAROSELLI CLAUDE — Crea un carosello Instagram per @aurumiles direttamente qui, senza Canva, nello stile delle slide della Lezione 1 (nero, Oswald corsivo + Inter, accento rosso o altro colore, foto vere ritagliate dagli originali, poco testo, 1080x1350). Usala quando Davide chiede un nuovo carosello senza specificare Canva, o dice «fammelo come le slide della lezione». Per i caroselli in Canva usa `caroselli-canva`.
---

# Caroselli Claude

Stile scelto da Davide il 13 ott 2026: **quello delle slide della Lezione 1** («bellissimo, vorrei replicarlo per tutti i caroselli»). Al massimo cambia il colore d'accento da un carosello all'altro. Il sistema è lo stesso delle lezioni: `script/slide_kit.py`, descritto in `memoria/wiki/slide-kit.md`.

## Prima: il contenuto

Si prepara come in `memoria/wiki/caroselli.md`: parte da un **bisogno**, non da un argomento; 5-6 domande a Davide, studi veri verificati (con stato di verifica), voce «tu» con Davide impersonale (tranne «viaggio dell'eroe» e «recap di un'esperienza»), 8 slide circa (errore + correzione), massimo 13-14. Slogan e CTA in caption. Niente promesse di risultato, niente linguaggio clinico.

## Poi: la grafica (questa skill)

1. **Foto**: la copertina (e al massimo una o due altre) con una foto vera di Davide. Si parte **sempre dall'originale** che Davide indica sulla scrivania (trovalo confrontando la copia che ti manda); ritaglia con `crop()` in 4:5 con lui al centro, 2160 px di larghezza.
2. **Componenti 4:5** del kit: `c_cover` (foto a tutto schermo, titolo in basso), `c_point` (etichetta, numero, titolo, testo), `c_compare` (errore / correzione), `c_list` (elenco numerato), `c_number` (numero enorme e didascalia, per dati e studi), `c_statement` (frase sola, enorme), `c_cta` (slogan, invito, profilo). Dentro i titoli `{parola}` = parola in accento.
3. **Colore d'accento**: `build(..., accent="rosso")` di default; `"oro"`, `"azzurro"`, `"verde"` o un hex. Chiedi a Davide solo se vuole cambiarlo.
4. Scrivi `script/carosello_NN.py` copiando `script/slide_lezione1.py` e usando `fmt="4:5"`. Output: PDF e PNG in una cartella del carosello, una pagina per slide.
5. **Guarda ogni PNG** (anche `contact_sheet`). Controlla: titoli corti (3-7 parole), righe che non vanno a capo male, nessun testo troppo vicino al bordo, numero pagina in basso a destra, il conteggio delle slide (come nella skill Canva).
6. Consegna i PNG numerati (1080x1350) nella cartella del carosello; la pubblicazione e la caption seguono `caroselli.md`.

## Regole

- Mai fatti su Davide inventati: la sua cronologia corretta è in `memoria/wiki/davide-materiale-corso.md`.
- Python 3.9: niente backslash dentro le espressioni delle f-string; Chrome headless deve poter scaricare i font Google.
- I componenti carosello vanno rifiniti sul primo carosello vero: se qualcosa non va, correggi il kit (non la singola pagina) e annotalo in `slide-kit.md`.

## Il carosello deve capirsi da solo (regole di Davide, 16 ott 2026, dopo #21)

Nessuno spiega le slide: non c'è una persona che parla, come nelle lezioni del Metodo Miles. Chi le legge in feed vede una slide alla volta, senza il resto. Il modello è il carosello fatto con Canva `Caroselli pubblicati/261002_Il minuto prima fa più paura del match/` (guardalo prima di scrivere): stessa lunghezza e stessa struttura di frasi, con il design nuovo del kit.

1. **Ogni slide si legge da sola.** Titolo = frase completa con il soggetto (non «NON SI ABITUANO TUTTE», ma «LE PARTI DEL CORPO NON SI ABITUANO TUTTE»). Un elenco, un confronto o un numero hanno sempre sopra la frase che dice cosa sono.
2. **Mai un dato senza dire cosa misura e perché c'entra.** Se la prova viene da un test di laboratorio che non è il tema (per #21: il test dell'acqua gelata), dillo in una riga («quanto resisti con la mano nell'acqua gelata») e spiega il concetto che dimostra (soglia / tolleranza) prima del numero.
3. **Frasi di senso compiuto, collegate.** Ogni frase risponde a «perché?» senza aiutare a voce. Se una frase ha bisogno di una spiegazione, aggiungila. Una frase = una riga distinta (`body` come lista, un elemento per frase), non frasi di seguito dopo il punto.
4. **Lunghezza come i caroselli Canva:** titolo 3-8 parole, corpo 20-35 parole in 2-3 frasi. La frase d'impatto finale va staccata dal resto con una riga vuota (`""` in `c_statement`).
5. **Etichetta (kicker) chiara e di tema**, come «PERCHÉ», «L'ERRORE», «IN SINTESI», senza numeri (il numero pagina c'è già). La copertina non ha etichetta: non inventare diciture («PUGILATO · IL CORPO» è stata una mia invenzione, tolta).
6. **Nessuna slide vuota:** sotto il testo una grafica correlata al discorso: `g_bars` (barre), `g_cards` (due numeri a confronto), foto (`img=` in `c_point`), icona. Se il bisogno è solo di riempire, prima chiediti se la slide serve.
7. **Chiusura con salvataggio:** `c_cta(..., save=(azione, rinforzo))`: icona segnalibro, azione in maiuscolo, riga di rinforzo in corsivo. Il carosello è 4:5, 1080×1350, uguale ai Canva.
8. **Prima di consegnare, rileggi ogni PNG come un follower che non ha visto le altre slide.** Se una slide ha bisogno di quella prima per capirsi, è scritta male.

### Dettagli di scrittura e di design confermati da Davide su #21 (17 ott 2026)

- **Definizione + conseguenza esplicita.** Dopo ogni definizione scrivi in una riga a parte, con freccia rossa, cosa ne deriva: «Soglia: quando un colpo comincia a far male. → la soglia resta praticamente uguale. / Tolleranza: quanto riesci a sopportarlo. → la tolleranza sale.» Non lasciare che il lettore colleghi da solo (`<br>` + freccia `&rarr;` nel colore d'accento).
- **Un dato di laboratorio ha tre pezzi:** cosa si è misurato (frase in maiuscolo sopra il grafico), il grafico, e una riga sotto che dice cosa dimostra («è la prova che la tolleranza aumenta»). È l'argomento `note` di `g_bars`.
- **Etichetta di ogni slide** (LA DIFFERENZA, COSA CAMBIA, LA TRAPPOLA, IN SINTESI…) più grande, 40 px: è il senso della slide e si deve vedere, ma resta più piccola del titolo. Il kit lo fa già.
- **Freccia «scorri» rossa** accanto al numero pagina su tutte le slide tranne la copertina (che ha «Scorri →») e l'ultima: lo fa `html_document` per `fmt="4:5"`.
- **Elenchi numerati:** i numeri 01-04 stanno al centro verticale della riga di testo (`c_list`).
- **Slide di impatto** (`c_statement`): testo centrato in verticale nella pagina, frase finale staccata da una riga vuota.
- **Foto che spiega:** se c'è un'azione (il colpo preso), usa la foto di quell'azione: soggetto (Davide, il volto colpito) in primo piano, non l'intera scena. Ritaglio con `crop()` dall'originale; il file e il percorso si dicono sempre a Davide in chat (nome + percorso completo), perché li incolla in altre chat.

### Consegna: caption, accessibilità, cartella (Davide, 17 ott 2026)

- **Struttura fissa della caption (Davide, 17 ott 2026, vale per tutte): emoji a inizio di ogni paragrafo; poi `🗡️ Continua il cammino.` / `Segui il metodo.` (lo slogan, due righe); poi `📌` salvataggio («Salvalo prima del… Mandalo a chi…») su un paragrafo suo; poi `💬` domanda ai commenti su un paragrafo suo; poi gli hashtag. Mai l'hashtag del brand o della palestra (`#aurummiles`, `#accademiaboxeostiamare`…): solo hashtag di contenuto, massimo 5.**
- **Caption con emoji come i reel** (🥊 gancio, 🧪 📍 ⚠️ 🛡️ per i paragrafi, 🗡️ slogan, 📌 CTA, come in `Reel fatti/` e nel #12): file `caption_<titolo>.txt`, testo e poi hashtag (max 5).
- **Testo alternativo per l'accessibilità**: file `accessibilita_<titolo>.txt` con una descrizione per ogni slide, scritta per chi non vede (colori, immagini, ogni testo riportato per intero, grafici e icone descritti), sotto i 420 caratteri ciascuna, da copiare nel campo «Testo alternativo» di ogni immagine. Instagram oggi accetta fino a circa 1.000 caratteri (l'indicazione storica era 100).
- **Cartella:** il carosello nasce in `Caroselli elaborati/AAMMGG_Titolo/`; quando Davide lo pubblica si sposta in `Caroselli pubblicati/` e il nome prende **la data di pubblicazione**. Aggiorna `OUT` dello script (`script/carosello_NN.py`), il backlog e `log.md`. (Per i caroselli non esiste una cartella «fatti»: l'equivalente di `Reel fatti/` è `Caroselli pubblicati/`.)

## Lingua (regola di Davide, 21 ott 2026)
Mai «a tutta» o altre frasi gergali incomplete nelle slide, nei caroselli e nelle caption: scrivere «al massimo», «alla massima velocità». Controllare prima di consegnare. Vedi `memoria/wiki/italiano-corretto-no-a-tutta.md`.
