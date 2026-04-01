# InvoicePilot — Design Document

> Этот документ содержит всё о визуале и UX продукта.
> Читать AI-дизайнеру перед любыми изменениями.

**Продукт:** https://invoicepilot-black.vercel.app
**Код:** `products/002-freelance-payment-agent/app/`
**Стили:** `app/app/globals.css`

---

## Общая концепция

**Стиль:** Dark, минималистичный, профессиональный. Похож на Linear/Vercel.
**Аудитория:** Фрилансеры — они привыкли к инструментам типа Notion, Linear, Figma.
**Настроение:** "Умный рабочий инструмент", не стартап-лендинг.
**Ширина контента:** max-width 720px, по центру. Не full-width.

---

## Цвета

### CSS переменные (в globals.css)

```css
--bg: #0A0A0F          /* Основной фон страницы — почти чёрный */
--surface: #111118     /* Фон карточек (card) */
--elevated: #1A1A28    /* Фон инпутов, hover состояния */
--border: #1E1E30      /* Границы карточек и инпутов */

--accent: #6366F1      /* Indigo — основной цвет кнопок, акцентов */
--accent-hover: #4F52D9 /* Indigo темнее — hover кнопок */

--text: #F0F0FF        /* Основной текст — почти белый */
--muted: #8B8FA8       /* Второстепенный текст, лейблы */
--dim: #525472         /* Третьестепенный — подсказки, мелкий текст */

--success: #22C55E     /* Зелёный — успех */
--warning: #F59E0B     /* Жёлтый/янтарный — предупреждение */
--danger: #EF4444      /* Красный — ошибка, финальное уведомление */
```

### Цвета просрочки инвойса (динамические)
```
1–29 дней  → #60A5FA (синий)     — "ещё терпимо"
30–59 дней → #F59E0B (янтарный)  — "внимание"
60+ дней   → #EF4444 (красный)   — "критично"
```

### Цвета версий письма
```
😊 Friendly    → #60A5FA (синий)
💼 Firm        → #F59E0B (янтарный)
⚠️ Final Notice → #EF4444 (красный)
```
Граница карточки письма тоже меняет цвет: `borderColor: '#60A5FA40'` (с прозрачностью 25%)

---

## Типографика

**Шрифт:** System font stack — `-apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`
Специального шрифта нет — использует системный шрифт (SF Pro на Mac, Segoe UI на Windows).

### Размеры текста
```
22px / 700  — заголовок страницы (InvoicePilot)
18px / 700  — суммы ($3,200), цены на странице upgrade
16px / 600  — заголовки форм ("New Invoice", "InvoicePilot Pro")
15px / 400  — основной текст, названия клиентов
13px / 400  — второстепенный текст (описание проекта, лейблы)
12px / 600  — бейджи версий писем ("😊 Friendly")
12px / 400  — подсказки, мелкий текст
10px / 400  — "days" под цифрой просрочки
```

---

## Компоненты

### .card
```css
background: #111118
border: 1px solid #1E1E30
border-radius: 12px
padding: 24px
```
Используется везде: форма добавления, каждый инвойс, письма.

### .btn-primary (основная кнопка)
```css
background: #6366F1
color: white
padding: 11px 20px
border-radius: 8px
font-size: 15px
font-weight: 600
hover: background #4F52D9
disabled: opacity 0.5
```
Примеры: "+ Add Invoice", "✨ Write Email", "Save Invoice"

### .btn-ghost (второстепенная кнопка)
```css
background: transparent
color: #8B8FA8
padding: 11px 20px
border: 1px solid #1E1E30
border-radius: 8px
hover: background #1A1A28, color #F0F0FF
```
Примеры: "Sign out", "Copy", "Cancel"

### input / textarea
```css
background: #1A1A28
border: 1px solid #1E1E30
border-radius: 8px
padding: 10px 14px
font-size: 15px
color: #F0F0FF
focus: border-color #6366F1
```

---

## Страницы

### / — Главная (список инвойсов)

