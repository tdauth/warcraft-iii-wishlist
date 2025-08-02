# Warcraft III Wishlist

Wishlist for the game Warcraft III: Reforged.

## Motivation

Unfortunately, the community has no access to the source code of the game.
There are a lot of things which need to be fixed and a lot of features the game is lacking.
This list can help to document all of this.
It might help people who have access to the source code of the game and are involved in its development to improve the game which will attract new players to the game and be benefitial for the company selling the game as well as the community creating content for it and playing it.

## Contribute

Feel free to create issues and pull requests for this repository on GitHub.

## Bug Fixes

* framehandles get invalid in save games and have to be recreated manually after loading a game. Accessing the old ones will crash the game.
* Render more than 2 different cliff types: Even with more different cliff types only the first 2 are rendered. The others are rendered as one of the first ones.
* Initialization of `region` variables leads to crashing the game on saving it.
* Ability `Agyd` (Create Corpse) is enabled even without the required dependencies in `areq`.
* Changing all ability object data fields via JASS should work.
* Fix event `EVENT_PLAYER_HERO_REVIVE_CANCEL` triggering when you click on the hero icon in the queue (current solution [HeroReviveCancelEvent v1.1](https://www.hiveworkshop.com/threads/herorevivecancelevent-v1-1.293491/)).
* Fix event `EVENT_PLAYER_HERO_REVIVE_START` not triggering when the revival of a hero is cancelled.
* Fix the function `GetTriggerUnit` returning the hero instead of the altar for event `EVENT_PLAYER_HERO_REVIVE_START` ([question](https://www.hiveworkshop.com/threads/getting-the-reviving-altar-for-event_unit_hero_revive_start.356746/)).
* Fix the function `BlzGroupAddGroupFast`. It does not add the given unit to the given group.
* Fix AI scripts crashing when the AI has too little space to build its base.
* Fix AI scripts crashing when the AI starts next to waygates or has to use them.
* Fix `GetUnitGoldCost` and `GetUnitWoodCost` crashing the game when used with IDs of hero unit types.
* Strings from research effects end up in the file war3map.wts where they do not belong:
```
STRING 3882
// Upgrades: R0CI (Brute Strength), effect1 (Effect 1)
{
rmvx
}
```
* war3mapMisc.txt entries like TWN1 cannot use translatable strings.
* Starfall buff effect cannot be changed ([source](https://www.hiveworkshop.com/threads/starfall-effect-not-changing.332390/)).
* Fix dependency equivalents using the same object data ID lead to crashes when buildings training them are selected. The crash is probably caused by some endless loop.
* Fix crashes and performance issues with pathing in big maps ([source](https://www.hiveworkshop.com/threads/suicideonplayer-crashes-the-game-on-reforged-only-on-some-maps.359199/)).
* Fix automatic deselections in multiplayer which might occur when selected unit groups are determined in trigger conditions ([source](https://www.hiveworkshop.com/threads/selection-bug.312500/)).
* Fix summon event for ability Pocket Factory ([source](https://www.hiveworkshop.com/threads/how-do-you-detect-a-pocket-factory-summon.330032/)). Summoned Pocket Factories and Clockwerk Goblins are not detected by the summon event.
* Fix changing unit icons when changing their skin with `BlzSetUnitSkin`. At the moment you have to change the owner of the unit to fix the icon.
* `SetTerrainTypeBJ` has no effect if Reforged graphics are enabled ([source](https://github.com/tdauth/warcraft-iii-wishlist/issues/4)).
* Fix SLK and TXT files being able to handle abilities and upgrades with more than 4 levels ([source](https://www.hiveworkshop.com/threads/abilitydata-slk-help.268472/#post-2715115)).

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
* The World Editor should allow saving the map not only as folder but binary file formats as text files JSON, XML or some similar format. Binary formats should only be used when checking check boxes on saving the map. This would massively help maintaining version control repositories and seeing diffs of files. This could even include MDL and MDX files. Every war3map.xxx file could have a corresponding text file like war3map.w3e -> war3map_w3e.json There is already the community driven project [WC3MapTranslator](https://github.com/ChiefOfGxBxL/WC3MapTranslator) but native support by the World Editor would be more convenient.
* Test settings UI for the world editor which allows you to specify your game lobby and player name before testing the map.
* More than 16 different tile and cliff types for terrain. Currently, the war3map.w3e format limits the reference to the tile and cliff type to 4 bits which means there can be only 16 different types.
* Remove the limit of 5 hero abilities per hero.
* Remove the limit of 4 abilities per item.
* Paged command buttons: Allow adding more than 16 unit/item types/abilities etc. to list fields and more than 6 items per inventory and add page buttons to change the currently displayed buttons/item icons.
* Allow sharing/unsharing control with single units.
* Allow loading custom SLK and TXT files per map even when clicking on the map in the lobby. This would allow defining custom object data not based on default object data but with complete custom IDs and strings there instead of the war3map.wts files or even complete object data sets. There could be a special file which is called war3mapLoadFiles.txt which contains user defined entries like

```
UI\LobbyStrings.txt
Units\DemonUnitStrings.txt
Units\DemonItemData.slk
```

There could be an API to load those files during the game:

```
native LoadTxt takes string filePath returns boolean
native LoadSlk takes string filePath returns boolean
native HasFile takes string filePath returns boolean
native HasDir takes string dirPath returns boolean
```

* More object data types based on game SLK files: unit sound sets, weather effects, lightnings, ubersplats, water etc.
* Object data without level-specific data: Allow setting one single value for every level for abilities and researches to avoid big object data files with high levels. This could be controlled with a boolean flag per ability/research. It would massively improve the map loading times.
* Custom races which are supported in the lobby, object editor, gameplay interface settings etc. There could be race map file like `war3mapRaces.txt` which contains this information about all races:

```
[Goblin] // Folder name in the UI folder and used for all assets.
Name=Goblin // Name shown in the lobby. Can refer a string from war3map.wts and FDF StringLists.

[Dwarf]
Name=TRIGSTR_1171
```

* Allow rotation of buildings including their pathing, ground and shadow textures.
* Add sight blockers for flying units. Currently, the occlusion height will only work for ground units.
* Show Destructible HP on selection by setting a boolean flag in the object editor.
* New boolean flag for unit types to enable hero glow.
* Modify string/text fields in editor in different languages and maintaining the different war3map.wts files automatically.
* Referencing strings from FDF files in object data instead of generating new translatable strings into the war3map.wts file. Basically the same as `GetLocalizedString` and `GetLocalizedHotkey` but in the object editor.
* Allow more custom script files to be imported and used by custom trigger scripts. The files could be specified in the file `war3mapExtra.txt``where their loading order is defined:

```
[CustomScripts]
CustomScriptsCount=2
CustomScript0=scripts/mycustomscript.j
CustomScript1=scripts/mycustomscript2.j
```

* Allow using all code from Blizzard.j and the map script in AI scripts and not only from common.j and common.ai.
* Support saving and loading custom campaigns as folders in the World Editor's Campaign Editor (just like for maps).
* Team colored icons: Some pixel flag/placeholder in icons should be for the unit's team color and filled with it.
* Message Log in multiplayer: Only supported by a [custom system](https://www.hiveworkshop.com/threads/barad%C3%A9s-log-1-0.356932/).
* Tiny Item abilities should get flags to check building limits and dependencies and if enabled disallow placing buildings if the requirements are not fulfilled. The requirements could be listed in the tooltip if they are not fulfilled.
* New research effects:
  * Change training/upgrade/building/repair time for a specific unit type.
  * Change stock replenish interval for a specific unit or item type.
  * Change stock maximum for a specific unit or item type.
* Allow specifying more model properties in object data like texture paths. This would allow users to use different skins more easily with existing models imported only once. It is already possible with replaceable textures for destructables but a list of texture paths would be better especially for units, heroes and buildings.
* Health bar for destructables in Reforged like it used to be in Frozen Throne.
* The Channel ability should get an option to be auto cast or not. This would help to create custom auto cast spells.
* Support multiple game versions by version checks at runtime:

```jass
constant native GetMajorVersion takes nothing returns integer
constant native GetMinorVersion takes nothing returns integer
constant native GetPatchVersion takes nothing returns integer
// For natives, functions, constants and global variables.
constant native SupportsIdentifier takes string name returns boolean
```

The user can declare any native even if it is not supported by the game version.
Having a native or identifier which is not supported by the game should not directly lead to not being able to start a map.
It should only be checked during runtime.
Not existing identifiers should have no effect at all and return default values depending on their declared type return type.
`null` for handles, `0` for reals, and `""` for strings.
Nothing is done if the return type is `nothing`.
Default behavior could be overwritten with a new `default` keyword which could lead to a custom user-defined function:

```
function BlzSetUnitSkinUserDefined takes unit whichUnit, integer skinId returns nothing
  // alternative behavior
endfunction

function BlzGetUnitSkinUserDefined takes unit whichUnit returns integer
  // alternative behavior
  return 0
endfunction

native BlzSetUnitSkin takes unit whichUnit, integer skinId returns nothing default BlzSetUnitSkinUserDefined
native BlzGetUnitSkin takes unit whichUnit returns integer default BlzGetUnitSkinUserDefined
```

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

type techstringlevelfield extends handle

constant techstringlevelfield TECH_SF_LEVEL_ICON = ConvertTechStringLevelField('gar1')

native BlzGetTechStringLevelField takes integer id, techstringlevelfield whichField, integer level returns string

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

This should change the UI and sound effects etc.

* Training Queue API:

```jass
native UnitGetTrainingQueueCount takes unit whichUnit returns integers
native UnitCancelTrainingQueue takes unit whichUnit, integer index returns boolean
native UnitGetTrainingQueueId takes unit whichUnit, integer index returns integer
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

This API would allow players to select more than 12 units at once.
The UI would need some adaptions to show more than 12 unit icons at once.

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

native TriggerRegisterDestructableEvent takes trigger whichTrigger, destructable d, destructableevent e returns nothing
native TriggerRegisterAnyDestructableEvent takes trigger whichTrigger, destructableevent e returns nothing

// some useful natives
native SetDestructableFace takes destructable d, real face returns nothing
native GetDestructableFace takes destructable d returns real
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

`GetKillingUnit` and `GetEventDamageSource` should work for the destructable events.

The same API could be added for Doodads.

* Technology API:

```jass
native GetTechIcon takes integer id, integer level returns string
native GetTechName takes integer id, integer level returns string
```

* Haunted/Entangled Gold Mines classification: Recreates a gold mine on death and is automatically used by AI.
* AI API:

```jass
native SetBuyItem takes integer qty, integer id, integer hero returns boolean
// the AI will only purchase mercenaries which have been configured by SetProduce if enabled.
native SetPurchaseMercenaries takes boolean state returns nothing
// Attacks all crates, gates etc. if enabled.
native SetAttackCrates takes boolean state returns nothing
```

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

The AI should be able to attack locations reachable from the water with their navy and transport units to isles if no zeppelin is available but a reachable transport ship.

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

* Frame API:

```jass
// All can be used GetLocalPlayer for specific players only.
native SetFramePortraitModel takes string model returns nothing
native GetFramePortraitModel takes nothing returns string
native SetFramePortraitCamera takes integer index returns nothing
native SetFramePortraitAnimation takes integer index returns nothing
native SetFramePortraitTeamColor takes playercolor whichColor returns nothing
```

* Unit Sound Set API:

```jass
native GetUnitSoundSet takes unit whichUnit returns string
native SetUnitSoundSet takes unit whichUnit, string soundName returns nothing
native GetUnitSoundRandom takes unit whichUnit returns string
native SetUnitSoundRandom takes unit whichUnit, string soundName returns nothing
native GetUnitSoundMovement takes unit whichUnit returns string
native SetUnitSoundMovement takes unit whichUnit, string soundName returns nothing
```

This has not the highest priority since something like this can be achieved by setting a unit's skin.

* Player Information API:

```jass
native GetPlayerPing takes player whichPlayer returns integer
native GetPlayerFps takes player whichPlayer returns integer
// Returns the locale configured for Warcraft III.
// Like BlzGetLocale but already synchronized.
native GetPlayerLocale takes player whichPlayer returns string

native GetLocalizedStringEx takes string source, string locale returns string
native GetLocalizedHotkeyEx takes string source, string locale returns integer
```

* Calendar API:

```jass
type timestamp extends handle

// Return's a players current local timestamp containing time zone information (automaticall synchronizes).
// Returns the host's current timestamp if the player is not a user.
native GetPlayerTimeStamp takes player whichPlayer returns timestamp
native GetTimeStampYear takes timestamp t returns integer
native GetTimeStampMonth takes timestamp t returns integer
native GetTimeStampDay takes timestamp t returns integer
native GetTimeStampHour takes timestamp t returns integer
native GetTimeStampMinute takes timestamp t returns integer
native GetTimeStampSecond takes timestamp t returns integer
native GetTimeStampMillisecond takes timestamp t returns integer
native GetTimeStampTimeZone takes timestamp t returns integer
native TimeStamp2ISO takes timestamp t returns string
native ISO2TimeStamp takes string iso returns timestamp
```

* Group API:

```jass
native GetClosestUnit takes group g, real x, real y returns unit
```

* Harvest API:

```jass
EVENT_PLAYER_UNIT_HARVEST_GOLD
EVENT_PLAYER_UNIT_HARVEST_LUMBER
EVENT_PLAYER_UNIT_RETURN_GOLD
EVENT_PLAYER_UNIT_RETURN_LUMBER

native GetTriggerHarvestAmount takes nothing returns integer
native GetTriggerHarvestMine takes nothing returns unit
native GetTriggerHarvestTree takes nothing returns destructable

native GetTriggerReturnAmount takes nothing returns integer
native GetTriggerReturnBuilding takes nothing returns unit

native GetUnitHarvestGold takes unit whichUnit returns integer
native GetUnitHarvestLumber takes unit whichUnit returns integer
native SetUnitHarvestGold takes unit whichUnit, integer amount returns nothing
native SetUnitHarvestLumber takes unit whichUnit, integer amount returns nothing
```

## JassHelper

* JassHelper changes the number of underscores for generated identifiers when saving the map. This will lead to bigger diffs in git than necessary and make it harder to see actual changes in the generated map scripts.
* JassHelper does not remove empty lines between static ifs which are false which makes the generated map script longer than necessary.
* JassHelper should have an option to not generated JASS comments into the generated map script to keep it smaller.

## Sources

* [List of WarCraft III Crashes](https://www.hiveworkshop.com/threads/list-of-warcraft-iii-crashes.194706/)
* [Request Features for JASS and the World Editor](https://www.hiveworkshop.com/threads/feedback-request-features-for-jass-and-the-world-editor.308099)
* [UnitEventEx](https://www.hiveworkshop.com/threads/uniteventex.306289/)
* [GetMainSelectedUnit](https://www.hiveworkshop.com/threads/getmainselectedunit.325337)
* [BlzSetAbilityRealLevelField is not working](https://www.hiveworkshop.com/threads/blzsetabilityreallevelfield-is-not-working.339882/)
* [TasAbilityField Test](https://www.hiveworkshop.com/pastebin/b2769ab71109c3634b3115937deaa34a.24187)
