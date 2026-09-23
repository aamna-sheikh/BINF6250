# Introduction
This project introduces Markov chains by building a text generation model in several steps. We first created a first-order model that tracks which words follow each other, then expanded it into an Nth-order model that can use multiple previous words as a state. We then used transition frequencies to calculate probabilities and randomly select words to generate new text. Finally, we tested an order-2 model on the full One Fish Two Fish text and then applied the same process to Shakespeare’s sonnets as a larger and more complex text.

# Pseudocode
1. Train Markov Model - First-Order

This function builds a first-order Markov model by looking at each word in the inputted text and the word that comes directly after it. It keeps track of these word pairs in a dictionary of dictionaries and counts how many times each transition appears. Artificial start and end states are also added so the model can keep track of where the text begins and ends.

```
IF a Markov model was not provided:
    CREATE an empty Markov model

BREAK inputted text into individual words

ADD *S* before the text and *E* after the text

LOOP through the words one position at a time:
    current_word = the word at the current position
    next_word = the word directly after it

    IF current_word is not in the model:
        ADD current_word to the model

    IF next_word has not been seen after current_word:
        START its transition count at 0

    ADD 1 to the transition count

RETURN the completed Markov model
```

# Example Output:

```

{'*S*': {'one': 1}, 'one': {'fish': 1}, 'fish': {'two': 1, 'red': 1, 'blue': 1, '*E*': 1}, 'two': {'fish': 1}, 'red': {'fish': 1}, 'blue': {'fish': 1}}
 
 ```

2. Nth order Markov Chain

This function expands the first-order Markov model so that the current state can be based on more than one word. The `order` determines how many previous words are used as the current state. First-order states are stored as individual words, while higher-order states are stored as tuples. The model then keeps track of the next word and counts how many times each transition appears.

```
IF a Markov model was not provided:
    CREATE an empty Markov model

BREAK the inputted text into individual words

ADD *S* states based on the order and ADD *E* at the end

LOOP through the words one position at a time:

    IF order is 1:
        current_state = the word at the current position
    OTHERWISE:
        current_state = the group of previous words as a tuple

    next_word = the word directly after the current state

    IF current_state is not in the model:
        ADD current_state to the model

    IF next_word has not been seen after current_state:
        START its transition count at 0

    ADD 1 to the transition count

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

This function is used to find the next predicted word in a given Markov model using a current word and calculated probabilities.

get all the possible next words from the current state

ADD the transition counts to find the total number of transitions

FOR each possible next word:
    calculate its probability and store the probability

IF a seed is provided:
    SET the random seed

RANDOMLY select the next word based on the probabilities

RETURN the selected word

# Example Test and Output:

```
# Test get_next_word using the fish transitions
test_model = {"fish": {"two": 1,"red": 1,"blue": 1,"*E*": 1}}

selected_word = get_next_word("fish", test_model, seed=42)

print("Starting word: fish")
print("Word selected with seed 42:", selected_word)

results:
Starting word: fish
Word selected with seed 42: red
 ```

4. Generate text from Markov Model - Generate Random Text

SET the random seed

get the first state from the maarkov model

IF the first state contains multiple words:
    find the order of the model and set the starting state based on the order
OTHERWISE:
    set the starting state to *S*

START with an empty sentence

LOOP until *E* is selected:
    get the next word from the current state

    IF the next word is *E*:
        STOP the loop

    ADD the next word to the sentence
    update the current state

join the generated words together

RETURN the completed sentence


5. All the Fish:

This cell is used to test the Markov model generation scheme implemented.

    INITIALIZE an empty dictionary as a Markov model.

    OPEN the file for one_fish_two_fish.txt.

    READ the data using open.

    FOR each individual line of the file, TRAIN the Markov model using previously implemented methods, allowing the model to learn the beginning and end of each line.

call generate_random_text on the trained model and print the results


6. Pick your Poison:

This cell is used to explore the application of Markov models on another text option. We chose to explore Shakespeare's sonnets.

    INITIALIZE an empty dictionary as a Markov model.

    Open the file for sonnets.txt.

    SPLIT the lines for the sonnets text by line break.

    FOR each sonnet, TRAIN the Markov model using previously implemented methods

call generate_random_text on the trained model and print the results


# Successes
- Despite all the changes to our group, we were still able to meet a few times face-to-face and talk through the structure of the project and our different approaches to the code.

- Having people join and leave changed how the project progressed, but we were able to adjust and keep moving. It also gave us some experience bringing new people up to speed quickly, which is something we will probably run into often when collaborating in research.

- Comparing our different approaches gave us more practice with GitHub and peer review, especially when deciding which parts of each person's code made the most sense to use in the final version.

# Struggles
- Communication was probably our biggest struggle. We had one member who was not able to participate and another member join the class late. We also had different schedules and time zones to work around.

- Since our final group came together late, we had to work pretty quickly to draft, compare, and implement our code. This took away some of the time we could have spent working together on the actual text generator.

- We got through the main functions, but our documentation could still be improved. We would have liked a cleaner and more consistent structure for showing each function, its input and output, the driver code, and the result.

# Personal Reflections
## Group Leader - Aamna

This was my first time working with Markov chains, so it took me a while at the beginning just to understand the logic and work through the first function. Once I understood how the states and transitions were being stored, the later functions started to make a lot more sense. Justin had more experience with Markov chains, so it was helpful to compare our approaches and learn from each other rather than just choosing whichever code worked. He showed me how to split the sonnets at the blank lines so the model could treat each sonnet separately, which was something I had not thought of doing.

This was also my first time being the project leader, and I think seeing the GitHub workflow from that side really helped with my understanding. I am still figuring out the most efficient way to merge changes. I noticed that I often ended up editing the final code myself after reviewing everyone's work rather than actually merging the changes through GitHub. That is something I want to get more comfortable with and do better on the next project.

One thing I did not expect to learn was how important it is to bring people up to speed quickly. With our group changing throughout the project, I realized this is probably something I will run into a lot in research and collaboration. It is a skill I did not really think about needing before this project, but I can see how important it is now.

## Other member

## Justin:

As a late joiner to this course, I used this project as a way to be introduced to the course's structure and work style. I am generally comfortable with Git and did not have too many issues with understanding the project's structure. I found the collaborative style of work meaningful and feel much more prepared going into next week.

# Generative AI Appendix
Aamna:
ChatGPT (GPT-5.6 Sol) was used as a learning and documentation aid during this project.

Understanding Markov models
I asked, “Can you explain how first order and nth order markov models work and what the difference is between them? explain them in the context of a computer scientist first and then a computational biologist using a biological example” to grasp a better understanding of the topic at large. I am new to programming as well as algorithms so youtube videos and walk through examples have helped me a lot.

Understanding Random Seed
I asked, “how does a random seed work in context to Markov models? why would we want to set in a specific random seed when the point of this function is to generate randomized text?” I used this to understand the logic behind setting a seed and its need when testing.

Pseudocode and Commenting
I asked, “can you help me make the pseudocode blocks match stylistically and make it less wordy? I originally wrote this out when I was still wrapping my head around the concept of Markov chains and felt as though the pseudocode was too wordy. It was able to quickly point out the areas that needed to be shortened although I did not get enough time to implement all of the suggestions it made.
