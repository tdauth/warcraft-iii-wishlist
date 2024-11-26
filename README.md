# Warcraft III Wishlist

Wishlist for the game Warcraft III: Reforged.

## Features

* Allow joining players during a started game. Games could run like servers and allow joining/leaving at any time:

```jass
constant playerevent EVENT_PLAYER_JOIN = ConvertPlayerEvent(xxx)
constant mapflag MAP_ALLOW_JOINING_PLAYERS = ConvertMapFlag(xxx)
```

* Saving and loading games and changing levels in multiplayer:

```jass
native ChangeLevelSync takes player host, string newLevel returns nothing
native LoadGameSync takes player host, string saveFileName returns
```

* Game caches in multiplayer per player:

```jass
native  ReloadGameCachesFromDiskSync takes player whichPlayer returns boolean
native  InitGameCacheSync    takes player whichPlayer, string campaignFile returns gamecache
native  SaveGameCacheSync    takes player whichPlayer, gamecache whichCache returns boolean
```

* Debugging of JASS code in maps: break points, stack traces, watching variables.
* Stack traces on crashes up to the line in the JASS map script or C++ code of the game.
* Test settings UI for the world editor which allows you to specify your game lobby and player name before testing the map.
* More than 16 different tile and cliff types for terrain. Currently, the war3map.w3e format limits the reference to the tile and cliff type to 4 bits which means there can be only 16 different types.
* Remove the limit of 5 hero abilities per hero.
* Remove the limit of 4 abilities per item.
* Paged command buttons: Allow adding more than 16 unit/item types/abilities etc. to list fields and more than 6 items per inventory and add page buttons to change the currently displayed buttons/item icons.
* Allow sharing/unsharing control with single units.
* More object data types based on game SLK files: unit sound sets, weather effects, lightnings, ubersplats, water etc.
* Object data without level-specific data: Allow setting one single value for every level for abilities and researches to avoid big object data files with high levels. This could be controlled with a boolean flag per ability/research. It would massively improve the map loading times.
* Custom races which are supported in the lobby, object editor, gameplay interface settings etc.
* Allow rotation of buildings including their pathing, ground and shadow textures.
* Add sight blockers for flying units. Currently, the occlusion height will only work for ground units.
* Show Destructible HP on selection by setting a boolean flag in the object editor.
* Modify string/text fields in editor in different languages and maintaining the different war3map.wts files automatically.
* Allow using all code from Blizzard.j and the map script in AI scripts.
* Support saving and loading custom campaigns as folders.
* Team colored icons: Some pixel flag/placeholder in icons should be for the unit's team color and filled with it.
* New research effects:
  * Change training/upgrade/building/repair time for a specific unit type.
  * Change stock replenish interval for a specific unit or item type.
  * Change stock maximum for a specific unit or item type.

* Object API:

```jass
native GetObjectBaseId takes integer id returns integer
native Id2String takes integer id returns string
native String2Id takes string source returns integer
```

* Add missing object data fields in JASS:

```jass
constant itemintegerfield ITEM_IF_GOLD = ConvertItemIntegerField('igol')
constant itemintegerfield ITEM_IF_LUMBER = ConvertItemIntegerField('xxx')
constant itemstringfield ITEM_SF_ICON = ConvertItemStringField('xxx')

// Allow access and changes of all build time fields (object editor). They aren't listed in the trigger actions etc.
```

* Add an object data field for the ability Black Arrow which specifies the maximum target unit level. Currently, it is hard coded with the value of 5.
* Hero ability API:

```jass
native SetHeroAbilityLevel takes unit hero, integer abilityId, integer level returns boolean
// Tinker Engineering effect
native ReplaceHeroAbility takes unit hero, integer abilityId, integer newAbilityId returns boolean
// Tome of Retraining effect
native UnskillHero takes unit hero returns nothing
native GetHeroSkill takes unit hero, integer index returns integer
native GetHeroSkillCount takes unit hero returns integer
```

* Player API:

```jass
native SetPlayerRace takes player whichPlayer, race whichRace returns nothing
```

* Unit Progress API:

