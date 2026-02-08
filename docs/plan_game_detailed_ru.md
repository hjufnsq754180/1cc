# ПЛАН РАЗРАБОТКИ НАШЕЙ ИГРЫ (ДЕТАЛЬНЫЙ, ПОЛНЫЙ)

Документ на русском языке. Unity 3D (FPP). Включает ВСЕ текущие детали игры:
ресурсы, рецепты, здания, исследования, волны, оборону, ратушу.

Формат: 
- ЧТО ДЕЛАЕМ
- КАКИЕ СИСТЕМЫ И СКРИПТЫ СОЗДАТЬ
- КАКИЕ ДАННЫЕ/АРТЕФАКТЫ ПОЛУЧАЕМ

Все цифры рецептов/таймингов берём из `docs/balance.csv`.

---

## 0) БАЗОВЫЕ ПРАВИЛА КОДА (ОБЯЗАТЕЛЬНО)
- Один класс = одна ответственность.
- Системы общаются через события.
- Данные в ScriptableObjects.
- Нет магических чисел/строк (все в конфигах).
- Кэширование компонентов, отписка от событий в OnDestroy.
- Правильные модификаторы доступа.

---

## 1) СПИСОК ВСЕХ РЕСУРСОВ (ПО ТИРАМ)

### T1 (база)
- Grave_Mix (смесь останков)
- Bone, Flesh, Cloth, Metal, Ash
- Grave_Stone, Chalk
- Bone_Powder, Tallow
- Stone_Block
- Energy_NE (виртуальный ресурс)

### T2 (средний)
- Crypt_Mix -> Hardware, Wax, Crypt_Dust
- Bonfire_Mix -> Char_Bone, Coal_Crumb, Ash
- Junk_Mix -> Rag, Utensil, Parchment
- Impregnated_Cloth
- Ritual_Gear
- Stabilizer
- Conveyor
- Carrier

### T3 (поздний)
- Cursed_Artifact
- Necro_Essence
- Component

### T4 (финал)
- Pure_Stabilizer
- Final (результат постройки Некро‑машины)

---

## 2) ВСЕ ЗАЛЕЖИ И ДОБЫЧА

### Узлы добычи (Node)
- Grave_Node (могилы/гробы)
- Crypt_Node (склеп‑кладовая)
- Bonfire_Node (останки кострища)
- Shed_Node (склад утвари)
- Quarry_Node (камень)
- Ruins_Node (руины/склепы для артефактов)

### Добывающие машины
1) Grave_Drill -> Grave_Mix x3 (10s)
2) Crypt_Dismantler -> Crypt_Mix x3 (10s)
3) Bonfire_Sieve -> Bonfire_Mix x3 (8s)
4) Shed_Dismantler -> Junk_Mix x3 (9s)
5) Quarry_Drill -> Grave_Stone x2 + Chalk x1 (10s)
6) Ruins_Excavator -> Cursed_Artifact x1 (20s)

---

## 3) ВСЕ РЕЦЕПТЫ ПРОИЗВОДСТВА (КРАТКО, ССЫЛКА НА CSV)

### Сортировка
- Sort_Grave_Mix: Grave_Mix x6 -> Bone x12, Flesh x12, Cloth x6, Metal x3, Ash x3 (6s)
- Sort_Crypt_Mix: Crypt_Mix x6 -> Hardware x6, Wax x6, Crypt_Dust x3 (6s)
- Sort_Bonfire_Mix: Bonfire_Mix x6 -> Char_Bone x6, Coal_Crumb x6, Ash x9 (6s)
- Sort_Junk_Mix: Junk_Mix x6 -> Rag x9, Utensil x3, Parchment x3 (6s)

### Переработка
- Bone_Crusher: Bone x12 -> Bone_Powder x9 (4s)
- Corpse_Vat: Flesh x12 -> Tallow x6 (6s)
- Impregnation_Cauldron: Cloth x6 + Tallow x3 -> Impregnated_Cloth x6 (5s)
- Sarcophagus_Tallow: Tallow x3 -> Energy_NE x90 (3s)
- Sarcophagus_Wax: Wax x1 -> Energy_NE x20 (3s)
- Sarcophagus_Coal: Coal_Crumb x2 -> Energy_NE x20 (3s)
- Stonecutter: Grave_Stone x2 -> Stone_Block x1 (6s)

### Ритуалы и T3
- Ritual_Press: Bone_Powder x9 + Metal x3 -> Ritual_Gear x3 (8s)
- Necro_Stabilizer: Bone_Powder x9 + Ash x3 -> Stabilizer x3 (6s)
- Alchemy_Lab: Ash x6 + Cursed_Artifact x1 -> Necro_Essence x3 (12s)
- Purification_Circle: Stabilizer x3 + Ash x2 + Crypt_Dust x1 + Wax x1 -> Pure_Stabilizer x1 (12s)
- Purification_Circle_Alt: Stabilizer x3 + Necro_Essence x1 + Crypt_Dust x1 -> Pure_Stabilizer x2 (16s)

