**This document describes the protocol for communication from the server to the client.**

# Keep Alive (0x00)

Name          | Type   | Comments
:------------ | :----: | :-------------------------------------
Keep Alive ID | VarInt | Some would consider this a *challenge*

# Join Game (0x01)

Name               | Type          | Comments
:----------------- | :-----------: | :-------------------------------------------------------
Entity ID          | Integer       |
Gamemode           | Unsigned Byte | 0 = Survival, 1 = Creative, 2 = Adventure, 3 = Spectator
Dimension          | Byte          | -1 = Nether, 0 = World, 1 = End
Difficulty         | Unsigned Byte | 0 = Peaceful, 1 = Easy, 2 = Normal, 3 = Hard
Max Players        | Unsigned Byte |
Level Type         | String        | default, flat, largeBiomes, amplified, default_1_1
Reduced Debug Info | Boolean       |

# Chat Message (0x02)

Name         | Type               | Comments
:----------- | :----------------: | :---------------------------------------------
Message Data | String (Chat JSON) | JSON data stored in a String, 32767 byte limit
Position     | Byte               | 0 = Chatbox, 1 = System Chatbox, 3 = Actionbar

# Time Update (0x03)

Name        | Type | Comments
:---------- | :--: | :-------------------------------------------------------------------------------------------------------
World Age   | Long | Counted in ticks, this shouldn't be changed(IE: by plugin commands)
Time of Day | Long | The world (or region) time, in ticks. If negative the sun will stop moving at the Math.abs() of the time

# Entity Equipment (0x04)

Name      | Type   | Comments
:-------- | :----: | :-------
Entity ID | VarInt |
Slot      | Short  |
Item      | Slot   |

# Spawn Position (0x05)

Name     | Type     | Comments
:------- | :------: | :-------
Location | Position | ????

# Update Health (0x06)

Name            | Type   | Comments
:-------------- | :----: | :------------------------------------------------------
Health          | Float  | 20 = Full Normal Health, Less than or equal to 0 = Dead
Food            | VarInt | Must be clamped between 0-20
Food Saturation | Float  | Must be clamped between 0-5, Integers only(????)

# Respawn (0x07)

Name       | Type          | Comments
:--------- | :-----------: | :-------------------------------------------------------
Dimension  | Integer       | -1 = Nether, 0 = World, 1 = End
Difficulty | Unsigned Byte | 0 = Peaceful, 1 = Easy, 2 = Normal, 3 = Hard
Gamemode   | Unsigned Byte | 0 = Survival, 1 = Creative, 2 = Adventure, 3 = Spectator
Level Type | String        | default, flat, largeBiomes, amplified, default_1_1

# Player Position And Look (0x08)

Name       | Type   | Comments
:--------- | :----: | :-----------------------------------------------------------------------------------------------
X Position | Double |
Y Position | Double |
Z Position | Double |
Yaw        | Float  | In degrees.
Pitch      | Float  | In degrees.
Flags      | Byte   | Determines the relative/absolute state. X = 0x01, Y = 0x02, Z = 0x04, Y_ROT = 0x08, X_ROT = 0x10

<!--
    Field     Bit
    X         0x01
    Y         0x02
    Z         0x04
    Y_ROT     0x08
    X_ROT     0x10
-->

# Handshake (0x09)

Name | Type | Comments
:--- | :--: | :-------------------------
Slot | Byte | 0-8, slot player selected)

# Use Bed (0x0A)

Name      | Type     | Comments
:-------- | :------: | :-------
Entity ID | VarInt   |
Location  | Position | ????

# Animation (0x0B)

Name      | Type          | Comments
:-------- | :-----------: | :-----------------------------------------------------------------------------------
Entity ID | VarInt        |
Animation | Unsigned Byte | 0 = Swing, 1 = Damage, 2 = Exit Bed, 3 = Eat, 4 = Crit Effect, 5 = Magic Crit Effect

