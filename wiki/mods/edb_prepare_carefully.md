# Анализ мода: EdB Prepare Carefully

## 1. Основные сведения
* **Название мода**: EdB Prepare Carefully
* **PackageId**: `EdB.PrepareCarefully` (оригинал) / `EdB.rus.PrepareCarefully` (русская версия)
* **Steam Workshop ID**: `735106432` (оригинал) / `838528063` (русская сборка в мастерской)
* **Автор**: edbmods (оригинал), Ghost (поддержка и адаптация русской версии)
* **Поддерживаемые версии игры**: `1.1`, `1.2`, `1.3`, `1.4`, `1.5`, `1.6` (Анализировалась версия для `1.6`)

---

## 2. Цель мода
Мод **EdB Prepare Carefully** предназначен для полной предстартовой кастомизации колонистов, их отношений, снаряжения, ресурсов и питомцев перед высадкой на планету:

* **Индивидуальная настройка поселенцев**: Ручной выбор возраста (биологического и хронологического), биографии (детство/взрослая жизнь), черт характера (`TraitDef`), навыков с уровнями страсти (`Passion`), способностей (`AbilityDef`), внешности (прически, бороды, татуировки, цвет кожи, тип тела, лицевые особенности), одежды и состояния здоровья (импланты, хронические болезни, ранения, протезы).
* **Кастомизация стартового имущества и животных**: Добавление или удаление любых ресурсов, предметов экипировки, оружия, медикаментов, материалов и прирученных животных из стартового пула сценария.
* **Настройка родственных связей**: Построение генеалогического древа и социальных связей (родители, дети, супруги, братья/сестры, возлюбленные) как между колонистами, так и с неигровыми пешками из мира (`WorldPawns`).
* **Система балансировочных очков (Points Limit)**: Возможность играть по правилам баланса со строгим лимитом очков стоимости создаваемой колонии либо в свободном режиме «песочницы» без ограничений.
* **Сохранение и загрузка пресетов**: Экспорт и импорт готовых персонажей (`.pawn`) и полных конфигураций сценария (`.preset`) в формате XML.

---

## 3. Зависимости
* **Обязательные моды**: `brrainz.harmony` / `net.pardeike.rimworld.mod.harmony` (библиотека Harmony).
* **Зависимости от DLC**: Полностью автономный базовый мод, при наличии DLC динамически активирует соответствующие вкладки и поля (гены Biotech, идеолигии Ideology, титулы Royalty).

---

## 4. Взаимодействие с ванильными DLC
Мод проверяет флаги активации DLC через класс `Verse.ModsConfig` и динамически адаптирует интерфейс и структуры данных:

* **Biotech (`ModsConfig.BiotechActive`)**:
  * Кастомизация генов пешки (`CustomizedGenes`, `CustomizedGene`): выбор ксенотипов, разделение на зародышевые (`Endogenes`) и внедренные (`Xenogenes`) гены, расчет метаболизма и сложности генов.
  * Корректировка позиционирования кнопки вызова мода на экране `Page_ConfigureStartingPawns`.
* **Ideology (`ModsConfig.IdeologyActive`)**:
  * Назначение персонального цвета (`favoriteColor`), привязка к идеолигии (`ApplyIdeoCustomizationToPawn`), выбор роли и образов по мемам.
* **Royalty (`ModsConfig.RoyaltyActive`)**:
  * Настройка дворянских титулов империи (`CustomizationTitle`), пермитов и пси-способностей (`CustomizedAbility`).
* **Anomaly (`ModsConfig.AnomalyActive`)**:
  * Поддержка специфических хеддифов, мутаций и скрытых модификаторов (`CustomizedMutant`, `CustomizedHediff`).

---

## 5. Взаимодействие со сторонними библиотеками и C#-код
Мод использует собственную скомпилированную сборку **`1.6/Assemblies/EdBPrepareCarefully.dll`** и активно использует библиотеку **Harmony** для интеграции в движок генерации RimWorld.

### Harmony-патчи:
1. **`RimWorld.Page_ConfigureStartingPawns.DoWindowContents(Rect rect)`** (`PrepareCarefullyButtonPatch`):
   * Внедряет в окно выбора поселенцев кнопку **«Prepare Carefully» / «Подготовиться тщательно»** (`EdB.PC.Page.Button.PrepareCarefully`), открывающую полноэкранный интерфейс редактора `Mod.Instance.Start(__instance)`.
2. **`Verse.Game.InitNewGame()`** (`ReplaceScenarioPatch`):
   * Вызывает `Mod.Instance.RestoreScenarioParts()` и `Mod.ClearInstance()` после завершения генерации новой карты для очистки временных состояний редактора.