### Логистика
- Conveyor_Workshop: Impregnated_Cloth x6 + Wood x6 -> Conveyor x3 (6s)
- Carrier_Workshop: Bone_Powder x2 + Impregnated_Cloth x1 -> Carrier x1 (12s)

### Финальные сборки
- Ritual_Assembly: Ritual_Gear x6 + Necro_Essence x3 + Stabilizer x3 -> Component x3 (10s)
- Necro_Machine: Component x30 + Stabilizer x30 + Necro_Essence x15 -> Final x1 (60s)

Все значения и output/час берём из `balance.csv`.

---

## 4) РАТУША (ГЛАВНЫЙ ГЕЙТ)

### Уровни:
1) TownHall_Ruins (старт)
2) TownHall_Level_I (1 час)
3) TownHall_Level_II (3 часа)
4) TownHall_Level_III (6 часов)
5) TownHall_Level_IV (10 часов)

### Апгрейды (цены/время в balance.csv):
- TownHall_I_Upgrade
- TownHall_II_Upgrade
- TownHall_III_Upgrade
- TownHall_IV_Upgrade

Ратуша блокирует доступ к исследованиям по тирам.

---

## 5) ИССЛЕДОВАНИЯ (ПОЛНЫЙ СПИСОК)

### Tier 0 (старт)
- Research_Basics
- Research_Stonework
- Research_Basic_Storage

### Tier 1
- Research_Conveyors_I
- Research_Defense_I
- Research_Crypt_Survey
- Research_Bonfire_Survey
- Research_Junk_Survey

### Tier 2
- Research_Impregnation
- Research_Ritual_Press
- Research_Stabilization
- Research_Storage_Priorities
- Research_Defense_II
- Research_Carriers

### Tier 3
- Research_Alchemy
- Research_Purification
- Research_Logistics_Dispatcher
- Research_Defense_III

### Tier 4
- Research_Necro_Machine
- Research_Defense_IV
- Research_MkIII

Все цены/время и требуемый уровень Ратуши — в `balance.csv`.

---

## 6) ОБОРОНА (TOWER DEFENCE)

### Структуры:
- Palisade
- Bone_Spikes
- Bell (сигнал)
- Bone_Ballista
- Ash_Cannon
- Tar_Trap
- Stone_Wall
- Ritual_Turret
- Ward_Dome
- Heavy_Turrets
- Reinforced_Gates

### Волны:
- Первые волны стартуют после 2 часов.
- Далее интервалы 8–20 минут (см. баланс волны в дизайне).

### Враги:
Мародер, Крестьянин, Гробокопатель, Стражник, Охотник, Факельщик, Инквизитор, Телега‑таран.

---

## 7) ВСЕ СИСТЕМЫ И СКРИПТЫ (ПОДРОБНО)

### 7.1 Data (ScriptableObjects)
Создать SO:
- ResourceConfig (id, name, icon, stackSize, tier)
- RecipeConfig (inputs, outputs, time, energy)
- BuildingConfig (id, size, ports, requiredResearch, allowedNodes)
- ResearchConfig (id, cost, time, requiredTownHallLevel)
- EnemyConfig (hp, speed, damage, special)
- WaveConfig (time, threat, composition)

### 7.2 Core Systems
- ServiceLocator
- EventBus (GameEventBus)
- TimeService (игровое время/скорость)

### 7.3 Player
- PlayerController (FPP)
- PlayerInteraction (raycast)
- PlayerInventory (ограниченные слоты)

### 7.4 Placement/Building
- BuildingPlacer
- BuildingGhost
- PlacementValidator
- Building (base class)
- BuildingPort (вход/выход)
- NodeAnchor (где можно ставить добывающие)

### 7.5 Production
- ProductionMachine (общее поведение)
- RecipeRunner (таймер циклов)
- InputBuffer / OutputBuffer
- PowerConsumer (проверка энергии)

### 7.6 Logistics
- ConveyorBelt
- ConveyorItem
- Splitter
- Filter
- StorageContainer
- StorageMode (enum: Storage/Provider/Requester/Buffer/Overflow)
- StoragePriority (in/out)
- LogisticsDispatcher (авто‑балансировщик)
- Carrier (скелет‑носильщик)
- CarrierController (поведение переноски)

### 7.7 Energy
- PowerNetwork
- PowerProvider (Sarcophagus)
- PowerConsumer (машины)
- PowerCable (если используем провода)

### 7.8 Research & Town Hall
- TownHall (уровни/апгрейды)
- ResearchSystem
- TechTreeUI

