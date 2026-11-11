# Introduction
Description of the project

# Pseudocode

```
CLASS Hidden Markov Model 
 
    CONSTRUCTOR(initial, transitions, emissions)
        derive states from the initial-probability keys
        derive alphabet from the emission-dictionary keys
        store the probabilities
        prepare their logarithms
 
METHOD viterbi(observations)
 
    IF observations is empty
        RETURN empty path, log probability 0
 
    IF any observation is outside the model's alphabet
        REPORT an invalid observation
 
    create score table, initially negative infinity
    create previous-state table, initially NONE
 
    INITIALIZATION
        FOR each state
            score[0][state] =
                log_initial[state]
                + log_emission[state][observations[0]]
 
    RECURSION
        FOR each position t after the first
            FOR each current state
                FOR each possible preceding state
                    candidate =
                        score[t - 1][preceding state]
                        + log_transition[preceding state][current state]
                        + log_emission[current state][observations[t]]
 
                    IF candidate is better than score[t][current state]
                        store candidate as the new score
                        store preceding state as the traceback pointer
 
    TERMINATION
        choose the state with the greatest score at the last position
        save that score as the best log probability
 
        IF that score is negative infinity
            REPORT that no valid path can produce the observations
 
    TRACEBACK
        put the chosen final state at the end of the path
        follow previous-state pointers backward
        RETURN the path and best log probability
```
# Notebook Usage

# Biological, Computational, Algorithmic Considerations 

# Resemblance to Bioinformatics Tool: HMMER 

# Successes
* *Output:* We were able to successfully produce an output for each of our methods within the class. Initially, we thought about creating one Viterbi method within the ```class Hidden Markov Model:```; however for testing purposes, we wanted to construct separate methods for each step (Initialization, Recursion, Termination). This allowed us to debug and test the code as we live-coded one by one together.   
* *Live-Coding Collaboration:* We were able to conduct a live-coding session where we could understand the code and methods of the Hidden Markov Model- Viterbi Algorithm. This facilitated our learning experiences to find gaps in our learning and work collaboratively to understand difficult concepts. 

# Struggles
* *Final Output:* The final output has a 'G' in index 2 (count using zero-based indexing) rather than a 'I', despite the max number of 'I' being greater than 'G'. A possible consideration is that the Viterbi algorithm takes the cumulative logarithmic traceback value, but we are still confused by what it means.
* *Jupyter Notebook:* A limit to the Jupyter notebook is not being able to separate the methods into separate code blocks for the class. We initially wanted to try and separate them for visualization and testing purposes, but there was a constraint in doing so. 

# Personal Reflections
## Group Leader
Group leader's reflection on the project

## Other member
Other members' reflections on the project

## Other member- Kailey 
The concept of the Viterbi Algorithm within Hidden Markov Models made sense to me, but viewing the mathematical formulas and all of the various variables was a bit overwhelming initially. During lecture, it was noted that the variables will change based on the instructor and the method being taught, and that was true. A few Youtube videos and articles in, there was large variation within the variables, but the formulas and concepts remained the same. 

Collaboration was a key factor on this project. Due to a few time constraints, we were not able to meet until Tuesday, but it ended up being the most productive. We shared our pseudocode and background information and took the time to live-code together. The concept of object-oriented programming is still new and complex to me, but I found this collaborative session to be useful. I gained insight into how classes work, how a constructor functions and how to effectively test each bit of code. The meeting was long, but very informative. Each person was able to understand the concept better without having to spend extra time working on it independently. 

# Generative AI Appendix
As per the syllabus