### Ключевые C#-модули и архитектура:
* **`EdB.PrepareCarefully.PawnCustomizer`**: Основной движок, преобразующий промежуточные DTO-структуры (`CustomizationsPawn`) в ванильные объекты `Verse.Pawn` через `PawnGenerator.GeneratePawn()` с последующим наложением параметров через `ApplyAllCustomizationsToPawn()`.
* **`EdB.PrepareCarefully.PawnGenerationRequestBuilder`**: Обёртка для формирования ванильного запроса `PawnGenerationRequest` с заданными ограничениями.
* **`EdB.PrepareCarefully.CostCalculator`**: Вычисляет суммарную стоимость пешек, снаряжения, животных и имплантов на основе рыночной стоимости предметов и весов навыков/черт.
* **`EdB.PrepareCarefully.Reflection.*`**: Набор рефлексивных утилит для доступа к приватным полям и методам внутренних классов RimWorld.
* **`EdB.PrepareCarefully.AlienRace` / `AlienRaceBodyAddon`**: Встроенный адаптер для чтения Defs мода `Humanoid Alien Races` (HAR).

---

## 6. Добавляемый контент и его характеристики

Мод является конфигурационным инструментом и не добавляет в игру новых предметов экипировки или зданий, однако определяет собственные системные Defs отношений для кастомизации генеалогии:

### Пользовательские Defs отношений (`CarefullyPawnRelationDef`):
Определены в `Common/Defs/CarefullyPawnRelationDefs/`:

| defName | Инверсивное отношение (`inverse`) | Категория |
|---|---|---|
| `Parent` | `Child` | Кровное родство |
| `Child` | `Parent` | Кровное родство |
| `Sibling` | `Sibling` | Кровное родство |
| `HalfSibling` | `HalfSibling` | Кровное родство |
| `Grandparent` | `Grandchild` | Кровное родство |
| `Grandchild` | `Grandparent` | Кровное родство |
| `UncleOrAunt` | `NephewOrNiece` | Кровное родство |
| `NephewOrNiece` | `UncleOrAunt` | Кровное родство |
| `Cousin` | `Cousin` | Кровное родство |
| `GreatGrandparent` | `GreatGrandchild` | Кровное родство |
| `GreatGrandchild` | `GreatGrandparent` | Кровное родство |
| `Spouse` | `Spouse` | Социальное родство |
| `ExSpouse` | `ExSpouse` | Социальное родство |
| `Lover` | `Lover` | Социальное родство |
| `Fiance` | `Fiance` | Социальное родство |
| `ExLover` | `ExLover` | Социальное родство |
| `Bond` | `Bond` | Связь с животным |

---

## 7. Поддержка и статус русификации
* **Наличие в Steam Workshop**: Да (`294100/838528063` — русская версия, `294100/735106432` — оригинал).
* **Встроенный перевод**: В сборке `838528063` присутствует полный и качественный русский перевод интерфейса (`Common/Languages/Russian/Keyed/EdBPrepareCarefully.xml`), охватывающий более 400 строковых ключей меню, подсказок и ошибок.
* **Требуемый метод русификации**: Для перевода достаточно исключительно работы с XML-файлами в `Languages/Russian/Keyed/`. Перекомпиляция C#-сборки не требуется, так как все интерфейсные надписи вынесены в `Translator.Translate()` / `TaggedString`.
* **Статус**: Согласно правилам, локализация уже встроена в исследуемую сборку `838528063`; создание дублирующих файлов в `russian/` не требуется.

---