```jass
// for the currently trained unit
native UnitSetTrainingProgress takes unit whichUnit, integer constructionPercentage returns nothing
// GetTriggerUnit() - could be scripted manually with a timer but would be nice to have
native UnitGetTrainingProgress takes unit whichUnit returns integer
native UnitSetResearchProgress takes unit whichUnit, integer constructionPercentage returns nothing // for the current research
native UnitGetResearchProgress takes unit whichUnit returns integer
native UnitSetRevivalProgress takes unit whichUnit, integer constructionPercentage returns nothing // for the currently revived hero
native UnitGetRevivalProgress takes unit whichUnit returns integer
```

* Unit transformation/morph API:

```jass
native MorphUnit takes unit whichUnit, integer newType, real duration returns nothing
native UnmorphUnit takes unit whichUnit returns nothing
native GetUnitMorphBaseTypeId takes unit whichUnit returns integer
native GetUnitMorphedTypeId takes unit whichUnit returns integer
native IsUnitMorphed takes unit whichUnit returns boolean
```

* Unit miss API:

```jass
native SetUnitMissChance takes unit whichUnit, integer percentage returns nothing
native GetUnitMissChance takes unit whichUnit returns integer
```

* Fix applying parameter `buffId` of native `UnitApplyTimedLife`.
* Timed Life API:

```jass
// Returns the current value of timed life for a unit.
native GetUnitTimedLife takes unit whichUnit returns real
// Returns the total value of timed life for a unit.
native GetUnitTimeLifeTotal takes unit whichUnit returns real
// Returns the current ability ID for a buff for timed life for a unit.
native GetUnitTimedLifeBuff takes unit whichUnit returns integer
```

* More unit events:

```jass
EVENT_ON_TRANSFORM
EVENT_ON_CARGO_LOAD
EVENT_ON_CARGO_UNLOAD
EVENT_ON_RESURRECTION
EVENT_ON_ANIMATE_DEAD
EVENT_ON_REINCARNATION_START
EVENT_ON_REINCARNATION_FINISH
EVENT_UNIT_DECAYED
EVENT_UNIT_EXPLODED
```

* Buff API:

```jass
constant unitevent EVENT_UNIT_BUFF_ADDED = ConvertUnitEvent(XXX)
constant unitevent EVENT_UNIT_BUFF_REMOVED = ConvertUnitEvent(XXX)
constant playerunitevent EVENT_PLAYER_UNIT_BUFF_ADDED = ConvertPlayerUnitEvent(XXX)
constant playerunitevent EVENT_PLAYER_UNIT_BUFF_REMOVED = ConvertPlayerUnitEvent(XXX)

native GetTriggerBuff takes nothing returns integer
native GetTriggerBuffSource takes nothing returns unit

native AddBuff takes unit whichUnit, integer buffID returns nothing
native RemoveBuff takes unit whichUnit, integer buffID returns nothing
```

* Selection Group API:

```jass
native SetPlayerSelectionGroupMaximum takes player whichPlayer, integer maximum returns nothing
native GetPlayerSelectionGroupMaximum takes player whichPlayer returns integer

// Returns the index of currently focused selected unit of a player (Tab key) from the group of all selected units.
native GetMainSelectedUnitIndex takes player whichUnit returns integer
native SetMainSelectedUnitIndex takes player whichUnit, integer index returns nothing

native GetMainSelectedUnit takes player whichUnit returns unit
native SetMainSelectedUnit takes player whichUnit, unit u returns nothing
```

* Missile API which allows creating missiles manually.
* Unit and item stock API which allows setting and getting the stock values for items from shops.

* Destructable/Doodad API:

