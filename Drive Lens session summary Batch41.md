# Drive Lens — Session Summary · Batch 40–41

**Тема:** ПЕРЕМОГА над багом safe-area — фікс висоти (40) + повернення Liquid Glass (41), обидва device-валідовано
**Governance:** wsd v2.16 · Lens_iOS_cookbook.md (через A56) · продовжує/закриває лінію Batch 36–39

-----

## Що зроблено (фінальний стан)

Двоетапний фікс, що закрив 3-чатовий баг нижньої safe-area. Обидва кроки підтверджені на девайсі (XS iOS 18 PWA — арбітр; 15 Pro iOS 26 — вторинний).

### Batch 40 — фікс висоти документа (корінь з Batch 39)

- Розділено правило: `html{min-height:calc(100% + env(safe-area-inset-top));overflow:hidden;overscroll-behavior:none}` (без `height:100%`); `body{height:100%;...}` лишилось.
- **Результат:** гола смуга знизу ЗНИКЛА. Бар сів на home-indicator. ✓ device.

### Batch 41 — повернення Liquid Glass (скло)

- `.bottom-nav`: `position:relative` (in-flow) → `position:absolute;left:0;right:0;bottom:0` усередині `#app`. **НЕ fixed** (fixed якориться до viewport → оголює зсув; absolute якориться до `#app` 100dvh).
- `.scroll-a`: повернено нижній відступ `padding:14px 14px calc(var(--nav-h,64px) + 14px)` (scroll-under clearance під overlay-бар; `--nav-h` через `measureNav`).
- **Результат:** контент скролиться під матовим баром, скло живе, зазор НЕ повернувся, desktop-width тримається. ✓ device (обидві теми).

**Поточна база коду: `Drive_Lens_preview_batch41.html`** (APP_BUILD batch:‘41’). Гейти: div 371/371, diff = тільки цільові зміни, `position:fixed` на барі = 0, measureNav/toast/капсула цілі.

-----

## Канонічні параметри (змінились цієї сесії)

- **PWA height fix:** `html{min-height:calc(100% + env(safe-area-inset-top))}` + `body{height:100%}` (розділене правило). → Cookbook **A55(1)**.
- **Liquid Glass bottom-bar:** `.bottom-nav position:absolute;bottom:0` в `#app(position:relative;height:100dvh)` + `.scroll-a` padding-bottom `calc(var(--nav-h,64px)+14px)`. → Cookbook **A55(2)**.

-----

## Governance оновлено цієї сесії

- **Cookbook A55** (новий) — «PWA standalone: модель висоти документа + overlay-бар зі склом». Два корені (висота / скло), фікси, анти, симптом-якір. + крос-рефи в індексі, A1, A10.
- **wsd v2.15** — доповнення 2.4 (ергономіка compare: керування внизу, фільтри вгорі) + прецедент 14.14 (Batch 36–41: корінь = висота, не позиціювання; міряй ПЕРШ ніж гадати; розділяй зчеплені фікси).
- **Cookbook A56** (новий) — повний копі-пейст еталон bottom-nav (скло + анімована капсула + іконки + теми + хуки).
- **wsd v2.16** — Кластер 1.7 (старт нового PWA-продукту: manifest + PWA-head + height-fix ПЕРШИМИ, до UI; прецедент 14.14).
- **Cookbook-аудит Round 1** (зроблено) — знайдено прогалини: (1) система анімацій/motion-мова — ВІДСУТНЯ; (2) sheet-меню + About + авторський кредит — ВІДСУТНЯ; (3) мікро-компоненти (toast/sync-pill/sparkline/confirm) — ВІДСУТНІ; (4) календар таба-Місяць не виділено + A50 у Частині B хоч реюзабельна. Канонізація — у наступному чаті.

-----

## Що НЕ зроблено / відкриті задачі

### HIGH — багатораундовий Cookbook-аудит + канонізація (головна місія наступного чату)

- Round 2→5 аудиту Cookbook проти batch41 (кожен раунд глибше — wsd 14.12). Канонізувати знайдене Round 1: (1) система анімацій/motion-мова; (2) sheet-меню + About + авторський кредит; (3) мікро-компоненти (toast/sync-pill/sparkline/confirm); (4) календар таба-Місяць + промоут A50.

### MEDIUM — build-задачі ПІСЛЯ аудиту (по черзі, device-арбітр)

- **Theme toggle** перебудувати під капсулу/dropdown-список (як nav-капсула + `.pk`) → оновити Cookbook A6.
- **Порт у QR/KPI:** tab-bar (A56) + theme-toggle. Палітра/кольори/іконки відрізняються → обов’язкові compare-раунди (2.4) на кожен стан. Excel→HTML (Кластер 6): batch-preview, потім звірка template.
- Drive Lens roadmap: консолідований патч профіль/налаштування шітів; JSON/CSV формат експорту.

