# Анализ мода: Archotech Weaponry (Continued)

## 1. Основные сведения
* **Название мода**: Archotech Weaponry (Continued)
* **PackageId**: `zal.archotechweaponry`
* **Steam Workshop ID**: `3312021019`
* **Авторы**: Zaljerem (продолжение), Edern, Sir Van, Ythern, Scorpio (оригинальная команда)
* **Поддерживаемые версии игры**: `1.5`, `1.6` (Анализировалась версия `1.6`)

---

## 2. Цель мода
Мод **Archotech Weaponry** добавляет ультимативное архотекское оружие для позднего этапа игры (End-Game / Late-Game). Архотекское оружие не только превосходит стандартное огнестрельное и холодное оружие по базовым характеристикам урона и пробития брони, но и наделено уникальными артефактными механиками:
* **Архотекское оружие дальнего боя**: Пистолет, винтовка, пистолет-пулемет, легкий пулемет, миниган, дробовик, снайперская винтовка, лук, арбалет и Пусковая установка Пустоты (Void Launcher).
* **Архотекское оружие ближнего боя**: Обычный и тяжелый архотекский меч.
* **Специальные механики**: Наводящиеся снаряды (Homing Projectiles), аннигиляция врагов в прах Пустоты (`VoidAshes`), боевой транс (`BattleTrance`), похищение жизненных сил и уникальные черты связи с личностью (Persona Bladelink Traits).

---

## 3. Зависимости
* **Обязательные моды / библиотеки**:
  * `brrainz.harmony` (*Harmony*)
* **Опциональный порядок загрузки (`loadAfter`)**:
  * `zal.morearchotechgarbage` (*More Archotech Garbage (Continued)*)
* **Совместимость с DLC**:
  * Интегрируется с DLC **Royalty** (связь с личностью, персональное оружие Bladelink).

---

## 4. Взаимодействие с ванильными DLC
* **Интеграция с DLC Royalty**:
  * Расширяет черты персонального оружия (`CompBladelinkWeapon`) с помощью собственного класса `ArchotechTraitExtension`.
  * Патч `1.6/Patches/Royalty/ThingsDefsMisc/Persona_Weapons_Patches.xml` связывает черты архотекского оружия с ванильными мечами связи.
* **Расширение ванильных классов**:
  * Расширяет `ThingDef` для оружия и грязи, `HediffDef`, `MentalStateDef`, `RulePackDef`, `SoundDef`.
* **Эффекты разрушения и распада**:
  * Убитые архотекским оружием противники могут распадаться в пепел Пустоты (`VoidAshes_A`, `VoidAshes_B`, `VoidAshes_C`).

---

## 5. Взаимодействие со сторонними библиотеками и C#-код
Мод содержит собственную C#-сборку **`1.6/Assemblies/ArchotechWeaponry.dll`** и активно использует **Harmony**.

### C#-Архитектура мода (`ArchotechWeaponry.dll`):

1. **Библиотечные компоненты (`ArchotechWeaponry.Comps`)**:
   * `CompArchotechWeapon`: Главный компонент архотекского оружия. Управляет зарядами, отслеживает убийства, генерирует кастомные Gizmo-кнопки способностей.
   * `CompLimitedUsage`: Ограничение количества выстрелов/использований спец-способностей оружия.
   * `Comp_HomingProjectile`: Превращает снаряды в наводящиеся ракеты/заряды, корригирующие траекторию полета на каждом тике (`Tick`) к цели.
   * `HediffComp_FilthOnDeath`: Спавнит пепел Пустоты при смерти пешки под воздействием архотекских Hediffs.

2. **Модификаторы Defs (`ArchotechWeaponry.Defs.Traits`)**:
   * `PlaguebearerExtension` — черта *Чумной вестник* (заражение болезнями при ударе).
   * `PrecognitionExtension` — черта *Прекогниция* (уклонение от атак и предсказание снарядов).
   * `WrathExtension` — черта *Гнев* (увеличение урона по мере получения урона/ярости).

3. **Психические состояния (`ArchotechWeaponry.MentalState`)**:
   * `MentalState_BattleTrance` — боевой транс, вводящий пешку в состояние нечувствительности к боли и непрерывного боя.