```jass
// the event stuff would be useful for items as well:
type destructableevent extends handle
constant native ConvertDestructableEvent          takes integer i returns destructableevent
constant destructableevent EVENT_DESTRUCTABLE_ATTACKED                             = ConvertDestructableEvent(xxx)
constant destructableevent EVENT_DESTRUCTABLE_DAMAGED                             = ConvertDestructableEvent(xxx)
constant destructableevent EVENT_DESTRUCTABLE_RESTORED_LIFE                             = ConvertDestructableEvent(xxx) // could also be triggered on repair events
constant destructableevent EVENT_DESTRUCTABLE_SELECTED                             = ConvertDestructableEvent(xxx) // also useful for items
constant destructableevent EVENT_DESTRUCTABLE_DESELECTED                            = ConvertDestructableEvent(xxx)
constant native GetTriggerDestructable takes nothing returns destructable
constant native GetRestoredDestructableLife takes nothing returns real

// some useful natives
native SetDestructableX takes destructable d, real x returns nothing
native SetDestructableY takes destructable d, real y returns nothing
native SetDestructableZ takes destructable d, real z returns nothing
native GetDestructableZ takes destructable d returns real
native SetDestructableVariation takes destructable d, integer variation returns nothing
native GetDestructableVariation takes destructable d returns integer
native SetDestructableScale takes destructable d, real scale returns nothing
native GetDestructableScale takes destructable d returns real
native IsDestructableHidden  takes destructable d returns boolean
native SetDestructablePathing takes destructable d, boolean enabled returns nothing
native IsDestructablePathingEnabled  takes destructable d returns boolean
native SetDestructableWalkable takes destructable d, boolean enabled returns nothing
native IsDestructableWalkableEnabled  takes destructable d returns boolean

// targetting which is useful for filtering certain destructables (for example if you want to filter for all trees in an area)
// there is already the constant UNIT_IF_TARGETED_AS for the integer field for units which can be used with the native ConvertTargetFlag
// either provide natives with integer fields of destructable types or these two natives:
native IsDestructableTargetedAs takes destructable d, targetflag t returns boolean
native SetDestructableTargetedAs takes destructable d, targetflag t, boolean flag returns nothing

type destructablerealfield handle
// support accessing all fields of destructables
```

The same API could be added for Doodads.

* Training Queue API:

```jass
native UnitGetTrainingQueueCount takes unit whichUnit returns integers
native UnitCancelTrainingQueue takes unit whichUnit, integer index returns boolean
native UnitGetTrainingQueueId takes unit whichUnit, integer index returns integer
```

* Technology API:

```jass
native GetTechIcon takes integer id, integer level returns string
native GetTechName takes integer id, integer level returns string
```

* Haunted/Entangled Gold Mines classification: Recreates a gold mine on death and is automatically used by AI.

* Minimap API:

```jass
// changes the zoom to scale starting from the point x and y on the map
// can be used with GetLocalPlayer
native SetMinimapScale takes real x, real y, real scale returns nothing
// gives you the boundaries of the minimap
constant native GetMinimapSizeX takes nothing returns real
constant native GetMinimapSizeY takes nothing returns real
native ConvertMapXToMinimapX takes real x returns real
native ConvertMapYToMinimapY takes real y returns real
```

* Gameplay constants API:

```jass
type gameplayconstant extends handle
type userinterfaceconfig extends handle

native SetGameplayConstant takes gameplayconstant g, real/integer/string/boolean value returns nothing
native GetGameplayConstant takes gameplayconstant g returns real/integer/string/boolean

// some can be used with GetLocalPlayer:
native SetUserInterfaceConfig takes userinterfaceconfig g, real/integer/string/boolean value returns nothing
native GetUserInterfaceConfig takes userinterfaceconfig g returns real/integer/string/boolean
```

* AI navy support and API:

```jass
// the player might buy ships including transport ships
native SetNavy takes boolean s returns nothing
native PurchaseTransportShip takes nothing returns nothing
```

* Chat API:

```jass
// returns all players who received the trigger chat message
native GetReceivingPlayers takes nothing returns force
```

* Message API:

```jass
// Simulate an error message.
native SimError takes force whichForce, string messager returns nothing
```

* Terrain API:

```jass
type terrainspecifictype extends handle

native ConvertTerrainSpecificType takes integer v returns terrainspecifictype

constant terrainspecifictype TERRAIN_CLIFF_TYPE_LAND = ConvertTerrainSpecificType(0)
constant terrainspecifictype TERRAIN_CLIFF_TYPE_WATER_SHALLOW = ConvertTerrainSpecificType(1)
constant terrainspecifictype TERRAIN_CLIFF_TYPE_WATER_DEEP = ConvertTerrainSpecificType(2)
constant terrainspecifictype TERRAIN_CLIFF_TYPE_RAMP = ConvertTerrainSpecificType(3)

native SetTerrainSpecificType takes real x, real y, terrainspecifictype t returns nothing
native GetTerrainSpecificType takes real x, real y returns terrainspecifictype

native SetTerrainCliffLevel         takes real x, real y, integer cliffLevel returns nothing
```

