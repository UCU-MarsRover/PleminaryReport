# CONTRIBUTING

Короткий гайд для командної роботи без конфліктів у LaTeX.

## Головний принцип

Для `Preliminary Design Description` діє правило:

- **1 subsection file = 1 PR**

Це мінімізує merge-конфлікти і пришвидшує рев'ю.

## Де писати контент

### Design section

Основний файл:

- `sections/preliminary_design_description.tex`

Він тільки підключає підсекції через `\input{...}`.

Контент пишеться в:

- `sections/design/<chapter>/<section>/<subsection>/<file>.tex`

Кожен файл у `sections/design/` відповідає **одному** найнижчому пункту структури (leaf subsection).

Приклад реального шляху:

- `sections/design/2_rover_design_description/2_3_communication_subsystem/2_3_5_link_performance_analysis.tex`

### Test plan

Для тест-плану залишаємо командні файли:

- `teams/<team>/tests.tex`

Ці файли визначають макроси `\XXXtestrows` і збираються у master-таблицю.

## Tiny contract на початку subsection-файлу

На початку кожного файлу `sections/design/*.tex` є міні-контракт:

- `Teams`
- `Requirements`
- `Status`

Не видаляйте цей блок. Оновлюйте поля по мірі прогресу.

## Правила для PR

### Обов'язково

1. Один PR змінює **один** файл у `sections/design/` (допустимі дрібні супутні правки типу опечатки в тому ж subsection).
2. Назва PR:
   - `DES <subsection-id>: short title`
   - приклад: `DES 5.5: link performance analysis`
3. У description PR вкажи:
   - які `REQ-*` закриті
   - які крос-посилання додані/оновлені
   - що ще лишилось (якщо є)

### Не робимо

- Не змішуємо кілька subsection в одному PR.
- Не переносимо великий контент між чужими subsection-файлами.
- Не редагуємо `sections/preliminary_design_description.tex`, якщо просто додаємо текст у subsection.

## Мінімальний чекліст перед пушем

1. Заповнені `Teams`, `Requirements` і `Status` у tiny contract.
2. Немає заглушок виду `[Add content for this subsection.]` у твоєму файлі.
3. Є хоча б одна явна згадка `REQ-*` у тексті підсекції (або таблиці/рисунку).
4. Документ компілюється локально.

## Компіляція

Компілюй з кореня репозиторію:

```bash
latexmk -pdf main.tex
latexmk -c
```

## Де зберігати фігури

- Командні фігури: `teams/<team>/figures/`
- Загальні фігури: `figures/`

Вставка прикладу:

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.9\linewidth]{teams/arm/figures/arm_cad}
    \caption{Robotic arm overview}
    \label{fig:arm_cad}
\end{figure}
```

## Часті помилки

| Помилка | Як правильно |
|---|---|
| Один PR змінює 3-4 subsection файли | Тримай PR у межах одного `sections/design/<file>.tex` |
| Залишені заглушки `[Add content ...]` | Замінити на реальний текст перед merge |
| Пишеш контент у `sections/preliminary_design_description.tex` | Писати в конкретний `sections/design/*.tex` |
| `REQ-???` залишився в готовому subsection | Замінити на фактичні `REQ-*` перед merge |

## Питання

Пишіть у командний чат або відкривайте issue в репозиторії.