<!--
ID  Animation
0   Swing arm
1   Take damage
2   Leave bed
3   Eat food
4   Critical effect
5   Magic critical effect
-->

# Spawn Player (0x0C)

Name         | Type     | Comments
:----------- | :------: | :---------------------------------------------------------
Entity ID    | VarInt   |
Player UUID  | UUID     |
X Position   | Integer  |
Y Position   | Integer  |
Z Position   | Integer  |
Yaw          | Angle    | ????
Pitch        | Angle    | ????
Current Item | Short    | 0 = No item, Negative numbers may cause client crash
Metadata     | Metadata | See entity-doc/metadata.md (no Metadata will crash client)

# Collect Item (0x0D)

Name                | Type   | Comments
:------------------ | :----: | :-------
Collected Entity ID | VarInt |
Collected Entity ID | VarInt |

# Spawn Object (0x0E)

Name      | Type        | Comments
:-------- | :---------: | :---------------------------
Entity ID | VarInt      |
Type      | Byte        |
X         | Integer     |
Y         | Integer     |
Z         | Integer     |
Yaw       | Angle       |
Pitch     | Angle       |
Data      | Object Data | See entity-doc/objectdata.md

# Spawn Mob (0x0F)

Name       | Type     | Comments
:--------- | :------: | :------------------------------------------------------------------------------
Entity ID  | VarInt   |
Type       | Byte     | See entity-doc/objects.md
X          | Integer  |
Y          | Integer  |
Z          | Integer  |
Yaw        | Angle    |
Pitch      | Angle    |
Head Pitch | Angle    |
Velocity X | Short    | Velocity is believed to be in units of 1/8000 of a block per server tick (50ms)
Velocity Y | Short    | Velocity is believed to be in units of 1/8000 of a block per server tick (50ms)
Velocity Z | Short    | Velocity is believed to be in units of 1/8000 of a block per server tick (50ms)
Metadata   | Metadata | See entity-doc/metadata.md

# Spawn Painting (0x10)

Name      | Type          | Comments
:-------- | :-----------: | :-------------------------------------------------------------------------------------------
Entity ID | VarInt        |
Title     | String        | Name of painting. max length == 13
Location  | Position      | Center Coords center is (max(0, width / 2 - 1), height / 2) (assumes (0, 0) is the top left)
Direction | Unsigned Byte | Direction of paiting. 0: north (-z), 1: west (-x), 2: south (+z), 3: east (+x)

# Spawn Experience Orb (0x11)

Name      | Type    | Comments
:-------- | :-----: | :-------------------------------------------
Entity ID | VarInt  |
X         | Integer |
Y         | Integer |
Z         | Integer |
Count     | Short   | Amount of exp this orb rewards on collection

# Entity Velocity (0x12)

Name       | Type   | Comments
:--------- | :----: | :------------------------------------------------------------------------------
Entity ID  | VarInt |
Velocity X | Short  | Velocity is believed to be in units of 1/8000 of a block per server tick (50ms)
Velocity Y | Short  | Velocity is believed to be in units of 1/8000 of a block per server tick (50ms)
Velocity Z | Short  | Velocity is believed to be in units of 1/8000 of a block per server tick (50ms)

# Destroy Entities (0x13)

Name       | Type          | Comments
:--------- | :-----------: | :----------------------------------------
Count      | VarInt        | Number of elements in the following array
Entity IDs | Array[VarInt] | list of entities to Destroy

# Entity (0x14)

Name      | Type   | Comments
:-------- | :----: | :-------
Entity ID | VarInt |

# Entity Relative Move (0x15)

Name      | Type   | Comments
:-------- | :----: | :----------
Entity ID | VarInt |
Delta X   | Byte   | 4 block max
Delta Y   | Byte   | 4 block max
Delta Z   | Byte   | 4 block max

<!-- This packet is sent when an entity moves less than 4 blocks in any direction.  
4+ blocks, send Entity Teleport instead -->

