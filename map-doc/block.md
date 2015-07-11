# Block Actions
## Note Block
It shows the note particle being emitted from the block as well as playing the tone.

### Byte 1
Instrument type.

Type ID | Type Name
:-----: | :------------
0       | Harp
1       | Double Bass
2       | Snare Drum
3       | Clicks/Sticks
4       | Bass Drum

### Byte 2
The pitch of the note (between 0-24 inclusive where 0 is the lowest and 24 is the highest).

## Piston
### Byte 1
The state the piston changes to - 0 for pushing, 1 for pulling.

### Byte 2
Direction.

Direction ID | Direction
:----------: | :--------
0            | Down
1            | Up
2            | South
3            | West
4            | North
5            | East

## Chest
Animates the chest's lid opening. Note that the notchian server will send this every 3s even if the state hasn't changed.

### Byte 1
Not used - always 1

### Byte 2
State of the chest - 0 for closed, 1 for open.
