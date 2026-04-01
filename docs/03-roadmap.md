# FreelancePaymentAgent — Детальный Roadmap

> Пошаговый план от идеи до $1k MRR
> Принцип: не пишем код пока нет 30 signups

---

## Неделя 0 — Взлётная полоса (сейчас)

**Цель:** Убедиться что всё готово до старта

### Вопросы которые нужно закрыть ПЕРЕД запуском:

**Технические:**
- [ ] Node.js установлен? (`node --version` в Terminal)
- [ ] Git установлен? (`git --version`)
- [ ] Есть GitHub аккаунт?
- [ ] Есть Anthropic API ключ?
- [ ] Есть Stripe аккаунт (или готов создать)?

**Продуктовые:**
- [ ] Название продукта выбрано?
- [ ] Домен куплен или зарезервирован?
- [ ] Понятна целевая аудитория (какой тип фрилансеров — дизайнеры? разработчики? все)?

**Финансовые:**
- [ ] Бюджет на первые 3 месяца (~$500-600 с учётом рекламы)?

---

## Неделя 1 — Валидация (нет кода!)

**День 1:** Landing page на Carrd
```
Структура (одна страница):
┌─────────────────────────────────────────┐
│  [Логотип / название]                   │
│                                         │
│  Stop chasing clients for money.        │
│  Let AI do it.                         │
│                                         │
│  ✓ Add your overdue invoice             │
│  ✓ AI writes the perfect follow-up      │
│  ✓ You send it in 10 seconds            │
│                                         │
│  [email field] [Join Waitlist →]        │
│                                         │
│  Used by 0 freelancers (будет расти)   │
└─────────────────────────────────────────┘
```

**День 2-3:** Пост на Reddit
- r/freelance (1M+ участников)
- r/webdev (800k+)
- r/graphic_design (400k+)
- Стиль: личная история о потере денег, не реклама

**День 4-5:** Другие каналы
- Twitter/X пост
- Facebook группы для фрилансеров
- LinkedIn пост (если есть аудитория)

**День 6-7:** Анализ результатов
```
30+ signups → 🟢 Строим (переходи к Неделе 2)
15-29       → 🟡 Меняем текст landing, пробуем другие subreddits
0-14        → 🔴 Пересматриваем идею или positioning
```

---

## Неделя 2 — Инициализация проекта

**Каждый день: не больше 4-5 часов кода, чёткий список задач**

**День 1: Setup**
```bash
# В терминале:
npx create-next-app@latest payagent --typescript --tailwind --app
cd payagent
npx shadcn@latest init
```
Результат: работает `localhost:3000` с базовой страницей

**День 2: Auth + Database**
- Создать проект на supabase.com
- Скопировать URL и anon key в `.env.local`
- Создать таблицу `invoices`:
```sql
CREATE TABLE invoices (
  id UUID DEFAULT gen_random_uuid() PRIMARY KEY,
  user_id UUID REFERENCES auth.users NOT NULL,
  client_name TEXT NOT NULL,
  client_email TEXT NOT NULL,
  amount DECIMAL(10,2) NOT NULL,
  due_date DATE NOT NULL,
  description TEXT,
  status TEXT DEFAULT 'pending',
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Безопасность: каждый видит только свои инвойсы
ALTER TABLE invoices ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Users see own invoices" ON invoices
  FOR ALL USING (auth.uid() = user_id);
```
- Страницы `/login` и `/signup` работают

**День 3: Dashboard**
- `/dashboard` — список инвойсов пользователя
- Дизайн: тёмная тема (как в нашем pipeline dashboard)
- Показывает: имя клиента, сумма, дата, статус (pending/overdue/paid)

**День 4: Форма добавления инвойса**
- `/invoices/new` — форма с полями
- После сохранения — редирект на dashboard
- Валидация (email, сумма > 0, дата)

**День 5: Review + fix**
- Пройти весь flow руками
- Исправить баги
- Commit + push на GitHub

---

## Неделя 3 — AI + Оплата

**День 1-2: Claude API**