# Entity Look (0x16)

Name      | Type    | Comments
:-------- | :-----: | :------------------------------
Entity ID | VarInt  |
Yaw       | Angle   | New angle, not a delta from old
Pitch     | Angle   | New angle, not a delta from old
On Ground | Boolean |

# Entity Look and Relative Move (0x17)

Name      | Type    | Comments
:-------- | :-----: | :------------------------------
Entity ID | VarInt  |
Delta X   | Byte    | 4 block max
Delta Y   | Byte    | 4 block max
Delta Z   | Byte    | 4 block max
Yaw       | Angle   | New angle, not a delta from old
Pitch     | Angle   | New angle, not a delta from old
On Ground | Boolean |

<!-- This packet is sent when an entity moves less than 4 blocks in any direction. -->

# Entity Teleport (0x18)

Name      | Type    | Comments
:-------- | :-----: | :------------------------------
X         | Integer |
Y         | Integer |
Z         | Integer |
Yaw       | Angle   | New angle, not a delta from old
Pitch     | Angle   | New angle, not a delta from old
On Ground | Boolean |

<!-- This packet is sent when and entity moves more than 4 blocks -->

# Entity Head Look (0x19)

Name      | Type   | Comments
:-------- | :----: | :------------------------------
Entity ID | VarInt |
Head Yaw  | Angle  | New angle, not a delta from old

# Entity Status (0x1A)

Name          | Type    | Comments
:------------ | :-----: | :-------------------
Entity ID     | Integer |
Entity Status | Byte    | See next table below

Entity Status | Meaning
:-----------: | :------------------------------------------------------------------------------------
1             | Sent when resetting a mob spawn minecart's timer - appears to be unused by the client
2             | Living Entity hurt
3             | Living Entity dead
4             | Iron Golem throwing up arms
5             | Wolf/Ocelot/Horse taming — Spawn “heart” particles <!-- This is 6 on wiki.vg -->
6             | Wolf/Ocelot/Horse tamed — Spawn “smoke” particles <!-- This is 7 on wiki.vg -->
7             | Wolf shaking water — Trigger the shaking animation <!-- This is 8 on wiki.vg -->
8             | (of self) Eating accepted by server <!-- This is 9 on wiki.vg -->
9             | Sheep eating grass <!-- This is 10 on wiki.vg -->
10            | Play TNT ignite sound
11            | Iron Golem handing over a rose
12            | Villager mating — Spawn “heart” particles
13            | Spawn particles indicating that a villager is angry and seeking revenge
14            | Spawn happy particles near a villager
15            | Witch animation — Spawn “magic” particles
16            | Play zombie converting into a villager sound
17            | Firework exploding
18            | Animal in love (ready to mate) — Spawn “heart” particles
19            | Reset squid rotation
20            | Spawn explosion particle — works for some living entities
21            | Play guardian sound — works for every entity
22            | Enables reduced debug for players
23            | Disables reduced debug for players

# Attach Entity (0x1B)

Name       | Type    | Comments
:--------- | :-----: | :----------------------------------
Entity ID  | Integer | -1 to detach, this is the passenger
Vehicle Id | Integer |
Leash      | Boolean | true == leashes entity to vehicle

# Entity Metadata (0x1C)

Name      | Type     | Comments
:-------- | :------: | :-----------------------------
Entity ID | VarInt   |
Metadata  | Metadata | ??? See entity-doc/metadata.md

# Entity Effect (0x1D)

Name           | Type    | Comments
:------------- | :-----: | :---------------------------------------------
Entity ID      | VarInt  |
Effect ID      | Byte    | See entity-doc/entity-effects.md Or below list
Amplifier      | Byte    | Effect level == Amplifier + 1
Duration       | VarInt  | Seconds
Hide Particles | Boolean |

