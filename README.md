# 🧬 hse_hw3_chromhmm

## 👨‍💻 Подготовил

Пашенцев Павел Владимирович

---

## 📘 Введение

🎯 Целью данной работы является аннотация генома клеточной линии **DND-41** на основе эпигенетических состояний, определяемых по данным ChIP-seq 🧬 для гистоновых модификаций.  
🧠 Для анализа использовалась программа **ChromHMM**, реализующая скрытую марковскую модель и алгоритм Баума-Велша 🔁.

🧫 Клеточная линия **DND-41** представляет собой линию Т-клеточной остролимфобластной лейкемии человека и интересна своими характерными паттернами регуляции транскрипции 📉 и эпигенетической репрессии 🔒. Это делает её подходящей моделью для анализа активности промоторов 🚦, энхансеров 🚀 и Polycomb-связанных регионов 🧲.

Также в работе проводится оценка воспроизводимости аннотированных состояний при помощи метода **SAGAconf** 🔍.

---

# 💻 Код

📁 Весь код с комментариями доступен в [src/main.ipynb](/src/main.ipynb)

## 🗂️ Исходные данные

⬇️ Скачаны ChIP-seq BAM-файлы с сайта UCSC для клеточной линии **DND-41**.

📦 Скачаем BAM-файлы контрольного эксперимента и выравнивания 10 гистоновых модификаций:

## 📌 Основное задание

| 🧪 Гистонная метка | 🧾 Название файла |
|---|---|
| Control | wgEncodeBroadHistoneDnd41ControlStdAlnRep1.bam |
| H4k20me1 | wgEncodeBroadHistoneDnd41H4k20me1AlnRep1.bam |
| Ctcf | wgEncodeBroadHistoneDnd41CtcfAlnRep1.bam |
| Ezh239875 | wgEncodeBroadHistoneDnd41Ezh239875AlnRep1.bam |
| H2az | wgEncodeBroadHistoneDnd41H2azAlnRep1.bam |
| H3k04me1 | wgEncodeBroadHistoneDnd41H3k04me1AlnRep1.bam |
| H3k04me2 | wgEncodeBroadHistoneDnd41H3k04me2AlnRep1.bam |
| H3k04me3 | wgEncodeBroadHistoneDnd41H3k04me3AlnRep1.bam |
| H3k09ac | wgEncodeBroadHistoneDnd41H3k09acAlnRep1.bam |
| H3k27ac | wgEncodeBroadHistoneDnd41H3k27acAlnRep1.bam |
| H3k36me3 | wgEncodeBroadHistoneDnd41H3k36me3AlnRep1.bam |

## 🎁 Бонусное задание 

📊 Для бонусного задания использованы вторые реплики данных:

| 🧪 Гистонная метка | 🧾 Название файла |
|---|---|
| Control | wgEncodeBroadHistoneDnd41ControlStdAlnRep2.bam |
| H4k20me1 | wgEncodeBroadHistoneDnd41H4k20me1AlnRep2.bam |
| Ctcf | wgEncodeBroadHistoneDnd41CtcfAlnRep2.bam |
| Ezh239875 | wgEncodeBroadHistoneDnd41Ezh239875AlnRep2.bam |
| H2az | wgEncodeBroadHistoneDnd41H2azAlnRep2.bam |
| H3k04me1 | wgEncodeBroadHistoneDnd41H3k04me1AlnRep2.bam |
| H3k04me2 | wgEncodeBroadHistoneDnd41H3k04me2AlnRep2.bam |
| H3k04me3 | wgEncodeBroadHistoneDnd41H3k04me3AlnRep2.bam |
| H3k09ac | wgEncodeBroadHistoneDnd41H3k09acAlnRep2.bam |
| H3k27ac | wgEncodeBroadHistoneDnd41H3k27acAlnRep2.bam |
| H3k36me3 | wgEncodeBroadHistoneDnd41H3k36me3AlnRep2.bam |

# 🧠 ChromHMM

🛠️ Соберем файлы в `cellmarkfiletable.txt` для дальнейшей работы с ChromHMM.

📄 Файлы:  
![cellmarkfiletable_rep1.txt](/data/cellmarkfiletable/cellmarkfiletable_rep1.txt)  
![cellmarkfiletable_rep2.txt](/data/cellmarkfiletable/cellmarkfiletable_rep2.txt)  
📂 Все файлы анализа хранятся в ![ChromHMM](/ChromHMM/)

# 🧾 Код

📦 Сначала проведем бинаризацию данных:

```
!java -mx5000M -jar /content/ChromHMM/ChromHMM.jar BinarizeBam -b 200  /content/ChromHMM/CHROMSIZES/hg19.txt /content/input_data/ cellmarkfiletable_rep1.txt   /content/binarizedData/rep1
```

