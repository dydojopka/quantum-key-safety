---
marp: true
theme: default
size: 16:9
paginate: true
math: katex
title: Квантовая передача ключа. Вероятность обнаружения ошибки при перехвате сообщения
description: Квантовое распределение ключа и расчёт вероятности обнаружения перехвата
footer: Квантовые вычисления
style: |
  section {
    --accent: #02b087;
    --ink: #102638;
    --paper: #ffffff;
    --mint: #dcf5ed;
    --emphasis: #006451;
    --muted: #486174;
    --rule: #b9cbd6;
    --amber-tint: #f9ead1;
    --amber-text: #71470b;
    background: var(--paper);
    color: var(--ink);
    border-top: 9px solid var(--accent);
    padding: 64px 88px 82px;
    align-content: start;
    font-family: "Segoe UI", Arial, sans-serif;
    font-size: 30px;
    line-height: 1.36;
  }
  h1 {
    color: inherit;
    font-size: 52px;
    line-height: 1.12;
    font-weight: 700;
    letter-spacing: -0.035em;
    margin: 0 0 27px;
  }
  p { margin: 0 0 19px; }
  strong, a { color: var(--emphasis); }
  ol, ul { margin: 0 0 20px; padding-left: 1.3em; }
  li + li { margin-top: 12px; }
  blockquote {
    border-left: 4px solid var(--accent);
    margin: 0 0 22px;
    padding: 6px 0 6px 24px;
    color: inherit;
    quotes: none;
  }
  blockquote::before, blockquote::after { content: none; }
  blockquote p { margin: 0; }
  table {
    display: table;
    width: 100%;
    margin: 2px 0 25px;
    border-collapse: collapse;
    font-size: 0.95em;
  }
  table tr:nth-child(n) {
    background: transparent;
    border: 0;
  }
  table thead th {
    background: var(--mint);
    color: var(--ink);
  }
  table td, table th {
    padding: 12px 16px;
    border: 0;
    border-bottom: 1px solid var(--rule);
  }
  table td:first-child, table th:first-child { text-align: left; }
  table td:not(:first-child), table th:not(:first-child) { text-align: center; }
  .katex-display { margin: 18px 0 25px; }
  footer, section::after {
    bottom: 26px;
    color: var(--muted);
    font-size: 17px;
    font-weight: 600;
  }
  footer { left: 88px; }
  section::after { right: 88px; }
  section.closing {
    --muted: #acc5d1;
    background: var(--ink);
    color: var(--paper);
    align-content: center;
    text-align: center;
  }
  section.closing h1 { font-size: 76px; margin-bottom: 22px; }
  section.closing p { font-size: 34px; margin: 0; }
  section.cover {
    --muted: var(--ink);
    background: var(--accent);
    border-top-color: var(--ink);
    align-content: center;
    padding-bottom: 105px;
  }
  section.cover p:first-child {
    font-size: 24px;
    font-weight: 600;
    margin-bottom: 27px;
  }
  section.cover h1 {
    font-size: 76px;
    margin-bottom: 29px;
  }
  section.cover p:last-of-type { font-size: 34px; max-width: 920px; }
  section.sift table {
    font-size: 25px;
    font-variant-numeric: tabular-nums;
  }
  section.sift table td, section.sift table th { padding: 9px 14px; }
  section.sift table td:first-child, section.sift table th:first-child { width: 30%; }
  section.sift table tbody tr:last-child td {
    background: #edf3f8;
    color: var(--muted);
  }
  section.sift table tbody tr:last-child td:has(strong) {
    background: var(--mint);
  }
  section.attack .katex-display,
  section.qber table tbody tr:last-child td:last-child {
    background: var(--amber-tint);
    color: var(--amber-text);
  }
  section.attack .katex-display {
    font-size: 1.02em;
    margin-top: 28px;
    padding: 12px 0 14px;
  }
  section.focus .katex-display {
    color: var(--emphasis);
    font-size: 1.4em;
    margin: 56px 0 48px;
  }
  section.results table td, section.results table th { padding: 19px 14px; }
  section.results table td:last-child, section.results table th:last-child {
    --emphasis: var(--ink);
    background: var(--accent);
  }
  section.results blockquote { margin-top: 12px; }
---

<!-- _class: cover -->

Курс «Квантовые вычисления»

# Квантовая передача ключа

Вероятность обнаружения ошибки  
при перехвате сообщения

---

# Два канала для распределения ключа

Алиса и Боб хотят получить общую секретную строку битов.

> **Квантовый канал**  
> Алиса кодирует случайные биты в поляризации фотонов и посылает их Бобу.

> **Открытый аутентифицированный канал**  
> Алиса и Боб сверяют базисы и проверяют часть полученных битов.

Полученный ключ используют для шифрования последующих сообщений.

<!-- [1], § 3.1, с. 13–15. [4], раздел III. -->

---

# Два базиса кодирования

Бит и базис Алиса выбирает независимо. У каждого выбора два равновероятных варианта.

| Базис | Бит 0 | Бит 1 |
| :--- | :---: | :---: |
| Прямой $Z$ | $\lvert 0\rangle$, $0^\circ$ | $\lvert 1\rangle$, $90^\circ$ |
| Диагональный $X$ | $\lvert +\rangle$, $45^\circ$ | $\lvert -\rangle$, $135^\circ$ |

