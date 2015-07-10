Entity Metadata Format
    Note that entity metadata is a totally distinct concept from block metadata. All entities must send at least one item of metadata, in most cases this will be the health item.
    The entity metadata format is quirky dictionary format, where the key and the value's type are packed in a single byte.
        To parse, repeat the following procedure:
        Read an unsigned byte
        If this byte == 127, stop reading
        Decompose the byte.
        The bottom 5 bits (0x1F) serve as an identifier (key) for the data to follow.
        The top 3 bits (0xE0) serve as a type.
        Read and unpack based on the type (below)
    Type	Meaning
    0	Byte
    1	Short
    2	Int
    3	Float
    4	UTF-8 String (VarInt prefixed)
    5	Slot
    6*	Int, Int, Int (x, y, z)
    7	Float, Float, Float (pitch, yaw, roll)
    *Not currently used

    In C-like pseudocode:
    do {
        item = readByte();
        if (item == 0x7F) break;
        var index = item & 0x1F;
        var type = item >> 5;

        if (type == 0) metadata[index] = readByte();
        if (type == 1) metadata[index] = readShort();
        if (type == 2) metadata[index] = readInt();
        if (type == 3) metadata[index] = readFloat();
        if (type == 4) metadata[index] = readString();
        if (type == 5) metadata[index] = readSlot();
        if (type == 6) {
            var vector;
            vector.x = readInt();
            vector.y = readInt();
            vector.z = readInt();
            metadata[index] = vector;
        }
        if (type == 7) {
            var rotation;
            rotation.pitch = readFloat();
            rotation.yaw = readFloat();
            rotation.roll = readFloat();
            metadata[index] = rotation;
        }
    } while (true);
    To create the byte, you can use this: (Type << 5 | Index & 0x1F) & 0xFF

Entity
Index	Type	Meaning
0	    Byte	Bit Mask	Meaning
                0x01	    On Fire
                0x02	    Crouched
                0x08	    Sprinting
                0x10	    Eating/Drinking/Blocking
                0x20	    Invisible
1	    Short	Air

Living Entity Extends Entity
Index	Type	Meaning
2	    String	Name Tag
3	    Byte	Always Show Name Tag
6	    Float	Health
7	    Int	    Potion Effect Color
8	    Byte	Is Potion Effect Ambient
9	    Byte	Number of Arrows in Entity
15  	Byte	Whether the entity has no ai.

Ageable Extends Living Entity
Index	Type	Meaning
12	    Byte	Entity's Age (Negative = Child)

ArmorStand Extends Living Entity
Index	Type	                            Meaning
10	    Byte	                            Bit Mask	Meaning
                                            0x01    	Small Armorstand
                                            0x02    	Has Gravity
                                            0x04    	Has Arms
                                            0x08    	Remove BasePlate
                                            0x16    	Marker (Zero bounding box)
11	Float, Float, Float (pitch, yaw, roll)	Head position
12	Float, Float, Float (pitch, yaw, roll)	Body position
13	Float, Float, Float (pitch, yaw, roll)	Left Arm position
14	Float, Float, Float (pitch, yaw, roll)	Right Arm position
15	Float, Float, Float (pitch, yaw, roll)	Left Leg position
16	Float, Float, Float (pitch, yaw, roll)	Right Leg position

Human Extends Living Entity
Index	 Type	        Meaning
10	     Unsigned Byte	Skin flags
16	     Byte	        Bit Mask   Meaning
                        0x02	   Hide Cape
17	     Float	        Absorption Hearts
18	     Int	        Score

Horse Extends Ageable
Index	 Type	Meaning
16	     Int	Bit Mask   Meaning
                0x02	   Is Tame
                0x04	   Has Saddle
                0x08	   Has Chest
                0x10	   Is bred
                0x20	   Is Eating
                0x40	   Is Rearing
                0x80	Mouth Open
19	     Byte	Value	Type
                0	    Horse
                1	    Donkey
                2	    Mule
                3	    Zombie
                4	    Skeleton
20	     Int	Bit Mask	Meaning
                0x00FF	Value	Color
                        0	    White
                        1	    Creamy
                        2	    Chestnut
                        3	    Brown
                        4	    Black
                        5	    Gray
                        6	    Dark Down
                0xFF00	Value	Style
                        0	    None
                        1       White
                        2       Whitefield
                        3       White Dots
                        4       Black Dots
