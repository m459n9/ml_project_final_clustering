# signal-types-classification

Решение Kaggle-задачи [signal-types-classification](https://www.kaggle.com/competitions/signal-types-classification):
кластеризация 23 479 сигналов сцинтилляционного детектора на три класса (гамма, нейтроны и
шумовые / неоднозначные события).

## Файлы

- `solution.ipynb` — основной ноутбук: загрузка, EDA, признаки, перебор моделей, финальная
  кластеризация и сохранение submission.
- `submission.csv` — итоговый файл для отправки на Kaggle (public score 0.80650).
- `kaggle_leaderboard.png` — скрин публичного лидерборда.
- `Run200_Wave_0_1.txt.gz` — сжатый исходник соревнования (~10 МБ).
  Ноутбук распаковывает его сам в `Run200_Wave_0_1.txt` при первом запуске.

## Запуск

```
pip install numpy pandas scipy scikit-learn matplotlib jupyter
jupyter notebook solution.ipynb
```

Ноутбук запускается сверху вниз. `random_state=42` зафиксирован, `submission.csv` перезаписывается
на последнем шаге. Если `Run200_Wave_0_1.txt` ещё нет в папке — он будет распакован из `.gz`
автоматически.

## Финальная модель

KMeans (k=3, n_init=80) на 25-мерной матрице: 17 положительных признаков (амплитуда, площади, PSD,
времена спада, шум) через `log1p` и `RobustScaler` + 3 знаковых признака формы через `RobustScaler`
+ 5 главных компонент нормированной формы импульса. На первый блок (амплитуда/площади) накладывается
вес `0.52`, на блок PSD/хвостовых отношений — `0.95`.

Размеры кластеров: 7358 / 11302 / 4819. Public score на Kaggle — 0.80650.
