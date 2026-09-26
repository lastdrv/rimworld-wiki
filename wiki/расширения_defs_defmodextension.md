# Расширения Defs (DefModExtension)

`DefModExtension` позволяет добавлять кастомные C#-данные и свойства в стандартные ванильные `Defs` (например, `ThingDef`, `BiomeDef`, `ResearchProjectDef`) без создания собственных подклассов Def.

---

## 1. Создание класса расширения на C#

Класс расширения наследуется от `Verse.DefModExtension`:

```csharp
using Verse;

namespace MyMod
{
    public class MyWeaponExtension : DefModExtension
    {
        public float armorPenetrationBonus = 0.15f;
        public bool causesBleeding = true;
        public string customSoundName;
    }
}
```

---

## 2. Подключение расширения в XML Def

В XML-файле расширение указывается внутри узла `<modExtensions>`:

```xml
<ThingDef ParentName="BaseHumanMakeableGun">
  <defName>Gun_SpecialRifle</defName>
  <label>special rifle</label>
  <modExtensions>
    <li Class="MyMod.MyWeaponExtension">
      <armorPenetrationBonus>0.30</armorPenetrationBonus>
      <causesBleeding>true</causesBleeding>
      <customSoundName>CustomShotSound</customSoundName>
    </li>
  </modExtensions>
</ThingDef>
```

---

## 3. Чтение данных расширения в C# коде

Для считывания данных используется метод расширения `GetModExtension<T>()` или `HasModExtension<T>()`:

```csharp
using Verse;

namespace MyMod
{
    public static class WeaponUtility
    {
        public static void ProcessWeapon(ThingDef weaponDef)
        {
            if (weaponDef.HasModExtension<MyWeaponExtension>())
            {
                MyWeaponExtension ext = weaponDef.GetModExtension<MyWeaponExtension>();
                Log.Message($"Weapon armor penetration bonus: {ext.armorPenetrationBonus}");
            }
        }
    }
}
```

### Преимущества `DefModExtension`
1. Сохраняет полную совместимость с ванильными типами (`ThingDef`).
2. Позволяет нескольким модам одновременно добавлять собственные данные к одному и тому же объекту через XML Patches.
