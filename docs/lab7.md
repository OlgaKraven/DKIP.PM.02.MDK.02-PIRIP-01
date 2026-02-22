# Лабораторная работа №7 — Разработка интерактивного UI-баннера (Hero Section)

---

## Цель

Создать **hero-секцию** (первый экран) для интерфейса по индивидуальной предметной области, которая:

* визуально “продаёт” сервис (ценность + выгоды);
* имеет корректную композицию (текст + визуал);
* содержит **CTA-кнопку**;
* использует **анимации** (transition/keyframes);
* содержит **overlay/градиенты** для читаемости;
* соответствует требованиям **контраста** (WCAG);
* адаптивна и семантически корректна.

---

# 1. Теория 

## 1.1. Что такое Hero Section и зачем она нужна

**Hero Section** — главный баннер “первого экрана” (above the fold), который отвечает на 3 вопроса:

1. **Что это?** (название/тип сервиса)
2. **Почему это полезно?** (1–2 выгоды)
3. **Что делать дальше?** (CTA-кнопка)

### Пример структуры смысла (копирайтинг)

* Заголовок: *“Бронирование переговорных за 30 секунд”*
* Подзаголовок: *“Выберите комнату, время и участников — статус занятости в реальном времени.”*
* CTA: *“Забронировать”* + вторичная ссылка *“Посмотреть тарифы”*

---

## 1.2. Семантическая структура Hero-блока

Hero — это часть `main`, обычно `<section>` с понятным заголовком `h1`.

### Пример семантики

```html
<header class="site-header">
  <a class="brand" href="#top">MeetRoom</a>
  <nav class="nav" aria-label="Основная навигация">
    <a href="#features">Возможности</a>
    <a href="#pricing">Тарифы</a>
    <a href="#contacts">Контакты</a>
  </nav>
</header>

<main id="main">
  <section class="hero" aria-labelledby="hero-title">
    <div class="hero__content">
      <h1 id="hero-title">Бронирование переговорных за 30 секунд</h1>
      <p class="hero__lead">Календарь, статусы занятости и фильтр по времени — всё в одном интерфейсе.</p>

      <div class="hero__actions">
        <a class="btn btn--primary" href="#cta">Забронировать</a>
        <a class="btn btn--ghost" href="#features">Посмотреть возможности</a>
      </div>

      <p class="hero__note">Без регистрации на демо-режиме</p>
    </div>

    <div class="hero__media" aria-hidden="true">
      <!-- визуальный блок/иллюстрация -->
    </div>
  </section>
</main>
```

---

## 1.3. UI-композиция: сетка, отступы, визуальная иерархия

Hero обычно состоит из 2 колонок:

* слева: текст и CTA;
* справа: иллюстрация / mockup / карточки.

### Пример композиции на Grid

```css
.hero{
  display: grid;
  gap: 24px;
  align-items: center;
  padding: 48px 16px;
}

@media (min-width: 900px){
  .hero{
    grid-template-columns: 1.1fr 0.9fr;
    padding: 72px 24px;
  }
}
```

---

## 1.4. Типографика в hero: clamp() и line-height

В hero заголовок должен быть крупным, но адаптивным.

### Пример типографики hero

```css
.hero__content h1{
  font-size: clamp(1.6rem, 3vw + 1rem, 3rem);
  line-height: 1.08;
  letter-spacing: -0.02em;
  margin: 0 0 12px;
}

.hero__lead{
  font-size: clamp(1rem, 0.6vw + 0.9rem, 1.25rem);
  line-height: 1.5;
  max-width: 55ch;
  margin: 0 0 20px;
}
```

---

## 1.5. CTA-кнопка: смысл и UI-поведение

CTA должна быть заметной, контрастной и понятной по действию.

### Пример CTA-кнопок (HTML)

```html
<div class="hero__actions">
  <a class="btn btn--primary" href="#cta">Начать</a>
  <a class="btn btn--ghost" href="#pricing">Тарифы</a>
</div>
```

### Пример поведения CTA (CSS)

```css
.btn{
  display:inline-flex;
  align-items:center;
  justify-content:center;
  gap: 10px;
  padding: 12px 16px;
  border-radius: 14px;
  text-decoration: none;
  font-weight: 600;
  transition: transform .12s ease, background .2s ease, box-shadow .2s ease;
}

.btn--primary{
  background: #2563eb;
  color: #fff;
  box-shadow: 0 10px 30px rgba(37,99,235,.25);
}
.btn--primary:hover{ transform: translateY(-1px); }
.btn--primary:active{ transform: translateY(0); }
.btn:focus-visible{ outline: 3px solid rgba(37,99,235,.35); outline-offset: 3px; }

.btn--ghost{
  border: 1px solid rgba(255,255,255,.35);
  color: #fff;
  background: transparent;
}
.btn--ghost:hover{ background: rgba(255,255,255,.08); }
```

