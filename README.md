# Preliminary Report — ERC 2026

LaTeX template for the **European Rover Challenge 2026 Preliminary Report**.

---


## DIAGRAMS STANDARDS

**Всі стандарти** описані у `DIAGRAM_STANDARDS.md`

## Quick start

Інсталяція інструментів:
```bash
# Повний набір (~4 GB) (рекомендую щоб не паритись):
sudo apt install texlive-full latexmk
```

Компіляція pdf файлу і очистити допоміжні файли:
```bash
latexmk -pdf main.tex && latexmk -c
```

> **Перед тим як писати** — відкрий `main.tex` і заповни метадані команди:
> `\teamname`, `\projectname`, `\affiliation`, `\submissiondate`, `\revisionnum`

---

## Структура репозиторію

```
.
├── main.tex                    # Точка входу — тільки \include, нічого більше
├── preamble.tex                # Всі \usepackage, стилі, кольори — не чіпай без потреби
│
├── sections/                   # Головні розділи звіту (структура фіксована ERC)
│   ├── cover.tex               # Титульна сторінка
│   ├── matrix_of_compliance.tex       # Розділ 1 — Matrix of Compliance
│   ├── preliminary_test_plan.tex      # Розділ 2 — Test Plan
│   ├── preliminary_design_description.tex  # Розділ 3 — Design Description
│   ├── rio_analysis.tex               # Розділ 4 — RIO Analysis
│   └── project_budget.tex             # Розділ 5 — Project Budget
│
├── teams/                      # Технічний контент кожної підкоманди
│   ├── arm/
│   │   ├── design.tex          # Опис конструкції маніпулятора
│   │   └── tests.tex           # Тести специфічні для маніпулятора
│   ├── drone/
│   │   ├── design.tex          # Опис конструкції дрона
│   │   └── tests.tex
│   ├── electronics/
│   │   ├── design.tex          # Схеми живлення, електроніка
│   │   └── tests.tex
│   ├── ground_station/
│   │   ├── design.tex          # Схеми комунікацій, GS архітектура
│   │   └── tests.tex
│   ├── navigation/
│   │   ├── design.tex          # Навігація, сенсори, ROS
│   │   └── tests.tex
│   ├── science/
│   │   ├── design.tex          # Науковий модуль
│   │   └── tests.tex
│   └── suspension/
│       ├── design.tex          # Підвіска, колеса
│       └── tests.tex
│
├── tables/                     # Великі таблиці винесені окремо
│   ├── compliance_summary.tex  # Зведена таблиця відповідності вимогам (Розділ 1)
│   ├── rio_table.tex           # Таблиця ризиків RIO (Розділ 4)
│   └── test_plan.tex           # Таблиця тестів (Розділ 2)
│
├── figures/                    # Всі графічні матеріали
│   ├── tikz/                   # TikZ діаграми як окремі .tex файли
│   │   └── *.tex               # \input{figures/tikz/назва.tex}
│   ├── photos/                 # Фото ровера, деталей, прототипів (.jpg, .png)
│   │   └── *.jpg / *.png
│   └── cad/                    # PDF-експорти з CAD або EasyEDA
│       └── *.pdf
│
└── appendix/
    └── requirements.tex        # Appendix 3 — повна таблиця вимог ERC
```

---

## Куди що класти — коротко

### Я з команди електроніки, пишу про схему живлення

1. Схему намалюй в **EasyEDA**, експортуй як **PDF** → поклади в `figures/cad/power_system.pdf`
2. Текстовий опис пиши в **`teams/electronics/design.tex`**
3. Вставляй схему так:
   ```latex
   \begin{figure}[h]
     \centering
     \includegraphics[width=\linewidth]{figures/cad/power_system}
     \caption{Power distribution system}
     \label{fig:power}
   \end{figure}
   ```
4. Тести для електроніки — в **`teams/electronics/tests.tex`**

---

### Я з команди ground station, маю TikZ діаграму комунікацій

1. TikZ-код діаграми поклади окремим файлом: **`figures/tikz/gs_communication.tex`**
2. Вставляй у `teams/ground_station/design.tex` через:
   ```latex
   \begin{figure}[h]
     \centering
     \input{figures/tikz/gs_communication}
     \caption{Ground station communication schema}
     \label{fig:gs-comm}
   \end{figure}
   ```
3. Сам опис — в **`teams/ground_station/design.tex`**

---

### Я заповнюю RIO таблицю ризиків

- Таблиця — в **`tables/rio_table.tex`**
- Методологія і пояснення — в **`sections/rio_analysis.tex`**
- Кольорову heat map ризиків (5×5 матрицю) — теж у `sections/rio_analysis.tex` як TikZ або таблиця

---

### Я заповнюю тест-план

- Таблиця тестів — в **`tables/test_plan.tex`**
- Вступний текст і фото тест-стендів — в **`sections/preliminary_test_plan.tex`**

---

### Я додаю фото прототипу

Поклади файл у **`figures/photos/`** і вставляй так:
```latex
\includegraphics[width=0.8\linewidth]{figures/photos/rover_prototype}
```
Розширення (`.jpg`, `.png`) вказувати не потрібно.

---

## Типи діаграм — який інструмент

| Тип діаграми | Інструмент | Куди класти |
|---|---|---|
| Схеми комунікацій, блок-діаграми, PBS | TikZ (в LaTeX) | `figures/tikz/*.tex` |
| Електричні схеми | EasyEDA → export PDF | `figures/cad/*.pdf` |
| Механічні креслення, CAD | SolidWorks / FreeCAD → export PDF | `figures/cad/*.pdf` |
| Фото, скріншоти | PNG / JPG | `figures/photos/` |

> **TikZ діаграми** завжди як окремий `.tex` файл через `\input{}` — не вставляй TikZ-код
> прямо в текст розділу, це ускладнює git diff і злиття змін.

---

## Вимоги ERC 2026

| Параметр | Значення |
|---|---|
| Формат | A4, searchable PDF |
| Максимум сторінок | 25 (без cover, TOC, Appendix 3) |
| Мова | Англійська |
| Мінімальний шрифт | 10 pt |
| Поля | ≥ 2.54 cm (1 inch) з усіх сторін |
| Назва файлу | `<TeamName>_PreliminaryReport_ERC2026.pdf` |

---

## Чеклист перед здачею

- [ ] Заповнені метадані в `main.tex` (`\teamname` і т.д.)
- [ ] Всі `[placeholder]` замінені реальним контентом
- [ ] Документ компілюється без помилок (`latexmk -pdf main.tex`)
- [ ] Перевірена кількість сторінок (максимум 25)
- [ ] Перевірені поля і розмір шрифту
- [ ] MoC `.xlsx` файл вбудований у PDF через PDF-редактор (наприклад [Foxit](https://www.foxit.com/))
- [ ] Файл названий правильно: `<TeamName>_PreliminaryReport_ERC2026.pdf`
- [ ] Окремо здана Preliminary RF Form (без неї — дискваліфікація)

---

## Як вбудувати MoC (.xlsx) у PDF

ERC вимагає щоб Matrix of Compliance (`.xlsx`) була **вбудована прямо в PDF** як вкладення
(це дає 6 балів замість 2). Після компіляції:

1. Відкрий скомпільований PDF у **Foxit PDF Editor** (безкоштовна версія підходить)
2. `Document → Attach a File` → вибери `MoC_<TeamName>_ERC2026.xlsx`
3. Збережи PDF

---
