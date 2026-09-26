# Анализ мода: MechlordChip

| Параметр | Значение |
|---|---|
| **Полное название** | MechlordChip |
| **Идентификатор (`packageId`)** | `xell.mechlordchip` |
| **Идентификатор Workshop (ID)** | `3546471813` |
| **Автор** | Xell |
| **Формат поставки** | Скомпилированная бинарная сборка (`Assemblies/MechlordChip.dll`) |
| **Поддерживаемые версии RimWorld** | **1.6** *(исследуемая)* |

---

## 1. Цель мода

**MechlordChip** — это эндгейм-мод, добавляющий ультра-мощный RPG-нейроимплант **«Чип Мехлорда»** (*Mechlord Chip*), превращающий пешку в кибернетического полубога / Мехлорда с собственной системой прокачки, талантов, нанитовыми способностями и механиками бессмертия.

### Основные возможности мода:
- **RPG-система прокачки чипа**: Получение опыта (XP) за уничтожение врагов, выполнение работы, проведение исследований и получение/нанесение урона.
- **Генерация случайных статов и талантов (RNG Stat/Talents)**: Выбор усовершенствований при повышении уровня (увеличение прочности частей тела, скорости работы, урона, критов и защиты).
- **Божественные нанитовые способности**:
  - `GenesisProtocol` / `TerraformPulse` — терраформирование и изменение рельефа карты.
  - `OmegaSingularity` — создание гравитационной сингулярности, вызывающей гигантские взрывы и разрушение структуры материи.
  - `NaniteStorm` / `NaniteBlightCloud` — вызов нанитового шторма для массового уничтожения противников.
  - `GlobalOverride` / `MassOverride` — мгновенное переподчинение группы или всех вражеских механоидов на карте.
  - `TimeDilation` — задержка и ускорение времени вокруг пешки.
- **Бессмертие и автоматическое воскрешение (`EternalNexus` / `MechlordReviveManager`)**:
  - Автоматическое предотвращение смерти пешки и восстановление тела из нанитов после критического урона.

---

## 2. Зависимости

- **Официальные DLC (Обязательно / Потребность)**:
  - **Biotech**: Мод глубоко завязан на механики Механитора (Mechanitor), лимиты пропускной способности (`Bandwidth`), диапазон управления (`MechanitorCommandRange`) и ремонт механоидов.
  - **Royalty**: Используется графика престижных глаз (`PrestigeEye`), орбитальные реле (`OrbitalStrikeRelay`) и механики Пси-фокуса.
- **Сторонние библиотеки**:
  - `brrainz.harmony` (**Harmony**) — необходим для работы низкоуровневых перехватов C#-кода.

---

## 3. Взаимодействие и переопределение ванильного функционала и DLC

### 1. Переопределение жизнедеятельности и здоровья пешки
- **Бессмертие и защита от смерти (`Patch_Pawn_HealthTracker_ShouldBeDead` / `Patch_Pawn_Kill_Prevent`)**:
  - Перехватывает смерть пешки с чипом Мехлорда. При критическом уроне пешка не погибает, а переходит в режим восстановления нанитами (`MechlordReviveManager`).
- **Идеальный метаболизм (`Patch_FoodUtility_PerfectMetabolism` / `Patch_Need_Food_PerfectMetabolism`)**:
  - Патчит потребность в еде — наниты поддерживают идеальный уровень сытости без необходимости принимать пищу.
- **Синтетическая выносливость (`Patch_Need_Rest_SyntheticEndurance`)**:
  - Отключает необходимость во сне и отдыхе.
- **Отмена деградации навыков (`Patch_SkillRecord_Learn_NoSkillLoss`)**:
  - Полностью блокирует падение и сброс опыта навыков с течением времени.
- **Увеличение прочности частей тела (`Patch_BodyPartDef_GetMaxHealth`)**:
  - Динамически умножает максимальное здоровье органов и конечностей пешки.

### 2. Интеграция с механиками Механитора (Biotech DLC)
- **Установка чипа (`Patch_CompUseEffect_InstallMechlink`)**:
  - Интегрируется с процессом установки Мех-связи (Mechlink) и нейроимплантов.
- **Управление механоидами (`Patch_MechanitorCommandRange` / `StatPart_Mechlord_Bandwidth`)**:
  - Расширяет радиус контроля механоидов и увеличивает пропускную способность Механитора.

---

## 4. Взаимодействие со сторонними библиотеками и другими модами

1. **Harmony (`brrainz.harmony`)**:
   - Содержит более 30 патч-классов (`Patch_Pawn_Kill`, `Patch_SkillRecord_Learn_NoSkillLoss`, `Patch_MassUtility_Capacity`, `Patch_VerbProperties_AdjustedRange` и др.).
