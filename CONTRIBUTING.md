# Як заповнювати звіт — гайд для команди

> Прочитай цей файл перед тим як писати будь-який LaTeX.
> Тут є відповіді на всі типові питання.

---

## Твій файл — твоя зона відповідальності

Кожна команда працює у **двох файлах** і **одній папці**:

```
teams/<твоя_команда>/
├── design.tex      ← технічний опис підсистеми (ти пишеш тут)
├── tests.tex       ← рядки тест-плану (ти пишеш тут)
└── figures/        ← твої діаграми і фото (ти кладеш сюди)
```

**Не чіпай** нічого за межами своєї папки, крім випадків коли явно домовились.

---

## Як вставити фото або CAD-схему

1. Поклади файл у `teams/<твоя_команда>/figures/`
   - Фото: `.jpg` або `.png`
   - CAD / EasyEDA схема: експортуй як **PDF** і клади `.pdf`

2. Вставляй у `design.tex` ось так:

```latex
\begin{figure}[H]
    \centering
    \includegraphics[width=0.9\linewidth]{teams/arm/figures/arm_cad}
    \caption{Robotic arm — mechanical overview}
    \label{fig:arm_cad}
\end{figure}
```

> Розширення файлу (`.jpg`, `.pdf`) вказувати **не потрібно**.

---

## Як вставити TikZ діаграму (схема комунікацій і т.д.)

1. TikZ-код зберігай окремим файлом у `teams/<твоя_команда>/figures/`:

```
teams/ground_station/figures/gs_communication.tex
```

2. Вставляй через `\input{}`:

```latex
\begin{figure}[H]
    \centering
    \resizebox{\linewidth}{!}{%
        \input{teams/ground_station/figures/gs_communication}%
    }
    \caption{Ground station communication schema}
    \label{fig:gs_comm}
\end{figure}
```

> `\resizebox{\linewidth}{!}{...}` автоматично масштабує діаграму під ширину сторінки.

---

## Як заповнити тест-план (`tests.tex`)

Твій `tests.tex` визначає макрос `\XXXtestrows` — набір рядків для великої таблиці тестів.

**Формат одного рядка:**
```
ID & Назва тесту & REQ-XX & Опис підходу і стенду & Критерій pass/fail & Місяць & Статус \\
```

**Приклад для arm:**
```latex
\newcommand{\armtestrows}{%
ARM-01 & End-effector precision & REQ-MEC-030 &
    Measure repeatability of grasp at 3 positions using calibration fixture. &
    Position error < 5\,mm across 10 trials. &
    Apr 2026 & Planned \\[4pt]
ARM-02 & Payload handling & REQ-MEC-031 &
    Lift 3\,kg payload at full reach and place on target. &
    No motor overload; placement error < 10\,mm. &
    Apr 2026 & Planned \\[4pt]
}
```

> Статуси: `Planned` / `In Progress` / `Pass` / `Fail`

---

## Як вставити таблицю специфікацій

Шаблон вже містить таблиці в кожному `design.tex`. Просто заміни `[X]` і `[Description]` на реальні значення:

```latex
\begin{table}[H]
    \centering
    \caption{Arm joint actuator specifications}
    \label{tab:arm_joints}
    \begin{tabular}{C{0.8cm} L{2.5cm} L{3cm} R{2.2cm} R{2.2cm}}
        \toprule
        \textbf{Joint} & \textbf{Name} & \textbf{Actuator} &
            \textbf{Torque (N$\cdot$m)} & \textbf{Speed (°/s)} \\
        \midrule
        J1 & Base rotation  & Dynamixel MX-106  & 8.4  & 45 \\
        J2 & Shoulder pitch & Dynamixel MX-106  & 8.4  & 45 \\
        J3 & Elbow pitch    & Dynamixel MX-64   & 6.0  & 60 \\
        \bottomrule
    \end{tabular}
\end{table}
```

---

## Корисні команди (вже визначені в preamble.tex)

### Статус відповідності вимогам
```latex
\compliant{}        % зелена C
\partlycompliant{}  % оранжева PC
\noncompliant{}     % червона NC
\notapplicable{}    % N/A
```

### Рівень ризику (для RIO таблиці)
```latex
\riskhigh{}    % червона плашка High
\riskmedium{}  % оранжева плашка Medium
\risklow{}     % зелена плашка Low
```

### Колір клітинки таблиці (для RIO матриці)
```latex
\cellcolor{red!40}     % RI >= 15
\cellcolor{orange!40}  % RI 6–14
\cellcolor{green!30}   % RI <= 5
```

---

## Типові помилки

| Помилка | Як правильно |
|---|---|
| Вставив фото як `.png` без шляху | `\includegraphics{teams/arm/figures/photo}` — вказуй повний шлях від кореня |
| TikZ-код прямо в `design.tex` | Виноси в окремий `.tex` у `figures/`, підключай через `\input{}` |
| `\fbox{[Insert diagram here]}` залишив у файлі | Видаляй заглушки якщо ще немає контенту — вони займають місце на сторінці |
| Дуже довгий текст | ERC рекомендує мінімум тексту, максимум схем і таблиць |
| Компіляція падає з `Undefined control sequence` | Запусти `latexmk -pdf main.tex` з кореневої папки, не з `teams/` |

---

## Компіляція

Завжди компілюй з кореневої папки репозиторію:

```bash
cd preliminary-report/   # корінь репо
latexmk -pdf main.tex

# Очистити допоміжні файли:
latexmk -c
```

---

## Командні файли — хто що заповнює

| Команда | design.tex | tests.tex |
|---|---|---|
| Arm | Маніпулятор: DoF, моторі, IK, контроль | ARM-01, ARM-02, ... |
| Drone | Тип дрона, авіоніка, комунікація, автономія | DRN-01, DRN-02, ... |
| Electronics | Живлення, обчислення, шини даних, PCB | ELE-01, ELE-02, ... |
| Ground Station | GS hardware/software, схема комунікацій, RF | GST-01, GST-02, ... |
| Navigation | Сенсори, SLAM, path planning, ROS стек | NAV-01, NAV-02, ... |
| Science | Інструменти, збір зразків, методологія | SCI-01, SCI-02, ... |
| Suspension | Тип підвіски, колеса, моторі, FEA | SUS-01, SUS-02, ... |

---

## Питання?

Питай у командному чаті або відкривай issue в репо.
