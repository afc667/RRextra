# 🛡️ VillagerRecruits Technical Wiki

Welcome to the internal technical documentation for the VillagerRecruits mod (1.20.1). This wiki is designed for developers building addons or extending the mod's core military systems.

## 🏛️ Navigation

### [1. Core Architecture](./Architecture.md)
*   Mod Entry Point (`Main.java`)
*   Job Blocks, Items, and Entity Types
*   Configuration Options (Server/Client)
*   Event Management

### [2. Entity Framework](./Entities.md)
*   Advanced Entity Hierarchy (Async Pathfinding)
*   State Machine (Data Accessors & Orders)
*   Persistent NBT Storage
*   Inventory & Equipment Sync

### [3. Artificial Intelligence](./AI.md)
*   Goal Priority List (Military Logic)
*   Targeting & Multi-threaded Search
*   Custom Navigation & Door Logic

### [4. Networking & Communication](./Networking.md)
*   Packet Categories (Input, Sync, UI)
*   Implementation of the `Message` Interface
*   World Sync & Broadcast Systems

### [5. World Systems](./WorldSystems.md)
*   Faction & Team Data Structures
*   Diplomacy & Faction Relations
*   Squad Management (`RecruitsGroup`)

### [6. Claims & World Map](./ClaimsAndMap.md)
*   Siege Formulas & Detection Logic
*   High-Performance Map Rendering
*   Patrol Routes & Waypoints

### [7. Compatibility & API](./Compatibility.md)
*   `IWeapon` Interface for Firearms
*   External Mod Support (Muskets, SmallShips, Epic Knights)
*   Sponge Mixin Technical Breakdown

---
*Generated for the VillagerRecruits Addon Development Project.*
