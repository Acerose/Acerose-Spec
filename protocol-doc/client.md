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
6             | Wolf/Ocelot/Horse taming — Spawn “heart” particles
7             | Wolf/Ocelot/Horse tamed — Spawn “smoke” particles
8             | Wolf shaking water — Trigger the shaking animation
9             | (of self) Eating accepted by server
10            | Sheep eating grass
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
