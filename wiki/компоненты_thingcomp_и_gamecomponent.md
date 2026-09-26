# Компоненты (ThingComp, GameComponent, WorldComponent)

В RimWorld паттерн компонентного программирования используется для расширения функционала предметов, зданий, игрового мира или процесса игры в целом.

---

## 1. Компоненты объектов (`ThingComp` и `CompProperties`)

`ThingComp` — это класс C#, привязываемый к конкретному объекту `Thing` (зданию, оружию, пешке).

### Шаг 1: Класс свойств (`CompProperties`)
Класс свойств передает параметры из XML в C#-компонент.

```csharp
using Verse;

namespace MyMod
{
    public class CompProperties_CustomPower : CompProperties
    {
        public float powerProduction = 500f;

        public CompProperties_CustomPower()
        {
            this.compClass = typeof(CompCustomPower);
        }
    }
}
```

### Шаг 2: Класс компонента (`ThingComp`)
```csharp
using Verse;

namespace MyMod
{
    public class CompCustomPower : ThingComp
    {
        public CompProperties_CustomPower Props => (CompProperties_CustomPower)props;

        public override void CompTick()
        {
            base.CompTick();
            if (parent.IsHashIntervalTick(250))
            {
                // Выполняется каждые 250 тиков
                Log.Message($"Generating {Props.powerProduction} W.");
            }
        }
    }
}
```

### Шаг 3: Подключение в XML Def
```xml
<ThingDef ParentName="BuildingBase">
  <defName>MyPowerGenerator</defName>
  <comps>
    <li Class="MyMod.CompProperties_CustomPower">
      <powerProduction>750</powerProduction>
    </li>
  </comps>
</ThingDef>
```

---

## 2. Глобальные компоненты игры (`GameComponent`)

`GameComponent` — класс, который создается при старте/загрузке игры и существует на всем протяжении текущей игровой сессии.

```csharp
using Verse;

namespace MyMod
{
    public class MyGameManager : GameComponent
    {
        public int globalCounter = 0;

        public MyGameManager(Game game)
        {
        }

        public override void GameComponentTick()
        {
            base.GameComponentTick();
            globalCounter++;
        }

        public override void ExposeData()
        {
            base.ExposeData();
            Scribe_Values.Look(ref globalCounter, "globalCounter", 0);
        }
    }
}
```

---

## 3. Компоненты карты и мира (`MapComponent`, `WorldComponent`)

- **`WorldComponent`**: Привязан к объекту глобальной планеты `World`. Сохраняет состояние глобальных событий и фракций.
- **`MapComponent`**: Создается для каждой сгенерированной карты (поселение, древняя лаборатория, локация задания). Позволяет отслеживать события конкретной карты (`MapComponentTick`).
