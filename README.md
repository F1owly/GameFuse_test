# GameFuse: Анализ и выделение игровых механик из отзывов
## Описание проекта
Данный проект направлен на выделение и анализ игровых механик из отзывов критиков и пользователей на сайте Metacritic. В рамках решения задачи была реализована обработка и кластеризация текстов, позволяющая выявить ключевые аспекты, описывающие части игрового процесса, поднимаемые в нескольких отзывах на одну игру.

В процессе работы были собраны и обработаны следующие данные:

Отзывы критиков: 500 отзывов на 8 игр с оценками и метками игр.
Отзывы пользователей: 7500 отзывов, содержащих мнения пользователей по этим же играм.
Все данные хранятся в файле Parse/datasets/final_datasets/total_critics.csv, включающем обработанные тексты, оценки и метки игр.

### Часть 1: Предобработка данных
Языковая фильтрация: Убедился, что все отзывы в файле критиков написаны на английском языке (некоторые отзывы на китайском были исключены с помощью библиотеки pycld2).
Очистка текста: Удалены посторонние символы и лишние данные.
Нормализация текста: Проведена токенизация и лемматизация текста для приведения к стандартной форме.
### Часть 2: Выделение игровых механик
Игровыми механиками считаются ключевые тематики, описывающие различные аспекты игрового процесса. Эти механики могут быть отражены в нескольких отзывах для одной игры и описывают схожие элементы игрового опыта.

### Часть 3: Выделение эмбеддингов и кластеризация
Для выявления механик выполнены следующие шаги:

Вычисление эмбеддингов: На основе TF-IDF представлений токенов с использованием модели BERT для создания эмбеддингов.
Кластеризация отзывов: Применен алгоритм K-Means из библиотеки scikit-learn, что позволило разбить отзывы на несколько кластеров. Каждый кластер соответствует определённому классу тематики отзывов.
Реализация всех этапов содержится в файле main.py, включающем два основных класса:

* Preprocess — предобработка данных
* Algorithm — кластеризация и выделение сущностей

Перед запуском последней части, необходимо будет сформировать эмбединги:
```python
bg3 = Preprocess("Parse/datasets/final_datasets/bg3_critics_stemmed.csv")

bg3.bert_embedings("cls")

bg3.save_csv("Parse/datasets/final_datasets/bg3_critics_embeding_cls")

```
После чего на основе полученого файла с эмбедингами отзывов можно демонстрировать работу функций из класса Algorithm
### Часть 4: Анализ результатов
В результате мы получили разбивку отзывов на классы, где каждая сущность является приоритетной темой, обсуждаемой в данном классе. Анализ же на основе высоких значений TF-IDF для ключевых слов позволит определить второстепенные механики, описанные в данном отзыве.

### Результаты и проблемы
Вначале была предпринята попытка применения классического подхода к тематическому моделированию с использованием моделей BERTopic и LDA. Однако эти методы не дали ожидаемых результатов, выделив лишь два несбалансированных класса для игры Baldur’s Gate 3. Вероятная причина в том, что модели тематического моделирования эффективно выявляют крупные темы, но не подходят для анализа более мелких и глубоких механик, часто обсуждаемых в одном отзыве.

### Заключение
Данный подход позволил нам выделить из отзывов основные тематики, связанные с игровыми механиками, и подготовить их для дальнейшего анализа. В перспективе можно рассмотреть более сложные методы извлечения аспектов для точного определения различных игровых механик.
