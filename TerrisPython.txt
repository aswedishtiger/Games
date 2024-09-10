class Room:
    def __init__(self, name, description):
        self.name = name
        self.description = description
        self.exits = {}
        self.items = {}

    def add_exit(self, direction, room):
        self.exits[direction] = room

class TextAdventureGame:
    def __init__(self):
        self.inventory = []
        self.current_room = None
        self.player_race = None
        self.rooms = {}
        self.initialize_rooms()

    def start(self):
        self.choose_race()
        print("Welcome, brave adventurer! Type 'help' for a list of commands.")
        print(self.current_room.description)

        while True:
            command = input("\n> ").lower().strip()
            self.process_command(command)

    def choose_race(self):
        print("Choose your race (elf, dwarf, human):")
        while True:
            race = input("> ").lower()
            if race in ["elf", "dwarf", "human"]:
                self.player_race = race
                print(f"You have chosen {self.player_race}.")
                break
            else:
                print("Invalid choice. Please choose 'elf', 'dwarf', or 'human'.")

    def initialize_rooms(self):
        # Create rooms
        village_square = Room("Village Square", "You stand in the bustling village square. To the north lies the Elven Forest, to the west is the Dwarven Mine, and to the east is the dark Goblin Camp.")
        elven_forest = Room("Elven Forest", "You find yourself in a serene, mystical forest. Tall trees with glowing leaves surround you. Elven warriors patrol the area. The village square is to the south.")
        dwarven_mine = Room("Dwarven Mine", "You are in the depths of a dwarven mine. The air smells of earth and iron. Dwarves labor tirelessly, mining precious ores. The village square is to the east.")
        goblin_camp = Room("Goblin Camp", "You have entered a chaotic goblin camp. Fires crackle, and goblins chatter wildly. It feels dangerous here. The village square is to the west.")
        dragon_cave = Room("Dragon Cave", "You stand before the legendary Dragon's Lair. The air is thick with the smell of sulfur, and the ground trembles beneath your feet. This is no place for the faint of heart.")

        # Link rooms
        village_square.add_exit("north", elven_forest)
        village_square.add_exit("west", dwarven_mine)
        village_square.add_exit("east", goblin_camp)
        elven_forest.add_exit("south", village_square)
        dwarven_mine.add_exit("east", village_square)
        goblin_camp.add_exit("west", village_square)
        goblin_camp.add_exit("north", dragon_cave)
        dragon_cave.add_exit("south", goblin_camp)

        # Add items to rooms
        elven_forest.items["elven bow"] = "A finely crafted bow made from enchanted wood."
        dwarven_mine.items["dwarven axe"] = "A heavy, double-bladed axe forged by master dwarven smiths."
        goblin_camp.items["goblin dagger"] = "A crude, yet sharp, dagger used by goblins."
        dragon_cave.items["dragon scale"] = "A shimmering scale, plucked from a dragon's hide."

        # Set starting room
        self.current_room = village_square

        # Store rooms in a dictionary
        self.rooms = {
            "Village Square": village_square,
            "Elven Forest": elven_forest,
            "Dwarven Mine": dwarven_mine,
            "Goblin Camp": goblin_camp,
            "Dragon Cave": dragon_cave
        }

    def process_command(self, input):
        command = input.split()
        if len(command) == 0:
            return

        action = command[0]
        if action == "look":
            self.look_around()
        elif action == "go" and len(command) > 1:
            self.move(command[1])
        elif action == "take" and len(command) > 1:
            self.take_item(command[1])
        elif action == "drop" and len(command) > 1:
            self.drop_item(command[1])
        elif action == "inventory":
            self.show_inventory()
        elif action == "help":
            self.show_help()
        elif action == "exit":
            print("Farewell, brave adventurer!")
            exit()
        else:
            print("Unknown command. Type 'help' for a list of commands.")

    def look_around(self):
        print(self.current_room.description)

    def move(self, direction):
        if direction in self.current_room.exits:
            self.current_room = self.current_room.exits[direction]
            print(f"You move {direction}.")
            print(self.current_room.description)
        else:
            print("You can't go that way.")

    def take_item(self, item):
        if item in self.current_room.items:
            self.inventory.append(item)
            print(f"You picked up the {item}.")
            del self.current_room.items[item]
        else:
            print("There is no such item here.")

    def drop_item(self, item):
        if item in self.inventory:
            self.inventory.remove(item)
            self.current_room.items[item] = f"The {item} is now lying on the ground."
            print(f"You dropped the {item}.")
        else:
            print("You don't have that item.")

    def show_inventory(self):
        if len(self.inventory) > 0:
            print("You are carrying:")
            for item in self.inventory:
                print(f"- {item}")
        else:
            print("You are not carrying anything.")

    def show_help(self):
        print("Available commands:")
        print("- look: Look around the room.")
        print("- go [direction]: Move in a direction (north, south, east, west).")
        print("- take [item]: Pick up an item.")
        print("- drop [item]: Drop an item.")
        print("- inventory: Check your inventory.")
        print("- help: Show this help message.")
        print("- exit: Exit the game.")

# Run the game
if __name__ == "__main__":
    game = TextAdventureGame()
    game.start()