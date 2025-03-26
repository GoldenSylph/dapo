# Инициатива создания децентрализованной автономной внутрипартийной организации (ДАВО) "Заря" для партии "Рассвет"

`Версия: Челябинск`

## Предисловие

Уважаемый Даниил Дмитриевич,

Вашему вниманию представляется документ, который описывает теоретическое распределённое программное обеспечение спроектированное для реализации и автоматизации управления мнением партии. Целью этого документа является донесение до Совета МО Челябинской области партии "Рассвет" ценности инициативы по разработке демо этой системы, а также в дальнейшем разработки полноценной и безопасной её версии.

## Глоссарий

### Основные понятия

- **ДАВО "Заря" или "Заря"** - распределённое ПО, для наблюдения, формирования и анализа партии "Рассвет".

- **Мнение партии ($𝓜$)** — формализованное представление совокупной позиции партии как кортежа двух матриц:
  - $𝓧$ — непрерывные случайные величины (количественные показатели),
  - $𝓨$ — дискретные случайные величины (категориальные оценки).

- **Матрица $𝓢_X$, $𝓢_Y$** — выборочные реализации значений в ячейках. Хранят значения, полученные через голосование DAO.

- **Агрегированное мнение ($Ĥ_𝓜$)** — результат применения агрегирующей функции $f(𝓢_X, 𝓢_Y)$, используемый для принятия решений и генерации текстов.

---

### Структура строк в матрицах внутри $𝓜$ и их кодов

- Строка каждой ячейки оформлена как: `<Тематика> — <Код органа>`
  - `СЗД` — общепартийный уровень (Съезд),
  - `СОВ` — уровень Центрального совета,
  - `ПРЛ` — уровень председателя,
  - `НН` — субъект РФ (регион под номером НН),
  - `НН.Х.ОБС` — Общее собрание МО под номером Х в регионе НН,
  - `НН.Х.СОВ` — Совет МО под номером Х в регионе НН,
  - `НН.СОВ` — Совет регионального отделения региона под номером НН,
  - `НН.КОН` — Конференция региона под номером НН.

---

### DAO (Децентрализованная автономная организация) и голосование

- **Смарт-контракт** - Кусок кода, который хранится, исполняется и верифицируется в блокчейне.

- **Смарт-контракт VotingRouter** — определяет, какой орган должен голосовать по изменению в ячейке.

- **Смарт-контракт VotingModule** — универсальный контракт голосования, параметризуемый кворумом, типом голосования и правилом принятия решений.

- **Смарт-контракт MatrixStorage** — хранилище выборок $S_X$, $S_Y$.

- **Смарт-контракт MatrixSchemaRegistry** — хранит семантику строк и столбцов матрицы, в том числе код органа.

- **Смарт-контракт OrganRegistry** — регистрирует все органы DAO, их участников и типы. Делегирует голосование.

- **Смарт-контракт PartyMembershipRegistry** — глобальный реестр всех участников партии, используемый для валидации права голосовать.

---

### Процедуры

- **Жизненный цикл ячейки** — путь от предложения нового значения → голосования → добавления значения в выборку.

- **Предложение** — содержит одно значение, тип ячейки (из $X$ или из $Y$), координаты и автоматически фиксируемую метку времени.

- **Голосование** — проходит через DAO-механику, орган определяется по строке ячейки и регламентируется уставом.

- **Агрегация** — после пополнения выборки происходит перерасчёт агрегированных значений (например, медиана, мода, доверительный интервал).

---

### Человекочитаемое мнение

- **Шаблон (Template)** — фрагмент текстового документа, в который подставляется значение из `Ĥ_𝓜`.

- **Человекочитаемая репрезентация** — автоматически сгенерированный фрагмент текста, отражающий текущую позицию DAO по конкретной ячейке.

- **Однозначность интерпретации** — гарантируется через `MatrixSchemaRegistry`, структурированный шаблон и детерминированную агрегацию.

