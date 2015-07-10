0x00    Keep Alive
    Keep Alive ID           VarInt

0x01    Join Game
    Entity ID               Integer
    Gamemode                Unsigned Byte
    Dimension               Byte
    Difficulty              Unsigned Byte
    Max Players             Unsigned Byte
    Level Type              String (default, flat, largeBiomes, amplified, default_1_1)
    Reduced Debug Info      Boolean

0x02    Chat Message
    Json Data   Chat (String, 32767 byte limit)
    Position    Byte (0: chatbox, 1: system chatbox, 3: actionbar)

0x03    Time Update
    World Age       Long (In ticks; not changed by server commands)
    Time of day     Long (The world (or region) time, in ticks. If negative the sun will stop moving at the Math.abs of the time)

0x04    Entity Equipment
    Entity ID      VarInt
    Slot           Short
    Item           Slot

0x05    Spawn Position
    Location    Position

0x06    Update Health
    Health              Float (<= 0 = dead, 20 = full)
    Food                VarInt (0-20)
    Food Saturation     Float (0.0 - 5.0 in integer increments)

0x07    Respawn
    Dimension       Integer (-1: Nether, 0: World, 1: End)
    Difficulty      Unsigned Byte (0: Peaceful, 1: Easy, 2: Normal, 3: Hard)
    Gamemode        Unsigned Byte (0: survival, 1: creative, 2: adventrue, 3: spectator)
    Level Type      String (default, flat, largeBiomes, amplified, default_1_1)

0x08    Player Position And Look
    X       Double
    Y       Double
    Z       Double
    Yaw     Float (degrees)
    Pitch   Float (degrees)
    Flags   Byte (determines the relative/absolute state of the above, see below for values)
        Field	  Bit
        X	      0x01
        Y	      0x02
        Z	      0x04
        Y_ROT	  0x08
        X_ROT	  0x10

0x09    Handshake
    Slot    Byte (0-8, slot player selected)

0x0A    Use Bed
    Entity ID   VarInt
    Location    Position

0x0B    Animation
    Entity ID   VarInt
    Animation   Unsigned Byte (see below)
        ID	Animation
        0	Swing arm
        1	Take damage
        2	Leave bed
        3	Eat food
        4	Critical effect
        5	Magic critical effect

0x0C    Spawn Player
    Entity ID       VarInt
    Player UUID     UUID
    X               Integer
    Y               Integer
    Z               Integer
    Yaw             Angle
    Pitch           Angle
    Current Item    Short (0 == no item, negative will crash client)
    Metadata        Metadata (no Metadata will crash client)
