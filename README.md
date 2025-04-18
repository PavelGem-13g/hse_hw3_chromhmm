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

!java -mx5000M -jar /content/ChromHMM/ChromHMM.jar BinarizeBam -b 200  /content/ChromHMM/CHROMSIZES/hg19.txt /content/input_data/ cellmarkfiletable_rep1.txt   /content/binarizedData/rep1

🚀 Затем запустим обучение модели с флагом `-printposterior` для бонуса:

!java -mx5000M -jar /content/ChromHMM/ChromHMM.jar LearnModel -b 200  -printposterior /content/binarizedData/rep1/ /content/data/rep1 15 hg19

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

📘 Подробности смотри в полной таблице выше ⬆️

## 🧭 Genome Browser – обновленный вид

✍️ Переименуем состояния в `expanded.bed`, получим `expanded_new.bed` и снова визуализируем:  
📷 ![](/UCSC/_named.png)

# 🎁 Бонусное задание

⚡ Переход сразу к **SAGAconf**, так как ChromHMM уже использован выше.

# 🧠 SAGAconf

🔄 Используем внутренний парсер для преобразования:

!python SAGAconf/SAGAconf_parser.py --out_format bed --saga chmm /content/data/rep1/POSTERIOR/ 200 /content/SAGAconf_parser/rep1  
!python SAGAconf/SAGAconf_parser.py --out_format bed --saga chmm /content/data/rep2/POSTERIOR/ 200 /content/SAGAconf_parser/rep2

▶️ Запускаем анализ:

!python SAGAconf/SAGAconf.py /content/SAGAconf_parser/rep1/parsed_posterior.bed /content/SAGAconf_parser/rep2/parsed_posterior.bed /content/SAGAconf_bonus/

## 📊 Таблица воспроизводимости эпигенетических состояний по SAGAconf

📈 (Таблица уже оформлена эмодзи — всё отлично! 👍)

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