---

### Прогнозирование и анализ

- **Нулевая гипотеза (`H₀`)** — проверка, изменится ли агрегированное мнение после добавления нового значения.

- **Анализ поведения `𝓜(t)`** — отслеживание динамики мнения партии во времени.

- **Фракционный анализ** — выявление групп DAO-участников, формирующих устойчивые паттерны голосования или оценок.

- **Показатели стабильности** — измерения колебаний, консенсуса и поляризации.

- **Сценарный анализ и стресс-тесты** — прогноз последствий, если несколько значений будут одновременно предложены или приняты.

## Мотивация

Современные партии располагают тоннами текста помимо Устава, Программы и Декларации ценностей в том или ином её виде. Из-за чего тяжело миновать разного рода боли, которые могут беспокоить любого члена партии. Например:

1) Чтобы партийным должностным лицам с широким кругом полномочий принимать решения качественно и понимая мнение партии верно, им необходимы консультации с коллегами или независимыми специалистами. Размеры эти консультаций по участникам и длительность их, в общем случае, непредсказуемы и могут быть избыточными или наоборот слишком бессодержательными. Но даже при всём при этом, остаётся шанс совершить ошибку.
2) Каждый раз, чтобы понять любой аспект свежего мнения партии (напр. Чтобы инициировать голосование или принять решение на месте), необходимо созывать соответсвующие управляющие органы партии. Это создаёт дополнительную временную и бюрократическую нагрузку.
3) Каждый документ порождаемый коллективной волей партии, основанной на её мнении, представляет из себя поправку. Каждая новая поправка - это головная боль для любого члена партии, который не хочет бегать по всей истории вопроса, а лишь хочет понять текущее состоянее дел.

Каждая из перечисленных болей, может решаться "Зарёй" таким образом:

1) "Заря" агрегирует всю информацию по любому делу или предмету обсуждения учитывая одновременно как фактический так и вероятностный его аспект. Поэтому она может давать рекомендации, которые могут заменить некоторые из необходимых для принятия решения телодвижений. 
   1) Например, вышел новый закон, нарушающий естесственные права граждан. Вы, директор МО "Рассвет". Вместо того, чтобы задавать вопрос, как члены партии зарегистрированные в МО видят этот закон, Вы идёте в "Зарю", запрашиваете статистически обоснованную оценку мнения с наперёд заданной точностью в 80%. "Заря" делает агрегацию мнения, и выдаёт какую-то конкретную качественную или количественную характеристику того, как себя могут почувствовать члены партии узнав об этом законе с вероятностью 80%. Следовательно, Вы реформируете свой вопрос Совету или Общему собранию МО из более общего, в более конкретный, экономя время на уточняющих вопросах и поправках к формулировкам вопроса.
2) Рекомендация "Зари" даёт возможность качественно фильтровать вопросы органам партии, для того чтобы понимать эти самые аспекты мнения партии. А иногда, рекомендация может совсем снимать необходимость созыва этих самых органов, для задавания вопросов, не порождая вторую тонну документации.
3) После каждой рекомендации "Заря" имеет возможность учитывать реакцию на эту рекомендацию, каждый раз дообучаясь понимать мнение партии. Поэтому любой член партии, может дать "запрос" в "Зарю" и получить как самое свежее мнение партии, так и историю его изменения. 

### А в итоге, зачем это нужно партии «Рассвет»?

У партии «Рассвет» — широкий спектр инициатив: от налоговой политики и поддержки образования до рекультивации свалок, цифровой трансформации и вопросов самоорганизации на местах. Каждая из этих тем:
- по-разному воспринимается в регионах и МО;
- отличается степенью реализуемости;
- вызывает разные оценки — и эмоциональные, и количественные.

В классической партийной системе всё это утопает в бесконечных поправках, субъективных спорах и протоколах, понятных только юристам. Партия «Рассвет» идёт дальше.

Система ДАВО "Заря" — это:

