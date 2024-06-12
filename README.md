### Util Functions
    - get_word_tag: Returns words and tags from the corpus. If some words are not in the vocabulary (which consists of HMM words), this function handles them.
    - preprocess: Checks for unknown words in the test corpus.
    - assign_unk: Handles unknown word tokens using morphology rules.
        
### Vocab
    - Stores words that appear more than twice and manages unknown tokens for rare words.
    - Unknown tokens (e.g., --unk-verb--, --unk-noun--) replace unknown words in both the training and test corpus, and are incorporated into emission, transition, and tag data structures.
    
### Dictionaries 
    - trans_counts: Records counts of transitions between tags.
    - emis_counts: Records counts of word occurrences given a tag.
    - tag_counts: Records counts of individual tags.

### Matrix
    - trans_matrix:
        Stores transition probabilities between tags, computed with smoothing
        The smoothing:
            𝑃(𝑡𝑖|𝑡𝑖−1)=𝐶(𝑡𝑖−1,𝑡𝑖)+𝛼 / 𝐶(𝑡𝑖−1)+𝛼∗𝑁
        𝛼: alpha(constant value)
        𝑁: num of tags
        𝐶(𝑡𝑖−1,𝑡𝑖): trans_counts
        𝐶(𝑡𝑖−1): tag_counts
    
    - emis_matrix:
        Stores emission probabilities of words given tags, also computed with smoothing:
        Dimension (num_tags, N), where num_tags is the number of possible parts-of-speech tags.
        The smoothing:
            𝑃(𝑤𝑖|𝑡𝑖)=𝐶(𝑡𝑖,𝑤𝑜𝑟𝑑𝑖)+𝛼 / 𝐶(𝑡𝑖)+𝛼∗𝑁
        𝛼: alpha(constant value)
        N: num of tags
        𝐶(𝑡𝑖,𝑤𝑜𝑟𝑑𝑖): emis_counts
        𝐶(𝑡𝑖): tag_counts
        
### Viterbi forward
    - best_probs:
        n x t matrix (n is num of unique POS tags and t is num of words in corpus)
        the best prob for the given current word's POS tag and the position.
    - best_path:
        n x t matrix (n is num of unique POS tags and t is num of words in corpus)
        the unique integer ID of the best prob
    - algorithm:
        Computes the probability of each possible tag for each word and tracks the best probabilities.

### Viterbi backward
    - pred: 
        It is an array(same size as corpus) that the proper pos tags will be entered.
    - z:
        It is an array that will be used for saving an index of the best prob thru best path.
    - algorithm:
         set the last element of z to argmax(best_probs[:,-1]) and go backwards thru best_paths to find the index of best prob.
         Then save proper pos tag into pred.
         
### citation:
    https://medium.com/analytics-vidhya/parts-of-speech-pos-and-viterbi-algorithm-3a5d54dfb346