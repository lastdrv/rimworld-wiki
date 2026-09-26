# Условная загрузка XML (MayRequire)

Атрибуты `MayRequire` и `MayRequireAnyOf` позволяют выполнять условную загрузку отдельных `Def`-файлов, конкретных узлов XML или элементов списков в зависимости от наличия определенных DLC (например, *Royalty*, *Ideology*, *Biotech*, *Anomaly*) или других сторонних модов.

---

## 1. Синтаксис `MayRequire`

Атрибут `MayRequire` проверяет наличие мода по его `packageId` (в нижнем регистре).

### Условная загрузка отдельных узлов и свойств
```xml
<ThingDef ParentName="BaseBuilding">
  <defName>MyBioReactor</defName>
  <label>bio reactor</label>
  <!-- Данный comp загрузится только если установлен и активен DLC Biotech -->
  <comps>
    <li CompClass="CompGeneExtractor" MayRequire="Ludeon.RimWorld.Biotech" />
  </comps>
</ThingDef>
```

### Условная загрузка полей рецепта или исследований
```xml
<RecipeDef>
  <defName>MakeSpecialItem</defName>
  <label>make special item</label>
  <products>
    <MyItem>1</MyItem>
  </products>
  <researchPrerequisite MayRequire="Ludeon.RimWorld.Ideology">IdeologyTech</researchPrerequisite>
</RecipeDef>
```

---

## 2. Использование `MayRequireAnyOf` и `MayRequireWithAll`

### `MayRequireAnyOf` (Условие ИЛИ)
Элемент загрузится, если активен **хотя бы один** из указанных модов (список разделяется запятыми):
```xml
<li MayRequireAnyOf="Ludeon.RimWorld.Biotech,Ludeon.RimWorld.Anomaly">
  <defName>GeneticAnomalyResearch</defName>
</li>
```

### `MayRequireWithAll` (Условие И)
Элемент загрузится только в том случае, если активны **все** указанные моды одновременно:
```xml
<li MayRequireWithAll="Ludeon.RimWorld.Royalty,Ludeon.RimWorld.Biotech">
  <defName>RoyalGeneExtractor</defName>
</li>
```

---

## 3. Распространенные `packageId` официальных DLC RimWorld

| Название DLC | `packageId` |
|---|---|
| **Royalty** | `Ludeon.RimWorld.Royalty` |
| **Ideology** | `Ludeon.RimWorld.Ideology` |
| **Biotech** | `Ludeon.RimWorld.Biotech` |
| **Anomaly** | `Ludeon.RimWorld.Anomaly` |

---

## 4. Загрузка целых Defs через `MayRequire`
Атрибут `MayRequire` можно ставить на корневой тег `Def`:
```xml
<ThingDef ParentName="BuildingBase" MayRequire="Ludeon.RimWorld.Biotech">
  <defName>GeneVault</defName>
  ...
</ThingDef>
```
Если условный мод не установлен, парсер RimWorld пропустит этот `Def` без выдачи ошибок в лог.
