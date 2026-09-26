# Патчирование XML (PatchOperations)

Патчирование XML — это механизм модификации существующих `Defs` ванильной игры или других модов без прямой замены их файлов. Патчи хранятся в папке мода `Patches/` и исполняются при загрузке игры.

---

## 1. Базовая структура патч-файла

Каждый патч-файл помещается в `Patches/*.xml` и содержит корневой тег `<Patch>`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<Patch>
  <Operation Class="PatchOperationAdd">
    <xpath>/Defs/ThingDef[defName="Apparel_Parka"]/statBases</xpath>
    <value>
      <Insulation_Cold>40</Insulation_Cold>
    </value>
  </Operation>
</Patch>
```

---

## 2. Основные виды `PatchOperation`

### `PatchOperationAdd`
Добавляет новые дочерние XML-узлы внутрь выбранного через `xpath` элемента.
```xml
<Operation Class="PatchOperationAdd">
  <xpath>/Defs/ThingDef[defName="Human"]/comps</xpath>
  <value>
    <li Class="MyMod.CompProperties_MyCustomComp" />
  </value>
</Operation>
```

### `PatchOperationReplace`
Заменяет существующий узел или значение на новое.
```xml
<Operation Class="PatchOperationReplace">
  <xpath>/Defs/ThingDef[defName="Gun_AssaultRifle"]/statBases/MarketValue</xpath>
  <value>
    <MarketValue>750</MarketValue>
  </value>
</Operation>
```

### `PatchOperationRemove`
Удаляет узел, указанный в `xpath`.
```xml
<Operation Class="PatchOperationRemove">
  <xpath>/Defs/ThingDef[defName="Apparel_BasicShirt"]/costList/Cloth</xpath>
</Operation>
```

### `PatchOperationInsert` / `PatchOperationAddModExtension`
Вставляет элемент перед/после указанного узла.

---

## 3. Логические и безопасные патч-операции

### `PatchOperationAttributeSet`
Устанавливает атрибут на узел (например, `Class="MyClass"` или `Abstract="True"`).

### `PatchOperationFindMod`
Выполняет группу патчей только в том случае, если загружен определенный мод (по `packageId` или названию):
```xml
<Operation Class="PatchOperationFindMod">
  <mods>
    <li>Ludeon.RimWorld.Biotech</li>
  </mods>
  <match Class="PatchOperationAdd">
    <xpath>/Defs/ThingDef[defName="Pawn"]/comps</xpath>
    <value>
      <li Class="MyMod.CompGeneReactor" />
    </value>
  </match>
  <nomatch Class="PatchOperationAdd">
    <xpath>/Defs/ThingDef[defName="Pawn"]/comps</xpath>
    <value>
      <li Class="MyMod.CompStandardReactor" />
    </value>
  </nomatch>
</Operation>
```

### `PatchOperationSequence`
Выполняет список патчей последовательно. Если хотя бы один патч завершается неудачей, вся последовательность отменяется.

### `PatchOperationConditional`
Проверяет наличие `xpath`. Если узел существует, выполняется ветка `<match>`, иначе `<nomatch>`.

---

## 4. Особенности XPath в RimWorld

- Ищите элементы по `defName`: `/Defs/ThingDef[defName="MyDefName"]`
- Выбирайте конкретные узлы в списках: `/Defs/ThingDef[defName="Human"]/statBases/MoveSpeed`
- Используйте безопасную проверку с `<match>` и `<nomatch>`, чтобы предотвратить ошибки загрузки при отсутствии целевых узлов.
