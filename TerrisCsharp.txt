using System;
using System.Collections.Generic;

class TextAdventureGame
{
    static Dictionary<string, Room> rooms = new Dictionary<string, Room>();
    static List<string> inventory = new List<string>();
    static Room currentRoom;
    static string playerRace;

    static void Main(string[] args)
    {
        ChooseRace();
        InitializeRooms();

        Console.WriteLine("Welcome, brave adventurer! Type 'help' for a list of commands.");
        Console.WriteLine(currentRoom.Description);

        while (true)
        {
            Console.Write("\n> ");
            string input = Console.ReadLine().ToLower().Trim();
            ProcessCommand(input);
        }
    }

    static void ChooseRace()
    {
        Console.WriteLine("Choose your race (elf, dwarf, human):");
        while (true)
        {
            playerRace = Console.ReadLine().ToLower();
            if (playerRace == "elf" || playerRace == "dwarf" || playerRace == "human")
            {
                Console.WriteLine($"You have chosen {playerRace}.");
                break;
            }
            else
            {
                Console.WriteLine("Invalid choice. Please choose 'elf', 'dwarf', or 'human'.");
            }
        }
    }

    static void InitializeRooms()
    {
        // Create rooms
        Room villageSquare = new Room("Village Square", "You stand in the bustling village square. To the north lies the Elven Forest, to the west is the Dwarven Mine, and to the east is the dark Goblin Camp.");
        Room elvenForest = new Room("Elven Forest", "You find yourself in a serene, mystical forest. Tall trees with glowing leaves surround you. Elven warriors patrol the area. The village square is to the south.");
        Room dwarvenMine = new Room("Dwarven Mine", "You are in the depths of a dwarven mine. The air smells of earth and iron. Dwarves labor tirelessly, mining precious ores. The village square is to the east.");
        Room goblinCamp = new Room("Goblin Camp", "You have entered a chaotic goblin camp. Fires crackle, and goblins chatter wildly. It feels dangerous here. The village square is to the west.");
        Room dragonCave = new Room("Dragon Cave", "You stand before the legendary Dragon's Lair. The air is thick with the smell of sulfur, and the ground trembles beneath your feet. This is no place for the faint of heart.");

        // Link rooms together
        villageSquare.AddExit("north", elvenForest);
        villageSquare.AddExit("west", dwarvenMine);
        villageSquare.AddExit("east", goblinCamp);
        elvenForest.AddExit("south", villageSquare);
        dwarvenMine.AddExit("east", villageSquare);
        goblinCamp.AddExit("west", villageSquare);
        goblinCamp.AddExit("north", dragonCave);
        dragonCave.AddExit("south", goblinCamp);

        // Add items
        elvenForest.Items.Add("elven bow", "A finely crafted bow made from enchanted wood.");
        dwarvenMine.Items.Add("dwarven axe", "A heavy, double-bladed axe forged by master dwarven smiths.");
        goblinCamp.Items.Add("goblin dagger", "A crude, yet sharp, dagger used by goblins.");
        dragonCave.Items.Add("dragon scale", "A shimmering scale, plucked from a dragon's hide.");

        // Set starting room
        currentRoom = villageSquare;

        // Add rooms to dictionary
        rooms.Add("Village Square", villageSquare);
        rooms.Add("Elven Forest", elvenForest);
        rooms.Add("Dwarven Mine", dwarvenMine);
        rooms.Add("Goblin Camp", goblinCamp);
        rooms.Add("Dragon Cave", dragonCave);
    }

    static void ProcessCommand(string input)
    {
        string[] command = input.Split(' ');
        switch (command[0])
        {
            case "look":
                Console.WriteLine(currentRoom.Description);
                break;

            case "go":
                if (command.Length > 1)
                {
                    Move(command[1]);
                }
                else
                {
                    Console.WriteLine("Go where?");
                }
                break;

            case "take":
                if (command.Length > 1)
                {
                    TakeItem(command[1]);
                }
                else
                {
                    Console.WriteLine("Take what?");
                }
                break;

            case "drop":
                if (command.Length > 1)
                {
                    DropItem(command[1]);
                }
                else
                {
                    Console.WriteLine("Drop what?");
                }
                break;

            case "inventory":
                ShowInventory();
                break;

            case "help":
                ShowHelp();
                break;

            case "exit":
                Console.WriteLine("Farewell, brave adventurer!");
                Environment.Exit(0);
                break;

            default:
                Console.WriteLine("Unknown command. Type 'help' for a list of commands.");
                break;
        }
    }

    static void Move(string direction)
    {
        if (currentRoom.Exits.ContainsKey(direction))
        {
            currentRoom = currentRoom.Exits[direction];
            Console.WriteLine($"You move {direction}.");
            Console.WriteLine(currentRoom.Description);
        }
        else
        {
            Console.WriteLine("You can't go that way.");
        }
    }

    static void TakeItem(string item)
    {
        if (currentRoom.Items.ContainsKey(item))
        {
            inventory.Add(item);
            Console.WriteLine($"You picked up the {item}.");
            currentRoom.Items.Remove(item);
        }
        else
        {
            Console.WriteLine("There is no such item here.");
        }
    }

    static void DropItem(string item)
    {
        if (inventory.Contains(item))
        {
            inventory.Remove(item);
            currentRoom.Items.Add(item, $"The {item} is now lying on the ground.");
            Console.WriteLine($"You dropped the {item}.");
        }
        else
        {
            Console.WriteLine("You don't have that item.");
        }
    }

    static void ShowInventory()
    {
        if (inventory.Count > 0)
        {
            Console.WriteLine("You are carrying:");
            foreach (string item in inventory)
            {
                Console.WriteLine($"- {item}");
            }
        }
        else
        {
            Console.WriteLine("You are not carrying anything.");
        }
    }

    static void ShowHelp()
    {
        Console.WriteLine("Available commands:");
        Console.WriteLine("- look: Look around the room.");
        Console.WriteLine("- go [direction]: Move in a direction (north, south, east, west).");
        Console.WriteLine("- take [item]: Pick up an item.");
        Console.WriteLine("- drop [item]: Drop an item.");
        Console.WriteLine("- inventory: Check your inventory.");
        Console.WriteLine("- help: Show this help message.");
        Console.WriteLine("- exit: Exit the game.");
    }
}

class Room
{
    public string Name { get; set; }
    public string Description { get; set; }
    public Dictionary<string, Room> Exits { get; set; }
    public Dictionary<string, string> Items { get; set; }

    public Room(string name, string description)
    {
        Name = name;
        Description = description;
        Exits = new Dictionary<string, Room>();
        Items = new Dictionary<string, string>();
    }

    public void AddExit(string direction, Room room)
    {
        Exits[direction] = room;
    }
}