---

## 1.6. Градиенты и overlay: читаемость текста поверх изображения

Если фон яркий, нужен **overlay** (полупрозрачный слой), чтобы текст не терялся.

### Пример overlay через псевдоэлемент

```css
.hero{
  position: relative;
  overflow: clip;
  border-radius: 24px;
  color: #fff;
  background: radial-gradient(1200px 600px at 10% 10%, rgba(37,99,235,.55), transparent 60%),
              linear-gradient(135deg, #0b1220, #0f172a);
}

.hero::before{
  content:"";
  position:absolute;
  inset:0;
  background: linear-gradient(90deg, rgba(0,0,0,.65), rgba(0,0,0,.15));
  pointer-events:none;
}
.hero__content, .hero__media{ position: relative; }
```

---

## 1.7. Анимации: transitions и keyframes

Анимация должна быть “лёгкой”: подчёркивает UI, не мешает.

### Пример 1: hover-анимации (transition)

```css
.hero__media{
  transform: translateY(0);
  transition: transform .35s ease;
}
.hero:hover .hero__media{
  transform: translateY(-6px);
}
```

### Пример 2: keyframes “плавающий” объект

```css
@keyframes floaty {
  0%   { transform: translateY(0); }
  50%  { transform: translateY(-10px); }
  100% { transform: translateY(0); }
}

.float{
  animation: floaty 4s ease-in-out infinite;
}
```

### Важно: reduce motion

```css
@media (prefers-reduced-motion: reduce){
  *{ animation: none !important; transition: none !important; }
}
```

---

## 1.8. Параллакс (по желанию): аккуратно и без “ломания”

Параллакс можно сделать лёгким: слои двигаются чуть по-разному.

### Вариант A (чистый CSS, простейший)

```css
.hero{
  background-attachment: fixed; /* осторожно: не везде идеально на mobile */
}
```

### Вариант B (JS-параллакс, мягкий)

```html
<script>
  const layer = document.querySelector(".hero__glow");
  window.addEventListener("scroll", () => {
    const y = window.scrollY * 0.15;
    layer.style.transform = `translateY(${y}px)`;
  });
</script>
```

---

## 1.9. Контраст и читаемость: что проверять

Текст должен читаться на фоне:

* достаточный контраст (WCAG);
* размер шрифта и line-height;
* overlay/подложка под текст.

### Пример “подложки” под текст (если фон сложный)

```css
.hero__content{
  background: rgba(0,0,0,.25);
  backdrop-filter: blur(6px);
  border: 1px solid rgba(255,255,255,.12);
  border-radius: 18px;
  padding: 18px;
}
```

---

## 1.10. Проверка контраста (WCAG): что сдавать в отчёт

В отчёте нужно:

* указать цвета текста и фона (hex);
* проверить контраст через инструмент;
* приложить скрин результата.

### Пример записи в отчёте

```text
Текст: #FFFFFF
Фон: #0F172A
Контраст: 12.6:1 (соответствует WCAG AA/AAA)
```
---
## 1.11 ## Полный пример “готового” Hero-блока (HTML + CSS + минимальный JS параллакс)

> Можно вставить как стартовую заготовку и заменить текст под свой сервис.

