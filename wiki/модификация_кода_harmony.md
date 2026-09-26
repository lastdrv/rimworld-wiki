# Модификация кода через Harmony

**Harmony** — это универсальная C#-библиотека для динамической модификации (патчинга) C#-методов во время выполнения игры (In-Memory Patching). Это основной инструмент создания C#-модов в RimWorld, заменяющий прямой модифицирующий код.

---

## 1. Инициализация Harmony в C#

Инициализация Harmony выполняется при загрузке сборок игры в классе со статическим конструктором и атрибутом `[StaticConstructorOnStartup]`.

```csharp
using HarmonyLib;
using Verse;

namespace MyMod
{
    [StaticConstructorOnStartup]
    public static class MyModInitializer
    {
        static MyModInitializer()
        {
            var harmony = new Harmony("com.mymod.rimworld");
            harmony.PatchAll(); // Автоматически применяет все классы с атрибутами [HarmonyPatch]
            Log.Message("[MyMod] Harmony patches applied successfully.");
        }
    }
}
```

---

## 2. Основные виды патчей

### `Prefix` (Выполняется перед оригинальным методом)
Позволяет перехватить входящие аргументы или вовсе отменить выполнение оригинального метода (если вернуть `false`).

```csharp
using HarmonyLib;
using Verse;

namespace MyMod
{
    [HarmonyPatch(typeof(Pawn), nameof(Pawn.PreApplyDamage))]
    public static class Patch_Pawn_PreApplyDamage
    {
        public static bool Prefix(Pawn __instance, ref DamageInfo dinfo, out bool absorbed)
        {
            absorbed = false;
            // Если пешка неуязвима, отменяем нанесение урона
            if (__instance.def.defName == "SuperPawn")
            {
                absorbed = true;
                return false; // Отменить оригинальный метод
            }
            return true; // Выполнить оригинальный метод
        }
    }
}
```

### `Postfix` (Выполняется после оригинального метода)
Позволяет изменить возвращаемое значение метода (`ref __result`) или произвести дополнительные действия.

```csharp
using HarmonyLib;
using Verse;

namespace MyMod
{
    [HarmonyPatch(typeof(Building_TurretGun), "CanSetFaction")]
    public static class Patch_Turret_CanSetFaction
    {
        public static void Postfix(Building_TurretGun __instance, ref bool __result)
        {
            // Разрешить переустановку фракции турелей
            __result = true;
        }
    }
}
```

---

## 3. Специальные аргументы Harmony

| Аргумент | Описание |
|---|---|
| `__instance` | Ссылка на экземпляр класса, для которого вызывается метод. |
| `__result` | Ссылка на возвращаемое оригинальным методом значение (в `Postfix`). |
| `__state` | Переменная для передачи данных из `Prefix` в `Postfix`. |
| `___privateField` | Доступ к приватным полям класса (три символа подчеркивания). |