#### Математически формализованное мнение партии

Мнение представлено в виде матрицы вероятностных оценок:
- количественных (например, «оптимальный налог — 13%»),
- качественных («инициатива приемлема», «мнение — сомнительно»),
- агрегированных на основе голосований.

---

#### Прогнозируемое управление

Каждое новое голосование не просто добавляет значение в матрицу — оно может изменить всё мнение.  
Перед тем как голосовать, можно узнать:
- приведёт ли новое значение к изменению официальной позиции;
- насколько партия склонна к принятию или отклонению;
- сохранится ли нулевая гипотеза, т.е. "партия по-прежнему считает X".

Это делает принятие решений осмысленным, а не импульсивным.

---

#### Многоуровневая система голосования

Каждая ячейка матрицы привязана к конкретному уставному органу:
- МО, если вопрос локальный;
- Конференции или Совету региона — если региональный;
- Съезду — если речь идёт о позиции всей партии.

DAO обеспечивает жёсткое соответствие Уставу и фиксирует:
- кто голосует;
- когда;
- с каким результатом.

---

#### Аналитика и контроль эволюции мнения

Мнение партии — живое. Система позволяет:
- отслеживать его изменения во времени,
- выявлять устойчивые и нестабильные темы,
- видеть зоны согласия и поляризации,
- прогнозировать, где и когда могут возникнуть внутренние фракции.

---

#### Человекочитаемые документы без поправок

Тексты формируются автоматически из текущего мнения:
- без ручной редакторской суеты;
- без конфликта версий;
- всегда свежие и однозначные.

Если мнение изменилось — изменилась и строчка в документе. А значит — нет рассогласования между тем, что партия говорит, и тем, что она думает.

---

#### Подводя итог

Система "Заря" позволяет:
- продвигать не просто инициативы, а разумные инициативы;
- избегать популизма и утопий;
- делать мнение партии верифицируемым, прогнозируемым и справедливым;
- а самое главное — создавать партию, которая действительно думает.

## Формальная вероятностная модель мнения партии

Как оцифровать неоцифруемое? Статистика!

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
\mathcal{M_t} \rightarrow \mathcal{M_{t+1}} = \mathcal{M_t} + \Delta\mathcal{M}
$$

Это конечно очень широкое определение, однако в реальной жизни, мы не можем знать априори параметры случайных величин. Поэтому необходимо перейти от формальной модели к эмпирической.

### Обоснование перехода к эмпирической модели

Формальная модель мнения партии, представленная как кортеж параметризованных распределений, теоретически точна и математически строгая. Однако при реализации жизненного цикла $M$ в инфраструктуре DAO в блокчейне, и например, на языке Solidity возникает ряд ограничений:

- Невозможно хранить непрерывные распределения непосредственно в on-chain (в блокчейне) памяти без значительных затрат.
- Отсутствие нативной поддержки вещественных чисел в Solidity ограничивает работу с параметрами распределений.
- Участие членов DAO, голосующих за значения и оценки, формирует выборки, а не готовые распределения.

В этой связи более реалистичным и практически пригодным подходом становится использование **накапливаемых выборок**. Таким образом, ячейки матриц мнения хранят не априорно заданные распределения, а выборки значений, которые можно агрегировать и аппроксимировать off-chain (на сервере) или по запросу on-chain.

Это позволяет связать формальную модель с технической реализацией в рамках DAO-процессов, не теряя смысловой строгости и соответствия исходной структуре.

## Примеры матриц эмпирической модели

Получается, что матрица $M$ теперь выглядит так: 

$$
M = (S_X, S_Y)
$$

где,
- $S_X$ - непрерывная половина мнения партии (или матрица выборок непрерывных случайных величин),
- $S_Y$ - дискретная или категориальная половина мнения партии (или матрица выборок категориальных случайных величин).

### Матрица выборок непрерывных случайных величин ($S_X$)

