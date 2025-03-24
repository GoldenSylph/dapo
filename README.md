# Мнение партии: формализация модели

## 1. Формальная модель

Мнение партии $\mathcal{M}$ формализуется как кортеж из двух матриц случайных величин:

$$
\mathcal{M} = (\mathbf{X}, \mathbf{Y})
$$

где:

- $\mathbf{X} \in \mathbb{R}^{m \times n}$ — матрица **непрерывных случайных величин**. Каждая ячейка $X_{i,j}$ описывает количественную оценку или показатель, важный для идеологической или программной позиции партии.
- $\mathbf{Y} \in \mathbb{Z}^{p \times q}$ — матрица **дискретных случайных величин**, описывающих качественные, категориальные или оценочные суждения партии по соответствующим вопросам.

Каждая ячейка матрицы содержит параметры случайной величины:
- для $\mathbf{X}$: $X_{i,j} \sim \mathcal{D}_{i,j}(\theta)$ — нормальное, логнормальное, бета и т.д.
- для $\mathbf{Y}$: $Y_{k,l} \sim \text{Categorical}(p_1, ..., p_r)$ — вероятности по категориям мнений.

Модель $\mathcal{M}$ может изменяться во времени:

$$
\mathcal{M}_t \rightarrow \mathcal{M}_{t+1} = \mathcal{M}_t + \Delta\mathcal{M}
$$

## Обоснование перехода к эмпирической модели

Формальная модель мнения партии, представленная как кортеж параметризованных распределений, теоретически точна и математически строгая. Однако при реализации жизненного цикла $M$ в инфраструктуре DAO на языке Solidity возникает ряд ограничений:

- Невозможно хранить непрерывные распределения непосредственно в on-chain памяти без значительных затрат.
- Отсутствие нативной поддержки вещественных чисел в Solidity ограничивает работу с параметрами распределений.
- Участие членов DAO, голосующих за значения и оценки, формирует выборки, а не готовые распределения.

В этой связи более реалистичным и практически пригодным подходом становится использование **накапливаемых выборок**. Таким образом, ячейки матриц мнения хранят не априорно заданные распределения, а выборки значений, которые можно агрегировать и аппроксимировать off-chain или по запросу on-chain.

Это позволяет связать формальную модель с технической реализацией в рамках DAO-процессов, не теряя смысловой строгости и соответствия исходной структуре.

## 3. Сводная формула и определение агрегирующей функции $f$

$$
M = (S_X, S_Y) \\
\hat{M} = f(S_X, S_Y)
$$

Где $f$ — это функция агрегирования, которая строит оценку текущего мнения партии на основании накопленных выборок. Эта функция применяется:

1. **В рамках внутренних процессов партии**: когда необходимо определить доминирующее мнение по инициативе, теме или показателю.
2. **При принятии решений согласно Уставу**: агрегированное значение или распределение может служить основанием для проведения голосования съездом, конференцией или советом.

### Формальное определение функции $f$

Функция $f$ определяется как преобразование:

$f : (S_X, S_Y) → (\hat{X}, \hat{Y})$


Где:
- $\hat{X}_{i,j} = \text{Estimate}(S_X[i][j])$ — оценка распределения (например, метод моментов, медиана, дисперсия) или агрегированная величина (например, среднее значение, доверительный интервал).
- $\hat{Y}_{k,l} = \text{Histogram}(S_Y[k][l])$ — оценка вероятностей категорий по частотам выборки.

### Пример использования в процедурах DAO:
- Если изменение мнения ($\hat{M}'$) приводит к отклонению от предыдущего состояния больше определённого порога (в соответствии с логикой Устава), инициируется процедура голосования.
- Порог может зависеть от веса оценки (например, по числу голосов, охвату, доверительной ширине распределения).
- Решение, принятое съездом, фиксирует новое агрегированное мнение партии, заменяя соответствующие ячейки в $M$.

Таким образом, $f$ является связующим звеном между выборочной динамикой DAO и институциональным механизмом принятия решений, закреплённым в Уставе партии.