🚀 Затем запустим обучение модели с флагом `-printposterior` для бонуса:

```
!java -mx5000M -jar /content/ChromHMM/ChromHMM.jar LearnModel -b 200  -printposterior /content/binarizedData/rep1/ /content/data/rep1 15 hg19
```

## 📊 Результаты

### 📌 Обработка основного датасета

| | | | | |
|---|---|---|---|---|
| ![](/ChromHMM/rep1/emissions_15%20(1).png) | ![](/ChromHMM/rep1/transitions_15%20(1).png) | ![](/ChromHMM/rep1/Dnd41_15_overlap%20(1).png) | ![](/ChromHMM/rep1/Dnd41_15_RefSeqTES_neighborhood%20(1).png) | ![](/ChromHMM/rep1/Dnd41_15_RefSeqTSS_neighborhood%20(1).png) |

### 📌 Обработка дополнительного датасета

| | | | | |
|---|---|---|---|---|
| ![](/ChromHMM/rep2/emissions_15%20(2).png) | ![](/ChromHMM/rep2/transitions_15%20(2).png) | ![](/ChromHMM/rep2/Dnd41_15_overlap%20(2).png) | ![](/ChromHMM/rep2/Dnd41_15_RefSeqTES_neighborhood%20(2).png) | ![](/ChromHMM/rep2/Dnd41_15_RefSeqTSS_neighborhood%20(2).png) |

# 🧬 Назначение биологических ролей

## 🔎 Genome Browser

🧭 Загрузим `expanded.bed` в Genome Browser и визуализируем роли состояний.  
📋 Используем рекомендации по 15 классам:

📷 ![](/UCSC/_numbers.png)

## 🧾 Таблица

| №  | Имя состояния               | Соответствующая метка        | Биологическая роль                                                                                                                                       |
|----|-----------------------------|-------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1  | Active Promoter             | H3K4me3, H3K27ac, H3K9ac      | Характерен высоким уровнем меток активных промоторов. Обогащён вблизи TSS (Transcription Start Site), CpG-островков и генов. Участвует в инициации транскрипции. |
| 2  | Weak Promoter               | H3K4me3, H3K4me2              | Умеренные уровни меток промоторов, менее активен, чем состояние 1. Возможно, регулирует транскрипцию в тканеспецифичной или временной манере.                |
| 3  | Poised Promoter             | H3K27me3, EZH2                | Комбинирует активные и репрессирующие метки. Обычно находится в неактивном состоянии, но может быть быстро активирован. Часто в эмбриональных/стволовых клетках. |
| 4  | Strong Enhancer             | H3K4me1, H3K27ac              | Высокоактивные энхансеры, регулирующие экспрессию удалённых генов. Часто активны в конкретных тканях или условиях.                                         |
| 5  | Strong Enhancer             | H3K4me1, H3K27ac              | Похож на состояние 4, но может различаться по контексту использования. Также представлен в интергенных регуляторных регионах.                             |
| 6  | Weak Enhancer               | H3K4me1                       | Слабые или подготовленные энхансеры, могут активироваться при определённых сигналах. Часто встречаются вблизи генов, не обязательно активны.               |
| 7  | Weak Enhancer               | H3K4me1                       | Также представляет собой слабый или неактивный энхансер. Обогащён около TES, может участвовать в завершении транскрипции или длинных регуляторных петлях.    |
| 8  | Insulator                   | CTCF                          | Обогащён меткой CTCF. Работает как граница между доменами хроматина (например, TAD), предотвращая неконтролируемое взаимодействие энхансеров и промоторов.   |
| 9  | Transcriptional Transition | H3K36me3                      | Переходная зона транскрипции, возможный участок переключения между активной и слабой транскрипцией. Расположен вдоль тела гена.                            |
| 10 | Transcriptional Elongation | H3K36me3                      | Связан с активной транскрипцией — удлинением мРНК-полимеразой II. Находится в телах активно транскрибируемых генов.                                         |
| 11 | Weak Transcribed            | Низкий уровень H3K36me3       | Участки с низким уровнем транскрипции, возможно, неполно активные или частично репрессированные участки тела гена.                                          |
| 12 | Polycomb Repressed          | H3K27me3, EZH2                | Репрессированные участки, связанные с Polycomb-комплексом. Часто участвуют в долговременном подавлении транскрипции, особенно генов развития.               |
| 13 | Heterochromatin             | H4K20me1                      | Плотно упакованный хроматин, слабо доступный для транскрипции. Часто расположен в перицентромерных, теломерных или ламина-ассоциированных регионах.         |
| 14 | Repetitive / CNV            | Низкий сигнал                 | Повторяющиеся последовательности или регионы вариабельного числа копий (CNV), низкий уровень всех меток. Часто неинформативен с точки зрения регуляции.     |
| 15 | Repetitive / CNV            | Низкий сигнал                 | То же, что и состояние 14. Вероятно, технический шум или структурные особенности генома (например, сателлитные ДНК, LINE/ALU-повторы).                        |


