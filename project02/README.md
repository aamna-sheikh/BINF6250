# Introduction
Description of the project

# Pseudocode
1. Train Markov Model - First-Order

This function builds a first-order Markov model by looking at each word in the inputted text and the word that comes directly after it. It keeps track of these word pairs in a dictionary of dictionaries and counts how many times each transition appears. Artificial start and end states are also added so the model can keep track of where the text begins and ends.

```
START with the inputted text and break it up into individual words

ADD *S* before the first word to represent the start of the text
ADD *E* after the last word to represent the end of the text

LOOP through the words one position at a time:

    current_word = the word at the current position
    next_word = the word directly after it

    CHECK if current_word is already in the Markov model
        IF NOT:
            ADD current_word to the model with an empty dictionary where its possible next words can be stored

    CHECK if next_word has already been seen after current_word
        IF NOT:
            ADD the word pair to the model and start its transition count at 0

    ADD 1 to the transition count for the current_word to next_word pair each time that same pair is seen in the text

RETURN the completed Markov model
```

# Example Output:

```

{'*S*': {'one': 1}, 'one': {'fish': 1}, 'fish': {'two': 1, 'red': 1, 'blue': 1, '*E*': 1}, 'two': {'fish': 1}, 'red': {'fish': 1} 'blue': {'fish': 1}}
 
 ```

2. Nth order Markov Chain

This function expands the first-order Markov model so that the current state can be based on more than one word. The `order` determines how many previous words are used as the current state. First-order states are stored as individual words, while higher-order states are stored as tuples. The model then keeps track of the next word and counts how many times each transition appears.

```
START with the inputted text and break it up into individual words

ADD *S* before the first word to represent the start of the text
ADD *E* after the last word to represent the end of the text

LOOP through the words one position at a time:

    IF the order is 1:
        current_state = the word at the current position

    OTHERWISE:
        current_state = the group of words based on the order and store them together as a tuple

    next_word = the word directly after the current_state

    CHECK if current_state is already in the Markov model
        IF NOT:
            ADD current_state to the model with an empty dictionary where its possible next words can be stored

    CHECK if next_word has already been seen after current_state
        IF NOT:
            ADD the state and next word pair to the model and start its transition count at 0

    ADD 1 to the transition count for the current_state to next_word pair each time that same transition is seen in the text

RETURN the completed Markov model
```
Note: The provided test uses the text "one fish two fish red fish blue red fish blue", but the expected output appears to correspond to "one fish two fish red fish blue fish". Because of this, my output includes the additional transitions found in the provided text. Since the assignment instructions state that the driver program should work without any alterations, I kept the provided text unchanged rather than modifying it to match the expected output.

# Example Output:

```

{('*S*', '*S*'): {'one': 1},
 ('*S*', 'one'): {'fish': 1},
 ('one', 'fish'): {'two': 1},
 ('fish', 'two'): {'fish': 1},
 ('two', 'fish'): {'red': 1},
 ('fish', 'red'): {'fish': 1},
 ('red', 'fish'): {'blue': 2},
 ('fish', 'blue'): {'red': 1, '*E*': 1},
 ('blue', 'red'): {'fish': 1}}
 
 ```

3. Generate text from Markov Model



# Successes
Description of the team's learning points

# Struggles
Description of the stumbling blocks the team experienced

# Personal Reflections
## Group Leader
Group leader's reflection on the project

## Other member
Other members' reflections on the project

# Generative AI Appendix
As per the syllabus
