# SimpleDungeonGenerator 
> A tool used for level generation in [UnrealEngine](https://www.unrealengine.com). 
---
### Features

- Load *unique* game levels with or without premade assets.
- Expandable and [open source](https://github.com/chdeese/SimpleLevelGenerator).

### Requirements

SimpleLevelGenerator's only dependency is [UnrealEngine 5](https://www.unrealengine.com).

### How it works

The level area is divided into chunks (or cubes). Rooms are placed within the area and chunks are reserved. Extra Rooms are generated if desired, then Passageways create a maze and connect each room.

### Implementation

To implement this system, four C++ classes will be used to manage rooms generated in the Scene. Two AActor subclasses `AGameLevel` and `ARoom` will each use a custom UDataAsset subclass, `URoomDataAsset` and `UEntityDataAsset`, for storing data related to spawning Rooms and Entities. No premade room or entities will be provided; instead, by default empty rooms will be generated unless room data is added by the user.