## 8. Дополнительная важная информация
* **Архитектура сериализации и форматы файлов (`.pcc` vs `.pcp`)**:
  * Мод хранит пользовательские данные в папке `%USERPROFILE%\AppData\LocalLow\Ludeon Studios\RimWorld by Ludeon Studios\PrepareCarefully\`.
  * **`.pcc` (Prepare Carefully Colonist)**: Файл конкретного поселенца (корневой тег `<character>`, DTO `SaveRecordPawnV5`). Загружается через кнопку «Загрузить» на панели списка пешек (`ControllerTabViewPawns.LoadColonyPawn`) и добавляет персонажа к текущему составу колонии.
  * **`.pcp` (Prepare Carefully Preset)**: Файл полного пресета сценария высадки (корневой тег `<preset>`, DTO `SaveRecordPresetV5`). При загрузке пресета метод `ManagerPawns.ClearPawns()` **уничтожает всех существующих пешек** на карте и в памяти, полностью заменяя стартовый состав и ресурсы.
* **Идентификация и разрешение коллизий через UUID (`<id>`)**:
  * Каждая пешка и временная сущность идентифицируется строковым GUID: `<id>xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx</id>`.
  * Все социальные связи (`SaveRecordRelationshipV3` / `SaveRecordParentChildGroupV5`) в пресетах связывают сущности по их строковым `source` и `target` ID.
  * Если при ручном дублировании XML-файлов персонажей не изменить или не удалить тег `<id>`, мод считает обе пешки одним объектом. При отсутствии или пустом значении тега `<id>` загрузчик (`PawnLoaderV5.ConvertSaveRecordToCustomizedPawn`) автоматически вызывает `Guid.NewGuid().ToString()`.
* **Встроенный механизм обратной совместимости и автомиграции Defs**:
  * В классе `PawnLoaderV5` реализована таблица автоматической миграции устаревших Defs из ранних версий игры и модов:
    * `Gun_SurvivalRifle` → `Gun_BoltActionRifle`, `Medicine` → `MedicineIndustrial`, `Component` → `ComponentIndustrial`, `WolfTimber` → `Wolf_Timber`.
    * Черты характера: `Prosthophobe` → `BodyPurist`, `Prosthophile` → `Transhumanist`, `SuperImmune` → `Immunity`.
    * Навыки: `Growing` → `Plants`, `Research` → `Intellectual`.
    * Протезы: `InstallSyntheticHeart` → `InstallSimpleProstheticHeart`, `InstallAdvancedBionic*` → `InstallArchtechBionic*`.
    * Автоматический fallback поиск `BackstoryDef` по усеченным префиксам идентификаторов.
* **Технические риски и стабильность в долгих партиях**:
  * Поскольку мод использует собственные промежуточные DTO-структуры (`CustomizationsPawn`), обратная компиляция пешки через `PawnCustomizer` может приводить к рассинхронизации внутренних трекеров ванильных пешек (гены Biotech, идеологии, трекеры потребностей).
  * Для максимальной бесконфликтности в больших сборках (100+ модов) в сообществе применяются **Character Editor** (редактирование объектов непосредственно в памяти) или **Pawn Editor** (Vanilla Expanded).

---

## 9. Примечания и технические данные
* **Корневые пространства имен сборки**:
  * `EdB.PrepareCarefully`
  * `EdB.PrepareCarefully.HarmonyPatches`
  * `EdB.PrepareCarefully.Reflection`
* **Ключевые C#-классы и их ответственность**:
  * `EdB.PrepareCarefully.PawnCustomizer` — преобразование DTO `CustomizationsPawn` в объекты `Verse.Pawn` и наложение характеристик.
  * `EdB.PrepareCarefully.ManagerPawns` — управление коллекциями `ColonyPawns`, `WorldPawns`, создание, загрузка (`LoadPawn`) и уничтожение (`DestroyPawn`, `ClearPawns`).
  * `EdB.PrepareCarefully.PawnLoaderV5` / `PawnSaver` — сериализация и десериализация файлов `.pcc` (`SaveRecordPawnV5`).
  * `EdB.PrepareCarefully.PresetLoaderV5` / `PresetSaver` — сериализация и десериализация файлов `.pcp` (`SaveRecordPresetV5`).
  * `EdB.PrepareCarefully.CostCalculator` — динамический пересчет стоимости пешек, имплантов, снаряжения и лимита очков сценария.
  * `EdB.PrepareCarefully.ColonistFiles` / `PresetFiles` — управление путями файловой системы к сохраненным конфигурациям.
  * `EdB.PrepareCarefully.DialogColonist` / `DialogLoadPreset` / `DialogLoadColonist` — интерфейсные диалоговые окна выбора и сохранения.
* **Harmony-инъекции**:
  * `PrepareCarefullyButtonPatch` → `[HarmonyPatch(typeof(Page_ConfigureStartingPawns), "DoWindowContents", new Type[] { typeof(Rect) })]`
  * `ReplaceScenarioPatch` → `[HarmonyPatch(typeof(Game), "InitNewGame", new Type[] { })]`
* **Пользовательские XML Defs**:
  * `EdB.PrepareCarefully.CarefullyPawnRelationDef` (`Parent`, `Child`, `Sibling`, `HalfSibling`, `Grandparent`, `Grandchild`, `UncleOrAunt`, `NephewOrNiece`, `Cousin`, `GreatGrandparent`, `GreatGrandchild`, `Spouse`, `ExSpouse`, `Lover`, `Fiance`, `ExLover`, `Bond`).
* **Ключевые XML-файлы**:
  * `Common/Defs/CarefullyPawnRelationDefs/CarefullyPawnRelations_FamilyByBlood.xml`
  * `Common/Defs/CarefullyPawnRelationDefs/CarefullyPawnRelations_FamilyByChoice.xml`
  * `Common/Defs/CarefullyPawnRelationDefs/CarefullyPawnRelations_Misc.xml`
  * `Common/Languages/Russian/Keyed/EdBPrepareCarefully.xml`