### LOW

- Status-line семантичне кольорове кодування (відкладений мікро-поліш).
- Auth gate: Chrome-баг (повторний пароль лишає на auth-сторінці) — окрема сесія.
- Drive Lens Stage 2: Supabase auth (server-side).

-----

## Файли в проекті

**Поточні:**

- `Drive_Lens_preview_batch41.html` — **база** (height fix + Liquid Glass, device-валідовано).
- `Work_Standard.md` — wsd **v2.16**.
- `Lens_iOS_cookbook.md` — **через A56**.
- `manifest.json` — без змін.

**Legacy (історія лінії):**

- `Drive_Lens_preview_batch40_cand1a.html` — проміжний (тільки height fix).
- `Drive_Lens_preview_batch39.html` — in-flow база (зазор лишався).
- `Drive_Lens_preview_batch38.html` та раніше — до фіксу.

-----

## Уроки цієї сесії (generalizable)

- **Симптом ЧИСЛОВО = відомій величині → виміряй і зістав ЇЇ ПЕРШ, ніж гадати про механізм.** Зазор знизу = top-inset → дивись на модель висоти, не на позиціювання бару. Вимір (колориметрія) припинив 3 чати гадань за один крок.
- **Біль може бути зміщений з іншого кінця системи.** Симптом унизу (бар), корінь угорі (зсув від top-status-bar). Перш ніж патчити поверхню — спитай, чи не «не той кінець».
- **Розділяй зчеплені фікси, тестуй по одній змінній за device-раунд.** Висота й скло — різні корені; змішування палило сесії. Спершу висота (✓), потім скло (✓).
- **`absolute`-в-positioned-предку > `fixed` для overlay в standalone PWA** — `fixed` якориться до слизького viewport, `absolute` до полагодженого `#app`.

-----

## Перехід у новий чат — стартове повідомлення

```
Привіт! Прошу прочитати wsd та файли:

1. Work_Standard.md — протокол (wsd v2.16)
2. Lens_iOS_cookbook.md — каталог iOS/UI патернів (через A56; читати при iOS/PWA/UI)
3. Drive_Lens_preview_batch41.html — поточна база Drive Lens (height fix + Liquid Glass, device-валідовано) — джерело правди коду для аудиту
4. Drive_Lens_session_summary_Batch41.md — контекст попередньої сесії

Контекст: закрили 3-чатовий баг safe-area (Batch 40 height fix + Batch 41 Liquid Glass overlay-бар, обидва device-валідовано; канонізовано A55/A56, wsd 1.7/2.4/14.14). Зробили Cookbook-аудит Round 1.

ГОЛОВНА МІСІЯ цього чату: БАГАТОРАУНДОВИЙ аудит Cookbook проти реального коду batch41 (Round 2→5, кожен раунд копає глибше — підхід довів користь, wsd 14.12), і канонізувати ВСЕ знайдене. Round 1 уже виявив 4 прогалини, з них канонізувати:
  (1) A57? Система анімацій / motion-мова — драбина тривалостей (.1–.26s + пружини 400/450ms), яка easing для чого, принцип «міряй→снап на лейаут, анімуй на дію»; уніфікує A18/A27/A54/A56.
  (2) A58? Sheet-меню система (genSheet/menuAction) + About-шіт + рядок авторського кредиту (Telegram) — НЕ повне меню (в QR/KPI там список SR — адаптувати).
  (3) A59? Мікро-компоненти: toast (--nav-h offset, слайд-ап), sync-pill (max-width:0→auto), sparkline (inline-SVG), confirmAction.
  (4) Календар: короткий запис про навігацію таба-Місяць (bindMonthSwipe/monthStep/openMonthPicker) + промоутнути реюзабельні частини A50 з Частини B.
Далі Round 2→5: шукати ще не зафіксоване (банер, accent-rail-стани, focus-режими форм, тощо).

ПІСЛЯ аудиту (наступні build-задачі, по черзі, device-арбітр):
- Theme toggle перебудувати під капсулу/dropdown-список (як nav-капсула + .pk) → потім оновити Cookbook A6.
- Тоді порт у минулі Lens (QR/KPI): tab-bar (A56) + theme-toggle по аналогії. УВАГА: палітра токенів, кольори й іконки в QR/KPI відрізняються → обов'язкові compare-раунди (wsd 2.4) на кожен стан іконки/капсули. Excel→HTML пайплайн (wsd Кластер 6): правки в batch-preview, потім звірка template.

Ергономіка compare (wsd 2.4): кнопку-перемикач варіанта — ВНИЗУ (зона пальця), фільтри — вгорі.
```