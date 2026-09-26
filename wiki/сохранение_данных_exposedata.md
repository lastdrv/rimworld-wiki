# Сохранение данных (ExposeData)

В RimWorld сохранение и загрузка данных об объектах, компонентах и состоянии мода при сохранении/загрузке игры (`.rws`) выполняется с помощью интерфейса `IExposable` и класса `Scribe`.

---

## 1. Метод `ExposeData()` и интерфейс `IExposable`

Любой класс, данные которого должны сохраняться в файле сохранения, должен реализовывать интерфейс `IExposable` и метод `ExposeData()`.

```csharp
using Verse;

namespace MyMod
{
    public class CustomDataHolder : IExposable
    {
        public int customCounter;
        public string ownerName;
        public Thing targetThing;

        public void ExposeData()
        {
            Scribe_Values.Look(ref customCounter, "customCounter", 0);
            Scribe_Values.Look(ref ownerName, "ownerName", "Default");
            Scribe_References.Look(ref targetThing, "targetThing");
        }
    }
}
```

---

## 2. Разновидности методов `Scribe`

### `Scribe_Values` (Простые типы данных)
Сохраняет примитивные типы (int, float, bool, string, Vector3, enums):
```csharp
Scribe_Values.Look(ref myBool, "myBool", false);
Scribe_Values.Look(ref myFloat, "myFloat", 1.0f);
```

### `Scribe_References` (Ссылки на существующие объекты игры)
Сохраняет ссылку на объект `Thing`, `Pawn`, `Faction`, который уже сохранен в игре под своим уникальным ID (`loadID`).
```csharp
Scribe_References.Look(ref assignedPawn, "assignedPawn");
```

### `Scribe_Deep` (Вложенные уникальные объекты)
Сохраняет новый экземпляр класса, созданный вашим модом и реализующий `IExposable`.
```csharp
Scribe_Deep.Look(ref customDataHolder, "customDataHolder");
```

### `Scribe_Collections` (Списки и словари)
Сохраняет коллекции (`List<T>`, `HashSet<T>`, `Dictionary<K, V>`):
```csharp
Scribe_Collections.Look(ref pawnList, "pawnList", LookMode.Reference);
Scribe_Collections.Look(ref stringList, "stringList", LookMode.Value);
```

---

## 3. Режимы `Scribe.mode`

Во время вызова `ExposeData()` метод выполняется либо в режиме сохранения (`Saving`), либо в режиме загрузки (`LoadingVars` / `PostLoadInit`).

При необходимости выполнить код строго после завершения загрузки ссылок используйте:
```csharp
if (Scribe.mode == LoadSaveMode.PostLoadInit)
{
    // Код проверки или восстановления связей объектов после загрузки
}
```