21	    String  Owner Name
22	    Int	    Value	  Type
                0	      No Armor
                1	      Iron Armor
                2	      Gold Armor
                3	      Diamond Armor

Bat Extends Living Entity
Index	Type	Meaning
16	    Byte	Is Hanging

Tameable
Extends Ageable
Index	Type	Meaning
16	Byte	Bit Mask	Meaning
0x01	Is Sitting
0x04	Is Tame
17	String	Owner Name

Ocelot
Extends Tameable
Index	Type	Meaning
18	Byte	Ocelot Type

Wolf
Extends Tameable
Index	Type	Meaning
16	Byte	Bit Mask	Meaning
Flags from Tameable
0x02	Is Angry
18	Float	Health
19	Byte	Begging
20	Byte	Collar Color

Pig
Extends Ageable
Index	Type	Meaning
16	Byte	Has Saddle
Rabbit
Extends Ageable
Index	Type	Meaning
18	Byte	Rabbit Type
Sheep
Extends Ageable
Index	Type	Meaning
16	Byte	Bit Mask	Meaning
0x0F	Value	Color
0	White
1	Orange
2	Magenta
3	Light Blue
4	Yellow
5	Lime
6	Pink
7	Gray
8	Silver
9	Cyan
10	Purple
11	Blue
12	Brown
13	Green
14	Red
15	Black
0x10	Is Sheared

Villager
Extends Ageable
Index	Type	Meaning
16	Int	Value	Profession
0	Farmer
1	Librarian
2	Priest
3	Blacksmith
4	Butcher

Enderman
Extends Living Entity
Index	Type	Meaning
16	Short	Carried Block
17	Byte	Carried Block Data
18	Byte	Is Screaming
Zombie
Extends Living Entity
Index	Type	Meaning
12	Byte	Is Child
13	Byte	Is Villager
14	Byte	Is Converting
Zombie Pigman
Extends Zombie

Blaze
Extends Living Entity
Index	Type	Meaning
16	Byte	On Fire

Spider
Extends Living Entity
Index	Type	Meaning
16	Byte	Climbing

Cave Spider
Extends Spider

Creeper
Extends Living Entity
Index	Type	Meaning
16	Byte	State (-1 = Idle, 1 = Fuse)
17	Byte	Is Powered

Ghast
Extends Living Entity
Index	Type	Meaning
16	Byte	Is Attacking

Slime
Extends Living Entity
Index	Type	Meaning
16	Byte	Size

Magma Cube
Extends Slime

Skeleton
Extends Living Entity
Index	Type	Meaning
13	Byte	Value	Meaning
0	Normal
1	Wither

Witch
Extends Living Entity
Index	Type	Meaning
21	Byte	Is Agressive

Iron Golem
Extends Living Entity
Index	Type	Meaning
16	Byte	Is Player Created

Wither
Extends Living Entity
Index	Type	Meaning
17	Int	Watched Target
18	Int	Watched Target
19	Int	Watched Target
20	Int	Invulnerable Time
Guardian
Extends Living Entity
Index	Type	Meaning
16	Byte	Bit Mask	Meaning
0x02	Is Elderly
0x04	Is retracting spikes
17	Int	Target entity id
Boat
Extends Entity
Index	Type	Meaning
17	Int	Time Since Hit
18	Int	Forward Direction
19	Float	Damage Taken

Minecart
Extends Entity
Index	Type	Meaning
17	Int	Shaking Power
18	Int	Shaking Direction
19	Float	Damage Taken / Shaking Multiplier
20	Int	Bit Mask	Meaning
0x00FF	Block Id
0xFF00	Block Data
21	Int	Block Y Position
22	Byte	Show Block

Furnace Minecart
Extends Minecart
Index	Type	Meaning
16	Byte	Is Powered

Item
Extends Entity
Index	Type	Meaning
10	Slot	Item

Arrow
Extends Entity
Index	Type	Meaning
16	Byte	Is Critical

Firework
Extends Entity
Index	Type	Meaning
8	Slot	Firework Info

Item Frame
Extends Entity
Index	Type	Meaning
8	Slot	Item
9	Byte	Rotation

Ender Crystal
Extends Entity
Index	Type	Meaning
8	Int	Health
