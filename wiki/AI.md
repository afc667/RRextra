# 🧠 Artificial Intelligence (AI.md)

VillagerRecruits replaces vanilla entity brains with a custom goal-based system optimized for military coordination and performance.

## 1. Goal Priority List (`goalSelector`)
Recruits follow a tiered priority system where lower numbers have higher precedence.

| Priority | Goal Class | Description |
| :--- | :--- | :--- |
| **0** | `RecruitFloatGoal` | Standard water buoyancy logic. |
| **0** | `RecruitEatGoal` | Triggers when hunger is low; searches inventory for food. |
| **1** | `RecruitQuaffGoal` | Automatically drinks beneficial potions from inventory. |
| **1** | `FleeTNT` / `FleeFire` | Emergency avoidance logic. |
| **1** | `RecruitProtectEntityGoal`| Moves to and defends a specific entity (owner/squad member). |
| **2** | `RecruitFollowOwnerGoal` | Standard movement logic to stay near the player. |
| **2** | `RecruitMeleeAttackGoal` | Combat logic for sword and axe units. |
| **3** | `RecruitMountEntity` | Logic for entering horses, boats, or minecarts. |
| **3** | `RecruitDismountEntity` | Logic for exiting vehicles when ordered. |
| **3** | `RecruitMoveToPosGoal` | Pathfinding to a specific coordinate marked on the map. |
| **3** | `RecruitHoldPosGoal` | Stationed defense within a defined radius. |
| **4** | `BlockWithWeapon` | Tactical use of job blocks as cover or vantage points. |
| **4** | `RestGoal` | Idle behavior for recovering morale. |
| **5** | `RecruitUpkeepPosGoal` | Guards a specific supply chest and resupplies from it. |
| **6** | `RecruitUpkeepEntityGoal` | Guarding a moving objective (e.g., a wagon). |
| **10** | `RecruitWanderGoal` | Standard idle wandering when no orders are active. |

## 2. Target Selection Logic (`targetSelector`)
Recruits use a specialized targeting system to distinguish between allies and enemies.

1.  **`RecruitProtectHurtByTargetGoal` (1)**: Attacks anything that hurts the entity they are protecting.
2.  **`RecruitOwnerHurtByTargetGoal` (2)**: Attacks anything that hurts their owner.
3.  **`RecruitHurtByTargetGoal` (3)**: Self-defense; also "alerts others" in the squad to the attacker.
4.  **`RecruitOwnerHurtTargetGoal` (4)**: Assists the owner in attacking a target.
5.  **`RecruitDefendVillageFromPlayerGoal` (7)**: Standard iron-golem-like village defense logic.

## 3. High-Performance Target Finding
Target finding is performed in `searchForTargets()`, which supports two modes:

### ⚡ Synchronous Mode
Standard single-threaded search. It inflates a 40-block `AABB`, retrieves all `LivingEntity` instances, and filters them using `TargetingConditions`.

### 🚀 Asynchronous Mode (Multi-threaded)
*   **Trigger:** Controlled by `UseAsyncTargetFinding` in config.
*   **Execution:** Offloads the sorting and filtering of entities to a separate thread pool (`AsyncManager.executor`).
*   **Efficiency:** Prevents "TPS spikes" when 100+ recruits are searching for targets simultaneously.

## 4. Custom Navigation
All recruits use `RecruitPathNavigation`, which inherits from `AsyncPathfinder`.
*   **Door Interaction:** `RecruitsOpenDoorGoal` allows recruits to use doors in friendly chunks.
*   **Path Calculation:** Can be offloaded to threads via `UseAsyncPathfinding`.
*   **Verticality:** Recruits have optimized logic for climbing stairs and navigating multi-level fortifications.
