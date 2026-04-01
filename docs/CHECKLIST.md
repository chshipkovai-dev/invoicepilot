# FreelancePaymentAgent — Master Checklist

> Главный чеклист. Отмечай каждый пункт по мере выполнения.
> Статус: 🟢 MVP задеплоен, валидация идёт (13 марта 2026)

---

## Прогресс

```
Phase 0: Подготовка / анализ   ██████████ 100% ✅
Phase 1: Валидация             ████░░░░░░  40% 🟡
Phase 2: MVP Build             ██████████ 100% ✅
Phase 3: Запуск                ░░░░░░░░░░   0% ⏳
Phase 4: Рост до $1k MRR       ░░░░░░░░░░   0% ⏳
```

**MRR сейчас:** $0
**Цель:** $1,014 MRR (26 клиентов × $39)
**Waitlist лендинг:** https://business-os-alpha-rust.vercel.app/waitlist
**Продукт (MVP):** https://invoicepilot-black.vercel.app
**GitHub продукта:** https://github.com/chshipkovai-dev/invoicepilot
**Emails собрано:** 1 (тест) | Цель: 30

---

## ✅ Phase 0 — Подготовка (завершено)

- [x] Исследование рынка (Reddit 3200+ фрилансеров, боль подтверждена)
- [x] Анализ конкурентов (HoneyBook, Bonsai, FreshBooks — нет AI писем)
- [x] Выбор стека (Next.js + Supabase + Claude API + Stripe + Vercel)
- [x] Определена цена ($39/мес)
- [x] Определён MVP scope
- [x] Создана документация

---

## 🟡 Phase 1 — Валидация (идёт)

**Цель: 30 signups на waitlist**

### Лендинг ✅
- [x] Название: InvoicePilot
- [x] Лендинг создан (Next.js /waitlist в business-os)
- [x] Сбор emails → Supabase таблица `waitlist`
- [x] Задеплоен: https://business-os-alpha-rust.vercel.app/waitlist

### Reddit 🟡
- [x] Пост в r/freelance опубликован (13.03.2026, ждёт одобрения модератора)
- [ ] Пост в r/webdev
- [ ] Пост в r/Entrepreneur
- [ ] Пост в r/graphic_design

**Финальный текст поста:**
```
Title: Anyone else just... stop following up on invoices because it feels too embarrassing?

Body:
I tracked my late payments last year and nearly threw up.
Lost around $8k — not because clients refused to pay,
but because I stopped chasing after the 2nd reminder.

There's something psychologically brutal about sending
"hey just checking in on that invoice again 😅" for the
third time. It feels like begging. So I just didn't.

Client owed me $2,400 for 3 months. I literally never
followed up after email #2. Eventually wrote it off.

Anyway. Just needed to vent. Starting to actually track
this stuff properly now.
```

### Критерий перехода к Phase 3:
- [ ] **30+ emails на waitlist** → запускаем Product Hunt
- [ ] Если 0-15 за неделю → меняем текст поста, пробуем другие subreddits

---

## ✅ Phase 2 — MVP Build (завершено 13 марта 2026)

### Стек
- Next.js 16 + TypeScript
- Supabase Auth (magic link) + PostgreSQL
- Claude Haiku — генерация писем
- Stripe — подписка $39/мес
- Vercel — хостинг

### Что построено
- [x] Supabase Auth — вход по magic link (без пароля)
- [x] Middleware — защита роутов, редирект на /login
- [x] Форма добавления инвойса (клиент, сумма, дни просрочки, проект)
- [x] Список инвойсов привязан к user_id (RLS)
- [x] Claude Haiku генерирует 3 письма: Friendly / Firm / Final Notice
- [x] Кнопка Copy для каждого письма
- [x] Страница /upgrade — Stripe Checkout $39/мес
- [x] Webhook — активирует подписку в таблице `subscribers`
- [x] Деплой на Vercel: https://invoicepilot-black.vercel.app

### Supabase таблицы
- `invoices` — инвойсы пользователя (user_id, client_name, amount, days_overdue, description)
- `subscribers` — подписчики Stripe (user_id, stripe_customer_id, active)
- `waitlist` — emails с лендинга (email, product)

### Файлы продукта
```
products/002-freelance-payment-agent/app/
├── app/
│   ├── page.tsx              — главная (список инвойсов + генерация писем)
│   ├── login/page.tsx        — вход по magic link
│   ├── upgrade/page.tsx      — страница оплаты $39/мес
│   ├── auth/callback/        — обработка magic link
│   └── api/
│       ├── generate/route.ts — Claude генерирует 3 письма
│       └── stripe/
│           ├── route.ts      — создаёт Stripe Checkout Session
│           └── webhook/      — обрабатывает оплату
├── lib/supabase.ts           — клиент Supabase
├── middleware.ts             — защита роутов
└── .env.local                — ключи (не коммитить!)
```

### Env variables (в Vercel)
- NEXT_PUBLIC_SUPABASE_URL
- NEXT_PUBLIC_SUPABASE_ANON_KEY
- SUPABASE_SERVICE_ROLE_KEY
- ANTHROPIC_API_KEY
- STRIPE_SECRET_KEY
- NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY
- STRIPE_WEBHOOK_SECRET
- NEXT_PUBLIC_APP_URL = https://invoicepilot-black.vercel.app

---

## ⏳ Phase 3 — Запуск

- [ ] Уведомить waitlist: написать всем кто подписался "продукт готов"
- [ ] Reddit r/freelance: пост "I built this because..." (с реальными результатами)
- [ ] Product Hunt: подготовить страницу
- [ ] Product Hunt: опубликовать в 12:01 PST
- [ ] **Первый платящий клиент** 🎯
- [ ] **5 платящих клиентов**

---

## ⏳ Phase 4 — Рост до $1k MRR

- [ ] SEO статья #1: "How to follow up on unpaid invoices"
- [ ] SEO статья #2: "Invoice follow-up email templates"
- [ ] 26 платящих клиентов → **$1,014 MRR** ✅
- [ ] Добавить Gmail OAuth (отправка прямо из приложения)
- [ ] Начать исследование продукта #003

---

## Ключевые метрики

| Метрика | Цель | Сейчас |
|---------|------|--------|
| Waitlist signups | 30+ | 1 |
| MRR | $1,014 | $0 |
| Платящих клиентов | 26 | 0 |
| Churn rate | < 5%/мес | — |

---

*Продукт #002 | InvoicePilot | Март 2026*
