# Анализ мода: Русский язык для Rimsenal - Core Murder Diversified

## 1. Основные сведения
* **Название мода**: Русский язык для Rimsenal - Core Murder Diversified
* **PackageId**: `RU.rimsenal.core`
* **Steam Workshop ID**: `2590686872`
* **Автор**: Камень
* **Поддерживаемые версии игры**: `1.0`, `1.1`, `1.2`, `1.3`, `1.4`, `1.5`, `1.6`

---

## 2. Цель мода
Мод является неофициальным любительским русификатором для крупного контентного мода **Rimsenal: Murder Diversified** (`725947920`). Он обеспечивает перевод всех текстовых описаний, названий предметов, технологий, урона, повреждений и интерфейсов на русский язык.

---

## 3. Зависимости
* **Обязательные моды**: 
  * `rimsenal.core` (Steam ID: `725947920`)
* **Порядок загрузки (`loadAfter`)**:
  * Должен загружаться строго после `rimsenal.core`.

---

## 4. Взаимодействие с ванильными DLC
* Прямого кода взаимодействия нет. Мод переводит теги XML Defs основного мода Rimsenal Core.

---

## 5. Взаимодействие со сторонними библиотеками
* Чистый XML-мод локализации. Код C# отсутствует.

---

## 6. Поддержка и статус русификации
* **Наличие в Steam Workshop**: Да (`294100/2590686872`).
* **Тип мода**: Является готовым сторонним модулем локализации для мода `725947920`.
* **Структура перевода**: 
  * Содержит файлы XML в папке `Languages/Russian/DefInjected/` для категорий `DamageDef`, `HediffDef`, `RecipeDef`, `ResearchProjectDef`, `ResearchTabDef`, `ThingCategoryDef`, `ThingDef`, `WorkGiverDef`.
* **Статус**: Перевод распространяется как отдельный мод. По умолчанию при анализе файлы в `russian/` не создавались. В случае явного запроса пользователя материалы данного мода могут быть перенесены в `russian/725947920/` для установки поверх основного мода.


---

## 7. Дополнительная важная информация
* **Качество перевода**: Высокое соответствие sci-fi сеттингу RimWorld. Специфические названия корпораций (Greydale, Jotunheim Prime, Tech-Hedron, Yeonwha Precision) и короткие аббревиатуры (`EMP`, `HP`, `UI`) сохранены в корректной транслитерации/оригинале.

---

## 8. Примечания и технические данные
* **Относительные пути файлов перевода**:
  * `Languages/Russian/DefInjected/ThingDef/Weapons_GD.xml`
  * `Languages/Russian/DefInjected/ThingDef/Weapons_JI.xml`
  * `Languages/Russian/DefInjected/ThingDef/Weapons_TE.xml`
  * `Languages/Russian/DefInjected/ThingDef/Weapons_YP.xml`
  * `Languages/Russian/DefInjected/ThingDef/Apparel_RS.xml`
  * `Languages/Russian/DefInjected/ResearchProjectDef/ResearchRimsenal.xml`
