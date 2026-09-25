# PAUSE — Product Requirements (Preview)

> ⚠️ **OBBLIGATORIO PRIMA DI QUALSIASI INTERVENTO:** leggere e rispettare
> [`/app/memory/CONSTITUTION.md`](./CONSTITUTION.md) — la Costituzione tecnica permanente
> e vincolante di PAUSE (regola "minimum change", niente riscritture, niente rigenerazione
> di contenuti/asset, niente AI a runtime, identità visiva dark-navy/cyan/glass).
> Lingua dell'utente: italiano.

## Summary
Mobile Expo app (React Native + FastAPI + MongoDB) that turns idle moments into curiosities/mini-lessons with an intentional pause between sessions.

## Preview setup (current session)
- Codebase copied from user-uploaded ZIP (PAUSE-5.15) into `/app`.
- Backend: FastAPI on `:8001`, MongoDB local, requirements installed.
- Frontend: Expo SDK 57, yarn install, expo running on `:3000` behind Kubernetes ingress.
- Env: `.env` files preserved (preview URLs); backend `.env` extended with `EMERGENT_LLM_KEY` (free) and `ENFORCE_LIMIT="false"`.
- Emergent LLM key configured; Stripe/ElevenLabs left blank (integrations idle).

## Visual change
- Topics tab (`app/(tabs)/explore.tsx`) redesigned to mirror the onboarding "topics" step:
  cinematic dark-navy gradient with orbs, cyan-glow title, sparkles hint card,
  and the glass CategoryGrid (`glass` prop). Behaviour (interest toggle,
  auto-save, LimitBadge, active-count) unchanged.

## Home deck & badge (sessione corrente)
- `src/components/home-story-deck.tsx`: cambio card immediato (commit su UI thread via `runOnUI`), molla più rapida
  (damping 22 / stiffness 190); tocco e nuovo swipe consentiti anche durante l'animazione; zoom "in arrivo"
  guidato dal verso di scorrimento (`travel`). Comportamento timeline/elastico ai bordi/idle sway invariato.
- `src/components/story-meta-chips.tsx`: badge tipo · categoria · durata uniti in una sola pillola centrata,
  orologio 3D (`assets/images/kind-clock.png`, lo stesso di StoryInfoGrid) al posto dell'icona Ionicons.
- Stile "glass" sulle card storie della Home: proposto e BOCCIATO dall'utente → non applicare.

## Copertine — ripresa richiesta dall'utente (25 settembre 2026)
- Richiesta: «Riprendi il lavoro di creazione / correzione copertine finché c'è credito», confermata; procedere autonomamente, rispondere in italiano.
- Baseline effettiva: **437 contenuti, 427 con copertina, 10 mancanti**. Tutte le esclusioni editoriali storiche risultano già sostituite; non riapplicare cancellazioni massive.
- Backup metadati e impronte dei contenuti: `memory/cover_batches/resume_baseline.json`.
- Generazione esistente: `backend/generate_covers.py --concurrency 1`, Gemini `gemini-3.1-flash-image-preview`, prompt approvati in `cover_prompt_overrides.json`, arresto su budget/quota. Nessuna modifica a UI, API, testo, audio o preferenze utente.
- Batch corrente: `memory/cover_batches/a08de6ae79d149199e4b0a75fc7b3477.json`; log `resume_generation.log` nella stessa cartella. Originali WebP in `backend/covers/`, hero 1200px e thumb 600px nello storage gestito.
- Revisione non distruttiva delle immagini effettive tramite `catalog_cover_sheets.py --allow-missing`; contatti e snapshot in `memory/cover_review_final/`. Sostituire soltanto errori confermati, dopo confronto visivo, conservando i vecchi asset.
- P0 completato e verificato: 10 mancanti completate, 17 copertine esistenti corrette (7 rigenerate e 10 ritoccate senza AI); una delle 10 nuove, l'orbita, ha ricevuto anche una correzione scientifica. Totale **27 contenuti aggiornati**, **437/437 con copertina**.
- Arresto effettivo: il provider ha risposto **Budget has been exceeded** al primo elemento di `corrections_editorial_02.json`. Nessun retry a pagamento; candidati generati in quel batch: 0. Non rilanciarlo automaticamente.
- P1 bloccato dal credito: 9 correzioni già motivate e pronte in `memory/cover_batches/corrections_editorial_02.json` (migrazione, due storie api, gladiatori, melodie, evoluzione, Zanzibar, spezie, caricatore). Conservate le copertine precedenti, nessun buco nel catalogo. Dettagli/backup in `memory/cover_batches/RESUME_REVIEW.md`.
- P2: nessuna modifica funzionale aggiuntiva richiesta.
- Verifica finale: **8/8 test backend PASS**, confronto SHA-256 non-cover invariato per tutti i 437 documenti, 54 risposte media hero/thumb HTTP 200 WebP per i 27 ID aggiornati. Home, categorie e 5 lettori verificati in anteprima mobile 390×844 senza errori console. Report `test_reports/iteration_1.json`, suite `backend/tests/test_iter33_cover_resume_readonly.py`. Nessuna chiamata AI nei test, nessun processo di generazione rimasto attivo. Verifica fisica su dispositivo dell'utente non eseguita.