**Структура:**
```
[Header]
  InvoicePilot (22px bold)
  AI writes your follow-up emails (13px muted)
  [+ Add Invoice] [Sign out]

[Form — если открыта]
  .card с полями: Client name | Amount ($)
                  Days overdue
                  Project (optional)
  [Save Invoice]

[Invoice list]
  Для каждого инвойса:
    .card flex row:
      [Бейдж дней]  [Имя клиента + проект]  [$сумма]  [✨ Write Email]

    Если письма сгенерированы — 3 карточки письма под инвойсом:
      .card с цветной границей + бейдж тона + [Copy]
      pre с текстом письма
```

**Бейдж просрочки:**
```
Маленький блок 64px ширина
Фон: цвет с opacity 15% (пример: #EF444415)
Граница: цвет с opacity 40%
Цифра: 18px / 800 — цвет просрочки
"days": 10px / muted
```

**Пустое состояние:**
```
.card по центру, padding 48px 24px
📋 (36px emoji)
"No invoices yet. Add your first overdue invoice above."
```

---

### /login — Страница входа

**Структура:**
```
Центр экрана, max-width 400px

InvoicePilot (24px bold)
"Sign in to manage your invoices" (14px muted)

.card с формой:
  Лейбл "Your email"
  input[type=email] placeholder "you@example.com"
  [Send magic link]
  "No password needed — we email you a sign-in link." (12px dim)
```

**После отправки — success state:**
```
.card текстом по центру, padding 40px 24px
📬 (40px emoji)
"Check your email" (18px bold)
"We sent a magic link to **email**. Click it to sign in — no password needed."
```

---

### /upgrade — Страница оплаты

**Структура:**
```
Центр экрана, max-width 440px

🚀 (40px emoji)
"Upgrade to Pro" (26px bold)
"You've used your 2 free invoices. Upgrade for unlimited AI follow-up emails."

.card:
  [InvoicePilot Pro] [$39 /month]
  ✓ Unlimited invoices
  ✓ AI writes 3 email versions per invoice
  ✓ Tone adapts to how overdue it is
  ✓ Cancel anytime

[Upgrade now — $39/month] (full width, 16px, 14px padding)
"Secure payment via Stripe. Cancel anytime." (12px dim)
← Back to invoices (link, 13px muted)
```

---

## Layout и отступы

```
Контейнер: max-width 720px, margin 0 auto, padding 40px 20px
Между секциями: marginBottom 24-32px
Между карточками в списке: gap 12px
Между письмами: gap 8px, marginTop 8px от инвойса
Лейблы над инпутами: marginBottom 6px
Между полями формы: gap 12px
Grid двух колонок в форме: gap 12px
```

---

## Функциональность (для дизайнера)

### Главная страница — flow
1. Если не залогинен → показывает hero с кнопкой "Get started" → редирект на /login
2. Если залогинен → показывает список инвойсов пользователя
3. "+ Add Invoice" → открывает форму прямо на странице (не отдельная страница)
4. "✨ Write Email" → кнопка меняется на "✍️ Writing..." → через ~3 сек появляются 3 карточки писем под инвойсом
5. "Copy" → копирует текст, меняется на "✓ Copied!" на 2 секунды
6. "Sign out" → выходит, возвращает на главную (→ редирект на /login)

### Лимит бесплатного тарифа
- 2 инвойса бесплатно
- При попытке добавить 3-й → редирект на /upgrade
- На /upgrade → Stripe Checkout → оплата → возврат на главную с `?upgraded=1`

### Magic link
- Пользователь вводит email → получает письмо → кликает ссылку → автоматически залогинен
- Нет паролей вообще

---

## Что можно улучшить (идеи для дизайнера)

- [ ] Добавить статус инвойса (Pending / Paid) — кнопка "Mark as paid"
- [ ] Счётчик сгенерированных писем ("You've generated 12 emails this month")
- [ ] Onboarding tooltip для новых пользователей
- [ ] Мобильная версия — бейдж дней и кнопка в column layout
- [ ] Анимация появления писем (fade in)
- [ ] Тёмная/светлая тема переключатель (сейчас только dark)

---

*InvoicePilot Design Doc | Март 2026*
