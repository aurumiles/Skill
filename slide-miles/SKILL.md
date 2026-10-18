---
name: slide-miles
description: SKILL SLIDE LEZIONI — Crea le slide 16:9 delle lezioni del Metodo Miles Combat System (registrate su Loom) nello stile approvato da Davide il 12 ott 2026: nero, rosso, Oswald corsivo + Inter, foto vere, poco testo, con il kit script/slide_kit.py (HTML → Chrome → PDF → PNG). Usala quando Davide chiede le slide di una lezione o di rifarle con altre foto. Per i caroselli usa `caroselli-claude`.
---

# Slide Miles

Leggi prima `memoria/wiki/slide-kit.md` (look, componenti, ricetta) e `memoria/wiki/metodo-piano-produzione.md` §3 bis (come copione, slide e checklist si legano).

## Procedura

1. **Parti dal copione** della lezione (`Lezione N_copione.docx`): la tabella «Le slide, collegate al copione» dice quali pagine servono. Per un carosello parti dal testo già approvato ([[caroselli]]).
2. Copia `script/slide_lezione1.py` in `script/slide_lezioneN.py` e cambia solo i contenuti, usando i componenti di `script/slide_kit.py`. Non reinventare CSS.
3. **Foto sempre dall'originale** (`crop()` / `extend()`): le foto stanno in `Desktop/Progetti social/Foto da poter utilizzare/` (cartelle dei match e dei servizi); trova i file originali (confronta con le copie che ti manda), ritaglia con lui al centro. Mai ridurre prima.
4. Costruisci (`python3 script/slide_lezioneN.py`) e **guarda ogni PNG**. In 16:9 niente testo in basso a sinistra (zona webcam ~380x330 px): usa `contact_sheet(..., webcam=True)`.
5. Una pagina per ogni «momento» (niente animazioni). Poche parole per pagina.
6. Aggiorna la tabella slide ↔ copione e salva in `…/Lezione N/slide/`.

## Regole del brand

- Nessuna promessa di risultato, niente linguaggio clinico; Davide si chiama Davide.
- L'epico poggia su una scena, un numero o una sua frase vera. Non inventare fatti su di lui: la cronologia corretta è in `memoria/wiki/davide-materiale-corso.md`.
- Caroselli: tema `carosello` (oro `#D8AF54`, Anton) finché Davide non decide di passare al rosso; 1080x1350.
- Python 3.9: niente backslash nelle espressioni delle f-string.
