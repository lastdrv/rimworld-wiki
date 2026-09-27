# Анализ мода: Archotech Weaponry (Continued) - Русификатор

## 1. Основные сведения
* **Название мода**: Archotech Weaponry (Continued) - Русификатор
* **PackageId**: `Dmitry6.zal.archotechweaponry.rus`
* **Steam Workshop ID**: `3595122671`
* **Автор**: Dmitry6
* **Поддерживаемые версии игры**: `1.5`, `1.6` (Анализировалась версия `1.6`)

---

## 2. Цель мода
Мод представляет собой русскоязычную XML-локализацию для мода **Archotech Weaponry (Continued)** (`3312021019`). Он переводит на русский язык названия и подробные описания всех видов архотекского оружия ближнего и дальнего боя, снарядов, пепла Пустоты, уникальных черт персонального оружия (Bladelink Traits), состояний здоровья и психических состояний (боевого транса).

---

## 3. Зависимости
* **Обязательные моды**: 
  * `zal.archotechweaponry` (Steam ID: `3312021019`)
* **Порядок загрузки (`loadAfter`)**:
  * Должен загружаться строго после `zal.archotechweaponry`.

---

## 4. Взаимодействие с ванильными DLC
* Прямого кода взаимодействия нет. Переводит теги XML Defs базового мода Archotech Weaponry.

---

## 5. Взаимодействие со сторонними библиотеками
* Чистый XML-мод локализации. Код C# отсутствует.

---

## 6. Поддержка и статус русификации
* **Наличие в Steam Workshop**: Да (`294100/3595122671`).
* **Тип мода**: Является готовым сторонним модулем локализации для мода `3312021019`.
* **Структура перевода**: 
  * `1.6/Languages/Russian/DefInjected/HediffDef/Hediffs_AT_Misc_HediffDef.xml`
  * `1.6/Languages/Russian/DefInjected/MentalStateDef/MentalStates_Mood_MentalStateDef.xml`
  * `1.6/Languages/Russian/DefInjected/RulePackDef/ArchotechName_RulePackDef.xml`
  * `1.6/Languages/Russian/DefInjected/ThingDef/FilthDef_ThingDef.xml`
  * `1.6/Languages/Russian/DefInjected/ThingDef/MeleeArchotech_ThingDef.xml`
  * `1.6/Languages/Russian/DefInjected/ThingDef/RangedArchotech_ThingDef.xml`
  * `1.6/Languages/Russian/DefInjected/ThoughtDef/WeaponTraitDefs_ThoughtDef.xml`
  * `1.6/Languages/Russian/DefInjected/WeaponTraitDef/WeaponTraitDefs_WeaponTraitDef.xml`
  * `1.6/Compat/MAG/Languages/Russian/DefInjected/RecipeDef/RecipeDefs_RecipeDef.xml`
* **Статус**: Перевод распространяется как отдельный мод. По умолчанию при анализе файлы в `russian/` не создавались. При необходимости материалы перевода могут быть перенесены в `russian/3312021019/` для установки поверх базового мода.

---

## 7. Дополнительная важная информация
* Все названия и описания архотекских технологических атрибутов и абстрактных концепций (Аннигиляция Пустоты, Прекогниция, Чумной вестник, Боевой транс) переведены на русский язык с высоким качеством и полным соблюдением художественного стиля RimWorld.

---

## 8. Примечания и технические данные
* **Относительные пути файлов перевода**:
  * `1.6/Languages/Russian/DefInjected/ThingDef/RangedArchotech_ThingDef.xml`
  * `1.6/Languages/Russian/DefInjected/ThingDef/MeleeArchotech_ThingDef.xml`
  * `1.6/Languages/Russian/DefInjected/WeaponTraitDef/WeaponTraitDefs_WeaponTraitDef.xml`
  * `1.6/Languages/Russian/DefInjected/HediffDef/Hediffs_AT_Misc_HediffDef.xml`
