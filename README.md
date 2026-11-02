# Dungeon Crawler

This project is a modular, event‑driven gameplay framework built in Unity, designed to demonstrate clean architecture and scalable AI behavior. It uses a fully extensible ***Finite State Machine (FSM)*** system combined with a shared ***Blackboard*** to drive intelligent enemy behavior, enabling Skeletons, Mages, and Dragons to react dynamically and transition through complex, multi‑phase combat patterns. Core gameplay systems communicate through a global ***EventBus***, allowing XP, quests, loot, UI, and spawners to respond to events without direct dependencies. All gameplay data like enemies, items, and damage types is defined through ***ScriptableObjects***, while UI and visual feedback rely on the ***Observer pattern*** to react automatically to player and enemy events. The inventory system uses ***Factory*** pattern for item usage.

## Preview
<img src="dungeon.png" width="600" height="600">

## Design patterns breakdown

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
- Combine this project with my dungeon generation project(https://github.com/hristodinkov/DungeonGenerator)
