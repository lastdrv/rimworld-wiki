# Настройки мода (ModSettings)

RimWorld предоставляет встроенный API для создания окна пользовательских настроек мода (`ModSettings` и класс `Mod`), доступного через игровое меню настроек модов.

---

## 1. Класс хранения настроек (`ModSettings`)

Класс сохраняет пользовательские параметры и записывает их в XML в папке пользовательских данных при выходе или сохранении.

```csharp
using Verse;

namespace MyMod
{
    public class MyModSettings : ModSettings
    {
        public bool enableExtraFeatures = true;
        public float multiplier = 1.5f;

        public override void ExposeData()
        {
            base.ExposeData();
            Scribe_Values.Look(ref enableExtraFeatures, "enableExtraFeatures", true);
            Scribe_Values.Look(ref multiplier, "multiplier", 1.5f);
        }
    }
}
```

---

## 2. Главный класс мода (`Mod`)

Класс, унаследованный от `Verse.Mod`, автоматически инициализируется при запуске игры и отвечает за отрисовку интерфейса настроек.

```csharp
using UnityEngine;
using Verse;

namespace MyMod
{
    public class MyMod : Mod
    {
        public static MyModSettings settings;

        public MyMod(ModContentPack content) : base(content)
        {
            settings = GetSettings<MyModSettings>();
        }

        public override string SettingsCategory()
        {
            return "My Custom Mod"; // Название мода в меню настроек
        }

        public override void DoSettingsWindowContents(Rect inRect)
        {
            Listing_Standard listingStandard = new Listing_Standard();
            listingStandard.Begin(inRect);

            listingStandard.CheckboxLabeled("MyMod_EnableFeature".Translate(), ref settings.enableExtraFeatures, "Tooltip text");
            listingStandard.SliderLabeled("MyMod_Multiplier".Translate(), settings.multiplier, 0.5f, 5.0f);

            listingStandard.End();
            base.DoSettingsWindowContents(inRect);
        }
    }
}
```
