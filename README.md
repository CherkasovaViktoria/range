# Мэтчинг
Цель
- разработать алгоритм, который для всех товаров из validation.csv предложит несколько вариантов наиболее похожих товаров из base;
- оценить качество алгоритма по метрике accuracy@5.
# Данные
- `base.csv` - анонимизированный набор товаров. Каждый товар представлен как уникальный id (0-base, 1-base, 2-base) и вектор признаков размерностью 72.
- `target.csv` - обучающий датасет. Каждая строчка - один товар, для которого известен уникальный id (0-query, 1-query, …) , вектор признаков И id товара из base.csv, который максимально похож на него (по мнению экспертов)
- `validation.csv` - датасет с товарами (уникальный id и вектор признаков), для которых надо найти наиболее близкие товары из base.csv
- `validation_answer.csv` - правильные ответы к предыдущему файлу.
# Признаки
Все данные переведены в вектора признаков и описания о них нет


Целевой признак

- `Target` в `target.csv` — вектор наиболее подходящий по мнению эксперта
- `validation_answer.csv` - правильные ответы к `validation.csv`

Задачей проекта было научиться находить K-ближайших соседей при помощи новой библиотеки по усмотрению.Обучить Ml-модель на предсказывание правильных векторов для соданных признаков и ответов

При исследовании поиска k-ближайших соседей и принципы действия рекомендательных систем были использованы ML модели:
CatBoostClassifier()
FAISS

- При первичном исследовании датасета были обнаружены  аномалии в данных.
Часть признаков были удаленыб часть масштабированы, что позволило увеличить точность в десятки раз.
- Было проведено исследование библиотеки FAISS с использованием индекса L2 
- Был рассмотрен новый способ масштабирования данных AdjustedScaler, что позволило еще немного увеличить метрику на первых этапах.
-после ручного перебора параметров FAISS выбор был остановлен на настройкахЖ
-- n_cells = 50
-- quantizer = faiss.IndexFlatL2()
-- idx_l2.nprobe = 20
что увеличило метрику accuracy@5 для пяти ближайших векторов до величины 69.5
- Далее был создан датасет с признаками начальными и признаками векторов, которые подобрал мне FAISS
- Дополнительно были добавлены положительные ответы, которые были изначально в датасете для снижения дисбаланса классов и облегчения обучения модели CatBoostClassifier()
- при помощи GridSearchCV() были подобраны лучшие параметры  {'depth': 10, 'iterations': 60, 'learning_rate': 0.04} при лучшей метрике The best score across ALL searched params:
 0.8173759629370252
- Далее был проведен тест на тестовой выборбе где accuracy оказалось равно 0.8915292372668309.
- После такого результата я приступила к обработке и преобразованию датасета validation.csv, чтобы получить итоговые результаты.

Работа пока еще не закончена, но осталися последний этап.
# Выводы:
- Впервые разрабатывала модель для поиска рекамендаций, это было интересно, хоть и на начальных этапах довольно сложно.Думаю что в этом направлении необходимо больше развиваться т к теории в основном курсе совсем не было, а эти методы довольно востребованы на рынке.
- Изучила некоторые возможности FAISS.
- Поработала дополнительно с CatBoostClassifier()
- Получила навык создания признаков и таргета для обучения.
 