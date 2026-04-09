# 🏛️ Core Architecture (Architecture.md)

This section provides a deep technical dive into the initialization, registration, and configuration systems of the VillagerRecruits mod.

## 1. Mod Entry Point (`Main.java`)
The `Main` class serves as the central hub for the mod's lifecycle.
*   **Mod ID:** `recruits`
*   **Network Channel:** `SIMPLE_CHANNEL` (using `CommonRegistry.registerChannel`) handles all custom packets.
*   **Initialization:** 
    *   Registers configurations (`RecruitsServerConfig`, `RecruitsClientConfig`).
    *   Initializes all `DeferredRegister` instances from the `init` package.
    *   Sets up compatibility flags by checking `ModList.get().isLoaded()` for:
        *   `musketmod` (MusketMod)
        *   `smallships` (Small Ships)
        *   `siegeweapons` (Siege Weapons)
        *   `rpgz` (RPGZ Mod)
        *   `corpse` (Corpse Mod)
        *   `magistuarmory` (Epic Knights Mod)

## 2. Registry Systems (`init/`)
The mod uses Forge's `DeferredRegister` pattern to manage game objects.

### 🧱 ModBlocks
*   `recruit_block`: The base job block for standard recruits.
*   `bowman_block`: Job block for bow-using recruits.
*   `nomad_block`: Job block for nomad recruits.
*   `crossbowman_block`: Job block for crossbow-using recruits.
*   `horseman_block`: Job block for mounted recruits.
*   `recruit_shield_block`: Job block for shield-bearing recruits.

### ⚔️ ModItems
*   **Spawn Eggs:** `recruit`, `bowman`, `nomad`, `recruit_shieldman`, `horseman`, `crossbowman`, `villager_noble`.
*   **Block Items:** Corresponding items for all blocks in `ModBlocks`.

### 👥 ModEntityTypes
*   `recruit`: The standard infantry unit.
*   `recruit_shieldman`: Defensive unit equipped with a shield.
*   `bowman`: Ranged unit using standard bows.
*   `crossbowman`: Ranged unit using crossbows.
*   `nomad`: Specialized ranged unit.
*   `horseman`: Mounted combat unit.
*   `messenger`: Utility unit for communication between players/factions.
*   `scout`: Unit used for exploration and map tasks.
*   `patrol_leader`: Commander unit for squad management.
*   `captain`: Advanced commander unit with ship compatibility.
*   `villager_noble`: The source of recruitment in villages.

## 3. Configuration System
The mod is highly configurable via TOML files in the `config/` directory.

### ⚙️ Server Configuration (`recruits-server.toml`)
| Category | Key | Default | Description |
| :--- | :--- | :--- | :--- |
| **Recruits** | `RecruitCurrency` | `minecraft:emerald` | Item used for hiring and payments. |
| | `RecruitsMaxXpLevel` | `20` | Maximum level a recruit can achieve. |
| | `RecruitsMaxXpForLevelUp` | `250` | XP required to reach the next level. |
| | `MaxRecruitsForPlayer` | `100` | Hard cap on recruits per player. |
| | `RecruitsPayment` | `false` | Whether recruits require regular salary. |
| | `RecruitsPaymentInterval`| `15` | Minutes between payments. |
| **Villages** | `OverrideIronGolemSpawn`| `true` | Replaces vanilla Golems with Recruits. |
| | `NobleVillagerSpawns` | `true` | Allows villagers to become Nobles. |
| **Hostiles** | `PillagerSpawnItems` | `false` | Gives Pillagers shields and swords. |
| **Performance**| `UseAsyncPathfinding` | `true` | Offloads pathfinding to separate threads. |
| | `UseAsyncTargetFinding`| `true` | Offloads targeting logic to separate threads. |
| **Claiming** | `AllowClaiming` | `true` | Enables the chunk protection system. |
| | `SiegeClaimsConquerTime`| `10` | Minutes required to capture a claim. |

### 🎨 Client Configuration (`recruits-client.toml`)
*   `PlayVillagerAmbientSound`: Toggles the "Huh?" sounds (Default: `true`).
*   `RecruitsLookLikeVillagers`: Toggles custom skins vs vanilla villager look (Default: `true`).
*   `CommandScreenToggle`: If `true`, the command menu stays open until pressed again (Default: `false`).
*   `UpdateMapTiles`: Updates the map terrain while the GUI is open (Default: `true`).

## 4. Event Handlers
*   **`RecruitEvents.java`**: Manages the `RecruitsPatrolSpawn` and `PillagerPatrolSpawn` logic.
*   **`CommandEvents.java`**: Handles radial menu interactions and command processing.
*   **`FactionEvents.java`**: Manages player-faction relations and team synchronization.
*   **`ClaimEvents.java`**: Manages the server-side ticking of sieges and chunk detection.
