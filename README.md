# Dungeon Crawler

This is my own Unity game, that I am still working on. I made it so I can explore design patterns in coding.  

The enemies (Skeletons, Mages and Dragons) are controlled by my own ***Finite State Machine (FSM)*** with a shared ***Blackboard***. With it, an enemy can react to the player and move between different states, and the Dragon even has two phases in its fight.

The systems in the game don't depend on each other directly. They talk through a global ***EventBus***, so XP, quests, loot, UI and spawners just listen to the events they need. All the game data, like enemies, items and damage types, is made with ***ScriptableObjects***. The UI uses the ***Observer pattern*** to update automatically when something happens to the player or an enemy, and the inventory uses the ***Factory pattern*** for using items.

## Preview
<img src="https://github.com/hristodinkov/Dungeon_crawler_project/blob/main/dungeon_enemies.gif" width="600" height="600">
<img src="https://github.com/hristodinkov/Dungeon_crawler_project/blob/main/DragonShowcase.gif" width="600" height="600">

## Design patterns that I used in this project

### 1. Finite State Machine (FSM) 
A modular system that drives all enemy behavior.
Each enemy (Skeleton, Mage, Dragon) is controlled by a dedicated FSM composed of States, Transitions, and Enter/Exit/Step hooks.
The FSM supports:

- Clear separation of behaviors

- Reusable state logic

- Predictable transitions

### 2. Hierarchical FSM (Dragon Boss)
The Dragon uses a multi‑layered FSM for its' 2 phases:

- Phase 1 FSM (ground melee behavior)

- Phase 2 FSM (airborne fireball loop)

This structure allows complex boss behavior while keeping each phase isolated and maintainable.

### 3. Blackboard Pattern
A shared data container passed to all states.
It stores:

- Transforms

- NavMeshAgent

- Animator

- Attack colliders

- Projectile prefabs

- Ranges, cooldowns, rotate speed

- Dragon‑specific fields

The Blackboard removes the need for states to hold their own references, reducing duplication and coupling.

### 4. EventBus Pattern (Global Events)
EnemyEventBus.OnEnemyDied broadcasts enemy death events to the entire game.
Systems listening to this event:

- XPManager (awards XP)

- QuestManager (updates objectives)

- EnemySpawner (respawns enemies)

- Loot system (drops items)

- UI (updates counters)

### 5. Observer Pattern (Player & Enemy UI)
UI and visual feedback react to gameplay events without referencing gameplay logic directly.

**EnemyObserver handles:**

- HP bar updates

- Hurt flash

- Death reactions

**PlayerObserver handles:**

- HP bar updates

- Vignette hurt effect

- Heal events

Observers subscribe/unsubscribe automatically on enable/disable.

### 6. Factory Pattern (Item Creation & Usage)
Two factory systems are used:

**Item Factory**
ItemData.CreateItem() generates runtime item instances from ScriptableObjects.

**UseItem Factory**
UseItemCreator.Create(item) returns:

- ConsumeItem

- EquipItem

 - Or null

This allows new item behaviors to be added without modifying existing code.

### 7. ScriptableObject Architecture
Used extensively for data‑driven design:

- **EnemyData** → Factory for creating Enemy instances

- **ItemData** → Defines item stats, visuals, and behavior

This allows designers to modify gameplay without touching code.

### 8. Event‑Driven Progression (XP & Quests)
Progression systems react to events rather than polling:

- XP increases when enemies die

- Level‑ups adjust stats and UI

- Quests update automatically

- Doors open when objectives complete

This keeps gameplay logic clean and reactive.

## Fututre Improvements
- Change placeholder assets
- Sync the animation and attack cycles
- Combine this project with my dungeon generation project(https://github.com/hristodinkov/DungeonGenerator)
