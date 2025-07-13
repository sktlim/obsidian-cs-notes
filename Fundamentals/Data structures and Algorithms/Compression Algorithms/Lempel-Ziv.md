## LZ77
> Lossless data compression algorithm based on **sliding window compression**
> **Key Idea:** Replace repeated occurrences of data with references to a single copy of data in the past

### How it works
LZ77 maintains a **sliding window** over the input data. It looks for the **longest match** of the upcoming data in a portion of data already seen (i.e. in the sliding window). If a match is found, it replaces the match with a **(distance, length)** pair, and otherwise, it outputs the **literal character**.

### Pseudocode
```python
window = ""         # sliding window (initially empty)
input_stream = ...  # the full input string to compress

while input_stream is not empty:
    match = find_longest_match(window, input_stream)

    if match exists:
        d = distance_to_start_of_match(match, window)
        l = length_of_match(match)
        c = input_stream[l]  # character after the match
    else:
        d = 0
        l = 0
        c = input_stream[0]  # first character of input

    output(d, l, c)

    # update window and input stream
    s = input_stream[:l + 1]        # take l+1 characters
    input_stream = input_stream[l + 1:]  # discard from input
    window += s                     # append to window
    if len(window) > MAX_WINDOW_SIZE:
        window = window[-MAX_WINDOW_SIZE:]  # keep only last MAX_WINDOW_SIZE chars
```
## LZ78
> **Dictionary-based compression algorithm** that builds a dictionary (or phrase table) of previously seen substrings **on the fly**. 
> **Key Idea:** Stores unique substrings with assigned indices

### How It Works
- Maintain a **dictionary** that maps substrings to indices.
- Initialize the dictionary as empty.
- Start reading characters from the input:    
    - Find the **longest prefix** in the dictionary that matches the current input.
    - Output the **index of that prefix** and the **next character**.
    - Add the new substring (prefix + next character) to the dictionary.

### Pseudocode