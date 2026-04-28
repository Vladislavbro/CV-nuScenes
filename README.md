# CV-nuScenes

Проект посвящен экспериментам с датасетом [nuScenes](https://www.nuscenes.org/download) для задач восприятия сцены в формате Bird's-Eye View (BEV). Основной фокус: сравнение camera-only и camera+radar моделей, проверка устойчивости к ухудшению входных данных и оптимизация confidence threshold в зависимости от расстояния до объекта.

## Что внутри

- `BEV_1.ipynb` - базовая работа с `nuScenes mini`, `lidarseg`, визуализация сенсоров и подготовка данных.
- `Baseline_models_comparisons.ipynb` - сравнение camera-only и camera+radar моделей на clean/dark/random camera failure сценариях.
- `Threshold_optimization_+_comparison.ipynb` - distance-based adaptive confidence threshold для BEV inference и сравнение с baseline.
- `Failed_Fine_tune_experiment.ipynb` - неудачный эксперимент fine-tuning с измененной loss function и повышенным весом дальних объектов.
- `images/` - сохраненные визуализации nuScenes-сцен с камер, радара и LiDAR.

## Данные и модели

Для экспериментов используется `nuScenes mini` и `nuScenes-lidarseg mini`.

Полезные ссылки:

- [nuScenes download page](https://www.nuscenes.org/download)
- [Google Drive с дополнительными файлами проекта](https://drive.google.com/file/d/1DRbeJ1OboL37W7Dl4eBbCTzvC-Uk1oLf/view?usp=sharing)

В ноутбуках также загружаются pre-trained checkpoints для camera-only и camera+radar вариантов модели.

## Краткие результаты

В baseline-сравнении camera+radar модель показала более высокую устойчивость, особенно при плохом освещении и отказе одной камеры:

- Clean: camera-only `52.32` Mean IoU, camera+radar `56.79` Mean IoU.
- Dark: camera-only `33.57` Mean IoU, camera+radar `44.60` Mean IoU.
- Random camera failure: camera-only `45.81` Mean IoU, camera+radar `49.28` Mean IoU.

В `Threshold_optimization_+_comparison.ipynb` дополнительно проверяется идея адаптивного порога уверенности: более строгий threshold для ближней зоны и более мягкий threshold для дальних объектов, где модель обычно менее уверена.

## Визуализации

Ниже приведены изображения из папки `images/`. Они показывают пример сцены nuScenes: radar/LiDAR вид сверху и шесть камер автомобиля.

![nuScenes scene visualization 1](images/image.png)

![nuScenes scene visualization 2](images/image%20copy.png)

![nuScenes scene visualization 3](images/image%20copy%202.png)

## Как запускать

Проект удобнее запускать в Google Colab или другой среде с GPU. Базовый порядок работы:

1. Открыть нужный ноутбук.
2. Установить зависимости из первых ячеек (`nuscenes-devkit`, `tensorboardX`, `timm`, `efficientnet_pytorch`, `lyft-dataset-sdk`).
3. Скачать `nuScenes mini`, `lidarseg mini`, карты и checkpoints.
4. Запустить подготовку данных и evaluation cells.
5. Сравнить результаты по IoU и визуализациям.

## Основная идея

BEV-представление объединяет информацию с камер, радара и LiDAR в единой системе координат. Это удобно для autonomous driving, потому что позволяет оценивать окружение автомобиля в метрическом пространстве. Эксперименты в этом репозитории показывают, что добавление радара улучшает устойчивость модели в сложных условиях, а адаптация inference threshold по расстоянию может быть полезной для дальних объектов.