### 7.9 Defense & Waves
- WaveManager
- SpawnPoint
- EnemyAI (базовый)
- TurretBase
- Projectile
- Wall/Barrier

### 7.10 UI
- BuildMenuUI
- InventoryUI
- StorageUI
- MachineUI (progress)
- ResearchUI
- TownHallUI
- WaveWarningUI

### 7.11 Save/Load
- SaveSystem
- SaveData (версии)

---

## 8) ПЛАН РАЗРАБОТКИ ПО ЭТАПАМ

### ЭТАП A: FOUNDATION (1-2 недели)
ЧТО ДЕЛАЕМ:
- Настраиваем Unity проект.
- Создаем базовые SO (ResourceConfig, RecipeConfig).
- Импортируем `balance.csv`.
СКРИПТЫ:
- ResourceConfigSO, RecipeConfigSO, CSVImporter.
РЕЗУЛЬТАТ:
- Данные доступны в редакторе, можно менять баланс.

### ЭТАП B: PLAYER + INTERACTION
ЧТО ДЕЛАЕМ:
- FPP движение, взаимодействие.
СКРИПТЫ:
- PlayerController, PlayerInteraction.

### ЭТАП C: BUILDING PLACEMENT
ЧТО ДЕЛАЕМ:
- Ghost preview, размещение, проверка.
СКРИПТЫ:
- BuildingPlacer, BuildingGhost, PlacementValidator.

### ЭТАП D: PRODUCTION
ЧТО ДЕЛАЕМ:
- Рецепты работают.
СКРИПТЫ:
- ProductionMachine, RecipeRunner, InputBuffer.

### ЭТАП E: LOGISTICS
ЧТО ДЕЛАЕМ:
- Конвейеры, склады, фильтры.
СКРИПТЫ:
- ConveyorBelt, StorageContainer, Splitter.

### ЭТАП F: ENERGY
ЧТО ДЕЛАЕМ:
- Энергосеть, питание машин.
СКРИПТЫ:
- PowerNetwork, PowerConsumer, PowerProvider.

### ЭТАП G: RESEARCH + TOWN HALL
ЧТО ДЕЛАЕМ:
- Ратуша + исследования.
СКРИПТЫ:
- TownHall, ResearchSystem, TechTreeUI.

### ЭТАП H: DEFENSE + WAVES
ЧТО ДЕЛАЕМ:
- Волны после 2 часов.
СКРИПТЫ:
- WaveManager, SpawnPoint, EnemyAI, TurretBase.

### ЭТАП I: TUTORIAL
ЧТО ДЕЛАЕМ:
- Сценарий первых 30-60 минут.
СКРИПТЫ:
- TutorialManager.

### ЭТАП J: SAVE/LOAD
ЧТО ДЕЛАЕМ:
- Сериализация мира.
СКРИПТЫ:
- SaveSystem, SaveData.

### ЭТАП K: BALANCE ITERATION
ЧТО ДЕЛАЕМ:
- Настраиваем output/час, тайминги, апгрейды Ратуши.
АРТЕФАКТЫ:
- Обновленный `balance.csv` и пресеты сложности.

---

## 9) СТРУКТУРА ПАПОК (ПОД UNITY)

Assets/
  Scripts/
    Core/Interfaces
    Core/Services
    Core/Events
    Gameplay/Characters
    Gameplay/Buildings
    Gameplay/Logistics
    Gameplay/Research
    Gameplay/Defense
    Data/ScriptableObjects
    Data/Configs
    Utilities
  Prefabs/
  Scenes/
  Audio/
  Materials/

---

## 10) КАК МЕНЯТЬ БАЛАНС

- Все числа в ScriptableObjects и CSV.
- CSV импортируется в SO через EditorTool.
- Есть Debug HUD с коэффициентами:
  - GlobalSpeedMultiplier
  - CostMultiplier
  - EnemyWaveMultiplier
- Можем быстро сдвигать 1/3/6/10 часов по Ратуше.

---

## 11) ЧТО ДОЛЖНО БЫТЬ В ПЕРВОМ VERTICAL SLICE

Минимум:
- 1 узел Grave_Node
- 1 Grave_Drill
- 1 Sort_Table
- 1 Bone_Crusher
- 1 Corpse_Vat
- 1 Sarcophagus
- 1 Conveyor
- 1 Storage

Проверка:
- цепочка работает,
- ресурсы ездят,
- игрок может строить и копить.

---

## 12) ЧЕК-ЛИСТ ГОТОВНОСТИ

- Все рецепты из `balance.csv` реализованы.
- Ратуша гейтит исследования.
- Волны стартуют после 2 часов.
- Конвейеры работают, склад копит.
- Апгрейды зданий используют ресурсы.
- Сейвы работают.

---

КОНЕЦ ДОКУМЕНТА.
