# Разработка системы предупреждения аварий на каршеринге

**Дано:** краткое описание таблиц:
- Общая информация о ДТП, таблица `collisions`. Имеет уникальный `'case_id'`. Эта таблица описывает общую информацию о ДТП. Например, где оно произошло и когда.
- Информация об участниках ДТП, таблица `parties`. Имеет неуникальный `'case_id'`, который сопоставляется с соответствующим ДТП в таблице collisions. Каждая строка здесь описывает одну из сторон, участвующих в ДТП. Если столкнулись две машины, в этой таблице должно быть две строки с совпадением `'case_id'`. Если нужен уникальный идентификатор, это `'case_id'` and `'party_number'`.
- Информация о пострадавших машинах, таблица `vehicles`. Имеет неуникальные `'case_id'` и неуникальные `'party_number'`, которые сопоставляются с таблицей `collisions` и таблицей `parties`. Если нужен уникальный идентификатор, это `'case_id'` and `'party_number'`.

**Цель:** создание системы для оценки вероятности получения повреждений транспортного средства по выбранному маршруту движения.

## Применяемые библиотеки: 

pandas, numpy, seaborn, plotly, sklearn, category_encoders, phik, optuna, xgboost, catboost, lightgbm, torch, scipy, association_metrics, shap

## Вывод

**1. Решение задачи на основе рекомендаций заказчика:** лучшие результаты показывает LGBMClassifier, recall=0.985 и весьма хорошие метрики f1=0.673 и AUPRC=0.707, но при всём это модель очень плохо определяет истинную метку положительного класса precision=0.511, и распознаёт правильно мтолько 51,7% меток. По сути мы практически не видим тех наблюдений, которые не приводят к авариям.

**2. Собственное решение задачи:** выполнено переопределение целевого признака на `'collision_damage'`. Выбран способ решения задачи бинарной классификации, где обозначено за 0 минимальные повреждения 'scratch', за 1 более серьёзные. Выезд на дорогу — это всегда риск получить повреждения, таким образом мы будет обучать модель определять, когда риск получить минимальные повреждения наиболее вероятен, нежели второй вариант, что напрямую решает задачу бизнеса о оптимизации расходов. Модель показывает вполне приемлемые метрики производительности recall=0.751 и precision=0.719, и распознаёт 70,6% меток. Если получить дополнительные признаки, указанных в рекомендациях выше, модель может показать отличные рабочие показатели для бизнеса. Таким образом можно утверждать, что ML решение на основе таргета `'collision_damage'` даёт более производительную модель, чем решение на основе таргета `'at_fault'`.

---
Автор: Евтухов Павел

E-mail: xswepp@tut.by

https://t.me/xswepp
