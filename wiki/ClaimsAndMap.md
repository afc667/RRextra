# 🗺️ Claims & World Map System (ClaimsAndMap.md)

VillagerRecruits features a territory management system integrated with a custom high-performance world map.

## 1. The Claim System (`RecruitsClaim.java`)
Claims are chunk-based territories that provide security and strategic control.

### 🛡️ Siege Mechanics & Logic
The siege system is tick-based, running every `100` ticks (`SIEGE_TICK_INTERVAL`).

1.  **Detection:** The system runs `tickDetection` every `300` ticks to check for hostile presence in a claim.
2.  **Activation:** A siege starts if the number of attackers in the claim exceeds the `SiegeClaimsRecruitsAmount` (Default: 10).
3.  **Speed Calculation:**
    *   Formula: `attackerCount / defenderCount`.
    *   No defenders: `2.0f` (Max speed).
    *   The result determines how fast the claim's health (Default: 100) depletes.
4.  **Damage:** The base damage is `3` per siege tick.
5.  **Conquest:** If health reaches `0`, `setSiegeSuccess()` is called, transferring the claim to the attacking faction.

### 🧱 Protection Logic
*   **Block Protection:** If `BlockPlacingBreakingOnlyWhenClaimed` is enabled, players can only modify blocks within their own faction's claims.
*   **Explosion Protection:** Claims can be configured to negate explosion damage via `ExplosionProtectionInClaims`.
*   **Permissions:** Claim owners can toggle `allowBlockInteraction`, `allowBlockPlacement`, and `allowBlockBreaking` for their territory.

## 2. World Map Technical Overview
The World Map (`WorldMapScreen.java`) is a specialized GUI for strategic management.

### 🖼️ Map Rendering (`ChunkTileManager.java`)
*   **Tiles:** The map is divided into `16x16` chunk tiles for efficient rendering.
*   **Caching:** Map data is cached locally to reduce performance impact.
*   **`ChunkImage`**: Handles the conversion of chunk terrain into a `NativeImage` for GL texture rendering.
*   **Updates:** Controlled by `UpdateMapTiles` in the client config; if enabled, the map updates in real-time as the world changes.

### 🖱️ Context Menu Interactions
Right-clicking the map opens a context menu that triggers network packets:
*   **`MessageUpdateClaim`**: Updates the claim boundary or settings.
*   **`MessageDeleteClaim`**: Removes the claim (Server-side validation included).
*   **`MessageTeleportPlayer`**: (Admin only) Instantly moves the player to the clicked coordinates.

### 📍 Routes & Waypoints
*   **`RouteRenderer`**: Draws the paths defined by players for their patrols.
*   **`WaypointEditPopup`**: Allows editing the specific wait times and actions (e.g., "Defend", "Patrol") at each node.
*   **Squad Sync:** When a route is assigned, the squad members update their AI goals to follow the calculated `Path` between waypoints.
