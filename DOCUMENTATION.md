# SimpleLevelGenerator
---
## How to Use:
This asset is designed to be expanded upon on a project to project basis for specific uses.
This asset generates its own rooms; however, this asset can incorperate premade room assets*.

#### &nbsp; Setup 
- Find SimpleLevelGenerator on fab.com
- Add it to an Unreal Project

#### &nbsp; Configuration
- Create a new Blueprint from AGameLevel
- Configure values in the details panel
- Optional: Add premade room data to the RoomData array to generate custom rooms*
- Note: Entities do not spawn if no Entity data is provided.

#### &nbsp; Lastly
- Add the new Blueprint to the scene

#### &nbsp; *Customization
- __Custom Rooms__: create new instances of URoomDataAsset
and add them to the RoomData in AGameLevel.

- __Custom Entities__: create new instances of UEntityDataAsset,
initialize variables of each instance. Add references to the
entity assets to the EntityData array in each ARoom blueprint.

- __Room and Entity functionality expansion__: add new variables to 
URoomDataAsset and UEntityDataAsset to expand data needed for generating
new Rooms/Entities. To edit functionality, alter the C++ classes directly, 
or add functionality to a blueprint created from either class.

## Classes:

![SimpleLevelGeneratorUML](SimpleLevelGenerator_UML_Original.png)

####  &nbsp; &nbsp; &nbsp; Table of Contents
- [UEntityDataAsset](#class-uentitydataasset-udataasset)
- [URoomDataAsset](#class-uroomdataasset-udataasset)
- [ARoom](#class-aroom-aactor)
- [AGameLevel](#class-agamelevel-aactor)



#### class UEntityDataAsset : UDataAsset 
> Stores data related to spawning Entities into the scene. (Expandable)

| Variable                                 | Use                                                                  |
|:-----------------------------------------|:---------------------------------------------------------------------|
| AActor EntityAsset | Stores an Actor asset to be spawned into the level. |

#### class URoomDataAsset : UDataAsset
> Stores data related to spawning Rooms into the scene. (Expandable)

| Variable                                 | Use                                                                  |
|:-----------------------------------------|:---------------------------------------------------------------------|
| ARoom RoomAsset | Stores a Room to be spawned into the level. |

#### class ARoom : AActor
> An enclosed area that is connected to others to create a Game Level.
> Can be generated or spawned.

| Variable                                 | Use                                                                  |
|:-----------------------------------------|:---------------------------------------------------------------------|
| TArray<AActor> OwnedActiveEntities | An array of each active entity this room has spawned. |
| TArray<UEntityDataAsset> EntityData | Stores data for entities to be spawned. |
| bool bGenerated | True if this room was not created from an asset. |
| bool bRepeatSpawns | Enables reoccuring entity spawns. |
| float SpawnCooldownSeconds | Prevents new entities from spawning for a specified duration. |

| Function                                 | Use                                                                  |
|:-----------------------------------------|:---------------------------------------------------------------------|
| **Public:**| |
| void SpawnEntity()  | Spawns an entity from EntityData. |

#### class AGameLevel : AActor
 > A group of connected rooms.

| Variable                                 | Use                                                                  |
|:-----------------------------------------|:---------------------------------------------------------------------|
| TArray<ARoom> RoomInstances | A list of every Room in this Game Level. |
| UDataAsset RoomData | Stores Room assets to be spawned and related data. |
| int MaxActiveEntities | The Maximum amount of spawned Entities spawned from this Game Level. |
| float Density | Scales distance between rooms in the level. |
| int MaxWidth | The width of the area that this Game Level will generate inside of. |
| int MaxLength | The length of the area that this Game Level will generate inside of. |
| bool bSpawnUniqueRooms | Generates simple unique rooms if true. Note: requires room assets if false. |
| bool bFlatMode | Tries to arrange each Room on a floor to a similar height. Note: if false, mushes floors together.|
| bool b3DMode | Generates more than one floor if true. |
| int MaxHeight | The height of the area that this Game Level will generate inside of. |


| Function                                  | Use                                                                 |
|:------------------------------------------|:--------------------------------------------------------------------|
| **Public:** |  |
| void BeginPlay() override | Creates the Game Level on scene play. |
| void Warmup() | Initializes data before spawning rooms. |
| void SelectRooms() | Randomly collect rooms and sort it into a stack by size. |
| void SpawnRooms() | Places room assets into the scene, largest first, smallest last. |
| void CreateNewRooms() | Creates simple new rooms and places them into a scene. |
| void ConnectRooms() | Generates maze-like passageways between rooms. |
| void Finalize() | Removes extra rooms. |
| void UpdateEntitySpawns() | Spawns Entities near players without surpassing MaxActiveEntities. |