| Показатель | Примеры значений выборки |
|-----------|--------------------------|
| Уровень безработицы в регионах (%) | `{5.4, 5.6, 6.0, 4.9, 5.2}` |
| Доля бюджета, направленная на образование (%) | `{8.1, 8.3, 7.9, 8.0, 8.2}` |
| Оценка экологического ущерба (баллы от 0 до 10) | `{6.7, 6.5, 6.9, 7.1, 6.8}` |

#### Интерпретация ($S_X$):

**Уровень безработицы в регионах:**

$$
S_X[0][0] = {5.4, 5.6, 6.0, 4.9, 5.2}
$$

**Доля бюджета на образование:**  

$$
S_X[1][1] = {8.1, 8.3, 7.9, 8.0, 8.2}
$$

**Экологический ущерб (0–10):**  

$$
S_X[2][2] = {6.7, 6.5, 6.9, 7.1, 6.8}
$$

### Матрица выборок категориальных случайных величин ($S_Y$)

| Вопрос оценки | Возможные категории | Примеры выборки |
|---------------|---------------------|------------------|
| Оценка инициативы об увеличении учительских зарплат | {«поддерживается», «сомнительно», «отвергается»} | `{«поддерживается», «поддерживается», «сомнительно»}` |
| Отношение к налоговой реформе | {«желательно», «приемлемо», «неприемлемо»} | `{«приемлемо», «желательно», «приемлемо», «неприемлемо»}` |
| Мнение о модели базового дохода | {«нужна», «сомнительна», «опасна»} | `{«нужна», «сомнительна», «нужна», «опасна»}` |

#### Интерпретация ($S_Y$):

**Инициатива о повышении зарплат учителям:**

$$
S_Y[0][0] = {"поддерживается", "поддерживается", "сомнительно"}
$$

**Отношение к налоговой реформе:**  

$$
S_Y[1][2] = {"приемлемо", "желательно", "приемлемо", "неприемлемо"}
$$

**Модель базового дохода:**  

$$
S_Y[3][1] = {"нужна", "сомнительна", "нужна", "опасна"}
$$

### Сводная формула и определение агрегирующей функции $f$

Хорошо, имеем эти матрицы. А как из них сделать какие-никакие выводы?

$$
M = (S_X, S_Y) \\
\hat{M} = f(S_X, S_Y)
$$

Где $f$ — это функция агрегирования, которая строит оценку текущего мнения партии на основании накопленных выборок. Эта функция применяется:

1. **В рамках внутренних процессов партии**: когда необходимо определить доминирующее мнение по инициативе, теме или показателю.
2. **При принятии решений согласно Уставу**: агрегированное значение или распределение может служить основанием для проведения голосования съездом, конференцией или советом.

#### Формальное определение функции $f$

Функция $f$ определяется как преобразование:

$f : (S_X, S_Y) → (\hat{X}, \hat{Y})$


Где:
- $\hat{X}_{i,j} = \text{Estimate}(S_X[i][j])$ — оценка распределения (например, метод моментов, медиана, дисперсия) или агрегированная величина (например, среднее значение, доверительный интервал).
- $\hat{Y}_{k,l} = \text{Histogram}(S_Y[k][l])$ — оценка вероятностей категорий по частотам выборки.

#### Пример использования в процедурах DAO:
- Если изменение мнения $M \rightarrow \hat{M}'$ приводит к отклонению от предыдущего состояния больше определённого порога (в соответствии с логикой Устава), инициируется процедура голосования.
- Порог может зависеть от веса оценки (например, по числу голосов, охвату, доверительной ширине распределения).
- Решение, принятое съездом, фиксирует новое агрегированное мнение партии, заменяя соответствующие ячейки в $M$.

Таким образом, $f$ является связующим звеном между выборочной динамикой DAO и институциональным механизмом принятия решений, закреплённым в Уставе партии.

## Децентрализованная автономная организация (DAO)

