---
name: guida-miles
description: SKILL GUIDE PDF — Crea una guida gratuita o un kit in PDF (A4, nero + un colore, Oswald corsivo + Inter, foto vere di Davide, logo Aurum Miles) nello stile approvato da Davide il 24-25 ott 2026 (guida alla forza, guida alla tattica). Usala quando Davide chiede una nuova guida, un freebie o un kit in PDF, o di rifare una guida esistente nello stesso stile, anche con colori diversi o con un co-marchio (es. Lorenzo Ionni Performance).
---

# Guida Miles

Modello grafico in `script/guida_kit.py` (classe `Guida`). Esempi vivi: `script/guida_tattica.py` (guida senza co-marchio, con foto, tavole SVG e scheda stampabile) e `script/build_guida_forza.py` (guida con co-marchio e due varianti, kit e freebie). Leggi prima `memoria/wiki/guida-forza-kit.md` e `memoria/wiki/guida-tattica.md` (cosa ha chiesto Davide e perché).

## Procedura

1. **Chiarisci con Davide** (domande chiare, un po' lunghe, con esempio): tema e a chi serve; dove va (kit di una lezione, freebie su Instagram, lead magnet); colore d'accento (default rosso `#E63946`, si può cambiare); se c'è un co-marchio (un preparatore, un'altra persona: serve nome, handle Instagram e due righe per il logo); quali foto.
2. **Contenuto prima della grafica.** Scrivi il testo con le regole di sempre: ricerca verificata sugli abstract, studi detti «uno studio / circa», niente prescrizioni (i numeri sono «un esempio», sempre la frase sul medico), niente fatti inventati su Davide, vocabolario pugilistico giusto (diretto, gancio, montante; mai «a tutta»). Se la guida ha un co-autore, fa rivedere il testo a lui prima di distribuire. Pagine da ~10: copertina, «Prima di cominciare» (cosa è e cosa NON è), corpo, errori/limiti, progressi, fonti + contatti.
3. **Foto.** Scegli con provini numerati (miniature con indice) tra le cartelle di Davide (`Foto da poter utilizzare/` con i servizi Carballo e De Santis e i match pro, `Servizio Fotografico Andrea De Santis/`). Criteri: Davide in azione e di livello, boxe ben visibile, volto visibile (di spalle solo se serve), compagno di sparring non riconoscibile. Ritaglia sempre dall'**originale** (funzione `foto()` in `guida_tattica.py`) e **guarda il ritaglio**: le fasce molto sottili (rapporto > 4:1) tagliano spesso il volto. Mai la stessa foto due volte nella stessa guida.
4. **Scrivi lo script** `script/guida_<tema>.py` copiando `guida_tattica.py`: crea `Guida(...)`, ritaglia le foto, costruisci le pagine con `g.cover(...)`, `g.page(html, cls, n)`, `g.box`, `g.table`, `g.who(...)` (ultima pagina), `g.fade_photo(...)` (foto a piena pagina sfumata nel nero, sfumatura cotta nei pixel), poi `g.render(pages, uscita.pdf)`. Classe `tt` sulla pagina per il testo discorsivo. Componenti: `.card`, `.card.gold` (evidenziata), `.grid2`, `.grid3`, `.big`, `.note`, `.idx2`, `ul.pt`, `.pic`, tabelle, pagina `paper` (bianca, da stampare).
5. **Varianti.** Kit di una lezione: `tag="Kit della Lezione N · Metodo Miles Combat System™"`, `end_line` analoga, `corso=None`. Freebie su Instagram che rimanda al corso: `tag="Tratto dalla guida Metodo Miles Combat System™"`, `corso="Tutti i dettagli e maggiori informazioni li trovi all'interno del corso <b>Metodo Miles Combat System</b>."` (riquadro rosso evidente in fondo). Un lead magnet che non c'entra col corso (es. la tattica) non porta la frase sul corso. Se servono due copie, lo script le genera entrambe (vedi `build_guida_forza.py`).
6. **Controlli.** `g.problemi` deve essere vuoto (avvisa se il testo esce dal foglio o finisce sopra una foto). Poi guarda le pagine: `pypdfium2` per le miniature e **Quick Look** (`qlmanage -t -s 900`) per la copertina, che è il lettore che usa Davide. Nei PDF non usare maschere CSS per le sfumature.
7. **Consegna.** Dì a Davide dove sta il file (percorso cliccabile), cosa varia tra le copie, cosa resta da controllare (handle, revisione del co-autore). Aggiorna il wiki (pagina della guida, `index.md`, `log.md`). Niente invii né pubblicazioni.

## Marchi
- Aurum Miles: elmo con ali bianco senza sfondo nel footer di ogni pagina (`helm-wings-white.png`); logo completo con la scritta in font romano (`aurum-milesi-white.png`, in `Foto da poter utilizzare/Loghi/Logo Aurum Miles/PNG/`) in copertina e nell'ultima pagina, attaccato all'handle `@aurumiles`. Su due righe più sotto il co-marchio, se c'è (quadrato accento con le iniziali + due righe di testo + handle).
- Colori: `accent` e `bg` sono gli unici due valori da cambiare; il resto si adatta.

## Prova di regressione
Dopo ogni modifica a `guida_kit.py` rigenera la guida alla forza in una cartella temporanea e confrontala pagina per pagina con quella approvata (differenza zero): così il modello non cambia di nascosto. Non salvare la prova nel progetto.
