import time

def print_pause(message):
    print(message)
    time.sleep(2)

def intro():
    print_pause("You find yourself in a dark forest.")
    print_pause("Rumor has it that a treasure is hidden somewhere nearby.")
    print_pause("Your mission is to find the treasure and escape alive.")

def choose_path():
    print("Where would you like to go?")
    print("1. Explore the dense forest.")
    print("2. Enter the abandoned cabin.")
    print("3. Walk along the riverbank.")
    print("4. Perform arithmetic operations.")
    choice = input("Enter the number of your choice: ")
    return choice

def explore_forest(inventory):
    print_pause("You venture into the dense forest.")
    if "magic amulet" in inventory:
        print_pause("You find nothing new here, as you've already explored this area.")
    else:
        print_pause("You discover a magic amulet on the ground. It might be useful.")
        inventory.append("magic amulet")

def enter_cabin(inventory):
    print_pause("You cautiously enter the abandoned cabin.")
    if "rusty key" in inventory:
        print_pause("There's nothing else of interest here.")
    else:
        print_pause("You find a rusty key on the table. It could open a locked door.")
        inventory.append("rusty key")

def walk_riverbank(inventory):
    print_pause("You follow the riverbank.")
    if "treasure" in inventory:
        print_pause("You've already found the treasure here.")
    elif "rusty key" in inventory:
        print_pause("You notice a locked chest hidden under some rocks.")
        print_pause("Using the rusty key, you unlock the chest and find the treasure!")
        inventory.append("treasure")
    else:
        print_pause("You see a locked chest but have no way to open it.")

def arithmetic_operations():
    print_pause("You chose to perform arithmetic operations.")
    while True:
        try:
            print("Choose an operation:")
            print("1. Addition")
            print("2. Subtraction")
            print("3. Multiplication")
            print("4. Division")
            print("5. Go back")
            operation = input("Enter the number of your choice: ")

            if operation == "5":
                print_pause("Returning to the main menu.")
                break

            num1 = float(input("Enter the first number: "))
            num2 = float(input("Enter the second number: "))

            if operation == "1":
                result = num1 + num2
                print_pause(f"The result of addition is: {result}")
            elif operation == "2":
                result = num1 - num2
                print_pause(f"The result of subtraction is: {result}")
            elif operation == "3":
                result = num1 * num2
                print_pause(f"The result of multiplication is: {result}")
            elif operation == "4":
                if num2 == 0:
                    print_pause("Error: Division by zero is not allowed.")
                else:
                    result = num1 / num2
                    print_pause(f"The result of division is: {result}")
            else:
                print_pause("Invalid choice. Please select a valid operation.")

        except ValueError:
            print_pause("Error: Invalid input. Please enter numbers only.")

def game():
    inventory = []
    intro()

    while True:
        choice = choose_path()

        if choice == "1":
            explore_forest(inventory)
        elif choice == "2":
            enter_cabin(inventory)
        elif choice == "3":
            walk_riverbank(inventory)
        elif choice == "4":
            arithmetic_operations()
        else:
            print_pause("Invalid choice. Please select a valid option.")
            continue

        if "treasure" in inventory:
            print_pause("Congratulations! You found the treasure and completed your adventure!")
            break

        print_pause("What would you like to do next?")

if __name__ == "__main__":
    game()