В соответствии с Уставом партии «Рассвет», система DAO должна строго отражать организационную структуру и порядок принятия решений. Все изменения матрицы мнения $\mathcal{M}$ допускаются **только через голосование**, и только тем органом, который уполномочен на основании уставного кодирования строки ячейки.

### Основные смарт-контракты

#### 1. `MatrixStorage.sol`
Хранит выборки значений ячеек: $S_X[i][j]$, $S_Y[k][l]$  
Обновление только по результатам голосования.

---

#### 2. `MatrixSchemaRegistry.sol`
Содержит описание строк и столбцов матрицы.  
Определяет: семантику строки, код органа управления, тип данных (X или Y).

---

#### 3. `VotingRouter.sol`
Определяет уставной орган для голосования по ячейке  
Делегирует выполнение соответствующему `VotingModule`.

---

#### 4. `VotingModule.sol`
Шаблон голосования с параметрами:
- тип (открытое / тайное);
- кворум;
- правило (50% или 2/3);
Используется для всех уставных органов.

---

#### 5. `OrganRegistry.sol`
Хранит и маршрутизирует органы по кодам:
- `СЗД` — общепартийный уровень (Съезд),
- `СОВ` — уровень Центрального совета,
- `ПРЛ` — уровень председателя,
- `НН` — субъект РФ (регион под номером НН),
- `НН.Х.ОБС` — Общее собрание МО под номером Х в регионе НН,
- `НН.Х.СОВ` — Совет МО под номером Х в регионе НН,
- `НН.СОВ` — Совет регионального отделения региона под номером НН,
- `НН.КОН` — Конференция региона под номером НН.

---

Орган может быть как председателем, так и собранием, так и советом, так и конференцией.  
Выбор между ними определяется текущей конфигурацией отделения.

#### 6. `PartyMembershipRegistry.sol`
Глобальный реестр всех участников партии.  
Используется для:
- проверки принадлежности к структуре;
- подтверждения права участия в голосовании.

### Взаимодействие между модулями

```text
User
 │
 ▼
VotingRouter
 │
 ▼
OrganRegistry ──┬──→ VotingModule
                │
                └──→ PartyMembershipRegistry
                       │
                       └──→ Валидация полномочий участников голосования
```

---

### Поддержка всех уставных органов (с пояснениями)

| Орган                            | Код          | Представляет                        | Тип DAO-голосования                 |
|----------------------------------|--------------|-------------------------------------|-------------------------------------|
| Съезд партии                     | `СЗД`        | Делегаты от >50% субъектов РФ       | Общепартийный                       |
| Председатель                     | `ПРЛ`        | Себя                                | Без голосования                     |
| Центральный совет                | `СОВ`        | Члены Центрального совета           | Межсъездовый орган                  |
| Региональная конференция         | `НН.КОН`     | Делегаты от >50% МО в регионе       | Высший орган региона                |
| Совет регионального отделения    | `НН.СОВ`     | Совет РО (если избран)              | Постоянно действующий региональный  |
| Общее собрание местного отделения| `НН.Х.ОБС`   | Все члены МО                        | Основной орган на местах            |
| Совет местного отделения         | `НН.Х.СОВ`   | Совет МО (если избран)              | Делегированное управление МО        |

---

### 📌 Инварианты системы

- Изменения $\mathcal{M}$ — только через голосование.
- Голосует только уставной орган, определённый по строке ячейки.
- Все участники голосования верифицируются через `PartyMembershipRegistry`.
- DAO-архитектура модульна, масштабируема и формально обоснована.

## Структура человекочитаемой интерпретации внутреннего состояния `M` (Результатов Сводной функции).

Для того чтобы продемонстрировать то, как происходит генерация человекочитаемой интерпретации, я приведу несколько примером, где демонстрируются полный циклы - от Предложения, до человекочитаемоего динамического документа.

### Принцип: единая глобальная матрица

На всю партию в рамках "Зари" существует одна глобальная матрица мнения:

$$
\mathcal{M} = (\mathbf{X}, \mathbf{Y})
$$

Все количественные и качественные показатели фиксируются в единой структуре. Разграничение регионов и местных отделений реализуется через структурированный идентификатор в строке ячейки.

**Примеры:**

| Строка                       | Интерпретация                            |
|------------------------------|------------------------------------------|
| Экология - 74.СОВ            | Позиция Совета РО по Челябинской области |
| Социальная защита - 77.3.СОВ | Совет МО №3 в Москве                     |
| Экономика - СОВ              | Федеральная позиция партии               |

### Этапы жизненного цикла ячейки

#### Инициация предложения изменения

Любой участник DAO может подать предложение на изменение ячейки $\mathcal{M}[i][j]$, указав:

- координаты (например, `X[15][3]`);
- строку (`Экология - 74.2.СОВ`);
- **одно значение**, предлагаемое к включению в выборку ячейки.

Значение может быть:
- числом (для $\mathbf{X}$);
- категорией (для $\mathbf{Y}$).

К предложению автоматически прикрепляются:
- **метка времени** подачи,
- **адрес инициатора**.

Значение включается в выборку **только после утверждения голосованием**.

---

#### Определение органа голосования

DAO парсит код из строки и определяет, кто уполномочен голосовать:

| Код строки  | Орган голосования                    |
|-------------|--------------------------------------|
| `СЗД`       | Съезд партии                         |
| `74.КОН`    | Конференция Челябинской области      |
| `74.2.СОВ`  | Совет МО №2 Челябинской области      |
| `50.1.СОВ`  | Совет МО №1 Московской области       |

---

#### Проведение голосования

| Орган                    | Кворум                      | Решение             | Форма голосования     |
|--------------------------|-----------------------------|---------------------|-----------------------|
| Съезд партии             | >50% субъектов РФ           | >50% или 2/3        | Открытое / Тайное     |
| Центральный совет        | >50% членов Совета          | >50% участвующих    | По регламенту         |
| Региональная конференция | >50% делегатов от МО региона| >50%                | По уставу / тайное    |
| Региональное собрание    | >50% членов Совета          | >50%                | По уставу             |
| Региональный совет       | >50% членов                 | >50%                | По регламенту         |
| Местное собрание         | >50% членов МО              | >50%                | По уставу             |
| Местный совет            | >50% членов Совета          | >50%                | По регламенту         |

---

#### Агрегация и обновление

Если голосование прошло успешно, предложенное значение:
- добавляется в выборку ячейки $S_X[i][j]$ или $S_Y[k][l]$;
- сохраняется вместе с меткой времени и авторством.

DAO может по расписанию жизненных циклов парти выполнять агрегирование выборок в $\hat{\mathcal{M}}$.

### Человекочитаемая репрезентация результатов агрегации

Модель мнения партии предполагает автоматическую генерацию текстовых документов на основе агрегированных значений матрицы:

$$
\hat{\mathcal{M}} = f(S_X, S_Y)
$$

В отличие от классической системы «документы + поправки», человекочитаемые документы в этой модели:

- не хранятся в виде статичных текстов;
- не изменяются вручную;
- а формируются динамически на основе текущего мнения DAO, зафиксированного в выборочных данных.

### Природа результата

Результатом работы сводной функции $f$ являются **различные описательные статистики**, полученные на основе выборочных распределений, накопленных в каждой ячейке:

- для числовых ячеек ($S_X$): средние значения, медианы, доверительные интервалы, квантильные оценки и т. д.;
- для категориальных ячеек ($S_Y$): частотные распределения, моды, вероятностные оценки по категориям и пр.

Формат представления этих значений зависит от контекста документа, но логика извлечения едина: агрегированное мнение — это производная от эмпирической выборки ячейки одной из матриц $M$.

### Связь с шаблонами