$$
\lvert +\rangle=\frac{\lvert 0\rangle+\lvert 1\rangle}{\sqrt{2}},
\qquad
\lvert -\rangle=\frac{\lvert 0\rangle-\lvert 1\rangle}{\sqrt{2}}
$$

<!-- [1], § 2.1 и § 3.1. [2], с. 8, 11–12. Соответствие битов углам взято из [4], раздел III. -->

---

# Измерение в двух базисах

Пусть Алиса послала состояние $\lvert +\rangle$ по каналу без помех.

**Базис $X$.** Боб получит бит $0$ с вероятностью $1$.

**Базис $Z$.** Боб получит $0$ или $1$ с равными вероятностями.

$$
\bigl|\langle 0 \mid + \rangle\bigr|^2
=
\bigl|\langle 1 \mid + \rangle\bigr|^2
=
\left(\frac{1}{\sqrt{2}}\right)^2
=
\frac{1}{2}
$$

Вероятность исхода равна квадрату модуля амплитуды.

<!-- [1], § 2.1.2. [2], § 1.1, с. 8, 11. [3], § 1.3, с. 8–9. -->

---

<!-- _class: sift -->

# Отбор битов по базисам

После измерений Алиса и Боб сообщают друг другу свои базисы.

| Позиция | 1 | 2 | 3 | 4 | 5 | 6 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: |
| Бит Алисы | 0 | 1 | 1 | 0 | 1 | 0 |
| Базис Алисы | Z | X | Z | X | X | Z |
| Базис Боба | Z | Z | Z | X | Z | X |
| Бит Боба | 0 | 0 | 1 | 0 | 1 | 1 |
| После сверки | **0** | × | **1** | **0** | × | × |

В этом примере без перехвата у обоих после отсева остаётся **010**.

<!-- [1], § 3.1, с. 14. Условный пример для шести фотонов. -->

---

<!-- _class: attack -->

# Перехват с повторной отправкой

Модель канала без шума и потерь. Ева перехватывает каждый фотон.

1. Ева случайно выбирает $Z$ или $X$ и измеряет фотон.
2. По результату она готовит новый фотон в своём базисе и посылает Бобу.

$$
\underbrace{\lvert 0\rangle}_{\text{фотон Алисы}}
\quad\xrightarrow{\text{Ева измеряет в }X}
\quad\underbrace{\lvert +\rangle}_{\text{фотон от Евы при исходе }0}
$$

Даже при совпадении базисов Алисы и Боба возможна ошибка.

<!-- [1], § 3.1, с. 15. [4], раздел III. -->

---

<!-- _class: qber -->

# Вероятность ошибки в одном бите

Считаем ошибки Боба там, где его базис совпал с базисом Алисы.

| Базис Евы | Вероятность выбора | Вероятность ошибки |
| :--- | :---: | :---: |
| Базис Алисы | $1/2$ | $0$ |
| Другой базис | $1/2$ | $1/2$ |

$$
q=\frac{1}{2}\cdot 0+\frac{1}{2}\cdot\frac{1}{2}
=\frac{1}{4}=25\,\%
$$

$q$ - ожидаемая доля битовых ошибок после отсева (QBER).

<!-- [1], § 3.1, с. 15. [4], раздел III. -->

---

<!-- _class: focus -->

# Проверка контрольных битов

Алиса и Боб случайно выбирают $m$ позиций после отсева, сравнивают значения и удаляют раскрытые биты.

$$
P_{\mathrm{det}}(m)=1-(1-q)^m
=1-\left(\frac{3}{4}\right)^m
$$

В модели полного перехвата ошибки независимы. Перехват обнаружен, если в выборке есть хотя бы одно несовпадение.

<!-- Формула следует из q = 1/4 в [1], § 3.1, при независимых действиях на каждом фотоне и проверке m разных позиций. -->

---

<!-- _class: results -->

# Сколько битов нужно проверить

Полный перехват. Вероятность найти хотя бы одну ошибку.

| Проверено битов $m$ | 1 | 5 | 10 | 11 | 17 |
| :--- | ---: | ---: | ---: | ---: | ---: |
| $P_{\mathrm{det}}$ | 25 % | 76,27 % | 94,37 % | 95,78 % | **99,25 %** |

> Для вероятности обнаружения **не ниже 99 %**  
> нужно проверить **как минимум 17 битов**.

<!-- Расчёт по формуле на слайде 8. Для m = 16 вероятность примерно 98,998 %. -->

---

# Перехват части сигналов

Ева атакует каждый фотон независимо с вероятностью $f$. Канал по-прежнему без шума и потерь.

$$
q=\frac{f}{4},
\qquad
P_{\mathrm{det}}(m)=1-\left(1-\frac{f}{4}\right)^m
$$

При $f=1/2$ доля ошибок составляет **12,5 %**, а проверка 17 битов выявляет перехват с вероятностью **89,67 %**.

<!-- Обобщение расчёта со слайдов 7–8 на независимый частичный перехват. -->

---

# Проверка в реальном канале

Если в выборке из $m$ битов нашли $k$ расхождений, оценка QBER равна

$$
\widehat q=\frac{k}{m}.
$$

- Расхождения могут возникать из-за атаки или шума аппаратуры и канала.
- При слишком высокой оценке ошибок сеанс прерывают.
- Если проверка пройдена, Алиса и Боб исправляют расхождения и сокращают ключ, чтобы ограничить сведения Евы.

<!-- [4], раздел III. [5], описание протокола и постобработки. -->

---

<!-- _class: closing -->

# Спасибо за внимание!
