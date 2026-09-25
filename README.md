# Machine-Unlearning-in-LLM

**Evaluating and Improving the Robustness of Machine Unlearning in Large Language Models against Adaptive Multi-Turn Knowledge Recovery Attacks**

Код дипломной работы. Языковую модель заставляют «забыть» часть знаний (unlearning на датасете TOFU), а затем адаптивные многоходовые атаки пытаются эти знания вернуть. Цель — измерить, насколько забывание устойчиво, и сделать его устойчивее. Работа идёт по «Плану кода» (части A–G), вычисления — в Google Colab.

## Блокноты

Каждая часть плана — отдельный блокнот. Открыть его можно кнопкой «Open in Colab» или в Colab через File → Open notebook → GitHub.

| Часть плана | Блокнот | Статус |
|---|---|---|
| A. Подготовка: доступы, машина, инструменты | [`notebooks/A_setup.ipynb`](notebooks/A_setup.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/IvanovskyDev/Machine-Unlearning-in-LLM/blob/main/notebooks/A_setup.ipynb) | работает |
| B. Окружения Python: `unl` и `atk` | [`notebooks/B_environments.ipynb`](notebooks/B_environments.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/IvanovskyDev/Machine-Unlearning-in-LLM/blob/main/notebooks/B_environments.ipynb) | работает |
| C. Модели и данные TOFU | — | следующая |
| D. Первые запуски руками | — | |
| E–F. Репозиторий, пакет `urec`, вехи M0–M8 | — | |

Что и зачем делает каждая строка частей A и B, объяснено для новичка в [`docs/A_B_explained.md`](docs/A_B_explained.md).

## Как устроен проект

Код пишется на Windows и отправляется в GitHub, всё, что требует GPU, считается в Google Colab, а результаты сохраняются на Google Drive. Машина Colab временная, поэтому у каждого файла заранее определено место:

| Где | Что лежит | Почему там |
|---|---|---|
| GitHub (этот репозиторий) | код, блокноты, конфиги, lock-файлы, итоговые таблицы и рисунки | маленькое, нужна история изменений |
| Google Drive, `MyDrive/unlearning_data/` | чекпоинты, результаты атак, логи, лабораторный журнал, lock-файлы окружений (в вехе M0 переедут в репозиторий) | уникальное, не должно пропасть при отключении Colab |
| Диск машины Colab, `/content/` | кэш Hugging Face, скачанные модели, окружения Python | большое, но за минуты скачивается заново |

Правила:

- Код и блокноты правятся в одном месте — на Windows — и уходят в GitHub; в Colab блокноты только открывают и запускают. Так правки не конфликтуют.
- Блокнот выполняется сверху вниз. Машина Colab каждый раз новая, поэтому каждая сессия начинается с шагов 1–2 части B (Drive, папки, переменные окружения, токен Hugging Face, uv), а окружения `unl` и `atk` собираются заново из lock-файлов (шаг 14 части B). `pip install -U` в них не делается.
- Токен Hugging Face хранится только в Colab Secrets (`HF_TOKEN`) и никогда не попадает в код.
- Каждый запуск записывается в журнал: где, чем, с какой командой, что получилось.
- Всё новое сначала проверяется на Llama-3.2-1B и forget01, потом на 3B.
- С части E логика переезжает в пакет `src/urec` с тестами, а блокноты только запускают шаги и рисуют.

## Структура

```
Machine-Unlearning-in-LLM/
├── README.md
├── .gitignore          # веса, чекпоинты, логи и секреты в git не попадают
├── docs/
│   └── A_B_explained.md       # разбор частей A и B для новичка
└── notebooks/
    ├── A_setup.ipynb          # часть A
    └── B_environments.ipynb   # часть B
```

Дальше по плану (блок 20) появятся `src/urec/`, `configs/`, `tests/`, `scripts/`, `envs/` и `results/`.