Каждая секция документа (например, в Markdown) может быть связана с координатами конкретной ячейки $\hat{X_{i,j}}$ или $\hat{Y_{k,l}}$ через шаблонизатор (например, Jinja2). При генерации:

- значения подставляются в текстовые фрагменты;
- логика вывода (например, «Партия считает, что…») управляется на основе правил форматирования.

### Условие однозначности

Чтобы документы можно было интерпретировать строго и однозначно, необходимо:

1. Каждая ячейка матрицы должна иметь фиксированное описание в ончейн-реестре (`MatrixSchemaRegistry`);
2. Шаблон должен ссылаться **только** на координаты ячейки;
3. Логика генерации документа должна быть детерминирована и открыта.

### Примеры агрегирования

#### Пример 1: Подстановка числовой оценки из $\hat{X}$

**Семантика ячейки**  
Партия считает, что средний уровень выбросов CO₂ в Челябинской области — важнейший экологический показатель.

- Координаты: `X[12][3]`
- Строка: `Экология — 74`
- Столбец: `Средний уровень выбросов CO₂`

**Агрегированные значения**  
- Среднее: 6.12  
- Доверительный интервал: [5.9, 6.3]

**Шаблон (Jinja2):**
```jinja2
{% set value = X["12"]["3"].mean %}
{% set ci = X["12"]["3"].confidence_interval %}
Партия считает, что средний уровень выбросов CO₂ в Челябинской области составляет {{ value }} т/чел/год 
(доверительный интервал: {{ ci[0] }} – {{ ci[1] }}).
```

**Результат:**  
Партия считает, что средний уровень выбросов CO₂ в Челябинской области составляет 6.12 т/чел/год (доверительный интервал: 5.9 – 6.3).

---

#### Пример 2: Подстановка категориального мнения из $\hat{Y}$

**Семантика ячейки**  
Совет МО №2 Московской области считает, что базовый доход — необходимая социальная мера.

- Координаты: `Y[21][0]`
- Строка: `Социальная политика — 50.2`
- Столбец: `Отношение к базовому доходу`

**Агрегированные значения**  
- "нужна" — 68%, "сомнительно" — 22%, "опасна" — 10%

**Шаблон (Jinja2):**
```jinja2
{% set result = Y["21"]["0"].mode %}
{% set support = Y["21"]["0"].distribution[result] %}
Совет МО №2 Московской области считает, что базовый доход — "{{ result }}" 
(доля поддержки — {{ support * 100 }}%).
```

**Результат:**  
Совет МО №2 Московской области считает, что базовый доход — "нужна" (доля поддержки — 68%).

---

#### Пример 3: Предложение о приёме нового члена партии

**Семантика ячейки**  
Местное собрание МО №3 Нижегородской области оценивает кандидатуру Иванова Ивана Ивановича на вступление в партию.

- Координаты: `Y[33][7]`
- Строка: `Кадровые вопросы — 52.3`
- Столбец: `Решение по приёму члена`

**Агрегированные значения**  
- "принимается" — 82%, "воздержаться" — 10%, "отказать" — 8%

**Шаблон (Jinja2):**
```jinja2
{% set decision = Y["33"]["7"].mode %}
{% set support = Y["33"]["7"].distribution[decision] %}
МО №3 Нижегородской области приняло решение: кандидатура Иванова Ивана Ивановича — "{{ decision }}" 
(за — {{ support * 100 }}% голосов).
```

**Результат:**  
МО №3 Нижегородской области приняло решение: кандидатура Иванова Ивана Ивановича — "принимается" (за — 82% голосов).

---

#### Пример 4: Приближённый к реальности и более подробный пример по изменению категориального мнения партии.

**Тема: Оценка экологических последствий рекультивации свалок в Челябинске**

---

**1. Контекст**

- Ячейка: `Y[15][3]`
- Строка: `Экология — 74.2`
- Столбец: `Оценка последствий рекультивации свалок (экспертная)`
- Тип: категориальная
- Новое значение: `"приемлемо"`
- Орган голосования: собрание МО №2 Челябинской области
- Результат: голосование прошло успешно

