# SimpleLevelGenerator




>Ideas:
>Implement Editor utility widget menu (research editor utility widgets)
>Override PostEditChangeProperty to update values changed in the details panel.
>(https://dev.epicgames.com/documentation/en-us/unreal-engine/API/Runtime/Engine/Components/UActorComponent/PostEditChangeProperty)

> Linear or Maze mode
-- Maze mode is the traditional dungeon layout.
-- Linear mode is a version of this dungeon generator that lays out the dungeon in a straight path.
---- Linear mode enables Split mode option that branches dungeon generation, potentially has a value for amount of branches and length of the path.
---- Split mode enables Connect mode option that connects the branches back up at a certain point.

> Extend enemy spawn complexity by incorperating a data table or struct for enemy spawn chance, stats, and multipliers.
3D mode fills prefabs and rooms upwards to make new floors. 
--- 3D mode enables Flat mode option that can be disabled to mush floors together to create a anthill-like structure.

> Add Room Sleeping/Waking to improve performance

## How to Use:
This asset is designed to be expanded upon on a project to project basis for specific uses.

#### Installation
- Find SimpleLevelGenerator on [fab.com](fab.com), click "Add To Library"
- Download the [Epic Games Launcher](https://store.epicgames.com/en-US/)
- Open Epic Games Launcher > Unreal Engine > Library
- Look for SimpleDungeonGenerator under Fab Assets
- Click "Add To Project"

#### Setup 
- This asset generates its own rooms; however, this asset can incorperate premade room assets*.
- Navigate to "Tools" inside of the Unreal Engine Editor
- Select "SimpleLevelGenerator Manager"

#### Configuration
- Create a new Blueprint from AGameLevel
- Configure values in the details panel
- Optional: Add premade room data to the RoomData array to generate custom rooms*

#### Lastly
- Add the new Blueprint to the scene.

#### *Customization
- __Custom Rooms__: create new instances of URoomDataAsset
and add them to the RoomData in AGameLevel

- __Custom Entities__: create new instances of UEntityDataAsset,
initialize variables of each instance. Add references to the
entity assets to the EntityData array in each ARoom blueprint

- __Room and Entity functionality expansion__: add new variables to 
URoomDataAsset and UEntityDataAsset to expand data needed for generating
new Rooms/Entities. To edit functionality, alter the C++ classes directly, 
or add functionality to a blueprint created from either class.


## Classes:


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

