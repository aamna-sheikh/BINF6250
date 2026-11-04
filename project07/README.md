# Introduction
In this project, we explored the Burrows-Wheeler Transform (BWT) and how it can be used for pattern matching and data compression, especially in bioinformatics. We started by building the BWT using rotations of a string, then explored how suffix arrays could be used to construct the same result without creating the entire rotation matrix.

From there, we worked with count and occurrence dictionaries to understand how backward searching works. We used these dictionaries to update the search boundaries and find where a query appears in the original reference sequence. We also implemented run-length encoding and decoding to explore how repeated characters can be compressed and reconstructed.

Throughout the project, we worked together to develop pseudocode, translate it into Python, and test our functions. This helped us better understand how the different parts of the algorithm connect and how indexing and data structures can make searching more efficient.

# A few things to keep in mind:
- We used inclusive occurrence counts in cal_occur(), meaning the count at each index includes the character at that position.
- For update_range() and find_match(), we used half-open search ranges, meaning the lower boundary is included and the upper boundary is excluded.
- Our run-length encoding stores the character followed by its count (e.g., aaabb becomes a3b2).

# Pseudocode

def BWT()
```
IF "$" is not in String:
  APPEND $ TO string

CREATE an empty list called word_matrix

FOR EACH char in string:
  SLICE last char and MOVE TO front
  UPDATE string TO new rotation
  APPEND new rotation to word_matrix

SORT word_matrix in lexicographic order

bwt = EMPTY string

FOR EACH rotation in word_matrix:
  GET the last character of the rotation
  ADD the character to bwt

RETURN bwt
```



def suffix_array()
```
IF "$" is not in string:
  APPEND $ TO en of string

CREATE an empty list called word

INITIALIZE starting position INDEX[i] TO 0

FOR EACH char in string:
  SLICE from the starting position[i] To END
  APPEND suffix AND stating position i to word 
  ADD 1 TO i

SORT word in Lexicographic order

CREATE an empty list called lex_sort

FOR EACH suffix AND index in word:
  APPEND index TO lex_sort

RETURN lex_sort
```



def BWT_from_suffix_array()
```
create an empty string called new_string

FOR EACH position IN input list:

  IF position == 0:
    GET LAST character of text

  ELSE:
    GET the character at position - 1 in text

  ADD character TO new_string

RETURN new_string
```



def cal_count()
```
CONVERT string TO list called word

SORT list in lexicographic order

CREATE an empty dictionary called count_dict

INITIALIZE starting position INDEX[i] TO 0

  FOR EACH char in sorted list:

  IF char is not already in count_dict:
    ADD char and its starting position i to count_dict

  ADD 1 TO i until all char in list have been processed

RETURN count_dict
```



def cal_occur()
```
  CONVERT bwt_string TO list called word
  SORT list in lexicographic order

  CREATE an empty dictionary called occur_dict

  FOR EACH char in sorted list:

    IF char is not already in the dict
    INITIALIZE count TO 0

      CREATE an empty list called occurrences

  FOR EACH character in original string

    IF character matches char:
    INCREASE count by 1

    APPEND count TO occurrences
  
      ADD char and occurences to occur_dict 

RETURN occur_dict
```
note for cal_occur: We noticed that the second example in the cal_occur() docstring has some incorrect occurrence counts. We followed the provided pseudocode and used inclusive running counts, which match the first example and correctly track how many times each character appears at every position.



def update_range()
```
Input: The current lower and upper boundaries, the count and occurrence dictionaries, and the character being processed.

Output: A tuple containing the updated lower and upper boundaries of the search range.

    IF character a is not in count OR occur:
        RETURN empty range (1, 0)

    IF lower is greater than or equal to upper:
        RETURN empty range (1, 0)

    GET the starting position of character a from count
    STORE this as character_start

    IF lower is 0:
        SET start_occurrences TO 0
        (no characters before the starting position)

    ELSE:
        GET the number of times character a appears
        BEFORE the lower boundary using occur[a][lower - 1]
        STORE this as start_occurrences

    IF upper is 0:
      SET end_occurences TO 0

    ELSE:
      GET the number of times character a appears
      UP TO AND INCLUDING the upper boundary
      STORE this as end_occurrences

    CALCULATE new_start by ADDING character_start,
    start_occurrences, and 1 for the $ row

    CALCULATE new_end by ADDING character_start
    and end_occurrences

    RETURN new_start and new_end as a tuple
```



def findmatch()
```
    IF query is empty:
      RETURN empty list

    IF "$" is not in reference:
      APPEND "$" to the end of the reference

    CREATE the suffix array from reference
    STORE this as build_suffix

    CREATE the BWT using reference and build_suffix
    STORE this as build_bwt

    CREATE the count dictionary using build_bwt
    STORE this as build_count_table

    CREATE the occurrence dictionary using build_bwt
    STORE this as build_occurrence_table

    SET lower to 0
    SET upper TO the length of build_bwt

    FOR each character in query from right to left:

      UPDATE lower and upper using update_range and the count and occurrence dictionaries

      IF lower is greater than or equal to upper:
        RETURN an empty list

  GET the suffix array positions from lower up to but not including upper
  STORE these as matching_positions

  SORT matching_positions in ascending order

  RETURN matching_positions
```



def run_length_encode()
```
  CREATE an empty string called encoded_text

    IF bwt_string is empty:
        RETURN encoded_text

    GET the first character of bwt_string
    STORE this as current_character

    FOR EACH character in bwt_string starting from index 1:

        IF character matches current_character:
            ADD 1 TO count

        ELSE:
            ADD current_character followed by count to encoded_text
            UPDATE current_character to the new character
            SET count TO 1

    ADD current_character followed by count to encoded_text

    RETURN encoded_text
```



