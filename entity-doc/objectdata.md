Object Data

In the "Spawn Object/Vehicle" packet, additional metadata about the spawned entity may be provided. The length and contents of this extra data depend on the type of object being spawned. No matter what, the server must send at least one integer to the client, although 0 is a valid value. If a number other than zero is provided (for all entities), the following data is appended to the end of the object data:
Field Name	Field Type	Example	Notes
Speed X	short	0	The speed of the object
Speed Y	short	0	The speed of the object
Speed Z	short	0	The speed of the object
Meaning of int field

Minecarts (id 10)
The int value itself specifies the minecart's functionality:
Value	Minecart functionality
0	Empty (ride-able) minecart
1	Chest minecart
2	Furnace (powered) minecart
3	TNT minecart
5	Hopper minecart
Item Frame (id 71)
Field Name	Field Type	Example	Notes
Orientation	int	3	0-3: South, West, North, East
Falling Block (id 70)
Field Name	Field Type	Example	Notes
Block Type	int	12	BlockID | (Metadata << 0x10)
Splash Potions (id 73)
Field Name	Field Type	Example	Notes
Entity ID	int	64	Potion data value
For more information on potion data values, see [1].
Fishing Float (id 90)
Field Name	Field Type	Example	Notes
Owner	int		The entity ID of the owner

Projectiles (Any projectile)
This includes ghast fireballs, arrows, and fishhooks (probably more).
Field Name	Field Type	Example	Notes
Entity ID	int	64	The entity ID of the thrower
