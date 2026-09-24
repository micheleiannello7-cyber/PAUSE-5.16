# PAUSE — Product Requirements (Preview)

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