def run_length_decode()
```
    CREATE an empty string called decoded_text
    SET i to 0

    WHILE i is less than the length of the endcoded text:

        GET the character at index i
        STORE this as character

        ADD 1 TO i

        CREATE an empty string called count_string

        WHILE i is less than the length of encoded
        AND the character at index i is a digit:

            ADD the digit to count_string
            ADD 1 TO i

        CONVERT count_string into an integer
        STORE this as count

        REPEAT character count times
        ADD the repeated characters to decoded_text

    RETURN decoded_text
```
# Tests and Results:

We tested our functions using different input strings to make sure they produced the expected results. We also included cases where a query was not found and checked that run-length decoding reconstructed the original string.

Some of our test results included:
- BWT: "banana$" returned "annb$aa"
- suffix_array: "banana$" returned [6, 5, 3, 1, 0, 4, 2]
- BWT_from_suffix_array: "banana$" and its suffix array returned "annb$aa"
- find_match: "ana" in "banana" returned [1, 3]
- find_match: "xyz" in "banana" returned []
- run_length_encode: "aaabbc" returned "a3b2c1"
- run_length_decode: "a3b2c1" returned "aaabbc"

The BWT and suffix array functions produced the expected results, and our pattern-matching function was able to identify the starting positions of a query in the reference string. We also tested update_range() using different boundary conditions, including cases with no matches, to make sure the search ranges were being updated correctly.

# Successes
One of our biggest successes was developing a better understanding of the Burrows-Wheeler Transform and how suffix arrays can be used for pattern matching. This project was a good opportunity to build on what we have been learning in class and apply it to a more complicated algorithm.

Another success was getting all nine functions to work together and produce the expected results. The provided walkthrough was extremely helpful in understanding how to build the different functions and seeing how they all connected. The provided pseudocode was also helpful and gave us a good starting point when translating the steps into Python.

We also worked together to troubleshoot issues, compare our approaches, and talk through the parts we found confusing. I think having these discussions and explaining our understanding to each other helped us both develop a clearer picture of the algorithm and how all the different pieces fit together.

# Struggles
I think our biggest challenge was understanding and implementing the update_range() function, especially figuring out how the lower and upper boundaries were being calculated. We spent quite a bit of time going back and forth on this because even when the results looked correct, we weren't always sure if we were following the right logic.

We found ourselves going back to the walkthrough quite a few times to better understand how the calculations worked. Testing different inputs was also helpful because it allowed us to see how changing the boundaries affected the results.

Another challenge was making sure all our functions were consistent with each other. We realized that the way we handled the occurrence counts and search boundaries in one function affected how the next function worked. Working through these issues together helped us better understand how the different parts of the algorithm connected, even though it took us longer than expected.

# Personal Reflections
## Group Leader
I think one of my biggest takeaways from this project was realizing how important it is to really understand the foundation of an algorithm before moving forward. Graziano and I spent about four hours on our first call just talking through the functions, and I think we only managed to get through the first two during that time. But looking back, I think that was probably one of the most valuable parts of the project (and class!) because it gave us a much better understanding of the concepts we needed for the remaining functions.

I also found it a little frustrating how often we had to go back to our previous functions when something wasn't working. At first, I kept thinking the issue was with the function we were currently working on, but I started realizing how much each function depended on the others. As frustrating as that was, I think it was a really good learning experience, and I feel like I've gotten a lot better at debugging and knowing where to look when something goes wrong.

Overall, I enjoyed working with Graziano and appreciated that we could spend time talking through our ideas and questioning things together. I think this project taught me just as much about patience and problem-solving as it did about the BWT algorithm itself.

## Member Graziano 
**Issues** I believe the biggest struggle was the understanding of the function update range. That is a big piece of the homework and it defines upper and lower bound. The calculation that will be the center of the encoding and decoding.

**What I Learn** While working on the project, we work as a team to resolve the pseudocode and understand to goal of the function and how each part was build. We for sure improved our reasoning skills and learn with algorithm.

**Next steps** I would like to keep building on this knowledge and how to use the Burrows-Wheeler Transform on different scenarios and problems. I also want to keep improving my ability to understand complex algorithms and its implementations.

# Future Improvements:

One thing we would like to explore further is how the BWT and suffix arrays can be implemented more efficiently, especially when working with much larger sequences. Our current approach helped us understand the steps behind the algorithms, but we recognize that building and sorting all the rotations may not be practical for larger genomic datasets.

# Generative AI Appendix
1. Description of which generative AI was used and its version.
Claude Opus 5, ChatGPT-6

2. The entire prompt that was used to generate the content.
Can you help me to fix my pseudocode to a the github format? I need a better explanation of the Burrows-Wheeler Transform(BWT) algorithm and its applications in bioinformatics.

3. An explanation of how it was used (e.g., to generate ideas). 
We used generative AI as an additional learning tool to better understand the concepts covered in the course materials. It was especially helpful when we needed further clarification on certain parts of Marcus's walkthrough and wanted to understand how the different functions connected. We also used it to generate additional test cases when we weren't sure whether our functions were working correctly, and to help organize and clean up our README.

4. A justification for why generative AI was used.
Sometimes just reading a concept or formula isn't enough to fully understand how it works. We used AI to ask follow-up questions, work through examples, and better understand the logic behind the algorithms. It was also helpful for troubleshooting and checking our work, especially when we wanted to test different inputs or make our explanations clearer. I am also very new to all this terminology so I used it to check if I was accurately using a term.
