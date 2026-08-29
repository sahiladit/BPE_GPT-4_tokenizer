# GPT-4 Style Tokenizer From Scratch

A simple **byte-level BPE tokenizer implemented from scratch in Python**, inspired by GPT-4 tokenization.

> This version implements BPE but **does not include GPT-4's regex pre-tokenization** yet.

## How It Works

```text
Text
 ↓
UTF-8 Encoding
 ↓
Byte IDs (0–255)
 ↓
BPE Merging
 ↓
Token IDs
```

Decoding reverses this:

```text
Token IDs
 ↓
Reverse BPE
 ↓
Byte IDs
 ↓
UTF-8 Decoding
 ↓
Text
```

## BPE Training

The tokenizer:

1. Converts text to UTF-8 bytes.
2. Counts adjacent token pairs.
3. Finds the most frequent pair.
4. Assigns it a new token ID starting from `256`.
5. Merges the pair.
6. Repeats until the required vocabulary size is reached.

Example:

```text
(101, 32) → 256
(256, 116) → 257
```

The learned merges are stored as:

```python
(token_a, token_b) → new_token_id
```

## Usage

```python
tok = tokenizer()

tok.train(text, vocab_size=300)

ids = tok.encode("Hello world!")

decoded = tok.decode(ids)

print(ids)
print(decoded)
```

Verify the tokenizer:

```python
assert text == tok.decode(tok.encode(text))
```

## Project Structure

```text
.
├── tokenizer.ipynb
├── assets.py
└── README.md
```

`assets.py` contains the BPE helper functions, while `tokenizer.ipynb` contains the tokenizer implementation and experiments.

## Current Limitations

This version does **not** implement:

* GPT-4 regex pre-tokenization
* GPT-4's exact vocabulary/merges
* Special tokens
* `tiktoken` compatibility

The next step is implementing the **GPT-4 regex tokenizer**.