Файл `/app/api/generate-emails/route.ts`:
```
Промпт для Claude:
"You are a professional invoice follow-up assistant.
Write 3 follow-up emails for this overdue invoice:
- Client: {client_name}
- Amount: ${amount}
- Days overdue: {days_overdue}
- Work done: {description}

Email 1 (friendly, day 3): short, assume they forgot
Email 2 (firm, day 7): mention the amount, ask for timeline
Email 3 (formal, day 14): serious tone, mention next steps

Return JSON: {email1: {subject, body}, email2: {...}, email3: {...}}"
```

**День 3: Страница писем**
- `/invoices/[id]/emails` — показывает все 3 письма
- Кнопка "Copy to clipboard" для каждого
- Кнопка "Mark as Paid" → меняет статус

**День 4: Stripe**
- Создать продукт в Stripe dashboard: $39/мес
- `/api/stripe/checkout` — создаёт сессию
- `/api/stripe/webhook` — обновляет статус подписки в БД
- Бесплатный тариф: 2 инвойса без оплаты

**День 5: Деплой**
```
1. Создать проект на vercel.com (импорт из GitHub)
2. Добавить все env variables в Vercel settings
3. Деплой
4. Проверить что Stripe webhook работает на production URL
```

---

## Неделя 4 — Полировка и запуск

**День 1-2: QA**
- Дать 3 знакомым фрилансерам — пусть попробуют без инструкций
- Записать все проблемы
- Исправить критичное

**День 3: Product Hunt**
- Создать страницу продукта (готовить за неделю!)
- Thumbnail 240×240 (логотип)
- Gallery скриншоты 1270×760
- Tagline: "AI writes invoice follow-ups — you just hit send"

**День 4: Контент для запуска**
- Написать финальный Reddit пост ("I built this because...")
- Написать email для waitlist (уведомление о запуске)
- Подготовить Twitter thread

**День 5: ЗАПУСК**
```
12:01 PST — Product Hunt публикация
+ Уведомление waitlist
+ Reddit пост
+ Twitter
```

---

## Месяц 2-3 — Рост

**SEO статьи (пишем сами или через Claude):**
1. "How to follow up on unpaid invoices without ruining the relationship"
2. "Invoice follow-up email templates: 10 examples that work"
3. "What to do when a client doesn't pay: complete guide"

Каждая статья → ссылка на PayAgent в конце.

**Google Ads:**
```
Бюджет: $5/день = $150/мес
Ключевые слова:
- "invoice follow up software" ($1.2/клик)
- "overdue invoice reminder" ($0.8/клик)
- "freelance payment collection tool" ($1.0/клик)
Ожидаемо: 100-150 кликов/мес → 5-8 signups → 1-2 платящих
```

**Реферальная программа (месяц 3):**
```
"Пригласи друга → получи 1 месяц бесплатно"
Фрилансеры активно общаются между собой
```

---

## Финансовая модель по месяцам

```
Месяц 1:  5 клиентов  = $195 MRR
          Расходы:       $170 (Claude $15 + Ads $150 + Vercel $0 + Supabase $0)
          Прибыль:       $25

Месяц 2:  15 клиентов = $585 MRR
          Расходы:       $200
          Прибыль:       $385

Месяц 3:  26 клиентов = $1,014 MRR ← ЦЕЛЬ
          Расходы:       $220
          Прибыль:       $794

Месяц 6:  60 клиентов = $2,340 MRR
          Прибыль:       ~$2,000/мес
```

---

## Если что-то пойдёт не так

| Ситуация | Действие |
|----------|---------|
| Мало signups на waitlist | Поменять заголовок, попробовать другой subreddit |
| Signups есть но не платят | Созвониться с 3 людьми, спросить почему |
| Churn растёт | Интервью с уходящими, исправить главную жалобу |
| Конкурент скопировал | Ускорить выпуск v0.2 (Gmail OAuth), они отстают на месяц |
| Reddit бан за спам | Не постить одно и то же, менять subreddits |

---

*Детальный Roadmap | FreelancePaymentAgent #002 | Март 2026*