```html
<!doctype html>
<html lang="ru">
<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width,initial-scale=1">
  <title>Hero — UI баннер</title>
  <meta name="description" content="Учебный Hero-блок для интерфейса сервиса">
  <meta name="author" content="Фамилия И.О., группа">
  <style>
    :root{
      --bg0:#0b1220;
      --bg1:#0f172a;
      --primary:#2563eb;
      --accent:#22c55e;
      --text:#ffffff;
      --muted:rgba(255,255,255,.82);
      --card:rgba(255,255,255,.06);
      --border:rgba(255,255,255,.12);
      --shadow:0 18px 50px rgba(0,0,0,.35);
      --r-xl:24px;
      --r-lg:18px;
    }

    *{ box-sizing:border-box; }
    body{
      margin:0;
      font-family:system-ui,-apple-system,"Segoe UI",Roboto,Arial,sans-serif;
      background: #060b16;
      color: var(--text);
      padding: 18px;
    }

    .hero{
      position:relative;
      overflow: clip;
      border-radius: var(--r-xl);
      background:
        radial-gradient(900px 500px at 10% 10%, rgba(37,99,235,.55), transparent 60%),
        radial-gradient(700px 400px at 90% 30%, rgba(34,197,94,.35), transparent 55%),
        linear-gradient(135deg, var(--bg0), var(--bg1));
      box-shadow: var(--shadow);
      padding: 28px 18px;
      display:grid;
      gap: 22px;
      align-items:center;
      min-height: 420px;
    }

    .hero::before{
      content:"";
      position:absolute;
      inset:0;
      background: linear-gradient(90deg, rgba(0,0,0,.62), rgba(0,0,0,.12));
      pointer-events:none;
    }

    .hero__content, .hero__media{ position:relative; }

    .hero__badge{
      display:inline-flex;
      gap:8px;
      align-items:center;
      padding: 6px 10px;
      background: rgba(255,255,255,.08);
      border: 1px solid var(--border);
      border-radius: 999px;
      font-size: .9rem;
      color: var(--muted);
      width: fit-content;
    }
    .dot{ width:8px; height:8px; border-radius:99px; background: var(--accent); }

    h1{
      margin: 12px 0 10px;
      font-size: clamp(1.65rem, 3vw + 1rem, 3.1rem);
      line-height: 1.07;
      letter-spacing: -0.02em;
    }

    .lead{
      margin: 0 0 16px;
      color: var(--muted);
      font-size: clamp(1rem, 0.6vw + .9rem, 1.25rem);
      line-height: 1.55;
      max-width: 58ch;
    }

    .benefits{
      display:flex;
      flex-wrap:wrap;
      gap:10px;
      margin: 0 0 18px;
      padding:0;
      list-style:none;
    }
    .benefits li{
      background: var(--card);
      border: 1px solid var(--border);
      border-radius: 999px;
      padding: 8px 12px;
      color: rgba(255,255,255,.9);
      font-size: .95rem;
    }

    .actions{
      display:flex;
      flex-wrap:wrap;
      gap: 12px;
      align-items:center;
    }

    .btn{
      display:inline-flex;
      align-items:center;
      justify-content:center;
      gap:10px;
      padding: 12px 16px;
      border-radius: 14px;
      text-decoration:none;
      font-weight: 650;
      border: 1px solid transparent;
      transition: transform .12s ease, background .2s ease, box-shadow .2s ease;
      will-change: transform;
    }
    .btn:focus-visible{
      outline: 3px solid rgba(37,99,235,.35);
      outline-offset: 3px;
    }
    .btn--primary{
      background: var(--primary);
      color:#fff;
      box-shadow: 0 12px 30px rgba(37,99,235,.25);
    }
    .btn--primary:hover{ transform: translateY(-1px); }
    .btn--primary:active{ transform: translateY(0); }

    .btn--ghost{
      background: transparent;
      border-color: rgba(255,255,255,.26);
      color:#fff;
    }
    .btn--ghost:hover{ background: rgba(255,255,255,.08); }

    .note{
      margin: 12px 0 0;
      color: rgba(255,255,255,.75);
      font-size: .92rem;
    }

    /* media mockup */
    .hero__media{
      display:grid;
      gap: 12px;
      justify-items:end;
      transform: translateY(0);
      transition: transform .35s ease;
    }
    .hero:hover .hero__media{ transform: translateY(-6px); }

    .mock{
      width: min(420px, 100%);
      border-radius: var(--r-lg);
      border: 1px solid rgba(255,255,255,.14);
      background: rgba(255,255,255,.06);
      padding: 14px;
      backdrop-filter: blur(8px);
    }

    .mock__row{ display:flex; gap:10px; align-items:center; }
    .pill{
      height: 10px; width: 54px; border-radius: 99px;
      background: rgba(255,255,255,.22);
    }
    .pill.w{ width: 92px; }
    .grid{
      display:grid;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
      margin-top: 12px;
    }
    .tile{
      height: 70px;
      border-radius: 14px;
      border: 1px solid rgba(255,255,255,.12);
      background: rgba(255,255,255,.07);
      position: relative;
      overflow:hidden;
    }
    .tile::after{
      content:"";
      position:absolute;
      inset:auto -40px -40px auto;
      width:120px; height:120px;
      background: radial-gradient(circle, rgba(37,99,235,.65), transparent 60%);
      transform: rotate(20deg);
    }

    /* keyframes */
    @keyframes floaty{
      0%{ transform: translateY(0); }
      50%{ transform: translateY(-10px); }
      100%{ transform: translateY(0); }
    }
    .float{ animation: floaty 4s ease-in-out infinite; }

    /* 2 columns */
    @media (min-width: 900px){
      .hero{ grid-template-columns: 1.1fr 0.9fr; padding: 64px 34px; }
      .hero__content{ padding-right: 10px; }
    }

    @media (prefers-reduced-motion: reduce){
      *{ animation:none !important; transition:none !important; }
    }
  </style>
</head>
<body>

<section class="hero" aria-labelledby="hero-title">
  <div class="hero__content">
    <div class="hero__badge"><span class="dot"></span> Новый интерфейс сервиса</div>

    <h1 id="hero-title">Бронирование переговорных за 30 секунд</h1>
    <p class="lead">
      Календарь, фильтр по времени и статусы занятости — быстро создавайте бронь и приглашайте участников.
    </p>

    <ul class="benefits">
      <li>Статусы в реальном времени</li>
      <li>Фильтр по времени</li>
      <li>Понятный календарь</li>
      <li>Подтверждение брони</li>
    </ul>

    <div class="actions">
      <a class="btn btn--primary" href="#cta">Забронировать</a>
      <a class="btn btn--ghost" href="#features">Возможности</a>
    </div>

    <p class="note">Контраст текста проверен по WCAG (приложить скрин в отчёт).</p>
  </div>

  <div class="hero__media float" aria-hidden="true">
    <div class="mock">
      <div class="mock__row">
        <div class="pill"></div>
        <div class="pill w"></div>
        <div class="pill"></div>
      </div>

      <div class="grid">
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
        <div class="tile"></div>
      </div>
    </div>
  </div>
</section>

</body>
</html>
```

