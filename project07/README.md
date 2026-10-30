# Introduction
Description of the project

# Pseudocode

```
BWT(string):
IF "$" is not in String;
  APPEND $ TO String

word_matrix = EMPTY list

FOR char in String:
  SLICE last char and MOVE TO front
  UPDATE string TO new rotation
  APPEND new rotation to word_matrix

SORT word_matrix in lexicographic order

bwt = EMPTY string

FOR EACH rotation in word_matrix:
  bwt += rotation[-1]

RETURN bwt

```

```
SUFFIX_ARRAY(string):
IF "$" is not in String;
  APPEND $ TO String

word = EMPTY list

INITIALIZE starting position INDEX[i] TO 0

FOR char in String:
  SLICE from the starting position[i] To END
  APPEND suffix AND stating position to word 
  ADD 1 TO i until "$"

SORT word in Lexicographic order

lex_sort EMPTY list

FOR EACH suffix AND index in word:
  APPEND index TO lex_sort

RETURN lex_sort
```

```
BWT_from_suffix_array(string,list[int]):
new_string = EMPTY string

FOR EACH position IN input list:
  IF position == 0:
    GET LAST character of text
  ELSE:
    GET character before the position
  ADD character TO new_string

RETURN new_string
```

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
