# Контекстное меню и задачи (FloatMenuOptionProvider и Jobs)

В RimWorld пользовательское взаимодействие с объектами и пешками строится на контекстном меню (правый клик мыши) и системе задач пешек (`JobDef` и `JobDriver`).

---

## 1. Добавление пунктов в меню правого клика (`FloatMenuOptionProvider`)

В версиях RimWorld [1.5+] добавление пунктов в контекстное меню для пешки выполняется через реализацию интерфейса `FloatMenuOptionProvider` или Harmony-патч на метод `FloatMenuMakerMap.ChoicesAtFor`.

```csharp
using System.Collections.Generic;
using UnityEngine;
using Verse;
using Verse.AI;

namespace MyMod
{
    public class FloatMenuOptionProvider_MendItem : FloatMenuOptionProvider
    {
        public override IEnumerable<FloatMenuOption> GetOptions(Pawn pawn, Vector3 clickPos)
        {
            foreach (LocalTargetInfo target in GenUI.TargetsAt(clickPos, TargetingParameters.ForBuilding(), true))
            {
                Thing building = target.Thing;
                if (building != null && building.HitPoints < building.MaxHitPoints)
                {
                    yield return new FloatMenuOption($"Ремонтировать {building.Label}", () =>
                    {
                        Job job = JobMaker.MakeJob(MyModJobDefs.MendBuildingJob, building);
                        pawn.jobs.TryTakeOrderedJob(job);
                    });
                }
            }
        }
    }
}
```

---

## 2. Создание работы и драйвера задачи (`JobDef` и `JobDriver`)

### Шаг 1: `JobDef` в XML
```xml
<JobDef>
  <defName>MyMod_MendBuildingJob</defName>
  <driverClass>MyMod.JobDriver_MendBuilding</driverClass>
  <reportString>repairing TargetA.</reportString>
  <allowSaveableEquipToFields>true</allowSaveableEquipToFields>
</JobDef>
```

### Шаг 2: Реализация `JobDriver` в C#
`JobDriver` описывает последовательность действий (тоита / Toils), которые пешка выполняет для достижения цели.

```csharp
using System.Collections.Generic;
using Verse;
using Verse.AI;

namespace MyMod
{
    public class JobDriver_MendBuilding : JobDriver
    {
        private Thing TargetBuilding => TargetA.Thing;

        public override bool TryMakePreToilReservations(bool errorOnFailed)
        {
            return pawn.Reserve(TargetBuilding, job, 1, -1, null, errorOnFailed);
        }

        protected override IEnumerable<Toil> MakeNewToils()
        {
            // Шаг 1: Подойти к зданию
            yield return Toils_Goto.GotoThing(TargetIndex.A, PathEndMode.Touch);

            // Шаг 2: Выполнять ремонт в течение определенного времени
            Toil mendToil = ToilMaker.MakeToil("MendToil");
            mendToil.defaultCompleteMode = ToilCompleteMode.Never;
            mendToil.initAction = () =>
            {
                ticksLeftThisToil = 300; // 5 секунд
            };
            mendToil.tickAction = () =>
            {
                TargetBuilding.HitPoints = System.Math.Min(TargetBuilding.MaxHitPoints, TargetBuilding.HitPoints + 1);
                if (TargetBuilding.HitPoints >= TargetBuilding.MaxHitPoints)
                {
                    ReadyForNextToil();
                }
            };
            mendToil.WithEffect(EffecterDefOf.ConstructMetal, TargetIndex.A);
            mendToil.WithProgressBar(TargetIndex.A, () => (float)TargetBuilding.HitPoints / TargetBuilding.MaxHitPoints);
            yield return mendToil;
        }
    }
}
```
