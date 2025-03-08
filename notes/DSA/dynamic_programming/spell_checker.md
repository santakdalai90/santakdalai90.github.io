# Problem Statement [WIP]
Given a word, check if it is spelled correctly. If not then suggest corrections.

# Logic
We can leverage the logic from edit distance problem here. 
## Operation allowed
1. Insertion
2. Deletion
3. Replacement
4. Transpose

We need a collection of valid words to check whether an input word is valid or not.

We build a dictionary using a text corpus. In the dictionary we store the word along with its frequency of occurrences in the text corpus.
Frequency is used to establish likelihood of the suggested corrections.




