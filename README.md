# Fantasy Text-Based Adventure Game

This is a high-fantasy themed text-based adventure game built in Python. The player can choose their race, explore different areas, pick up items, and interact with the environment using simple text commands.

## Key Features

- **Race Selection**: At the start of the game, you can choose to play as an **elf**, **dwarf**, or **human**, each with their own unique traits.
- **Exploration**: Navigate through a series of fantasy-themed locations, such as the **Elven Forest**, **Dwarven Mine**, **Goblin Camp**, and **Dragon Cave**.
- **Inventory System**: Pick up and drop various fantasy-themed items like the **elven bow**, **dwarven axe**, **goblin dagger**, and **dragon scale**.
- **Room Descriptions**: Every room has a unique description, and you can interact with the environment by looking around.
- **Text-based Commands**: Simple commands allow you to move, pick up items, drop them, and interact with the game world.

## How to Run

1. Clone or download this repository.
2. Ensure you have Python installed on your system (version 3.6 or later).
3. Run the game with the following command:
   ```bash
   python text_adventure_game.py
   ```

## Commands

- `look`: Describes the current room and its contents.
- `go [direction]`: Moves you to a different room in the specified direction (e.g., `go north`).
- `take [item]`: Picks up an item from the room and adds it to your inventory (e.g., `take bow`).
- `drop [item]`: Drops an item from your inventory in the current room (e.g., `drop axe`).
- `inventory`: Shows all the items you're currently carrying.
- `help`: Displays a list of available commands.
- `exit`: Exits the game.

## Example Gameplay

```
Choose your race (elf, dwarf, human):
> elf
You have chosen elf.
Welcome, brave adventurer! Type 'help' for a list of commands.
You stand in the bustling village square. To the north lies the Elven Forest, to the west is the Dwarven Mine, and to the east is the dark Goblin Camp.

> look
You stand in the bustling village square. To the north lies the Elven Forest, to the west is the Dwarven Mine, and to the east is the dark Goblin Camp.

> go north
You move north.
You find yourself in a serene, mystical forest. Tall trees with glowing leaves surround you. Elven warriors patrol the area. The village square is to the south.

> take bow
You picked up the elven bow.

> inventory
You are carrying:
- elven bow

> go south
You move south.
You stand in the bustling village square. To the north lies the Elven Forest, to the west is the Dwarven Mine, and to the east is the dark Goblin Camp.

> exit
Farewell, brave adventurer!
```

## Rooms and Items

- **Village Square**: The central area with exits to other key locations.
- **Elven Forest**: A magical forest where you can find an **elven bow**.
- **Dwarven Mine**: A deep, dark mine where you can find a **dwarven axe**.
- **Goblin Camp**: A dangerous goblin-infested area where you can pick up a **goblin dagger**.
- **Dragon Cave**: The lair of a legendary dragon, where you can obtain a **dragon scale**.

## Expanding the Game

This game can be expanded by adding:
- New locations
- Additional items
- NPCs (non-player characters)
- Combat mechanics
- Story elements or quests