* Illusions API:

```jass
native CreateIllusion takes unit source, real x, real y, real facing, real duration returns unit
native CreateIllusionById takes integer unitId, real x, real y, real facing, real duration returns unit

native IllusionGetBaseUnit takes unit u returns unit
native IllusionGetRemainingDuration takes unit u returns real
native IllusionGetElapsedDuration takes unit u returns real
native IllusionGetTotalDuration takes unit u returns real
native IllusionGetDeathEffect takes unit u returns string
native IllusionGetDamageDealtFactor takes unit u returns real
native IllusionGetDamageTakenFactor takes unit u returns real

native IllusionSetRemainingDuration takes unit u, real duration returns nothing
native IllusionSetTotalDuration takes unit u, real duration returns nothing
native IllusionSetDeathEffect takes unit u, string sfxpath returns nothing
native IllusionSetDamageDealtFactor takes unit u, real factor returns nothing
native IllusionSetDamageTakenFactor takes unit u, real factor returns nothing
native IllusionShowTimedLifeIndicator takes unit u, boolean enabled returns nothing
```

* Ability API:

```jass
native BlzGetUnitAbilityCount takes unit whichUnit returns integer
native BlzGetItemAbilityCount takes item whichItem returns integer


native UnitHideAttackAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideMoveAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideStopAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideHaltAbility takes unit whichUnit, boolean flag returns nothing
native UnitHidePatrolAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideRallyPointAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideSelectUserAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideSelectHeroAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideHeroSkillsAbility takes unit whichUnit, boolean flag returns nothing
native UnitHideBuildAbility takes unit whichUnit, boolean flag returns nothing

// can be used with GetLocalPlayer:

native SetAreaOfEffectModel takes string model returns nothing
native SetAreaOfEffectTexture takes string texture returns nothing

native SetRallyPointModel takes string model returns nothing
native SetRallyPointTexture takes string texture returns nothing
```

* Multiboard API:

```jass
// Uses GetTriggerPlayer for the corresponding player.
native TriggerRegisterMultiboardMinimizeEvent takes trigger whichTrigger returns event
native GetTriggerMultiboard takes nothing returns multiboard
```

* File API:

```jass
native WriteFile takes player whichPlayer, string name, string content return boolean
native ReadFile takes player whichPlayer, string name return string
```

## Bug Fixes

* framehandles get invalid in save games and have to be recreated manually after loading a game. Accessing the old ones will crash the game.
* Render more than 2 different cliff types: Even with more different cliff types only the first 2 are rendered. The others are rendered as one of the first ones.
* Initialization of `region` variables leads to crashing the game on saving it.
* Ability `Agyd` (Create Corpse) is enabled even without the required dependencies in `areq`.
* Changing all ability object data fields via JASS should work.
* Fix event `EVENT_PLAYER_HERO_REVIVE_CANCEL` triggering when you click on the hero icon in the queue (current solution [HeroReviveCancelEvent v1.1](https://www.hiveworkshop.com/threads/herorevivecancelevent-v1-1.293491/)).
* Fix event `EVENT_PLAYER_HERO_REVIVE_START` not triggering when the revival of a hero is cancelled.
* Fix function `GetTriggerUnit` returning the hero instead of the altar for event `EVENT_PLAYER_HERO_REVIVE_START` ([question](https://www.hiveworkshop.com/threads/getting-the-reviving-altar-for-event_unit_hero_revive_start.356746/).

## Sources

* [List of WarCraft III Crashes](https://www.hiveworkshop.com/threads/list-of-warcraft-iii-crashes.194706/)
* [Request Features for JASS and the World Editor](https://www.hiveworkshop.com/threads/feedback-request-features-for-jass-and-the-world-editor.308099)
* [UnitEventEx](https://www.hiveworkshop.com/threads/uniteventex.306289/)
* [GetMainSelectedUnit](https://www.hiveworkshop.com/threads/getmainselectedunit.325337)
* [BlzSetAbilityRealLevelField is not working](https://www.hiveworkshop.com/threads/blzsetabilityreallevelfield-is-not-working.339882/)
* [TasAbilityField Test](https://www.hiveworkshop.com/pastebin/b2769ab71109c3634b3115937deaa34a.24187)
