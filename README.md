> живе доки: назавжди (вхід у репо продукту) · розкладка — Р-7, `lens-governance:kernel/Lens_REPO_LAYOUT.md` §4

# Drive Lens

PWA для обліку пробігу й поїздок службового авто (сімейство Lens, етап 1). Один HTML-файл.
Правила роботи — у ядрі родини [`Konst-Andre/lens-governance`](https://github.com/Konst-Andre/lens-governance) (`CLAUDE.md` · `kernel/`). Цей репо самодостатній: код, канон, самері — тут.

## Де що

| тека | роль |
|---|---|
| `docs/` | **лише сайт / білд**: `Drive_Lens_preview_batch42.html` — останній білд (сайт поки не опубліковано: хостинг не підключено, 05.10.2026) |
| `lens/` | канон: `Drive_Lens_INDEX.md` (що живе) · `Drive_Lens_CHERGA.md` (відкрите) · концепт · знахідки аудиту логіки |
| `sessions/` | живі самері (стеля 2) |
| `archive/` | витіснене: старі версії концепту |

## Як почати сесію

Сесія Claude Code: **першим — адаптувати репо під каркас** за `lens-governance:tools/claude-code/ADOPT.md` (тут ще нема `CLAUDE.md`, `tools/env_check.sh`, журналу аудиту — `frame_check` покаже). Далі: `lens/Drive_Lens_CHERGA.md` цілком → найновіше самері в `sessions/`.
Гейт продукту: `python3 <lens-governance>/kernel/Lens_validate.py --product .`