Effect ID | Effect
:-------: | :--------------
1         | Speed
2         | Slowness
3         | Haste
4         | Mining Fatigue
5         | Strength
6         | Instant Health
7         | Instant Damage
8         | Jump Boost
9         | Nausea
10        | Regeneration
11        | Resistance
12        | Fire Resistance
13        | Water Breathing
14        | Invisibility
15        | Blindness
16        | Night Vision
17        | Hunger
18        | Weakness
19        | Poison
20        | Wither
21        | Health Boost
22        | Absorption
23        | Saturation

# Remove Entity Effect (0x1E)

Name      | Type   | Comments
:-------- | :----: | :--------------
Entity ID | VarInt |
Effect ID | Byte   | See table above

# Set Experience (0x1F)

Name             | Type   | Comments
:--------------- | :----: | :-------------------------------------------
Experience bar   | Float  | 0 - 1 values
Level            | VarInt |
Total Experience | VarInt | See table below for total experience formula

Level Range | Formula
:---------- | :---------------------------------
0-15        | 2[Level] + 7
16-30       | 2.5[Level]^2 - 40.5[Level] + 360
31+         | 4.5[Level]^2 - 162.5[Level] + 2220

# Entity Properties (0x20)

Name                 | Type            | Comments
:------------------- | :-------------: | :----------------------------------------
Entity ID            | VarInt          |
Number of Properties | Int             | Number of elements in the following array
Property             | Array[Property] | check below table for property definition

## Property

Name                | Type            | Comments
:------------------ | :-------------: | :----------------------------------------
Key                 | String          |
Value               | Double          |
Number of Modifiers | VarInt          | Number of elements in the following array
Modifiers           | Array[Modifier] | See below array for modifier definition

## Modifier

