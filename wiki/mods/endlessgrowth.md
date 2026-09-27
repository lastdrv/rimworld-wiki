# Анализ мода: EndlessGrowth

## 1. Основные сведения

| Параметр | Значение |
|---|---|
| **Полное название** | EndlessGrowth |
| **Идентификатор (`packageId`)** | `SlimeSenpai.EndlessGrowth` |
| **Идентификатор Workshop (ID)** | `2894613990` |
| **Автор** | Slime-Senpai |
| **Версия мода / Поддержка** | 1.4, 1.5, **1.6** *(исследуемая)* |
| **Официальный репозиторий** | [GitHub EndlessGrowth](https://github.com/Slime-Senpai/EndlessGrowth) |

---

## 2. Цель мода

**EndlessGrowth** снимает жесткое ванильное ограничение RimWorld на максимальный 20-й уровень навыков пешек (*Shooting, Construction, Mining, Medicine, Cooking и др.*), позволяя колонистам бесконечно развивать навыки дальше (до 100 уровня и выше) с экспоненциальной кривой опыта (XP).

### Основные задачи и возможности:
- Снятие лимита 20 уровня навыка с возможностью настройки пользовательского потолка уровня (`maxLevel`, по умолчанию `-1` — бесконечно).
- Экспоненциальный рост стоимости XP: для достижения 100 уровня с 99 потребуется 650 000 XP.
- Масштабирование качества создаваемых предметов на верстаках в зависимости от высокого уровня навыка (включая повышенный шанс создания *Masterwork* и *Legendary* предметов).
- Расширение диапазона допустимых уровней навыков в настройках рецептов верстаков (`Bill.allowedSkillRange`).

---

## 3. Зависимости

- **Обязательные моды**:
  - `brrainz.harmony` (**Harmony**) — мод полностью построен на Transpiler-патчах CIL-кода.
- **Официальные DLC**:
  - Совместим со всеми DLC (Royalty, Ideology, Biotech, Anomaly, Odyssey). Специфичных XML Defs для DLC не требуется.

---

## 4. Взаимодействие и переопределение ванильного функционала и DLC

Мод модифицирует базовую механику работы навыков и качества крафта ванильной игры через C#-патчи:

### 1. Переопределение лимитов навыка (`SkillRecord`)
- **Расчет требуемого XP (`SkillRecord.XpRequiredToLevelUpFrom`)**:
  - Ванильная кривая `XpForLevelUpCurve` заменяется на расширенную `XpForInfiniteLevelUpCurve` (от 1000 XP на 0 уровне до 650 000 XP на 99 уровне).
- **Повышение уровня и сброс (`SkillRecord.Learn` / `SkillRecord.Interval`)**:
  - Убираются проверки `levelInt == 20` и `levelInt >= 20`.
  - Вводится принудительный спад опыта на уровнях выше 20 (`-12 XP` за интервал).
- **Названия уровней (`SkillRecord.LevelDescriptor`)**:
  - Добавляет генерацию наименований текстовых рангов для уровней 21–100 через ключи `Skill21` ... `Skill100`.

### 2. Генерация качества предметов (`QualityUtility`)
- **Перерасчет модификатора качества (`QualityUtility.GenerateQualityCreatedByPawn`)**:
  - Заменяет ванильный `switch` на вычисление по новой кривой `QualityModifierCurve` (20 ур. = 4.2; 50 ур. = 6.0; 100 ур. = 10.0).
  - Снимает ограничения `Math.Clamp` до качества `Legendary`.
- **Уведомления о крафте (`QualityUtility.SendCraftNotification`)**:
  - Добавляет возможность отключения всплывающих сообщений о создании шедевров/легендарных вещей в настройках мода, чтобы избежать спама на высоких уровнях.

### 3. Задачи и рецепты верстаков (`Bill` & `Dialog_BillConfig`)
- **Диапазон навыков рецептов**:
  - Автоматически расширяет верхний предел слайдера допустимых навыков в окне настройки рецептов с 20 до `GetMaxLevelForBill()` (до 100 уровня или выше).

---

## 5. Взаимодействие со сторонними библиотеками

1. **Harmony (`brrainz.harmony`)**:
   - Главная функциональная библиотека. Все патчи реализованы с помощью низкоуровневой модификации IL-инструкций (`CodeMatcher` и `CodeInstruction`).
2. **Mad Skills**:
   - Автоматически обнаружает присутствие мода *Mad Skills* и накладывает постфикс-патч на `RTMadSkills.Patch_SkillRecordInterval.VanillaMultiplier`, предотвращая сброс или обнуление мультипликатора на уровнях > 20.
3. **Vanilla Traits Expanded**:
   - Автоматически обнаружает черту *Perfectionist* из мода *Vanilla Traits Expanded* и удаляет Transpiler-патчем ограничение легендарного качества предметов.

---

## 6. Добавляемый контент и его характеристики

Мод **EndlessGrowth** является системным модификатором механики навыков и качества крафта C#. Он **не добавляет новых физических предметов, построек или инцидентов**, но расширяет ролевую систему и интерфейс:

### 1. Добавляемые титулы и ранги навыков (Уровни 21–100)

* **Уровневые ранги навыков (`Languages/Russian/Keyed/Skills.xml`)**:
  * Добавляет 80 новых текстовых рангов (титулов) для отображения в окне характеристик колонистов при достижении уровней выше 20:
    * **Уровень 21–25**: *Мастер-виртуоз*, *Великий мастер*, *Непревзойденный*.
    * **Уровень 26–50**: *Трансцендентный*, *Легенда*, *Мифический мастер*.
    * **Уровень 51–75**: *Полубог*, *Начинающий Бог*.
    * **Уровень 76–100**: *Бог*, *Бог Богов*, *Творец*.

### 2. Изменение характеристик и формул расчетов (Параметры C#)

* **Требуемый опыт для уровня (`XpForInfiniteLevelUpCurve`)**:
  * **Уровень 20**: 20,000 XP.
  * **Уровень 50**: ~150,000 XP.
  * **Уровень 99 -> 100**: 650,000 XP.
* **Модификатор генерации качества (`QualityModifierCurve`)**:
  * Уровень 20 = Модификатор `4.2` (~33% шанс на *Masterwork*).
  * Уровень 50 = Модификатор `6.0` (повышенный шанс на *Legendary*).
  * Уровень 100 = Модификатор `10.0` (гарантированное создание шедевров/легендарных вещей).

---

## 7. Поддержка и статус русификации

- ✅ **Названия уровней навыков (`Languages/Russian/Keyed/Skills.xml`)**:
  - В исходном коде мода переведены все 80 новых рангов навыков с 21 по 100 уровень (*"Трансцендентный"*, *"Полубог"*, *"Начинающий Бог"*, *"Бог"*, *"Бог Богов"* и т.д.).
- ℹ️ **Меню настроек мода (`Languages/Russian/Keyed/EndlessGrowth_Keyed_settings.xml`)**:
  - В базовом репозитории GitHub файл настроек отсутствует, однако в версии из **Steam Workshop (ID: `2894613990`)** русификация меню настроек **полностью присутствует** в файле `EndlessGrowth_Keyed_settings.xml`.
  - Все ключи меню (`EndlessGrowth_pleaseRestart`, `EndlessGrowth_maxLevelExplanation`, `EndlessGrowth_unlimitedPriceExplanation`, `EndlessGrowth_gapForMaxBillExplanation`, `EndlessGrowth_craftNotificationsEnabledExplanation`) полностью переведены на русский язык.

---

## 8. Дополнительная важная информация

- **Настройки мода (`EndlessGrowthModSettings`)**:
  - `maxLevel` (int): Максимальный уровень (по умолчанию `-1` — без ограничений).
  - `unlimitedPrice` (bool): Разрешить цене продажи предмета быть выше цены покупки.
  - `gapForMaxBill` (int): Зазор между максимальным уровнем пешки и слайдером рецептов (по умолчанию `20`).
  - `craftNotificationsEnabled` (bool): Включение/отключение уведомлений о создании *Masterwork* / *Legendary* предметов.

---

## 9. Примечания и технические данные для разработки

В данном разделе приведены ключевые C#-классы, структура патчей и CIL Transpiler операции мода **EndlessGrowth** (версия **1.6**).

### Иерархия ключевых классов C# (`namespace SlimeSenpai.EndlessGrowth`)

```text
Verse.Mod -> SlimeSenpai.EndlessGrowth.EndlessGrowthMod
Verse.ModSettings -> SlimeSenpai.EndlessGrowth.EndlessGrowthModSettings
SlimeSenpai.EndlessGrowth.HarmonyPatches
SlimeSenpai.EndlessGrowth.CommonPatches
```

### Таблица Harmony-патчей и их назначение

| Целевой класс | Целевой метод | Тип патча | Назначение патча |
|---|---|---|---|
| `SkillRecord` | `XpRequiredToLevelUpFrom` | `Transpiler` | Подменяет `XpForLevelUpCurve` на `XpForInfiniteLevelUpCurve`. |
| `SkillRecord` | `Learn` | `Transpiler` | Удаляет ванильные проверки `levelInt == 20` и `levelInt >= 20`. |
| `SkillRecord` | `LevelDescriptor` | `Postfix` | Переводит ранг через `("Skill" + level).Translate()`. |
| `SkillRecord` | `Interval` | `Transpiler` | Применяет спад опыта на уровнях выше 20. |
| `QualityUtility` | `GenerateQualityCreatedByPawn` | `Transpiler` | Внедряет `QualityModifierCurve` и расширяет лимит качества до `Legendary`. |
| `QualityUtility` | `SendCraftNotification` | `Prefix` | Подавляет уведомления при `craftNotificationsEnabled == false`. |
| `Bill` | `Constructor` | `Postfix` | Устанавливает `allowedSkillRange.max` равным `GetMaxLevelForBill()`. |
| `Dialog_BillConfig` | `DoWindowContents` | `Transpiler` | Подменяет ограничение `20` в UI на вызов `GetMaxLevelForBill()`. |
| `Tradeable` | `InitPriceDataIfNeeded` | `Transpiler` | Разрешает цену продажи выше покупки при `unlimitedPrice == true`. |
