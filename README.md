# Machine-Unlearning-in-LLM

**Evaluating and Improving the Robustness of Machine Unlearning in Large Language Models against Adaptive Multi-Turn Knowledge Recovery Attacks**

Код дипломной работы. Языковую модель заставляют «забыть» часть знаний (unlearning на датасете TOFU), а затем адаптивные многоходовые атаки пытаются эти знания вернуть. Цель — измерить, насколько забывание устойчиво, и сделать его устойчивее. Работа идёт по «Плану кода» (части A–G), вычисления — в Google Colab.

## Блокноты

Каждая часть плана — отдельный блокнот. Открыть его можно кнопкой «Open in Colab» или в Colab через File → Open notebook → GitHub.

| Часть плана | Блокнот | Статус |
|---|---|---|
| A. Подготовка: доступы, машина, инструменты | [`notebooks/A_setup.ipynb`](notebooks/A_setup.ipynb) [![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/IvanovskyDev/Machine-Unlearning-in-LLM/blob/main/notebooks/A_setup.ipynb) | в работе |
| B. Окружения Python: `unl` и `atk` | — | следующая |
| C. Модели и данные TOFU | — | |
| D. Первые запуски руками | — | |
| E–F. Репозиторий, пакет `urec`, вехи M0–M8 | — | |

## Как устроен проект

Код пишется на Windows и отправляется в GitHub, всё, что требует GPU, считается в Google Colab, а результаты сохраняются на Google Drive. Машина Colab временная, поэтому у каждого файла заранее определено место:

| Где | Что лежит | Почему там |
|---|---|---|
| GitHub (этот репозиторий) | код, блокноты, конфиги, lock-файлы, итоговые таблицы и рисунки | маленькое, нужна история изменений |
| Google Drive, `MyDrive/unlearning_data/` | чекпоинты, сырые журналы атак, логи, лабораторный журнал, характеристики машин | уникальное, не должно пропасть при отключении Colab |
| Диск машины Colab, `/content/` | кэш Hugging Face, скачанные модели, окружения Python | большое, но за минуты скачивается заново |

Правила:

- Код и блокноты правятся в одном месте — на Windows — и уходят в GitHub; в Colab блокноты только открывают и запускают. Так правки не конфликтуют.
- Блокнот выполняется сверху вниз и начинается с ячейки «Старт сессии»: Drive, папки, переменные окружения, токен Hugging Face.
- Токен Hugging Face хранится только в Colab Secrets (`HF_TOKEN`) и никогда не попадает в код.
- Каждый запуск записывается в журнал: где, чем, с какой командой, что получилось.
- Всё новое сначала проверяется на Llama-3.2-1B и forget01, потом на 3B.
- С части E логика переезжает в пакет `src/urec` с тестами, а блокноты только запускают шаги и рисуют.

## Структура

```
Machine-Unlearning-in-LLM/
├── README.md
├── .gitignore          # веса, чекпоинты, логи и секреты в git не попадают
└── notebooks/
    └── A_setup.ipynb   # часть A
```

Дальше по плану (блок 20) появятся `src/urec/`, `configs/`, `tests/`, `scripts/`, `envs/`, `results/` и `docs/`.