Modifier Name                                                                    | Description and Known Values                                                                                                                                                                            | Known Attributes Modified
:------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------------------------------------------------------------------------------------------------
Random spawn bonus                                                               | Generated upon spawning; a random number from a Gaussian distribution ranging from 0.0 to 0.05*. For Zombie Knockback Resistance, another value between 0.0 and 0.05* is also generated.                | generic.followRange (Operation 1; all mobs), Knockback Resistance (Operation 0; Villagers and Zombies only)
Tool modifier                                                                    | Value varies based on tool.                                                                                                                                                                             | generic.attackDamage (Operation 0; all tools; UUID CB3F55D3-645C-4F38-A497-9C13A33DB5CF)
Weapon modifier                                                                  | Value varies based on weapon.                                                                                                                                                                           | generic.attackDamage (Operation 0; all tools; UUID CB3F55D3-645C-4F38-A497-9C13A33DB5CF (same UUID as Tool modifier))
Sprinting speed boost                                                            | Fixed value of 0.3* used by all mobs (including players) when sprinting.                                                                                                                                | generic.movementSpeed (Operation 2; all living entities; UUID 662A6B8D-DA3E-4C1C-8813-96EA6097278D)
Fleeing speed boost                                                              | Fixed value of 2 used by all passive mobs when fleeing.                                                                                                                                                 | generic.movementSpeed (Operation 2; all passive mobs; UUID E199AD21-BA8A-4C53-8D13-6182D5C69D3A)
Attacking speed boost                                                            | Fixed value of 6.2* for Endermen and 0.45* for Zombie Pigmen; exists only when attacking.                                                                                                               | generic.movementSpeed (Operation 0; Endermen - UUID 020E0DFB-87AE-4653-9556-831010E291A0, Zombie Pigmen - UUID 49455A49-7EC5-45BA-B886-3B90B23A1718)
Baby speed boost                                                                 | Fixed value of 0.5; exists only for baby Zombies and baby Zombie Villagers.                                                                                                                             | generic.movementSpeed (Operation 1; Baby Zombies; UUID B9766B59-9566-4402-BC1F-2EE2A276D836)
Drinking speed penalty                                                           | Fixed value of -0.25 for Witches when drinking a potion.                                                                                                                                                | generic.movementSpeed (Operation 0; Witches; UUID 5CD17E52-A79A-43D3-A529-90FDE04B181E)
Random zombie-spawn bonus                                                        | Generated upon spawning; a random number between 0.0 and 1.5.                                                                                                                                           | generic.followRange (Operation 2; Zombies)
Leader zombie bonus                                                              | Has a (small) random chance of being generated on a zombie when spawned. For Spawn Reinforcements Chance, random number between 0.5 and 0.75. For generic.maxHealth, random number between 1.0 and 4.0. | zombie.spawnReinforcements (Operation 0; Zombies), generic.maxHealth (Operation 2; Zombies)
Zombie reinforcement caller charge                                               | Fixed value of -0.05* created each time a zombie spawns another zombie as reinforcement.                                                                                                                | zombie.spawnReinforcements (Operation 0; Zombies)
Zombie reinforcement callee charge                                               | Fixed value of -0.05* created for each zombie spawned as a reinforcement.                                                                                                                               | zombie.spawnReinforcements (Operation 0; Zombies)
potion.moveSpeed or potion.moveSpeed # (where # is the potion's amplifier)       | Fixed value of 0.2* when under the Speed effect, multiplied by the effect's level (amplifier + 1).                                                                                                      | generic.movementSpeed (Operation 2; All living entities; UUID 91AEAA56-376B-4498-935B-2F7F68070635)
potion.moveSlowdown or potion.moveSlowdown # (where # is the potion's amplifier) | Fixed value of -0.15* when under the Slowness effect, multiplied by the effect's level.                                                                                                                 | generic.movementSpeed (Operation 2; All living entities; UUID 7107DE5E-7CE8-4030-940E-514C1F160890)
potion.damageBoost or potion.damageBoost # (where # is the potion's amplifier)   | Fixed value of 1.3 when under the Strength effect, multiplied by the effect's level.                                                                                                                    | generic.attackDamage (Operation 2; All living entities; UUID 648D7064-6A60-4F59-8ABE-C2C23A6DD7A9)
potion.weakness or potion.weakness # (where # is the potion's amplifier)         | Fixed value of -0.5 when under the Weakness effect, multiplied by the effect's level.                                                                                                                   | generic.attackDamage (Operation 0; All living entities; UUID 22653B89-116E-49DC-9B6B-9971489B5BE5)
potion.healthBoost or potion.healthBoost # (where # is the potion's amplifier)   | Fixed value of 4 when under the Health Boost effect, multiplied by the effect's level.                                                                                                                  | generic.maxHealth (Operation 0; All living entities; UUID 5D6F0BA2-1186-46AC-B896-C61C5CEE99CC)
Unknown synced attribute modifier                                                | Unknown; created when client reads attribute data sent by server.                                                                                                                                       | varies

## Known Key Values

Key                         | Default           | Min | Max             | Label
:-------------------------- | :---------------: | :-: | :-------------: | :--------------------------
generic.maxHealth           | 20.0              | 0.0 | Double.MaxValue | Max Health
generic.followRange         | 32.0              | 0.0 | 2048.0          | Follow Range
generic.knockbackResistance | 0.0               | 0.0 | 1.0             | Knockback Resistance
generic.movementSpeed       | 0.699999988079071 | 0.0 | Double.MaxValue | Movement Speed
generic.attackDamage        | 2.0               | 0.0 | Double.MaxValue |
horse.jumpStrength          | 0.7               | 0.0 | 2.0             | Jump Strength
zombie.spawnReinforcements  | 0.0               | 0.0 | 1.0             | Spawn Reinforcements Chance

### Modifier Data Structure

Field Name | Field Type
:--------- | :--------:
UUID       | UUID
Amount     | Double
Operation  | Byte

# Chunk Data (0x21)

Name | Type | Comments
:--- | :--: | :-------
     |      |
