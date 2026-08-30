# Building a Simple LLM Prototype

## Overview & Approach

To build a simple LLM (Large Language Model) prototype, this guide outlines a minimal, educational implementation based on the guidance from *Build a Large Language Model (From Scratch)* by Sebastian Raschka (2024, Manning Publications). This approach uses PyTorch and the transformer architecture, focusing on clarity and core concepts rather than production-level complexity.

The prototype includes:
- Data preparation and tokenization
- Token and positional embeddings
- A self-attention layer and mini transformer block
- A basic training loop for next-token prediction

---

## Part 1: Minimal End-to-End Prototype

### 1. Data Preparation and Tokenization

- Download a sample text (e.g., a public domain story)
- Tokenize the text (split into words or characters)
- Map tokens to integer IDs

```python
import urllib.request
import re

# Download sample text
download_url = "https://raw.githubusercontent.com/rasbt/LLMs-from-scratch/main/ch02/01_main-chapter-code/the-verdict.txt"
file_path = "the-verdict.txt"
urllib.request.urlretrieve(download_url, file_path)

with open(file_path, "r", encoding="utf-8") as f:
    raw_text = f.read()

tokens = [item for item in re.split(r'([,.!?;:\s])', raw_text) if item.strip()]
vocab = {token: idx for idx, token in enumerate(sorted(set(tokens)))}
token_ids = [vocab[token] for token in tokens]
```

---

### 2. Embedding and Positional Encoding

- **Token embedding**: Converts token IDs to dense vectors
- **Positional embedding**: Adds position information

```python
import torch

vocab_size = len(vocab)
embedding_dim = 32
max_length = 16

token_embedding = torch.nn.Embedding(vocab_size, embedding_dim)
pos_embedding = torch.nn.Embedding(max_length, embedding_dim)

input_ids = torch.tensor(token_ids[:max_length]).unsqueeze(0)  # (1, max_length)
pos_ids = torch.arange(max_length).unsqueeze(0)
input_emb = token_embedding(input_ids) + pos_embedding(pos_ids)
```

---

### 3. Simple Self-Attention Layer

```python
class SimpleSelfAttention(torch.nn.Module):
    def __init__(self, emb_dim):
        super().__init__()
        self.query = torch.nn.Linear(emb_dim, emb_dim)
        self.key = torch.nn.Linear(emb_dim, emb_dim)
        self.value = torch.nn.Linear(emb_dim, emb_dim)

    def forward(self, x):
        Q = self.query(x)
        K = self.key(x)
        V = self.value(x)
        attn_scores = torch.matmul(Q, K.transpose(-2, -1)) / (K.size(-1) ** 0.5)
        attn_weights = torch.nn.functional.softmax(attn_scores, dim=-1)
        return torch.matmul(attn_weights, V)

attention = SimpleSelfAttention(embedding_dim)
attn_output = attention(input_emb)
```

---

### 4. Mini Transformer Block

```python
class MiniTransformerBlock(torch.nn.Module):
    def __init__(self, emb_dim):
        super().__init__()
        self.attn = SimpleSelfAttention(emb_dim)
        self.ln1 = torch.nn.LayerNorm(emb_dim)
        self.ff = torch.nn.Sequential(
            torch.nn.Linear(emb_dim, emb_dim * 4),
            torch.nn.GELU(),
            torch.nn.Linear(emb_dim * 4, emb_dim)
        )
        self.ln2 = torch.nn.LayerNorm(emb_dim)

    def forward(self, x):
        x = x + self.attn(self.ln1(x))
        x = x + self.ff(self.ln2(x))
        return x

block = MiniTransformerBlock(embedding_dim)
output = block(input_emb)
```

---

### 5. Simple LLM Model and Training Loop

```python
class SimpleLLM(torch.nn.Module):
    def __init__(self, vocab_size, emb_dim, max_length):
        super().__init__()
        self.token_emb = torch.nn.Embedding(vocab_size, emb_dim)
        self.pos_emb = torch.nn.Embedding(max_length, emb_dim)
        self.block = MiniTransformerBlock(emb_dim)
        self.out = torch.nn.Linear(emb_dim, vocab_size)

    def forward(self, x):
        pos = torch.arange(x.size(1)).unsqueeze(0)
        x = self.token_emb(x) + self.pos_emb(pos)
        x = self.block(x)
        return self.out(x)

model = SimpleLLM(vocab_size, embedding_dim, max_length)
optimizer = torch.optim.AdamW(model.parameters(), lr=1e-3)
loss_fn = torch.nn.CrossEntropyLoss()

inputs = torch.tensor(token_ids[:max_length]).unsqueeze(0)
targets = torch.tensor(token_ids[1:max_length+1]).unsqueeze(0)

model.train()
optimizer.zero_grad()
logits = model(inputs)
loss = loss_fn(logits.view(-1, vocab_size), targets.view(-1))
loss.backward()
optimizer.step()
```

---

### Summary

This prototype demonstrates the core LLM components: tokenization, embedding, self-attention, transformer block, and a simple training step. It is designed for educational purposes and can be run on a standard laptop. For more advanced features, batching, and scaling, refer to the book's full code and explanations.

