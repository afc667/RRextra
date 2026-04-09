# 🏃‍♂️ Entity Framework (Entities.md)

VillagerRecruits uses a sophisticated entity hierarchy to handle inventory, equipment synchronization, multi-threaded pathfinding, and a robust state machine.

## 1. Class Hierarchy
`PathfinderMob` → `AsyncPathfinderMob` → `AbstractInventoryEntity` → `AbstractRecruitEntity` → `RecruitEntity`

*   **`AsyncPathfinderMob`**: Overrides standard navigation to support off-thread pathfinding calculations, critical for performance in large-scale battles.
*   **`AbstractInventoryEntity`**: Adds a 15-slot internal container and equipment sync logic.
*   **`AbstractRecruitEntity`**: The core "Recruit" logic, including orders, levels, and survival systems.

## 2. Data Synchronization (`EntityDataAccessor`)
The mod uses `SynchedEntityData` to keep client-side and server-side states in sync.

| Constant | Type | Description |
| :--- | :--- | :--- |
| `STATE` | `Integer` | Aggro Mode: 0=Neutral, 1=Aggressive, 2=Raid, 3=Passive. |
| `FOLLOW_STATE` | `Integer` | Order: 0=Wander, 1=Follow, 2=Hold Pos, 3=Back to Pos, 5=Protect, 6=Work. |
| `OWNER_ID` | `Optional<UUID>` | UUID of the player who hired the recruit. |
| `GROUP` | `Optional<UUID>` | UUID of the `RecruitsGroup` (Squad). |
| `XP` / `LEVEL` | `Integer` | Progression stats for the level-up system. |
| `HUNGER` / `MORAL` | `Float` | Survival mechanics (0.0 to 100.0). |
| `VARIANT` / `COLOR` | `Integer` / `Byte` | Visual appearance (skins and team colors). |
| `LISTEN` | `Boolean` | If false, the recruit ignores all commands. |
| `IS_FOLLOWING` | `Boolean` | True if the recruit is currently actively moving toward its owner. |

## 3. Persistent Data (NBT Structure)
Recruits save a significant amount of data to ensure their state is preserved across world reloads.

### 💾 Key NBT Tags:
*   `Items`: A `ListTag` of the 15-slot inventory.
*   `OwnerUUID` / `Group`: UUIDs for ownership and squad membership.
*   `AggroState` / `FollowState`: Current operational modes.
*   `HoldPosX/Y/Z`: The fixed coordinate for "Hold Position" or "Guard" duty.
*   `UpkeepPosX/Y/Z`: The location of the upkeep/supply chest.
*   `Xp` / `Level` / `Kills`: Tracked combat performance.
*   `Hunger` / `Moral`: Current survival needs.
*   `paymentTimer`: Time remaining until the next salary is due.

## 4. Inventory & Equipment System
Managed in `AbstractInventoryEntity.java`, the system bridges the gap between Minecraft's 6 equipment slots and the mod's 15-slot internal container.

### 🎒 Slot Mapping:
*   `0`: Head (Helmet)
*   `1`: Chest (Chestplate)
*   `2`: Legs (Leggings)
*   `3`: Feet (Boots)
*   `4`: Off-hand (Shield/Arrows)
*   `5`: Main-hand (Weapon)
*   `6-14`: Storage slots (Food/Supplies)

### 🛠️ Key Methods:
*   `setItemSlot(EquipmentSlot slot, ItemStack stack)`: Overridden to automatically update the corresponding slot in the internal `inventory` container.
*   `setEquipment()`: Randomizes starting gear based on `RecruitsServerConfig` sets.
*   `updateShield()`: Handles the visual and mechanical state of blocking when a shield is equipped.

## 5. Progression & Leveling
Recruits gain XP from dealing damage and killing hostiles.
*   **Max Level:** Defined by `RecruitsMaxXpLevel` (Default 20).
*   **Buffs:** Every level up grants permanent `AttributeModifier` bonuses to `MAX_HEALTH`, `ATTACK_DAMAGE`, `KNOCKBACK_RESISTANCE`, and `MOVEMENT_SPEED`.
*   **Moral Effects:** High morale provides speed and damage buffs, while low morale causes `WEAKNESS`, `MOVEMENT_SLOWDOWN`, and `CONFUSION`.
