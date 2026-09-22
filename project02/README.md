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

3. Generate text from Markov Model - Get Next Word

This function is used to find the next predicted word in a given markov model using a current word and calculated probabilities.

    GET the list of options for the Markov model to choose from using the list of keys in the dictionary of the current word. This requires converting the list of dictionary keys into a list to bypass Python typing issues.

    GET the list of weights for each word by collecting the values from the dictionary of the current word. This requires converting the list of values into a list, and then into a NumPy array to bypass Python typing issues.

    CALCULATE the sum of weights.

    FIND the next predicted word using NumPy random choice based on the options and weights previously retrieved.

    RETURN the predicted next word.

4. Generate text from Markov Model - Generate Random Text

This function is used to generate a full piece of text given a trained Markov model.

    START by setting the numpy random seed to the given argument.

    INITIALIZE a list of words to store the predicted text.
    
    FIND the order of the Markov model by finding the number of words within one of the dictionary keys.

    SET a starting context using the start token \*S\* multiplied by the previously found order.

    LOOP until the end token \*E\* is predicted. Within each iteration:

        PREDICT the next word given the current context using the previously defined "Get Next Word" method.

        IF the next word is not the end token \*E\*:
            
            ADD the predicted word to the list of words.

        ADD the predicted word to the current context.
        
        REMOVE the oldest word from the current context.
    
    WHEN the end token \*E\* is added to the current context:

        END the loop.

        JOIN the predicted words using spaces into one string.

    RETURN the full predicted text.

5. All the Fish:

This cell is used to test the Markov model generation scheme implemented.

    INITIALIZE an empty dictionary as a Markov model.

    OPEN the file for one_fish_two_fish.txt.

    READ the data using open.

    FOR each individual line of the file, TRAIN the Markov model using previously implemented methods, allowing the model to learn the beginning and end of each line.

    GENERATE AND PRINT predicted text using the previously implemented methods.

6. Pick your Poison:

This cell is used to explore the application of Markov models on another text option. We chose to explore Shakespeare's sonnets.

    INITIALIZE an empty dictionary as a Markov model.

    Open the file for sonnets.txt.

    SPLIT the lines for the sonnets text by line break.

    FOR each sonnet, TRAIN the Markov model using previously implemented methods

GENERATE AND PRINT predicted text using the previously implemented methods.



# Successes
Description of the team's learning points

# Struggles
Description of the stumbling blocks the team experienced

# Personal Reflections
## Group Leader
Group leader's reflection on the project

## Other member

## Justin:

As a late joiner to this course, I used this project as a way to be introduced to the course's structure and work style. I am generally comfortable with Git and did not have too many issues with understanding the project's structure. I found the collaborative style of work meaningful and feel much more prepared going into next week.


# Generative AI Appendix
As per the syllabus