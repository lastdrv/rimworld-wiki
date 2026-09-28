# Анализ мода: EPOE - Forked Russian Language (Русификатор)

## 1. Основные сведения
* **Название мода**: Expanded Prosthetics and Organ Engineering - Forked Russian Language
* **PackageId**: `RU.pashka.vat.epoeforked`
* **Steam Workshop ID**: `2735140780`
* **Автор**: pashka
* **Поддерживаемые версии игры**: `1.2`–`1.6` (Анализировалась версия `1.6`)
* **Базовые моды**: `Expanded Prosthetics and Organ Engineering - Forked` (`1949064302`) и `EPOE-Forked: Royalty DLC expansion` (`2008970276`)

---

## 2. Цель мода
Мод **EPOE - Forked Russian Language** представляет собой комплексную русификацию экосистемы EPOE-Forked. Он обеспечивает полный и качественный перевод всех названий предметов, органов, хирургических рецептов, подсказок интерфейса, исследовательских веток и патчей совместимости для основного мода и его дополнения под Royalty.

---

## 3. Зависимости
* **Обязательные базовые моды**:
  * `vat.epoeforked` (*EPOE-Forked*)
* **Опциональный порядок загрузки (`loadAfter`)**:
  * `vat.epoeforked`, `vat.epoeforkedroyalty`.
  * Рекомендуется располагать в списке модов **ниже** основных модов для полного переопределения локализации.

---

## 4. Взаимодействие с ванильными DLC
* Содержит готовые блоки перевода для дополнений *Royalty*, *Biotech*, *Ideology* и *Anomaly* в подпапках `OtherMods/`.
* Не вносит изменений в игровую механику.

---

## 5. Взаимодействие со сторонними библиотеками
* C#-код отсутствует.
* Полная поддержка перевода ключей настроек мода `XML Extensions` (`OtherMods/XML_Extensions/Languages/Russian/Keyed/`).

---

## 6. Поддержка и статус русификации
* **Наличие в Steam Workshop**: Да (`294100/2735140780`).
* **Полнота перевода**: 100% покрытие обоих базовых модов (`EPOE-Forked` и `EPOE-Forked Royalty`), включая рецепты разборки, ампутаций, синтеза и меню настроек.
* **Структура файлов локализации**:
  * `Languages/Russian/DefInjected/ThingDef/` — протезы, органы, верстаки.
  * `Languages/Russian/DefInjected/RecipeDef/` — все виды хирургических операций.
  * `Languages/Russian/DefInjected/HediffDef/` — статусы аугментаций и имплантов.
  * `Languages/Russian/DefInjected/ResearchProjectDef/` — древо технологий.
  * `Languages/Russian/Keyed/` — интерфейсные сообщения.
* **Требуемый метод русификации**: XML-локализация. C#-пересборка не требуется.
* **Статус**: Согласно правилам, по умолчанию файлы в `russian/` не создавались.

---

## 7. Дополнительная важная информация
* **Качество терминологии**: Автор перевода использует каноничные для русскоязычного сообщества RimWorld медицинские и научно-фантастические термины, выверяя описания операций под ванильный стиль Ludeon.

---

## 8. Примечания и технические данные
* **Каталоги перевода**: `Languages/Russian/`, `OtherMods/*/Languages/Russian/`
* **Ключевые переведенные модули**:
  * `EPOEFR` (EPOE-Forked Royalty)
  * `EPOEFDCO` (Direct Crafting Option)
  * `EPOEFRWP` (Remove Workbenches Option)
  * `XML_Extensions` (Модуль расширенных настроек)
