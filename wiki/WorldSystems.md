# 🌍 World Systems (WorldSystems.md)

VillagerRecruits introduces global management layers that persist across server restarts, handling the social, political, and strategic aspects of the mod.

## 1. Faction System (`RecruitsFaction.java`)
A Faction (or Team) is the primary organizational unit for players and recruits.

### 📊 Data Structure:
*   **`stringID`**: Unique internal name (immutable).
*   **`teamDisplayName`**: Custom name shown in GUIs.
*   **`teamLeaderID` / `teamLeaderName`**: UUID and name of the player who founded the faction.
*   **`members`**: A list of `RecruitsPlayerInfo` containing all joined players.
*   **`joinRequests`**: Players waiting for approval.
*   **`npcs` / `maxNPCs`**: Current and maximum count of recruits allowed in the faction.
*   **`unitColor` / `teamColor`**: Aesthetic settings for armor trims and map overlays.
*   **`banner`**: NBT data for the faction's custom flag.

### 💾 Persistence:
Factions are saved to the world's NBT via `toNBT()` and loaded via `fromNBT()`. The data is managed by `RecruitsFactionManager`.

## 2. Diplomacy & Relations
The `RecruitsDiplomacyManager` handles the complex relationships between different factions.

### 🤝 Diplomacy States:
*   **`ALLY`**: Recruits will not attack members of this faction and will assist them in combat.
*   **`NEUTRAL`**: Default state. No automatic aggression, but no assistance either.
*   **`HOSTILE`**: Recruits will automatically target and attack members of this faction on sight.

### 🔍 logic:
*   Relations are asymmetrical (Faction A can be Hostile to B, while B remains Neutral).
*   Relations are updated via `MessageChangeDiplomacyStatus` and synced to all clients.

## 3. Squad Management (`RecruitsGroup.java`)
Groups (Squads) are sub-units within a faction, allowing a player to command multiple recruits as a single entity.

### 🛠️ Key Features:
*   **`groupUUID`**: Unique identifier for the squad.
*   **`members`**: List of recruit UUIDs assigned to this group.
*   **Command Sync**: When a command is sent to a Group (e.g., "Group 1, Charge!"), all members update their `FOLLOW_STATE` or `STATE` simultaneously.
*   **UI Integration**: Groups are visible in the radial menu and the "Group Manager" screen.

## 4. Manager Classes
These classes handle the global lifecycle of world data:
*   **`RecruitsPlayerUnitManager`**: Tracks how many recruits each individual player owns to enforce the `MaxRecruitsForPlayer` config.
*   **`RecruitPlayerUnitSaveData`**: The actual `SavedData` implementation that stores player-to-unit mappings in the world folder.
*   **`RecruitsGroupsManager`**: Handles the creation, deletion, and membership of all recruit squads.
