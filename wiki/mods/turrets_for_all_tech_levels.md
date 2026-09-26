# Анализ мода: Turrets For All Tech Levels

## 1. Основные сведения
* **Название мода**: Turrets For All Tech Levels (в запросе: *Turrets For All Post-Medieval Tech Levels*)
* **PackageId**: `bambaryla.AllBambaTurrets`
* **Steam Workshop ID**: `3800286808`
* **Авторы**: Bambaryla, Haderoth, Samael Gray
* **Поддерживаемые версии игры**: `1.6` (Анализировалась версия `1.6`)

---

## 2. Цель мода
Мод **Turrets For All Tech Levels** является четвертой частью серии модов «All-Bamba». Он добавляет специализированные стационарные турели, тяжелую артиллерию, ракетные турели и EMP-пулеметы для всех постнормандских/электрических технологических уровней:

1. **Промышленный уровень (Industrial)**:
   * Промышленный пулеметный турельный комплекс (`ABT_Turret_Industrial_Gun`).
   * Ракетная турель (`ABT_Turret_Industrial_Rocket`).
   * Тяжелая промышленная артиллерия (`ABT_Turret_Industrial_Artillery`).
   * Промышленный EMP-пулемет (`ABT_Turret_Industrial_EMP`).
2. **Космический уровень (Spacer)**:
   * Космическая пулеметная турель (`ABT_Turret_Spacer_Gun`).
   * Космическая ракетная турель (`ABT_Turret_Spacer_Rocket`).
   * Космическая осадная артиллерия (`ABT_Turret_Spacer_Artillery`).
   * Космическая EMP-турель (`ABT_Turret_Spacer_EMP`).
   * Зенитная противовоздушная турель (`ABT_Turret_Spacer_AA` — в режиме Combat Extended).
3. **Ультратехнологичный уровень (Ultra / Advanced Spacer)**:
   * Ультра-пулеметная турель (`ABT_Turret_AdvSpacer_Gun`).
   * Ультра-ракетная турель (`ABT_Turret_AdvSpacer_Rocket`).
   * Ультра-тяжелая артиллерия (`ABT_Turret_AdvSpacer_Artillery`).
   * Ультра-EMP турель (`ABT_Turret_AdvSpacer_EMP`).

---

## 3. Зависимости
* **Обязательные моды / DLC**: Отсутствуют (работает автономно).
* **Опциональный порядок загрузки (`loadAfter`)**:
  * `CETeam.CombatExtended` (*Combat Extended*)
  * `issaczhuang.muzzleflash` (*Muzzle Flash*)

---

## 4. Взаимодействие с ванильными DLC
* **Расширение ванильных классов**:
  * Расширяет `Building_TurretGun`, `ThingDef` построек и снарядов, `ResearchProjectDef`.
  * Добавляет оборонительные постройки в категорию `Security`.
* **Исследования**:
  * Добавляет проекты исследований `ABT_Turrets_Industrial`, `ABT_Turrets_Spacer`, `ABT_Turrets_AdvancedSpacer`.

---

## 5. Взаимодействие со сторонними библиотеками
C#-код в виде отдельного DLL отсутствует. Мод использует сложную гибридную XML-архитектуру управления папками **`LoadFolders.xml`**:

* **Ванильный режим (Vanilla)**:
  * Загружает `1.6/Vanilla/Defs/ThingDefs_Buildings/` — турели функционируют на основе ванильной логики `Building_TurretGun`.
* **Режим Combat Extended (CE)**:
  * Если активирован мод Combat Extended, `LoadFolders.xml` переключает загрузку на `1.6/CE/Defs/ThingDefs_Buildings/`. Турели переходят на использование боеприпасов CE, прямой баллистики и добавляют специализированную зенитную противовоздушную турель.
* **Интеграция с Muzzle Flash**:
  * Патч `1.6/MuzzleFlash/Patches/MuzzleFlash.xml` динамически привязывает уникальные визуальные эффекты дульного пламени при стрельбе турелей.
* **Интеграция с TurretPipeline / Reel**:
  * Патч `1.6/TurretPipeline/Patches/Reel.xml` обеспечивает анимации вращения и подачу лент.

---

## 6. Поддержка и статус русификации
* **Наличие в Steam Workshop**: Да (`294100/3800286808`).
* **Встроенный перевод**: Английский (`English`).
* **Требуемый метод русификации**: XML-локализация (`DefInjected` + `Keyed`).
* **Статус**: Согласно правилам, по умолчанию файлы в `russian/` не создавались. На данный момент отдельного мода-русификатора в Workshop не обнаружено.

---

## 7. Дополнительная важная информация
* **Дизайн и вдохновение**:
  * Внешний вид турелей вдохновлен реальным военным вооружением, игрой Танки Онлайн, а космические турели — игрой *Helldivers 2*.
* **Автоматическое переключение режимов**:
  * Использование файла `LoadFolders.xml` избавляет игрока от необходимости ставить патчи вручную: мод сам определяет наличие Combat Extended и Muzzle Flash.

---

## 8. Примечания и технические данные
* **Управление папками `LoadFolders.xml`**:
  * Динамическая загрузка `1.6/Vanilla`, `1.6/CE`, `1.6/MuzzleFlash`, `1.6/TurretPipeline`.
* **Ключевые ThingDefs турелей**:
  * Industrial: `ABT_Turret_Industrial_Gun`, `ABT_Turret_Industrial_Rocket`, `ABT_Turret_Industrial_Artillery`, `ABT_Turret_Industrial_EMP`
  * Spacer: `ABT_Turret_Spacer_Gun`, `ABT_Turret_Spacer_Rocket`, `ABT_Turret_Spacer_Artillery`, `ABT_Turret_Spacer_EMP`, `ABT_Turret_Spacer_AA`
  * AdvSpacer: `ABT_Turret_AdvSpacer_Gun`, `ABT_Turret_AdvSpacer_Rocket`, `ABT_Turret_AdvSpacer_Artillery`, `ABT_Turret_AdvSpacer_EMP`
