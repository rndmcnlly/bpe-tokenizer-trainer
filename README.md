# bpe-tokenizer-trainer
A vibe-coded toy for exploring byte-pair encoding (BPE)

### USER (Adam Smith)
Briefly explain BPE (byte pair encoding) and include pseudo code for something that would take in a giant byte array and generate a BPE vocab for it.

### ASSISTANT (Claude Opus 4.1)
## BPE (Byte Pair Encoding)

BPE is a compression algorithm that iteratively replaces the most frequent pair of adjacent tokens with a new token. Originally designed for text compression, it's now widely used in NLP for tokenization (GPT, BERT, etc.).

**How it works:**
1. Start with a vocabulary of individual bytes/characters
2. Find the most frequent adjacent pair in the data
3. Merge that pair into a new token and add it to vocabulary
4. Replace all occurrences of that pair with the new token
5. Repeat until reaching desired vocabulary size

## Pseudo Code

```python
function build_bpe_vocab(data: byte_array, vocab_size: int):
    # Initialize vocab with all unique bytes (256 possible values)
    vocab = {i: bytes([i]) for i in range(256)}
    next_token_id = 256
    
    # Convert input to list of byte tokens
    tokens = list(data)
    
    while len(vocab) < vocab_size:
        # Count all adjacent pairs
        pair_counts = {}
        for i in range(len(tokens) - 1):
            pair = (tokens[i], tokens[i + 1])
            pair_counts[pair] = pair_counts.get(pair, 0) + 1
        
        # Find most frequent pair
        if not pair_counts:
            break
        best_pair = max(pair_counts, key=pair_counts.get)
        
        # Create new token for this pair
        vocab[next_token_id] = vocab[best_pair[0]] + vocab[best_pair[1]]
        
        # Replace all occurrences of pair with new token
        new_tokens = []
        i = 0
        while i < len(tokens):
            if i < len(tokens) - 1 and (tokens[i], tokens[i + 1]) == best_pair:
                new_tokens.append(next_token_id)
                i += 2
            else:
                new_tokens.append(tokens[i])
                i += 1
        
        tokens = new_tokens
        next_token_id += 1
    
    return vocab, tokens  # vocab maps token_id -> bytes
```

The resulting vocabulary contains the original 256 bytes plus learned multi-byte sequences, enabling efficient encoding of common patterns in the data.

### USER (Adam Smith)
Now use a self contained html page (in a fenced code block) to offer me an artifact where I can paste in some ASCII text, analyze it, show me how new tokens are created, and then show how the original text (or some new next) gets tokenized. I guess it needs a vocab size number input that can default to something good. The number of steps should be limited by the target vocab size, and we should get a little html progress bar to show how we are working through large training inputs.

### ASSISTANT (Claude Sonnet 4)

...