---

# 2. Задания

> Делается один hero-блок для выбранной предметной области.

---

## 2.1. Создать hero-блок 

Требования:

* `h1` + короткий `lead` (1 абзац);
* список выгод (2–4 пункта) или бейджи;
* CTA-кнопка (основная) + вторичная ссылка/кнопка.

---

## 2.2. Типографика 

* `clamp()` для `h1`;
* `line-height` и `max-width` для читабельности.

---

## 2.3. CTA кнопка 

* состояние `hover`, `active`, `focus-visible`;
* кликабельность и контраст.

---

## 2.4. Анимации 

Минимум:

* 1 анимация через `transition` (hover/появление),
* 1 анимация через `@keyframes` (фон/элемент).

---

## 2.5. Градиенты / overlay 

* фон с градиентом или картинкой;
* overlay/подложка для читаемости текста.

---

## 2.6. Контраст и читаемость 

* проверить контраст (WCAG);
* приложить скрин в отчёт.

---

## 2.7. Параллакс (по желанию)

Любой лёгкий вариант (CSS/JS).
Если реализован корректно — может повысить оценку при спорных моментах.

---

## 2.8. Адаптивность 

* mobile-first;
* минимум 1 breakpoint (`min-width`), где hero становится “2 колонки”.

---

## 2.9. Публикация 

* GitHub репозиторий;
* ссылка в отчёте;
* скрин hero на моб/десктоп.

---

# 3. Контрольные вопросы

1. Что делает `clamp()`?
2. Почему нужен overlay?
3. Что такое `prefers-reduced-motion`?
4. Какие 2 приёма вы использовали для читабельности?
5. Как вы обеспечили доступность кнопок (focus-visible)?

---

# 4. Чек-лист для самопроверки

| Баллы | Критерии выполнения                                                                                                           |
| ----: | ----------------------------------------------------------------------------------------------------------------------------- |
|     6 | Выполнены все пункты 2.1–2.9: hero, типографика clamp, CTA состояния, 2 анимации, overlay, WCAG проверка, адаптив, публикация |
|     5 | Почти всё выполнено, небольшие недочёты (например, слабая анимация или неидеальная читаемость)                                |
|   3–4 | Есть hero и CTA, но отсутствуют важные элементы (overlay/анимации/контраст)                                                   |
|   1–2 | Частично: только статичная секция без системы                                                                                 |
|     0 | Не соответствует заданию / нет публикации                                                                                     |


---

### 5. Для выполнения требуется

1. **Шаблон отчёта**, указанный в описании лабораторной работы.
2. **Вариант задания**, закреплённый за студентом на семестр и опубликованный в ЛМС.
3. **Требования к оформлению** — согласно методическим указаниям в ЛМС или соответствующему разделу документации.

---

# 6. Формат сдачи работы

1. `index.html`
2. `css/styles.css`
3. (если делали параллакс на JS) `js/app.js`
4. Отчёт `.pdf` или `.docx` (скрины + WCAG)
5. Ссылка на GitHub
6. Архив `LR7_Фамилия_Группа.zip`

---

### 7. Запрет

❗ **Запрещается использование систем искусственного интеллекта для выполнения лабораторной работы.**

Работы, выполненные с использованием ИИ, полностью или частично скопированные, а также не сданные, оцениваются в **0 баллов** без возможности доработки.

---

## 8. Шаблон отчёта

👉 [Скачать шаблон отчёта](assets/files//LR2%20%28Shablon%29.docx)


