# XML Defs и структура элементов

В RimWorld `Def` (сокращение от Definition) — это фундаментальный элемент декларативного моддинга. Все объекты игры (предметы, пешки, здания, рецепты, исследовательской работы, погодные эффекты) описываются с помощью XML Defs и не требуют написания C#-кода для базовой конфигурации.

---

## 1. Базовая структура Def

Каждый XML-файл с определениями должен иметь корневой тег `<Defs>`. Внутри него содержатся конкретные объекты-определения.

```xml
<?xml version="1.0" encoding="utf-8"?>
<Defs>
  <ThingDef ParentName="BaseBuilding">
    <defName>MyCustomStructure</defName>
    <label>custom structure</label>
    <description>A durable custom building structure.</description>
    <statBases>
      <MaxHitPoints>250</MaxHitPoints>
      <WorkToBuild>1000</WorkToBuild>
    </statBases>
  </ThingDef>
</Defs>
```

### Обязательные и ключевые поля
- **`defName`**: Уникальный строковый идентификатор объекта. Используется игрой и другими модами для ссылок. Должен быть уникальным во всех загруженных модах.
- **`label`**: Отображаемое название объекта в интерфейсе игры (нижний регистр; игра автоматически капитализирует где нужно).
- **`description`**: Текстовое описание объекта во всплывающей подсказке или окне информации.
- **`ParentName`**: Имя родительского шаблона (`Abstract="True"`), от которого наследуются поля.

---

## 2. Наследование и шаблоны (`Abstract="True"`)

Для уменьшения дублирования кода RimWorld поддерживает систему наследования XML с помощью атрибутов `ParentName` и `Name`.

### Создание абстрактного родителя
```xml
<ThingDef Name="MyBaseGun" Abstract="True">
  <category>Item</category>
  <equipmentType>Primary</equipmentType>
  <statBases>
    <Flammability>0.5</Flammability>
  </statBases>
</ThingDef>
```

### Наследование от абстрактного родителя
```xml
<ThingDef ParentName="MyBaseGun">
  <defName>MyCustomRifle</defName>
  <label>custom rifle</label>
  <description>An advanced rifle.</description>
</ThingDef>
```

---

## 3. Переопределение и удаление полей при наследовании

- **Переопределение значения**: Если дочерний элемент указывает тег, уже объявленный в родителе, значение дочернего элемента переписывает родительское.
- **`Inherit="False"`**: Используется для полной замены или отмены родительских списков (`<comps>`, `<statBases>`).
  ```xml
  <statBases Inherit="False">
    <MaxHitPoints>500</MaxHitPoints>
  </statBases>
  ```

---

## 4. Классы C#, связанные с Defs

Каждый `Def` в XML соответствует объекту класса в C# (`Verse.Def` или его наследникам):
- `<ThingDef>` -> `Verse.ThingDef` (предметы, здания, пешки).
- `<BiomeDef>` -> `RimWorld.BiomeDef` (биомы).
- `<RecipeDef>` -> `Verse.RecipeDef` (рецепты крафта/операций).
- `<HediffDef>` -> `Verse.HediffDef` (состояния здоровья, болезни, травмы).
- `<JobDef>` -> `Verse.JobDef` (задачи пешек).

Если требуется создать собственный тип Def, в XML указывается атрибут `Class`:
```xml
<MyMod.MyCustomDef Class="MyMod.MyCustomDef">
  <defName>CustomDefEntry</defName>
  <customField>123</customField>
</MyMod.MyCustomDef>
```
