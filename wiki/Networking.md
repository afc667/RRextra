# 📡 Networking & Communication (Networking.md)

VillagerRecruits uses a high-volume packet system to maintain synchronization between the player's client (GUI/Input) and the server's authoritative logic.

## 1. Network Architecture
The mod uses Forge's `SimpleChannel` registered in `Main.java` as `SIMPLE_CHANNEL`.

### 📩 Message Interface (`de.maxhenkel.corelib.net.Message`)
All packets implement this interface, requiring:
*   `toBytes(FriendlyByteBuf)`: Writing data to the network buffer.
*   `fromBytes(FriendlyByteBuf)`: Reading data from the network buffer.
*   `executeServerSide(NetworkEvent.Context)`: Logic that runs on the server (e.g., giving orders).
*   `executeClientSide(NetworkEvent.Context)`: Logic that runs on the client (e.g., updating map overlays).

## 2. Packet Functional Groups
With over 100 custom packets, the system is divided into specialized categories:

### 🎮 Unit Command Packets (C2S)
Sent when the player interacts with the radial menu or recruit GUIs.
*   **`MessageAggro`**: Changes the recruit's `STATE` (Passive/Aggressive).
*   **`MessageMovement`**: Updates the `FOLLOW_STATE` (Follow/Hold/Wander).
*   **`MessageGroup`**: Assigns/Removes a recruit from a specific squad (`UUID groupUUID`).
*   **`MessageFaceCommand`**: Forces a recruit to look in a specific direction.
*   **`MessageStrategicFire`**: Commands ranged units to target a specific area or entity.

### 🏛️ Faction & Diplomacy Packets (C2S/S2S)
*   **`MessageCreateTeam`**: Handles faction registration (`String name`, `int color`).
*   **`MessageChangeDiplomacyStatus`**: Updates relations between two factions (`UUID targetFaction`, `int status`).
*   **`MessageAddPlayerToTeam`**: Invites/Adds a player to an existing faction.
*   **`MessageSaveTeamSettings`**: Updates team-wide flags like Friendly Fire.

### 🗺️ World Map & Claim Packets (S2C)
Critical for keeping the map UI updated without constant polling.
*   **`MessageToClientUpdateClaims`**: Sends a full or partial list of claimed chunks to the player.
*   **`MessageToClientUpdateFactions`**: Syncs the status and color of all known factions.
*   **`MessageUpdateClaim`**: Notifies the client of a specific claim's health or siege status change.
*   **`MessageToClientReceiveRoute`**: Syncs patrol routes and waypoints for rendering on the map.

### 💰 Economy & Recruitment (C2S)
*   **`MessageHire`**: Processes the purchase of a recruit from a village.
*   **`MessageDoPayment`**: Handles the manual or automatic payment of salaries.
*   **`MessagePromoteRecruit`**: Logic for upgrading a recruit to a higher tier (e.g., Recruit to Shieldman).

## 3. Implementation Details
*   **Security:** Packets like `MessageCommandScreen` verify the sender's UUID on the server before executing logic.
*   **Efficiency:** World sync packets (`ToClient`) use `FriendlyByteBuf` to compress chunk coordinates and faction metadata.
*   **Context:** Most `executeServerSide` methods retrieve the `ServerPlayer` from the context to determine the range and authority of the command.