4. **Патчи Harmony (`ArchotechWeaponry.Harmony.Patches`)**:
   * `[HarmonyPatch(typeof(VerbProperties), "AdjustedCooldown")]` (`PatchAdjustedCooldown`) — перерасчет задержки атак на основе качества оружия и черт.
   * `[HarmonyPatch(typeof(CompBladelinkWeapon), "CanAddTrait")]` (`PatchCanAddTrait`) — валидация назначения черт архотекского оружия.
   * `[HarmonyPatch(typeof(Pawn), "GetGizmos")]` (`PatchPawnGizmos`) — внедрение кастомных Gizmo способностей пешкам с архотекским оружием.
   * `[HarmonyPatch(typeof(Pawn), "PreApplyDamage")]` & `[HarmonyPatch(typeof(Pawn), "PostApplyDamage")]` — перехват урона для уклонения (Прекогниция), похищения жизни и аннигиляции Пустоты.
   * `[HarmonyPatch(typeof(Projectile), "Tick")]` & `[HarmonyPatch(typeof(Projectile), "Launch")]` — управление самонаведением снарядов (`Comp_HomingProjectile`).

---

## 6. Добавляемый контент и его характеристики

Мод **Archotech Weaponry (Continued)** добавляет ультимативный арсенал оружия ближнего и дальнего боя, черты персонального оружия Royalty, Hediffs некроза и эффекты аннигиляции:

### 1. Оружие дальнего боя и его характеристики (`ThingDef` Ranged)

| Название оружия | Дальность | Время прицеливания / Коулдаун | Урон / Снаряд | Бронепробитие | Особенности и эффекты |
|---|---|---|---|---|---|
| **AW_ArchotechPistol** | 25 | 0.17 сек. / 1.17 сек. | 8 (очередь x2) | 45% | Малый вес (0.7 кг), высокая точность в упор (90%). Накладывает VoidToxin. |
| **AW_ArchotechSMG** | 28 | 0.67 сек. / 1.33 сек. | 11 (очередь x4) | 45% | Скорострельное пистолет-пулеметное орудие. |
| **AW_ArchotechShotgun** | 20 | 0.75 сек. / 0.83 сек. | 20 (двойной залп) | 50% | Останавливающее действие (3.5). Разрывает цели на ближней дистанции. |
| **AW_ArchotechRifle** | 35 | 1.67 сек. / 1.33 сек. | 25 (одиночный) | 35% | Высокая точность на средней и дальней дистанции (85%). |
| **AW_ArchotechLMG** | 30 | 1.58 сек. / 1.83 сек. | 8 (очередь x16) | 45% | Легкий пулемет с высокой плотностью огня. |
| **AW_ArchotechMinigun** | 34 | 2.33 сек. / 2.33 сек. | 8 (очередь x35) | 35% | Огромный шквал огня. Накладывает VoidToxin и VoidNecrosis. |
| **AW_ArchotechSniper** | 47 | 2.33 сек. / 1.83 сек. | 25 (очередь x2) | 45% | Предельная точность стрельбы (95% на 47 клеток). |
| **AW_ArchotechBow** | 23 | 1.17 сек. / 2.33 сек. | 20 (стрела) | 70% | Стрелы Пустоты с высоким бронепробитием. |
| **AW_ArchotechCrossbow**| 26 | 2.67 сек. / 3.17 сек. | 25 (болт) | 80% | Сверхвысокое бронепробитие брони и щитов. |
| **AW_ArchotechVoidlauncher**| 150 | 5.00 сек. / 2.33 сек. | 15 (Voidball) | 20% | **Самонаводящийся снаряд (`Comp_HomingProjectile`)**. Ограниченный запас зарядов (6). Накладывает максимальный тяжелый некроз (1.0). |

### 2. Оружие ближнего боя (`ThingDef` Melee)

* **`AW_ArchotechSword` (Архотекский меч)**:
  * **Урон**: Рубящий 25 / Колющий 25. Задержка атак 1.7 сек.
  * **Бронепробитие (`MeleeArmorPenetration`)**: 95%.
  * **Эффекты**: Накладывает нелетальный `VoidToxin` (0.15 за удар) и летальный `VoidNecrosisTemp` (0.10 за удар).
