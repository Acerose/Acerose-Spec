**This document describes the protocol for communication from the server to the client.**

## Keep Alive (0x00)
| Name           | Type    | Comments                               |
|:---------------|:-------:|:---------------------------------------|
| Keep Alive ID  | VarInt  | Some would consider this a *challenge* |

## Join Game (0x01)
| Name                 | Type          | Comments                                                 |
|:---------------------|:-------------:|:---------------------------------------------------------|
| Entity ID            | Integer       |                                                          |
| Gamemode             | Unsigned Byte | 0 = Survival, 1 = Creative, 2 = Adventure, 3 = Spectator |
| Dimension            | Byte          | -1 = Nether, 0 = World, 1 = End                          |
| Difficulty           | Unsigned Byte | 0 = Peaceful, 1 = Easy, 2 = Normal, 3 = Hard             |
| Max Players          | Unsigned Byte |                                                          |
| Level Type           | String        | default, flat, largeBiomes, amplified, default_1_1       |
| Reduced Debug Info   | Boolean       |                                                          |

## Chat Message (0x02)
| Name                 | Type               | Comments                                           |
|:---------------------|:------------------:|:---------------------------------------------------|
| Message Data         | String (Chat JSON) | JSON data stored in a String, 32767 byte limit     |
| Position             | Byte               | 0 = Chatbox, 1 = System Chatbox, 3 = Actionbar     |

## Time Update (0x03)
| Name                 | Type  | Comments                                                                                                 |
|:---------------------|:-----:|:---------------------------------------------------------------------------------------------------------|
| World Age            | Long  | Counted in ticks, this shouldn't be changed(IE: by plugin commands)                                      |
| Time of Day          | Long  | The world (or region) time, in ticks. If negative the sun will stop moving at the Math.abs() of the time |

## Entity Equipment (0x04)
| Name           | Type    | Comments |
|:---------------|:-------:|:---------|
| Entity ID      | VarInt  |          |
| Slot           | Short   |          |
| Item           | Slot    |          |

## Spawn Position (0x05)
| Name           | Type      | Comments |
|:---------------|:---------:|:---------|
| Location       | Position  | ????     |

## Update Health (0x06)
| Name            | Type      | Comments                                                |
|:----------------|:---------:|:--------------------------------------------------------|
| Health          | Float     | 20 = Full Normal Health, Less than or equal to 0 = Dead |
| Food            | VarInt    | Must be clamped between 0-20                            |
| Food Saturation | Float     | Must be clamped between 0-5, Integers only(????)        |

## Respawn (0x07)
| Name            | Type          | Comments                                                 |
|:----------------|:-------------:|:---------------------------------------------------------|
| Dimension       | Integer       | -1 = Nether, 0 = World, 1 = End                          |
| Difficulty      | Unsigned Byte | 0 = Peaceful, 1 = Easy, 2 = Normal, 3 = Hard             |
| Gamemode        | Unsigned Byte | 0 = Survival, 1 = Creative, 2 = Adventure, 3 = Spectator |
| Level Type      | String        | default, flat, largeBiomes, amplified, default_1_1       |

## Player Position And Look (0x08)
| Name            | Type      | Comments                                                                                         |
|:----------------|:---------:|:-------------------------------------------------------------------------------------------------|
| X Position      | Double    |                                                                                                  |
| Y Position      | Double    |                                                                                                  |
| Z Position      | Double    |                                                                                                  |
| Yaw             | Float     | In degrees.                                                                                      |
| Pitch           | Float     | In degrees.                                                                                      |
| Flags           | Byte      | Determines the relative/absolute state. X = 0x01, Y = 0x02, Z = 0x04, Y_ROT = 0x08, X_ROT = 0x10 |
<!--
    Field     Bit
    X         0x01
    Y         0x02
    Z         0x04
    Y_ROT     0x08
    X_ROT     0x10
-->

## Handshake (0x09)
| Name           | Type      | Comments                   |
|:---------------|:---------:|:---------------------------|
| Slot           | Byte      | 0-8, slot player selected) |

## Use Bed (0x0A)
| Name           | Type      | Comments                   |
|:---------------|:---------:|:---------------------------|
| Entity ID      | VarInt    |                            |
| Location       | Position  | ????                       |

## Animation (0x0B)
| Name           | Type          | Comments                                                                             |
|:---------------|:-------------:|:-------------------------------------------------------------------------------------|
| Entity ID      | VarInt        |                                                                                      |
| Animation      | Unsigned Byte | 0 = Swing, 1 = Damage, 2 = Exit Bed, 3 = Eat, 4 = Crit Effect, 5 = Magic Crit Effect |
<!--
ID  Animation
0   Swing arm
1   Take damage
2   Leave bed
3   Eat food
4   Critical effect
5   Magic critical effect
-->

## Spawn Player (0x0C)
| Name            | Type          | Comments                                                 |
|:----------------|:-------------:|:---------------------------------------------------------|
| Entity ID       | VarInt        |                                                          |
| Player UUID     | UUID          |                                                          |
| X Position      | Integer       |                                                          |
| Y Position      | Integer       |                                                          |
| Z Position      | Integer       |                                                          |
| Yaw             | Angle         | ????                                                     |
| Pitch           | Angle         | ????                                                     |
| Current Item    | Short         | 0 = No item, Negative numbers may cause client crash     |
| Metadata        | Metadata      | ???? (no Metadata will crash client)                     |
