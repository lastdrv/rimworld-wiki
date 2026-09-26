# Анализ мода: MedPod

| Параметр | Значение |
|---|---|
| **Полное название** | MedPod |
| **Идентификатор (`packageId`)** | `sumghai.Medpod` |
| **Автор** | sumghai |
| **Версия мода** | `1.6.7` |
| **Поддерживаемые версии RimWorld** | 1.4, 1.5, **1.6** *(исследуемая)* |
| **Официальный репозиторий** | [GitHub MedPod](https://github.com/sumghai/MedPod/) |

---

## 1. Цель мода

**MedPod** добавляет в RimWorld высокотехнологичные медико-регенеративные капсулы (Medical Pods / VetPods), вдохновленные научно-фантастическими произведениями (фильм *«Прометей»*). Капсулы способны автоматически диагностировать и полностью излечивать практически любые травмы, болезни, инфекции, органные повреждения, зависимости, отравления и генетические дефекты у людей и животных.

### Основные решаемые задачи:
- Автоматизация лечения колонистов, пленников и животных без расхода медикаментов.
- Избавление от нелечимых ванильными средствами состояний (старческие болезни, глубокие шрамы, зависимости).
- Сокращение рутинной работы врачей на поздних этапах игры за счет авто-диагностики и восстановления тканей сканирующим подвижным мостиком (Gantry).

---

## 2. Зависимости

- **Обязательные моды**:
  - `brrainz.harmony` (**Harmony**) — используется для патчинга логики кроватей, рендеринга пешек и искусственного интеллекта работы.
- **Официальные DLC (Опционально)**:
  - **Royalty**: Добавляет элитную королевскую капсулу `MedPod_Lux` (подходит для спален дворян).
  - **Biotech**: Фильтрация ксенотипов (`DisallowedXenotypes`) и совместимость с генетическими состояниями.

---

## 3. Взаимодействие и переопределение ванильного функционала и DLC

### Ванильная игра и DLC Royalty / Biotech / Ideology / Anomaly
- **Подкласс `Building_Bed`**: Медкапсула `Building_BedMedPod` наследуется от ванильного класса кроватей `Building_Bed`. Пешка, находящаяся в капсуле, одновременно считается «спящей» в кровати, но ее потребление пищи и движение фиксируются на время процедуры.
- **Royalty DLC Integration**:
  - Содержит файл `Royalty/Defs/ThingDefs_Buildings/Buildings_Furniture_MedPod_Lux.xml`.
  - Капсула `MedPod_Lux` удовлетворяет требованиям королевских спален (Royal Title Requirements) и дает положительные мысли от роскоши.
- **Biotech DLC Integration**:
  - Используется условная атрибутика `[MayRequireBiotech]`.
  - Поддерживается проверка совместимости по полю `DisallowedXenotypes`.
- **Переопределение AI и расписания пешек**:
  - Введены новые `WorkGiver` и `JobDriver` для самостоятельного направления пациентов в капсулу (`JobGiver_PatientGoToMedPod`), а также переноса больных/раненых врачами и надзирателями (`WorkGiver_DoctorCarryFromBedToMedPod`, `WorkGiver_WardenCarryFromBedToMedPod`).
- **Переопределение рендеринга (`Harmony_PawnRenderer`)**:
  - Патчит отрисовку тела пешки в момент нахождения внутри капсулы, чтобы тело корректно закрывалось крышкой капсулы и мостиком анимации.

---

## 4. Взаимодействие со сторонними библиотеками и другими модами

Мод содержит развитую систему условной совместимости с другими популярными модами через `ModCompatibility.cs` и XML-патчи в `Common/Patches/`:

1. **Harmony (`brrainz.harmony`)**:
   - Применяет префиксы и постфиксы к методам `Building_Bed`, `PawnRenderer`, `RestUtility`, `Pawn_DraftController`, `FloatMenuOptionProvider`.
2. **Dubs Bad Hygiene (`DBH`)**:
   - Патчит `WorkGiver_washPatient`, чтобы гигиенические потребности пешки не прерывали процедуру лечения внутри MedPod.
3. **Alpha Genes**:
   - Перехватывает метод `MakeDowned` / `BreakSomeBones` в патче `AlphaGenesCompatibility`, отменяя перелом костей во время лечения в капсуле.
4. **Combat Extended (`CE`)**:
   - XML-патчи настроек кровотечений и специфических травм CE.
5. **Android Tiers / Mechanical Humanlikes**:
   - Настройка правил лечения механических пешек и андроидов (ограничение неорганических рас через `DisallowedRaces`).
6. **Другие поддерживаемые моды (XML Patches)**:
   - *Altered Carbon*, *Hospitality*, *Save Our Ship 2*, *Replimat*, *EPOE Forked*, *DiseasesPlus*, *Smokeleaf Industry Reborn* и др.

---

## 5. Поддержка русского языка

В мод **MedPod** по умолчанию **встроена полноценная официальная русская локализация**:
- **Локализация XML Defs (`DefInjected/`)**:
  - `ThingDef/Buildings_Furniture_MedPod.xml` — полностью переведены названия, описания и характеристики всех медкапсул и ветеринарных капсул.
  - `ThingDef/Buildings_Furniture_DLC_MedPod.xml` — переведена элитная королевская капсула `MedPod_Lux` для DLC Royalty.
  - `ThingDef/Items_Exotic.xml` — переведен изолинейный процессор (*Isolinear Processor*).
  - `JobDefs/Jobs_MedPod.xml`, `WorkGiverDefs/WorkGivers_MedPod.xml`, `ResearchProjectDef/ResearchProjects.xml` — переведены работы, задачи и исследовательские проекты.
- **Интерфейсные строки и сообщения C# (`Keyed/MedPod.xml`)**:
  - Содержит более 100 переведенных ключей UI, сообщений инспектора, предупреждений, контекстного меню и уведомления об удалении черт характера.
- **Оценка качества перевода**:
  - В целом перевод полный и качественный, однако присутствуют отдельные легкие грамматические недочеты в автоматических форматируемых строках (например: *"Спасти {0} до Медкапсула"*, *"Медкапсула не может восстановить {0}s!"*).

---

## 6. Дополнительная важная информация

- **Энергопотребление и фазы работы**:
  - Капсула имеет 7 состояний (`MedPodStatus`): `Idle`, `DiagnosisStarted`, `DiagnosisFinished`, `HealingStarted`, `HealingFinished`, `PatientDischarged`, `Error`.
  - В режиме ожидания (`Idle`) потребляет базовое электричество (~100 Вт).
  - В режиме диагностики (`Diagnosis`) энергопотребление возрастает (например, до 500 Вт).
  - В режиме активного лечения (`Healing`) энергопотребление достигает пика (до 2000–4000 Вт в зависимости от модификации).
- **Ветеринарные капсулы (VetPods)**:
  - Существуют версии `VetPod`, `VetPod_Large`, `VetPod_Mega` для животных разного размера (`BodySize`).
- **Строительство и компоненты**:
  - Для постройки требуется редкая деталь **Isolinear Processor** (`Items_Exotic.xml`), выпадающая с торговцев или при разборе древних ассетов.

---

## 7. Примечания и технические данные для разработки


Данный раздел содержит архитектурные сведения о C#-классах, структурах данных и методах мода **MedPod** (версия **1.6**), необходимые для проведения повторных или углубленных исследований без повторной декомпиляции.

### Иерархия ключевых классов C# (`namespace MedPod`)

```text
Verse.Building -> RimWorld.Building_Bed -> MedPod.Building_BedMedPod
Verse.ThingComp -> MedPod.CompAnimatedGantry
Verse.ThingComp -> MedPod.CompMedPodSettings
Verse.ThingComp -> MedPod.CompTreatmentRestrictions
Verse.Mod -> MedPod.MedPodMod
FloatMenuOptionProvider -> MedPod.FloatMenuOptionProvider_RescuePawnToMedPod
```

### Ключевые классы и их контракты

#### 1. `MedPod.Building_BedMedPod` (Основное здание капсулы)
- **Поля состояния**:
  - `public MedPodStatus status`: текущая фаза работы (`Idle`, `DiagnosisStarted`, `HealingStarted` и др.).
  - `public int DiagnosingTicks`, `public int HealingTicks`: счетчики тиков фаз.
  - `public float DiagnosingPowerConsumption`, `public float HealingPowerConsumption`: уровни энергопотребления.
  - `public List<HediffDef> AlwaysTreatableHediffs`, `NeverTreatableHediffs`, `UsageBlockingHediffs`: фильтры травм.
  - `public List<string> DisallowedRaces`, `List<XenotypeDef> DisallowedXenotypes`: ограничения рас и ксенотипов.
- **Основные методы**:
  - `public override void Tick()`: главный цикл состояния, управляющий логикой диагностики и лечения.
  - `public bool CanTreatPawn(Pawn pawn)`: проверка возможности размещения и излечения указанной пешки.
  - `public void StartDiagnosis(Pawn patient)`: запуск процесса сканирования.
  - `public void HealPatient(Pawn patient)`: вызов процедур исцеления выявленных `Hediff`.

#### 2. `MedPod.CompAnimatedGantry` (Анимация сканирующего мостика)
- Управляет движением дуги/мостика капсулы (`gantryPositionPercentInt`, `gantryDirectionForwards`).
- Отрисовывает смещение текстуры сканера в зависимости от прогресса лечения.

#### 3. `MedPod.MedPodHealthAIUtility` (Вспомогательный AI-анализатор)
- `public static bool ShouldGoToMedPod(Pawn pawn)`: вычисляет, требуется ли пешке лечение в MedPod (наличие травм/болезней).
- `public static List<Hediff> GetTreatableHediffs(Pawn pawn, Building_BedMedPod medpod)`: возвращает список излечимых состояний пешки.

#### 4. `MedPod.MedPodMod` (Главная точка входа Harmony-патчей)
- Инициализирует `Harmony("com.MedPod.patches")`.
- Динамически подключает патчи для сторонних модов (`AlphaGenesCompatibility`, `DbhCompatibility`).

---

### Таблица основных XML Defs мода

| DefName | Тип Def | Описание |
|---|---|---|
| `MedPod_Standard` | `ThingDef` | Стандартная медкапсула для людей |
| `MedPod_Lux` | `ThingDef` | Элитная медкапсула (DLC Royalty) |
| `VetPod` | `ThingDef` | Малая ветеринарная капсула для мелких животных |
| `VetPod_Large` | `ThingDef` | Средняя ветеринарная капсула |
| `VetPod_Mega` | `ThingDef` | Крупная ветеринарная капсула (слоны, муффало) |
| `IsolinearProcessor` | `ThingDef` | Изготовитель/компонент для постройки капсул |
| `Job_PatientGoToMedPod` | `JobDef` | Задача пешки идти на лечение в MedPod |
| `WorkGiver_DoctorRescueToMedPod` | `WorkGiverDef` | Работа врача по переносу раненого в капсулу |
