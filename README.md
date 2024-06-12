### For loading data from corpus, the address of corpus might be different.
1. Util Functions
    I used some helper functions for handling oov and getting tags & words from corpus.
    For the OOV, I used Molphology rules used to assign unknown word tokens.
    get_word_tag:
        It is a function that returns words & tags from corpus.
        If some words is not in vocab(vocab is hmm words/ I will explain it further later.) 
    preprocess:
        It is a function checks the unknown words from test corpus.
    assign_unk:
        It is a function that returns unknown word tokens.
        
2. vocab
    I stores the words that are used more than twice and unknown tokens for unknown words.
    A set of unknown-tokens, such as '--unk-verb--' or '--unk-noun--' will replace the unknown words in both the training and test corpus and will appear in the emission, transmission and tag data structures.
    
3. dictionaries 
    By using helper functions, create three different dictionaries.
    trans_counts:
        keys : sets of previous tag and current tag
        values : counts of each set
    emis_counts:
        keys : sets of tag and word
        values : counts of tag and corresponding word
    tag_counts:
        keys : tags
        values : counts of each tag

4. 2 matrixes 
    trans_matrix:
        stores the probability to go from one part of speech to another.
        Compute the prob by smoothing.
        The smoothing:
            𝑃(𝑡𝑖|𝑡𝑖−1)=𝐶(𝑡𝑖−1,𝑡𝑖)+𝛼 / 𝐶(𝑡𝑖−1)+𝛼∗𝑁
        𝛼: alpha(constant value)
        𝑁: num of tags
        𝐶(𝑡𝑖−1,𝑡𝑖): trans_counts
        𝐶(𝑡𝑖−1): tag_counts
    
    emis_matrix:
        Dimension (num_tags, N), where num_tags is the number of possible parts-of-speech tags.
        Compute the prob by smoothing.
        The smoothing:
            𝑃(𝑤𝑖|𝑡𝑖)=𝐶(𝑡𝑖,𝑤𝑜𝑟𝑑𝑖)+𝛼 / 𝐶(𝑡𝑖)+𝛼∗𝑁
        𝛼: alpha(constant value)
        N: num of tags
        𝐶(𝑡𝑖,𝑤𝑜𝑟𝑑𝑖): emis_counts
        𝐶(𝑡𝑖): tag_counts
        
5. viterbi forward
    best_probs:
        n x t matrix (n is num of unique POS tags and t is num of words in corpus)
        the best prob for the given current word's POS tag and the position.
    best_path:
        n x t matrix (n is num of unique POS tags and t is num of words in corpus)
        the unique integer ID of the best prob
    algorithm:
        Walk forward through the corpus.
        For each word, compute a probability for each possible tag.

6. viterbi backward
    pred: 
        It is an array(same size as corpus) that the proper pos tags will be entered.
    z:
        It is an array that will be used for saving an index of the best prob thru best path.
    algorithm:
         set the last element of z to argmax(best_probs[:,-1]) and go backwards thru best_paths and find the index of best prob.
         Then save proper pos tag into pred.
         
citation:
    https://medium.com/analytics-vidhya/parts-of-speech-pos-and-viterbi-algorithm-3a5d54dfb346