## 🧭 Genome Browser – обновленный вид

✍️ Переименуем состояния в `expanded.bed`, получим `expanded_new.bed` и снова визуализируем:  
📷 ![](/UCSC/_named.png)

# 🎁 Бонусное задание

⚡ Переход сразу к **SAGAconf**, так как ChromHMM уже использован выше.

# 🧠 SAGAconf

🔄 Используем внутренний парсер для преобразования:

```
!python SAGAconf/SAGAconf_parser.py --out_format bed --saga chmm /content/data/rep1/POSTERIOR/ 200 /content/SAGAconf_parser/rep1  
!python SAGAconf/SAGAconf_parser.py --out_format bed --saga chmm /content/data/rep2/POSTERIOR/ 200 /content/SAGAconf_parser/rep2
```

▶️ Запускаем анализ:

```
!python SAGAconf/SAGAconf.py /content/SAGAconf_parser/rep1/parsed_posterior.bed /content/SAGAconf_parser/rep2/parsed_posterior.bed /content/SAGAconf_bonus/
```

## 📊 Таблица воспроизводимости эпигенетических состояний по SAGAconf

| State | Название                     | R² Score | Интерпретация                                                                                 |
|-------|-------------------------------|----------|-----------------------------------------------------------------------------------------------|
| 1     | Active Promoter               | 0.985    | 🔥 Очень высокая воспроизводимость — активные промотеры определяются почти одинаково         |
| 2     | Weak Promoter                 | 0.866    | 👍 Хорошее соответствие — умеренно активные участки узнаются обоими методами                 |
| 3     | Poised Promoter               | 0.898    | 👍 Надёжное совпадение, несмотря на гибкость состояния                                        |
| 4     | Strong Enhancer               | 0.667    | 🟡 Умеренное соответствие — чувствительно к выбору треков, возможно тканеспецифичное          |
| 5     | Strong Enhancer               | 0.506    | 🟡 Посредственная согласованность — возможна биологическая вариативность                     |
| 6     | Weak Enhancer                 | 0.732    | 🟢 Довольно устойчиво определяются                                                            |
| 7     | Weak Enhancer                 | 0.404    | 🔻 Низкая воспроизводимость — чувствительно к шуму и границам сегментов                     |
| 8     | Insulator                     | 0.278    | 🔻 Очень низкое согласие — CTCF-сайты определяются по-разному, зависит от точности пиков     |
| 9     | Transcriptional Transition    | 0.392    | 🔻 Низкая согласованность — границы тела генов размечаются по-разному                       |
| 10    | Transcriptional Elongation    | 0.482    | 🟡 Средняя воспроизводимость — методы различно интерпретируют длину транскрибируемых областей|
| 11    | Weak Transcribed              | 0.871    | 🟢 Хорошо воспроизводится, несмотря на "слабость" сигнала                                     |
| 12    | Polycomb Repressed            | 0.480    | 🟡 Полумерное соответствие — возможны разные подходы к подавленным областям                  |
| 13    | Heterochromatin               | 0.639    | 🟡 Средняя согласованность — могут быть ошибки из-за структурных сложностей                 |
| 14    | Repetitive / CNV              | 0.336    | 🔻 Слабо воспроизводится — повторяющиеся регионы плохо размечаются                           |
| 15    | Repetitive / CNV              | 0.335    | 🔻 То же самое                                                                                 |

---

## 🧾 Основные выводы

✅ **Высокая воспроизводимость** — промоторы и транскрибируемые участки  
⚠️ **Умеренное согласие** — энхансеры, гетерохроматин  
❌ **Низкая воспроизводимость** — CTCF, transition/elongation, CNV

📌 Для спорных случаев — ручная проверка в Genome Browser и учёт биологических факторов 🧠

## 🎨 Визуализация

| 🧪 Калибровка | 📊 Матрица вероятностей | 🔥 Интенсивность | 🌡️ Карта |
|---|---|---|---|
| ![](/data/SAGAconf/clb_subplot-_1_.png) | ![](/data/SAGAconf/binned_posterior_heatmap.png) | ![](/data/SAGAconf/heatmap_w.png) | ![](/data//SAGAconf/heatmap-_1_.png) |