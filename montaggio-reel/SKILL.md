---
name: montaggio-reel
description: MONTAGGIO REEL — Monta un reel di @aurumiles dal girato col telefono al file finale (tagli, zoom sul viso, sottotitoli oro/bianco, B-roll da Pexels, grafiche animate, effetti sonori, mix) nello stile di Davide. Usala quando Davide dice «monta questo reel», «usa il prompt di montaggio del corso» o mette un video nella cartella `Video e editing`. I gusti stanno in `memoria/wiki/stile-montaggio-reel.md`: leggili sempre prima, non stanno qui.
---

# Montaggio reel

Questa skill è il **metodo**. I **gusti** di Davide (colori, font, posizioni, cosa gli piace e cosa no) stanno in `memoria/wiki/stile-montaggio-reel.md` e cambiano nel tempo: leggili ogni volta, non copiarli qui.

## Prima di cominciare
1. Leggi `memoria/wiki/stile-montaggio-reel.md` (i gusti, con date) e le «Aperto» in fondo a ogni sezione: sono domande da fare.
2. Leggi `memoria/wiki/come-lavoriamo.md` (sezione reel) e `memoria/wiki/fratture-del-video.md` (gancio, prima immagine, chiusura).
3. Il video sta in una cartella `Video e editing/` dentro la cartella del reel. **Lavora solo lì** (Davide la crea apposta; la sessione si sposta in quella cartella, con il suo ok). Crea `originali` (sola lettura), `lavoro`, `consegna` e `lavoro/stato.md`.
4. **Strumenti condivisi** in `/Users/davidemilesi/montaggio-strumenti/` (fuori dalle cartelle dei reel, per non ripesare 1,5 GB a ogni video): `modelli/` (trascrizione `ggml-large-v3-turbo.bin` e RNNoise `sh.rnnn`), `font/` (Anton, Inter), `playwright/` (libreria per le grafiche). Nel reel nuovo non si riscaricano: si collegano con `ln -s /Users/davidemilesi/montaggio-strumenti/modelli lavoro/modelli`, lo stesso per `font`, e `ln -s /Users/davidemilesi/montaggio-strumenti/playwright/node_modules lavoro/grafiche/node_modules` (con `package.json` copiato in `lavoro/grafiche`). Se il sistema chiede un ok per leggere quella cartella, spiegalo a Davide. Se mancano, avvisalo prima di scaricare.
5. I prompt del corso GabrySolution sono in `prompt/montaggio-reel-1-reel-base.md` e `prompt/montaggio-reel-2-broll-grafiche-musica.md` (testo originale, non modificarli). Seguili passo per passo, con le loro domande una alla volta; i valori dello stile vengono dalla pagina di stile.

## Il flusso (script in `script/montaggio-reel/`, da copiare in `lavoro/` e adattare)
1. **Strumenti**: `ffmpeg-full` (`/opt/homebrew/opt/ffmpeg-full/bin/ffmpeg`), whisper.cpp con modello `large-v3-turbo`, RNNoise `sh.rnnn`, OpenCV, `playwright-core` con il Chrome già installato. Dire cosa pesano e aspettare l'ok prima di installare o scaricare.
2. **Trascrizione per blocchi** (`trascrivi-segmenti.py`): silenzi con `silencedetect`, ogni blocco di parlato trascritto da solo. Tabella dei tagli da far approvare.
3. **Video**: viso con Vision (`viso.swift`), tagli e zoom (`fase1-video.py`). **Audio**: `fase2-audio.py`.
4. **Sottotitoli**: `fase3-sottotitoli.py` (gruppi scritti a mano: la parola chiave è l'ultima), `fase4-ass.py` e, con le scene a tutto schermo, `fase4b-ass-v3.py`.
5. **B-roll**: `fase5-broll.py`. **Grafiche**: `grafiche/gancio.html`, `grafiche/scene.html` con `render-*.mjs`, `mask.swift` per il ritaglio della persona; composizione con `fase6-grafiche.py`.
6. **Effetti e mix**: `fase7-sfx.py` (effetti da zero), `fase8-mix.py` (voce, effetti, eventuale musica, −14 LUFS).
7. **Testi** (`consegna/testi.md`) con affermazioni presenti nella trascrizione, nessuna pubblicazione.

## Trappole già incontrate (non rifarle)
- `ffmpeg` standard non ha i filtri `ass`, `zscale`, `tonemap`, `arnndn`: usa `ffmpeg-full`. L'opzione per i grafi da file è `-/filter_complex file`, non `-filter_complex_script`.
- Con `-ss` prima di `-i` il filtro `subtitles` sbaglia i tempi: cerca dopo l'input.
- Inserti e grafiche: pts esatti con `setpts=(N+sf)/(30*TB)`, `tpad` e `trim=end_frame`. **Verifica sempre fotogramma per fotogramma** con `psnr` verso il file precedente (l'indice `n` di `psnr` parte da 1). Le sequenze PNG con trasparenza vanno prima convertite in `.mov` (`-c:v png -pix_fmt rgba`): lette direttamente perdevano fotogrammi.
- Whisper dà tempi imprecisi sul file intero (le pause finiscono dentro le parole): ritrascrivi blocco per blocco. Il testo dei blocchi può sbagliare genere e parole («stata fortunata»): confrontalo con la ritrascrizione del file finale.
- `clip-path` con testo in Chrome smette di funzionare oltre scala ~60: usa una dissolvenza a schermo pieno alla fine dell'ingresso. Scene con elementi fermi risultano «tratti fermi»: aggiungi una deriva lenta e controlla con `freezedetect`.
- Pexels rifiuta `curl` (403): leggi le pagine con il browser dell'app. Il download dal browser dell'app **non salva il file**: Davide scarica dalla pagina e lo mette in `lavoro/musica` (o in `lavoro/b-roll`). Non aggirare i blocchi.
- Non usare `rm` con asterischi: il sistema lo blocca; cancella file precisi.
- Immagine finale: **non ricodificare** quando cambia solo l'audio (`-c:v copy`) e confronta l'hash dei pacchetti video.

## Consegna
`<titolo>_completo.mp4` in `consegna`, più `testi.md`, `fonti-immagini.txt`, `effetti-sonori.md` (e `fonti-musica.txt` se c'è musica). Tieni le versioni precedenti. Di' sempre cosa hai misurato e cosa deve controllare Davide (guardare dal telefono, ascoltare in cuffia), con il secondo preciso quando qualcosa non torna.

## Pulizia a reel finito (solo dopo l'ok di Davide, mai prima)
Si cancellano per percorso preciso (senza asterischi) i file rigenerabili: i fotogrammi di `lavoro/grafiche/scene` e `gancio`, gli audio intermedi in `lavoro/audio-mix` (si tengono `voce.wav`, `sfx.wav`, il mix finale e i `.json`), i video intermedi (si tiene il video senza sottotitoli con le grafiche). **Non si toccano mai** `originali/` e `consegna/`. Dire a Davide cosa è stato liberato. Prima volta: 23 nov 2026, ~684 MB liberati.

## Dopo il lavoro
Se Davide dà un'indicazione nuova di gusto, **aggiorna `memoria/wiki/stile-montaggio-reel.md`** (valore, data, perché) e una riga in `memoria/wiki/log.md`. Se scopri una trappola nuova, aggiungila qui sopra. Non rendere questa skill un posto dove salvare i gusti.
