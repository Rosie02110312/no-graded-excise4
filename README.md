import random 
import time

import random
import time

# Define all available cards
cards = [
    {"Name": "Diablo", "Health": 100, "Attack": 90, "Defense": 60},
    {"Name": "Medusa", "Health": 100, "Attack": 70, "Defense": 70},
    {"Name": "Jester", "Health": 120, "Attack": 60, "Defense": 90},
    {"Name": "Troll", "Health": 150, "Attack": 40, "Defense": 94},
    {"Name": "Specter", "Health": 100, "Attack": 70, "Defense": 70},
    {"Name": "Mist", "Health": 100, "Attack": 75, "Defense": 65},
    {"Name": "Savage", "Health": 100, "Attack": 90, "Defense": 50},
    {"Name": "Marauder", "Health": 100, "Attack": 85, "Defense": 50},
    {"Name": "Wimp", "Health": 110, "Attack": 40, "Defense": 85},
    {"Name": "Sorcerer", "Health": 100, "Attack": 70, "Defense": 55},
]

def assign_random_decks():
    player_deck = random.sample(cards, 5)
    opponent_deck = random.sample([card for card in cards if card not in player_deck], 5)
    return player_deck, opponent_deck

def display_deck(deck):
    for i, card in enumerate(deck):
        print(f"{i+1}. {card['Name']} - Health: {card['Health']}, Attack: {card['Attack']}, Defense: {card['Defense']}")

def battle(player_card, opponent_card):
    print(f"\nYour {player_card['Name']} attacks opponent's {opponent_card['Name']}!")
    # Player's attack reduces opponent's defense first
    if player_card['Attack'] > opponent_card['Defense']:
        damage_to_health = player_card['Attack'] - opponent_card['Defense']
        opponent_card['Defense'] = 0
        opponent_card['Health'] -= damage_to_health
        print(f"Your attack broke through! {opponent_card['Name']} loses {damage_to_health} health.")
    else:
        opponent_card['Defense'] -= player_card['Attack']
        print(f"Your attack weakened {opponent_card['Name']}! Its defense drops to {opponent_card['Defense']}.")

    if opponent_card['Health'] <= 0:
        print(f"{opponent_card['Name']} has been defeated!")
        return True  # Opponent card defeated
    else:
        print(f"{opponent_card['Name']} now has {opponent_card['Health']} health and {opponent_card['Defense']} defense.")
        return False  # Battle continues

def game():
    player_deck, opponent_deck = assign_random_decks()
    
    print("Welcome to the Card RPG Game!")
    print("Your deck:")
    display_deck(player_deck)
    
    while player_deck and opponent_deck:
        print("\nOpponent's turn...")
        time.sleep(2)
        opponent_card = random.choice(opponent_deck)
        print(f"Opponent has chosen: {opponent_card['Name']} - Health: {opponent_card['Health']}, Attack: {opponent_card['Attack']}, Defense: {opponent_card['Defense']}")
        
        print("\nYour turn! Choose a card from your deck:")
        display_deck(player_deck)
        
        player_choice = int(input("Choose a card (1-5): ")) - 1
        player_card = player_deck[player_choice]

        if battle(player_card, opponent_card):
            opponent_deck.remove(opponent_card)
        
        if not opponent_deck:
            print("\nCongratulations! You have defeated all opponent's cards!")
            break
        

        print("\nNow it's the opponent's turn to attack!")
        time.sleep(2)
        opponent_card = random.choice(opponent_deck)
        
        print(f"Opponent attacks with {opponent_card['Name']}!")
        display_deck(player_deck)
        player_choice = int(input("Choose a card to defend (1-5): ")) - 1
        player_card = player_deck[player_choice]
        
        if battle(opponent_card, player_card):
            player_deck.remove(player_card)

        if not player_deck:
            print("\nSorry, you have lost all your cards. The opponent wins!")
            break
        
        print("\nA new round begins!")
        time.sleep(3)

game()