---

**2. Обновление выборки**

После голосования значение добавлено в выборку:

```json
S_Y[15][3] = ["приемлемо", ..., "приемлемо"]
```

---

**3. Агрегация (сводная функция `f`)**

Вычисленные результаты:

```json
{
  "приемлемо": 72%,
  "сомнительно": 18%,
  "неприемлемо": 10%
}
```

Мода: `"приемлемо"`

---

**4. Шаблон (Jinja2)**

```jinja2
{% set mode = Y["15"]["3"].mode %}
{% set p = Y["15"]["3"].distribution[mode] %}
Совет МО №2 Челябинской области считает, что экологические последствия рекультивации свалок являются "{{ mode }}" (поддержка — {{ p * 100 }}% DAO-участников).
```

---

**5. Итоговый текст**

> Совет МО №2 Челябинской области считает, что экологические последствия рекультивации свалок являются "приемлемыми" (поддержка — 72% DAO-участников).

---

#### Пример 5: Приближённый к реальности и более подробный пример по изменению непрерывного мнения партии.

**Тема: Увеличение команды OSINT и факт-чекинга в пресс-службе партии**

---

### 1. Контекст

- Ячейка: `X[18][2]`
- Строка: `Информационная политика — 00`
- Столбец: `Оценка целевой численности команды OSINT`
- Тип: непрерывная
- Код органа: `00` (федеральный уровень → Съезд партии)

---

### 2. Процесс голосования

1. **Инициация**  
   Один из делегатов предлагает добавить новое значение в выборку: `12` (человек в команде).

2. **Определение органа**  
   DAO по коду `00` определяет, что голосование должно проходить на **Съезде партии**.

3. **Проведение голосования**
   - Участвуют делегаты от более чем 50% субъектов РФ.
   - Требуется простое большинство голосов для утверждения.
   - Голосование проводится открыто.

4. **Результат**
   Голосование завершено успешно. Значение `12` утверждено.

---

### 3. Обновление выборки

Новое значение добавляется к выборке:

```json
S_X[18][2] = [10, 11, 12, 13, 12]
```

---

### 4. Агрегация (сводная функция `f`)

На основе выборки вычисляется:

- Среднее: `11.6` → округлено вниз: `11`
- Доверительный интервал (95%): `[11.0, 12.2]` → округлено вниз: `[11, 12]`

---

### 5. Шаблон (Jinja2)

```jinja2
{% set mean = X["18"]["2"].mean|int %}
{% set ci = X["18"]["2"].confidence_interval %}
{% set ci_lower = ci[0]|int %}
{% set ci_upper = ci[1]|int %}
Партия считает, что оптимальная численность OSINT-команды пресс-службы составляет {{ mean }} человек 
(доверительный интервал: {{ ci_lower }} – {{ ci_upper }}).
```

---

### 6. Итоговая человекочитаемая интерпретация

> Партия считает, что оптимальная численность OSINT-команды пресс-службы составляет 11 человек (доверительный интервал: 11 – 12).

## Используемые инструменты разработки

Для разработки демо предлагается использовать:

1) Для смарт-контрактной части (блокчейн части): язык программирования Solidity и систему сборки смарт-контрактов Hardhat.
2) Для фронтенд части (интерфейсной части): фреймворк React и набор компонентов для взаимодействия с блокчейном Wagmi.
3) Для бэкенд части (cерверной):
   1) Язык программирования: Python
   2) Для RESTful API cервера: фреймворк Django или Flask
   3) Для просчёта выборочных статистик мнения партии: фреймворк Pandas и scikit-learn.
   4) Для генерации человекочитаемой рекомендации ДАВО "Заря": любой шаблонизатор на языке Python.
4) Для базы данных шаблонов рекомендаций: публичная GitHub организация под рабочим названием ДАВО "Заря" и репозиторий с названием "Шаблоны". 