---

## Part 2: Component Breakdown & Supporting Examples

To provide supporting examples for the transformer architecture components in a simple LLM prototype, this section provides concrete explanations and code snippets from *Build a Large Language Model (From Scratch)* by Sebastian Raschka. Each component—token embedding, positional encoding, self-attention, transformer block, and output layer—is explained with its role, a practical example, and how it fits into the overall architecture.

---

### 1. Token Embedding

- **Role**: Converts discrete token IDs into dense, continuous vectors that the model can process.

#### Example

```python
import torch

vocab_size = 50257  # Example GPT-2 vocab size
embedding_dim = 256

embedding_layer = torch.nn.Embedding(vocab_size, embedding_dim)
inputs = torch.randint(0, vocab_size, (8, 4))  # batch of 8, 4 tokens each
token_embeddings = embedding_layer(inputs)

print(token_embeddings.shape)  # Output: torch.Size([8, 4, 256])
```

Each token ID is mapped to a 256-dimensional vector. For a batch of 8 samples, each with 4 tokens, the output is an $8 \times 4 \times 256$ tensor.

---

### 2. Positional Encoding

- **Role**: Adds information about the position of each token in the sequence, since self-attention is position-agnostic.

#### Example

```python
import torch

max_length = 4
pos_embedding_layer = torch.nn.Embedding(max_length, embedding_dim)
pos_ids = torch.arange(max_length)
pos_embeddings = pos_embedding_layer(pos_ids)

print(pos_embeddings.shape)  # Output: torch.Size([4, 256])

input_embeddings = token_embeddings + pos_embeddings  # Broadcasting over batch
print(input_embeddings.shape)  # Output: torch.Size([8, 4, 256])
```

Each position in the sequence gets a unique embedding, which is added to the token embedding.

---

### 3. Self-Attention

- **Role**: Allows each token to attend to all other tokens in the sequence, capturing contextual relationships.

#### Example

```python
import torch
import torch.nn as nn

class SelfAttention_v1(nn.Module):
    def __init__(self, d_in, d_out):
        super().__init__()
        self.W_query = nn.Parameter(torch.rand(d_in, d_out))
        self.W_key = nn.Parameter(torch.rand(d_in, d_out))
        self.W_value = nn.Parameter(torch.rand(d_in, d_out))

    def forward(self, x):
        keys = x @ self.W_key
        queries = x @ self.W_query
        values = x @ self.W_value
        attn_scores = queries @ keys.T
        attn_weights = torch.softmax(attn_scores / keys.shape[-1]**0.5, dim=-1)
        context_vec = attn_weights @ values
        return context_vec

# Usage
sa = SelfAttention_v1(256, 256)
context_vectors = sa(input_embeddings[0])  # For one sample in the batch
print(context_vectors.shape)  # Output: torch.Size([4, 256])
```

Each token's representation is updated based on a weighted sum of all tokens in the sequence.

---

### 4. Transformer Block

- **Role**: Combines self-attention, feed-forward layers, layer normalization, and shortcut (residual) connections. This block is stacked multiple times in the model.

#### Example

```python
import torch.nn as nn

class TransformerBlock(nn.Module):
    def __init__(self, emb_dim):
        super().__init__()
        self.att = SelfAttention_v1(emb_dim, emb_dim)
        self.ln1 = nn.LayerNorm(emb_dim)
        self.ff = nn.Sequential(
            nn.Linear(emb_dim, emb_dim * 4),
            nn.GELU(),
            nn.Linear(emb_dim * 4, emb_dim)
        )
        self.ln2 = nn.LayerNorm(emb_dim)

    def forward(self, x):
        x = x + self.att(self.ln1(x))  # Residual connection
        x = x + self.ff(self.ln2(x))   # Residual connection
        return x

# Usage
block = TransformerBlock(256)
output = block(input_embeddings[0])
print(output.shape)  # Output: torch.Size([4, 256])
```

The transformer block preserves the input shape and enriches each token's representation with context and nonlinearity.

---

### 5. Output Layer

- **Role**: Projects the final hidden states to the vocabulary size to predict the next token (for language modeling) or to the number of classes (for classification).

#### Example

```python
import torch.nn as nn

output_layer = nn.Linear(256, vocab_size)
logits = output_layer(output)  # output from transformer block
print(logits.shape)  # Output: torch.Size([4, 50257])
```

Each token position now has a vector of logits over the vocabulary, ready for softmax and prediction.

---

### Summary Table

| Component | Example Shape | Key Function |
| :--- | :--- | :--- |
| **Token Embedding** | `[batch, seq, emb_dim]` | Map token IDs to dense vectors |
| **Positional Encoding** | `[seq, emb_dim]` | Add position info to embeddings |
| **Self-Attention** | `[seq, emb_dim]` | Contextualize each token with all others |
| **Transformer Block** | `[seq, emb_dim]` | Stackable unit: attention + feed-forward + norm |
| **Output Layer** | `[seq, vocab_size]` | Predict next token or class |

These examples are directly aligned with the educational code and explanations in the referenced book, and can be adapted for both small prototypes and larger models.