2. **RPG Stats integration**:
   - Патчит интерфейс и вычисление характеристик для совместимости с RPG-модами (`Patch_Need_Rest_RPGStats`, `Patch_StatWorker_RPGStats`).

---

## 5. Поддержка и создание русификации

- 🔍 **Наличие в Steam Workshop**:
  - Мод обнаружен в каталоге Workshop по пути `C:\Program Files (x86)\Steam\steamapps\workshop\content\294100\3546471813`.
- ⚙️ **Требования к русификации**:
  - **Игровой контент (Defs, письма)**: В оригинальной бинарной сборке встроенный перевод отсутствует (присутствуют только `English` и `German`). XML-файлы (`DefInjected` и `Keyed`) позволяют на 100% русифицировать предмет Чипа, нейроимплант Hediff и игровые письма (`Mechlord_Letter_Label` / `Mechlord_Letter_Text`).
  - **Меню настроек мода (Mod Settings Window)**: Все строки меню настроек (например, `"RPG Mode Settings"`, `"rpg talent points per level"`, `"Ranged cooldown talent multiplier"` и др.) **захардкожены в C#-коде** (`MechlordMod.cs` в `MechlordChip.dll`) в виде текстовых литералов без использования методов `.Translate()`.
  - **Вывод по локализации**: Стандартными XML-файлами перевести Меню настроек мода **невозможно**. Для русификации меню настроек требуется либо пересборка C#-ассамблеи `MechlordChip.dll` (с добавлением вызовов `.Translate()`), либо создание C# Harmony-патча для метода `MechlordMod.DoSettingsWindowContents`.
- 📦 **Статус файлов русификации**:
  - Каталог `russian/3546471813/` был удален по решению пользователя, так как частичная XML-русификация без меню настроек не удовлетворяет требованиям. Полный перевод возможен только при доработке C#-кода мода.


---

## 6. Дополнительная важная информация

- **Глобальные менеджеры компонентов (`WorldComponent` / `GameComponent`)**:
  - `Mechlord_GameComp`: Отслеживает глобальный статус Мехлорда и активные эффекты.
  - `WorldComponent_NaniteSingularity`: Управляет распространением нанитовой сингулярности по глобальной карте мира.
  - `WorldComponent_MechlordTestQuest`: Управляет цепочкой квестов и испытаний для получения чипа.
- **Кастомные звуки и эффекты**:
  - Содержит собственные аудио-ресурсы в формате OGG/WAV для сингулярности (`omega_collapse.wav`, `omega_pulse.wav`) и звук пробития щита (`MechlordShield_Break.ogg`).

---

## 7. Примечания и технические данные для разработки

Данный раздел содержит архитектурные сведения о декомпилированных C#-классах, структурах данных и Harmony-патчах мода **MechlordChip** (версия **1.6**).

### Иерархия ключевых классов C# (`namespace MechlordChip`)

```text
Verse.HediffWithComps -> MechlordChip.Hediff_MechlordChip
Verse.ThingComp -> MechlordChip.Comp_MechlordChip
Verse.GameComponent -> MechlordChip.Mechlord_GameComp
RimWorld.Planet.WorldComponent -> MechlordChip.WorldComponent_NaniteSingularity
RimWorld.Planet.WorldComponent -> MechlordChip.WorldComponent_MechlordTestQuest
RimWorld.Planet.WorldComponent -> MechlordChip.WorldComponent_LevelingModeChoice
Verse.Thing -> MechlordChip.Thing_OmegaSingularity
Verse.Thing -> MechlordChip.Thing_GeomanticPulse
```

### Ключевые Harmony-патчи C#

| Целевой класс | Целевой метод | Тип патча | Назначение патча |
|---|---|---|---|
| `Pawn_HealthTracker` | `ShouldBeDead` | `Postfix` | Предотвращает смерть пешки с чипом Мехлорда. |
| `Pawn` | `Kill` | `Prefix` | Перенаправляет смерть в `MechlordReviveManager`. |
| `SkillRecord` | `Learn` | `Prefix` | Блокирует падение опыта (No Skill Loss). |
| `FoodUtility` | `WillEat` | `Postfix` | Внедряет идеальный метаболизм (Perfect Metabolism). |
| `Need_Rest` | `RestIntervalTick` | `Prefix` | Отключает потребность во сне (Synthetic Endurance). |
| `BodyPartDef` | `GetMaxHealth` | `Postfix` | Динамически увеличивает максимальное здоровье органов пешки. |
| `MechanitorTracker` | `DrawCommandRadius` | `Postfix` | Расширяет радиус контроля Механитора. |
