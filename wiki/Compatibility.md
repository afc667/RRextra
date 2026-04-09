# 🤝 Compatibility & API (Compatibility.md)

VillagerRecruits is built with modularity in mind, allowing it to integrate deeply with other Forge mods through a custom compatibility layer and Mixins.

## 1. Weapon Integration (`IWeapon.java`)
The `IWeapon` interface allows the mod to support modded weapons (like firearms) without hard-dependency.

### ⚔️ Methods to Implement:
*   **`getWeaponLoadTime()`**: Determines how long a recruit must "wind up" before firing.
*   **`getProjectile(LivingEntity shooter)`**: Defines the custom projectile entity to be spawned.
*   **`performRangedAttackIWeapon()`**: Custom logic for handling the attack cycle.
*   **`isGun()` / `isBow()` / `isCrossBow()`**: Categorizes the weapon for AI behavior adjustments.

### 🔫 MusketMod Support
The mod natively includes implementations for `musketmod` items:
*   `MusketWeapon`: High accuracy, long load time.
*   `BlunderbussWeapon`: Wide spread, multiple projectiles.
*   `PistolWeapon`: Short range, faster fire rate.

---

## 2. Advanced Mod Integrations
*   **SmallShips**: Custom logic in `SmallShips.java` allows `CaptainEntity` to "sail" ships and provides a seat-assignment system for large squads.
*   **Corpse Mod**: When enabled, recruits leave a lootable corpse on death instead of simply dropping items as entities.
*   **Epic Knights**: Includes specific weapon and armor attribute mappings to ensure recruits use `magistuarmory` gear effectively.

---

## 3. Sponge Mixins (`mixin/`)
Mixins are used to inject recruit-related logic into vanilla Minecraft classes.

| Mixin | Target Class | Purpose |
| :--- | :--- | :--- |
| **`MobMixin`** | `Mob` | Injects custom target-finding logic to ensure vanilla mobs interact correctly with the faction system. |
| **`AbstractHorseMixin`** | `AbstractHorse` | Allows recruits to control and steer horses during the `HorsemanAttackAI` cycle. |
| **`LivingEntityMixin`** | `LivingEntity` | Handles custom damage calculations, including morale-based resistance and vulnerability. |
| **`WalkNodeEvaluatorMixin`** | `WalkNodeEvaluator`| Optimizes pathfinding nodes for recruits, allowing them to better understand "dangerous" areas (like fire or falling blocks). |
| **`RestrictSunGoalMixin`** | `RestrictSunGoal` | Prevents recruits from automatically seeking shade like vanilla villagers, allowing for daytime military operations. |

---

## 4. API for Addons
While the mod doesn't have a separate `api.jar`, developers can extend it by:
1.  **Inheriting from `AbstractRecruitEntity`**: To create new unit types.
2.  **Implementing `IWeapon`**: To add custom ranged weaponry support.
3.  **Listening to `RecruitEvent`**: Standard Forge events are fired for `Hired`, `Dismissed`, and `Promoted` actions.
