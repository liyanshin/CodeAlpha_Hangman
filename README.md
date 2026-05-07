import random
# ─────────────────────────────────────────────
#  TASK 1: Hangman Game
#  Goal: Text-based hangman — guess a word one
#        letter at a time (max 6 wrong guesses).
# ─────────────────────────────────────────────
WORDS = ["code", "alpha", "kalyani", "string", "laptop"]
HANGMAN_STAGES = [
    # 0 wrong guesses
    """
  +---+
  |   |
      |
      |
      |
      |
=========
""",
    # 1
    """
  +---+
  |   |
  O   |
      |
      |
      |
=========
""",
    # 2
    """
  +---+
  |   |
  O   |
  |   |
      |
      |
=========
""",
    # 3
    """
  +---+
  |   |
  O   |
 /|   |
      |
      |
=========
""",
    # 4
    """
  +---+
  |   |
  O   |
 /|\\  |
      |
      |
=========
""",
    # 5
    """
  +---+
  |   |
  O   |
 /|\\  |
 /    |
      |
=========
""",
    # 6 — game over
    """
  +---+
  |   |
  O   |
 /|\\  |
 / \\  |
      |
=========
""",
]
MAX_WRONG = 6
def display_state(word, guessed, wrong_count):
    """Print the gallows, the hidden word, and wrong-guess info."""
    print(HANGMAN_STAGES[wrong_count])
    # Show correctly guessed letters; hide the rest with '_'
    display_word = " ".join(
        letter if letter in guessed else "_" for letter in word
    )
    print(f"  Word: {display_word}")
    print(f"  Wrong guesses left: {MAX_WRONG - wrong_count}")
    print(f"  Letters guessed:    {', '.join(sorted(guessed)) or 'none'}\n")
def get_guess(guessed):
    """Prompt the player for a valid, unseen single letter."""
    while True:
        guess = input("  Guess a letter: ").strip().lower()
        if len(guess) != 1 or not guess.isalpha():
            print("Please enter a single letter.")
        elif guess in guessed:
            print(f"You already guessed '{guess}'. Try another.")
        else:
            return guess
def play():
    """Run one full game of Hangman."""
    word = random.choice(WORDS)
    guessed = set()   # all letters the player has tried
    wrong = 0         # number of incorrect guesses
    print("\n============================")
    print("   Welcome to Hangman! 🎉")
    print("============================\n")
    while wrong < MAX_WRONG:
        display_state(word, guessed, wrong)
        # Check win condition before asking for input
        if all(letter in guessed for letter in word):
            print(f"  🎉 You won! e word was '{word}'.\n")
            return
        guess = get_guess(guessed)
        guessed.add(guess)
        if guess in word:
            print(f"  ✅ '{guess}' is in the word!\n")
        else:
            wrong += 1
            print(f"  ❌ '{guess}' is NOT in the word.\n")
    # Ran out of guesses
    display_state(word, guessed, wrong)
    print(f"  💀 Game over! e word was '{word}'.\n")
def main():
    while True:
        play()
        again = input("  Play again? (y/n): ").strip().lower()
        if again != "y":
            print("\n  anks for playing! Goodbye 👋\n")
            break
if __name__ == "__main__":
    main()