* **`AW_ArchotechHeavySword` (Тяжелый архотекский меч)**:
  * **Урон**: Рубящий 35 / Колющий 35. Задержка атак 3.0 сек.
  * **Бронепробитие**: 95%.
  * **Эффекты**: Усиленный некроз (0.25 за удар).

### 3. Черты персонального оружия Royalty DLC (`WeaponTraitDef`)

* **`ArchotechPlaguebearer` (Чумоносец)**: Наносит дополнительное заражение чумой и болезнями при попадании (`extraSeverityOnHit = 0.25`).
* **`ArchotechPrecognition` (Предвидение)**: Дает $+8$ к уклонению в ближнем бою и $+3$ к шансу попадания. 5% шанс полной отмены урона.
* **`ArchotechSensitivity` (Чувствительность)**: $+80\%$ к пси-чувствительности и $+8$ к максимальному лимиту энтропии.
* **`ArchotechWrath` (Гнев)**: Накапливает ярость во время боя, вызывая ментальное состояние боевого транса (`MentalState_BattleTrance`).

### 4. Грязь и пепел Пустоты (`ThingDef` Filth)

* **`AW_Filth_VoidAshes` (Пепел Пустоты)**:
  * Спавнится C#-компонентом `HediffComp_FilthOnDeath` в месте погибшей от некроза пешки.
  * Влияние на окружение: Красота $-12$, Чистота $-15$. Работа по уборке: 70.

---

## 7. Поддержка и статус русификации
* **Наличие в Steam Workshop**: Да (`294100/3312021019`).
* **Встроенный перевод**: Английский (`English`).
* **Существующий перевод в Workshop**: Имеется отдельный мод-русификатор **`3595122671`** (*Archotech Weaponry (Continued) - Русификатор* от автора *Dmitry6*).
* **Требуемый метод русификации**: XML-локализация (`DefInjected`). C#-пересборка не требуется.
* **Статус**: Согласно правилам, по умолчанию файлы в `russian/` не создавались. В случае запроса пользователя перевод из `3595122671` может быть скопирован в `russian/3312021019/`.

---

## 8. Дополнительная важная информация
* **Баланс**: Предметы архотекского оружия представляют собой ультимативный Late-Game контент. Они невероятно сильны, обходят стандартную броню и требуют значительных затрат ресурсов при крафте в сочетании с модом *More Archotech Garbage*.
* **Стабильность**: Сборка адаптирована автором Zaljerem под RimWorld 1.6 и 1.5, ошибок при инициализации Harmony не возникает.

---

## 9. Примечания и технические данные
* **Идентификатор Harmony**: `"com.edern.archotechweaponry"`
* **C#-Классы**:
  * `ArchotechWeaponry.Comps.CompArchotechWeapon`
  * `ArchotechWeaponry.Comps.Comp_HomingProjectile`
  * `ArchotechWeaponry.Comps.CompLimitedUsage`
  * `ArchotechWeaponry.Comps.HediffComp_FilthOnDeath`
  * `ArchotechWeaponry.Defs.Traits.PlaguebearerExtension`
  * `ArchotechWeaponry.Defs.Traits.PrecognitionExtension`
  * `ArchotechWeaponry.Defs.Traits.WrathExtension`
  * `ArchotechWeaponry.MentalState.MentalState_BattleTrance`
* **Ключевые ThingDefs дальнего боя**: `AW_ArchotechPistol`, `AW_ArchotechRifle`, `AW_ArchotechShotgun`, `AW_ArchotechSniperRifle`, `AW_ArchotechMinigun`, `AW_ArchotechBow`, `AW_ArchotechVoidLauncher`
* **Ключевые ThingDefs ближнего боя**: `AW_ArchotechSword`, `AW_ArchotechHeavysword`
* **Звуковые эффекты**: `Arc_Bow.ogg`, `Arc_Gun.ogg`, `Arc_Shotgun.ogg`, `Arc_Sniper.ogg`, `VoidLauncher.ogg`
