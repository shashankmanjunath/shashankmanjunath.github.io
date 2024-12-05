---
title: 'An Introduction to Transformers'
date: 2024-12-05
permalink: /posts/2024/11/intro-transformers/
comments: true
---

{% include base_path %}

Although there are many attention and transformer tutorials and papers online, I
found myself working through a number of them to pick up different bits of
information to allow me to better understand transformers for my applications. I
have collated my notes here in the hopes that they will help someone else.

As always, please send me an email if you find any typos, calculation errors or
other mistakes in this post.

# The Attention Mechanism

The core of the transformer model is the attention mechanism. In this post, we
focus on scaled dot-product attention. The intuition for attention is that we
want to focus on specific parts of a dataset. We learn weights which "focus," or
"attend" to a specific part of our input data.

<h2 id="embeddings">Embeddings</h2>

Let us first think about how we get data from input to the attention mechanism.
First, we must define some notation. Let $X \in \R^{S \times d}$ be the input
sequence of length $S$ and dimension $d$. We take our input data, which can be
something such as a sentence, and embed it into a $d$-dimensional vector. For
example, consider the sentence "the dog is eating." We define a fixed vocabulary
of words, with each word mapping to a $d$-dimensional vector. We take our
sequence of words, "the dog is eating," which contains 4 words, and transform it
into a matrix with $X$ with size $4 \times d$. The row vector $X_0$ is the
vector representing the word "the," $X_1$ represents "dog", and so on.

In general, word embeddings $X$ can be calculated from the original sentences in
a few different ways. We provide a non-comprehensive list of examples below.

1. **One-hot encoding/Lookup**. We define a weight matrix $W^w \in \R^{a \times d}$, where
   $a$ is the size of the vocabulary and $d$ is our embedding dimension size.
   When we get our input words ("the dog is eating"), we one-hot encode a vector
   with the locations of our lookup words creating a lookup vector
   $L \in \R^{L \times a}$ where $L$ is our sequence length (4 in the case of
   "the dog is eating). We then matrix multiply $L W^w = X$ to obtain our word
   representations. Equivalently, we can look-up our word representations using
   a lookup table and concatenate them together. These word representations can
   be learned or initialized at the beginning of training or left static.
2. **Existing word embedding algorithms** (TF-IDF, Word2Vec, etc.). These existing
   algorithms embed each word into a high-dimensional vector, which we then
   concatenate together to form $X$.
3. **Linear projection**. This is the approach taken by Vision Transformer, or
   ViT [1]. In this method, we break an input image into $N$ 2D patches, and
   flattens each patch into a fixed-size vector $x$. This vector is then
   projected through a learnable linear projection, i.e. a fully connected
   layer to create an embedding for each 2D patch.

### One-hot Encoding/Lookup Implementation

Lookup words can be easily implemented using a Python dictionary or other hash
map implementation. PyTorch specifically provides a nice `nn.Embedding` layer
which can be used to implement this type of embedding.

## Class Token

Many transformer models, such as BERT and ViT, prepend a trainable embedding to
the embedded vectors from the input sequence [1, 2]. This embedding is trained
along with the rest of the network, but is the only portion of the output
sequence at the end of the transformer on which the classification head
operates. Since the class token can "attend" to the whole sequence (further covered
in the Scaled Dot Product Attention section), it can aggregate information from
the whole sequence. Training the classification head on only the classification
head improves efficiency by allowing fewer parameters in the classification
head. Other options for aggregating information after transformer layers include
global average pooling; indeed, this works just as well as class tokens in the
case of ViT [1].

<h2 id="positional-encodings">Positional Encodings</h2>

When passing an embedded sequence to a transformer model, we encode
the position of each token in the sequence using a positional encoding. The
transformer architecture is permutation-invariant. More formally, consider a
sequence of vectors $\{x_1, \cdots, x_n\}$ and a permutation $\sigma$ of the set
$\{1, \cdots, n\}$. Let $\phi$ denote the transformer model. Without positional
encoding, we have:

$$
\phi(\{x_{\sigma(1)}, \cdots, x_{\sigma(n)}\}) = \sigma(\phi(\{x_1, \cdots, x_n\}))
$$

This is not a desirable property. For example, in the case of language
processing, the sequence of words matters. The sentences "the dog ate a chicken"
and "the chicken ate a dog" have the same words, but different meaning.
Therefore, we include a positional encoding, which ensures that we avoid
permutation invariance.

There are many ways to design positional encodings. One of the more common
methods is sine/cosine positional encoding, proposed in the original transformer
paper [3]. Consider position $p \in \Z^+$ and dimension $i \in \Z^+$, where
$\Z^+$ denotes positive integers including 0. Let $d_{\text{model}}$ denote the
dimension of embeddings in the model. We create positional encodings as
follows:

$$
\begin{align*}
P_{p, 2i} &= \sin\left(\left(\frac{p}{10000}\right)^{2i/d_{\text{model}}}\right) \\
P_{p, 2i+1} &= \cos\left(\left(\frac{p}{10000}\right)^{2i/d_{\text{model}}}\right)
\end{align*}
$$

# Inner Workings: Scaled Dot Product Self-Attention

We now define how we actually "attend" to different parts of our data matrix $X
\in \R^{S \times d}$. This is called "Scaled Dot Product Self-Attention." In
particular, we refer to this as "self-attention" since our matrices are defined
in relation to our input data matrix $X$. Let $W^q \in \R^{d \times d_k}, W^k
\in \R^{d \times d_k}$, and $W^v \in \R^{d \times d_v}$. These are the key,
query, and value weight matrices respectively. We calculate the following
quantities:

$$
\begin{align*}
    Q &= X W^q \in \R^{S \times d_k} \\
    K &= X W^k \in \R^{S \times d_k} \\
    V &= X W^v \in \R^{S \times d_v}
\end{align*}
$$

We then apply the scaled dot product attention operator.

$$
\text{attention}(Q, K, V) = \text{softmax}\left(\frac{Q K^\top}{\sqrt{d_k}}\right) V
$$

The attention operator is a function which outputs a matrix of size $\R^{S \times d_v}$

At a higher level, we perform the following steps:

1. Compute the dot product between queries $Q$ and keys $K$. Recall that $Q = X
   W^q$ and $K = X W^k$. The dot product $Q K^\top$ can be thought of as computing
   how similar the query and key vectors are. In this way, the whole input
   sequence can be compared against itself. $X$ is the same for $Q$ and $K$, so
   the dot product is defined by the $W^q$ and $W^k$ vectors.

   $$
   Q K^\top = X W^q (X W^k)^\top = X W^q (W^k)^\top X^\top
   $$

2. Scale the dot product by $\sqrt{d_k}$.
3. Use a softmax function to convert the raw attention logits to attention
   weights. Consider a vector $x \in \R^d$ with $x_i$ the $i$th component of $x$. Recall the softmax function $\sigma: \R^d \rightarrow \R^d$:

   $$
   \sigma(x)_i =  \frac{\exp(x_i)}{\sum_{i=1}^d \exp(x_i)}
   $$

   Note that the softmax function maps to the range $(0, 1)$. Therefore, we can
   consider the output of the softmax operation to be the weights of the
   attention mechanism.

4. Apply the value matrix to the attention weights. Since $V = X W^v$,
   we can consider this matrix multiplication a weighted sum of the previously
   calculated attention weights.

## Some Geometric Intuition

Many papers on transformers discuss "projection onto subspaces," so let's
consider some geometric intuition with vectors in $\R^2$. The goal of the scaled
dot product attention mechanism is to have the surrounding embeddings give
context to the current embedding. Consider a vector in $\R^2$, $x_1 = (1, 0)$.
We have another in our dataset, $x_2 = (0, 1)$. We want to contextualize $x_2$
with respect to $x_1$. For example, maybe $x_1$ corresponds to the word "blue"
and $x_2$ corresponds to the word "car." In this case, $x_1$ clearly affects the
context of $x_2$ in our sentence. Consider the following query, key, and value
matrices using $d_k = d_v = 2$. For ease of explanation, we use simple rotation
matrices.

$$
\begin{align*}
    W^q &= \begin{bmatrix}
                \cos{\left(-\frac{\pi}{4}\right)} & -\sin{\left(-\frac{\pi}{4}\right)} \\
                \sin{\left(-\frac{\pi}{4}\right)} &  \cos{\left(-\frac{\pi}{4}\right)}
           \end{bmatrix} \\
    W^k &= \begin{bmatrix}
                \cos{\left(\frac{\pi}{8}\right)} & -\sin{\left(\frac{\pi}{8}\right)} \\
                \sin{\left(\frac{\pi}{8}\right)} & \cos{\left(\frac{\pi}{8}\right)}
           \end{bmatrix} \\
    W^v &= \begin{bmatrix}
            \cos{\left(\frac{5 \pi}{16}\right)} & -\sin{\left(\frac{5 \pi}{16}\right)} \\
            \sin{\left(\frac{5 \pi}{16}\right)} & \cos{\left(\frac{5 \pi}{16}\right)}
           \end{bmatrix}
\end{align*}
$$

First we apply $XW^q$ and $XW^k$.

$$
\begin{align*}
    X   &= \begin{bmatrix}
            1 & 0 \\
            0 & 1
           \end{bmatrix} \\
    Q = X W^q &= \begin{bmatrix}
                   \cos{\left(-\frac{\pi}{4}\right)} & -\sin{\left(-\frac{\pi}{4}\right)} \\
                   \sin{\left(-\frac{\pi}{4}\right)} &  \cos{\left(-\frac{\pi}{4}\right)}
                 \end{bmatrix} \\
    K = X W^k &= \begin{bmatrix}
                   \cos{\left(\frac{\pi}{8}\right)} & -\sin{\left(\frac{\pi}{8}\right)} \\
                   \sin{\left(\frac{\pi}{8}\right)} & \cos{\left(\frac{\pi}{8}\right)}
                 \end{bmatrix} \\
\end{align*}
$$

Let's think about this visually. We have some rotation matrices working on our
defined embeddings, which are unit vectors in $\R^2$, as shown in the following
figure.


<p align="center">
    <img src="/images/2024-12-05-intro-transformers-images/xqk_vectors.svg" />
</p>

Now we can calculate $Q K^\top$. Consider the matrix multiplication operation we
are performing. We perform the dot product over the rows of $Q$ and the columns
of $K^\top$, corresponding to the rows of $K$. Geometrically, since $\langle a,
b \rangle = |a||b|\cos(\theta)$, and our vector lengths are equal, when two of
our vectors have a smaller angle between them we will expect a larger value.
Therefore, we expect a large value in $Q K^\top$ corresponding to $Q_1$ and
$K_2$, and a small value in $Q K^\top$ corresponding to $Q_2$ and $K_1$. Since
we scale $Q K^\top$ by a constant $\sqrt{d_k}$, the relationship (higher/lower)
will remain for $\frac{Q K^\top}{\sqrt{d_k}}$.

<p align="center">
    <img src="/images/2024-12-05-intro-transformers-images/unsoftmax_attn.svg" />
</p>

From the output matrix, we can see a high correlation between $Q_1$ and $K_2$, a
low correlation between $Q_2$ and $K_1$, and a moderate correlation between the
remaining entries, as expected. We now apply the softmax function, which
converts the raw values in each row into a probability distribution which sums
to 1.

<p align="center">
    <img src="/images/2024-12-05-intro-transformers-images/softmax_attn.svg" />
</p>

Based on the softmax outputs, we can now see the _relative_ intensities of the
relationship for each row. While $Q_1$ had the highest correlation, or smallest
angle, with $K_2$, since it also had a moderate correlation with $K_1$, the
values in the $Q_1$ row are fairly similar. In contrast, the angle between $Q_2$
and $K_1$ was quite large, while the angle between $Q_2$ and $K_2$ was
moderate. Therefore, a large allocation of the softmax value is on the $Q_2$ and
$K_2$ correlation, even though it is not the largest by raw value (by raw value
the largest correlation/smallest angle is $Q_1$ and $K_2$).

Now we apply the value matrix $V$ to our softmax outputs. Since the range of the
softmax function is $(0, 1)$, if we consider each row as a vector, we now have
all vectors in the first quadrant of $\R^2$. We project these onto the selected
value vectors through another matrix multiplication. For shorthand, let us now
use the following abbreviations:

$$
\begin{align*}
  A_1 &= \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)_1 \\
  A_2 &= \text{softmax}\left(\frac{QK^\top}{\sqrt{d_k}}\right)_2
\end{align*}
$$

Consider the row vectors $A_1$ and $A_2$ and the _columns_ of $V$, $V^{(1)}$ and
$V^{(2)}$.

<p align="center">
    <img src="/images/2024-12-05-intro-transformers-images/sm_v_vectors.svg" />
</p>

In the previous portion of this section, we interpreted the dot product as
calculating the angle between two vectors. Now we take an alternate, but
similar, interpretation of the dot product: projecting one vector onto another.
Matrix multiplication can be considered projection of the rows of the left
matrix, in this case $A$, onto the column space of the right matrix, in this
case $V$. Consider the _row_ of $A$, $A_1$ and the _column_ of $V$, $V^{(1)}$.
$A_1$ and $V^{(1)}$ are pointed (almost nearly) in the same direction. How would
we scale $V^{(1)}$ if we wanted to create $A_1$ using $V^{(1)}$? We would
decrease the size of it. Similarly, consider $A_1$ and $V^{(2)}$. These are
roughly perpendicular/orthogonal. Therefore, if we were projecting $A_1$ onto
the basis vector $V^{(2)}$, we would expect very little of $A_1$ to remain. This
is what we find when we perform the projection - the $A_1$ and $V^{(1)}$ entry
of the matrix has a relatively large value of 0.720, while the $A_2$ and
$V^{(2)}$ entry of the matrix has a relatively small value of -0.007. We can
similarly analyze $A_2$ and find a slightly smaller contribution from $V^{(1)}$
and a slightly larger contribution from $V^{(2)}$.

<p align="center">
    <img src="/images/2024-12-05-intro-transformers-images/attn_output.svg" />
</p>

While this is a contrived situation for explaining the mechanics of scaled
dot-product attention, the power of this method is in the learnable $W^q$,
$W^k$, and $W^v$ matrices. By learning the parameters of these matrices, we can
generate strong connections between varied parts of our input data.

## Multi-Head Self-Attention

A single attention head, i.e. one set of $W$ matrices $W^k$, $W^q$, and $W^v$,
provides limited modeling capacity. We typically use multiple heads on a single
group of data. Each head has a unique initialization of the $W$ matrices,
leading to a unique set of query, key and value vectors, and eventually a
different set of output attention values. The score from each head individually
are concatenated together into a single larger matrix.

More specifically, Let $N_h$ be the number of heads. From each attention head,
we obtain a matrix $S \times d_v$ from each head. We can then
concatenate these matrices together into another matrix of size $S
\times (N_h \cdot d_v)$.

# Encoder-Decoder Transformer Architectures

Transformer architectures come in many shapes and sizes. For example, the ViT
architecture only uses an encoder network, and performs an image classification
task directly on the embedded latent vector. Other models, such as language
translation models, use decoder architectures to output a new sequence derived
from the input sequence. In this portion of the article, I will attempt to
explain, in detail, how these architectures function.

## Encoder Architecture

The encoder architecture first takes the input embeddings as [discussed
above](#embeddings). We then add positional encodings to the input embeddings as
discussed in [the positional embeddings section](#positional-encodings). We then
create $N_{\text{blocks}}$ transformer blocks:

<p align="center">
    <img src="/images/2024-12-05-intro-transformers-images/transformer_encoder.svg" />
</p>

We first apply multi-head self attention as previously discussed. Let the output
of the multi-head self attention be a matrix $F \in \R^{S \times t}$. We then
apply a _position-wise feedforward network_ to $F$. This is simply a fully
connected neural network with two layers. We call it _position-wise_ since the
input and output dimensions are the same. However, this is an expansion then
contraction network. We map from the input size $t$ to a larger dimension, which
we denote $t_{\text{inter}}$. We then apply a nonlinearity such as a ReLU
function, and apply another fully connected layer which maps from
$t_{\text{inter}}$ to $t$. Note that we do not typically apply a nonlinearity to
the output of the second fully-connected layer.

We also include some residual networks and layer normalization in the block, as
denoted in the preceding image.

We stack these blocks sequentially $N_{\text{blocks}}$ times to create the
encoder architecture.

## Training with Language

Before discussing the decoder architecture, we must discuss training natural
language models. Consider the following situation: we have the words "the car
drove through the interchange at high speed." We want to train our model to
predict the output in Spanish. When training, we have the true label of the
translation. Instead of trying to predict the whole sentence at once, we try to
predict each token individually. We first encode our whole input English
sentence using the encoder, creating the latent vector outputs from the encoder.
We then iterate through the output Spanish sentence, predicting each word one at
a time. More specifically, we predict on the first token $S_1$, generating
output $O_1$. We then concatenate these two tokens to create the new input
token: $S_2 = [S_1, O_1]$. We predict on this to create $O_2$, and create $S_3 =
[S_1, O_1, O_2]$. We repeat this process for the length of the output sequence,
then mask out any tokens we padded on for batching reasons. We then calculate
our loss and backpropagate for training. We provide pseudocode using Python and
PyTorch to illustrate this process below.

```python
# Creating empty tensor to hold model predictions
all_preds = torch.tensor([], requires_grad=True)

# Extracting first english token to start process
all_outs_tokens = english_tokens[:, :1]

# Iterating over all available English tokens
for i in range(english_tokens.shape[1]):
    # Creating model predictions up to the current location in the loop using the previously predicted tokens
    model_preds = model(all_preds_tokens)

    # Joining predicted logits
	all_preds = torch.cat((all_preds, model_preds), dim=1)

	# Get the token output rather than the output probabilities
	cur_pred_token = torch.argmax(model_preds, dim=1)
	all_preds_tokens = torch.cat((all_preds_tokens, cur_pred_token), dim=1)

# Masking out the extra padded tokens so that they do not contribute to our loss
# We remove the first token since it is a start token which we do not predict
all_preds = all_preds * label_mask[:, 1:, :]
labels = labels * label_mask

# Backpropagation Step. We predict from the first token onwards since the first token in the label is a start token which we do not need to predict.
loss = criterion( all_preds, labels[:, 1:, :])
loss.backward()
optimizer.step()
```

<h3 id="mask-parallel-training">Parallelizing Training with Masks</h3>

This process can be parallelized with masking. Instead of iterating through the
entire sentence and predicting the next token sequentially, we can mask off the
future tokens in our attention map and prevent the model from considering tokens
ahead of the current one when training. Consider a 3-word sentence.
We would mask the attention scores in the following manner when passing data
through the decoder part of the transformer architecture , with $w_i$ denoting
an attention score:

$$
\text{input} = \begin{bmatrix}
    w_0 & -\infty & -\infty \\
    w_0 & w_1 & -\infty \\
    w_0 & w_1 & w_2
\end{bmatrix}
$$

This allows us to train more quickly since we can parallelize the prediction of
each individual token without allowing the model to attend to tokens in the
future of the current state. The term for not allowing the model to attend to
future tokens is called "causality," which is the term typically used in
literature to describe the motivation for this masking design.

## Decoder Architecture

Now that we understand how the model is trained, we can discuss the decoder
architecture. Once we have created the outputs from our encoder architecture, we
feed it individually to each portion of our decoder architecture. Each decoder
block is very similar to an encoder block, and is shown in the following image.

<p align="center">
    <img src="/images/2024-12-05-intro-transformers-images/transformer_decoder.svg" />
</p>

At the end of the decoder, we use a single fully-connected layer and a softmax
activation to obtain output probabilities.

Note the "masked multi-head attention" block at the beginning of each decoder
block. This implements the parallel training discussed in the [parallelizing
training section](#mask-parallel-training).

# Conclusion

In conclusion, we have covered the basics of the attention mechanism and how a
basic transformer architecture proposed by Vasiwani et. al. functions and can be
trained [3]. I hope you found this article useful!

# References

[1] A. Dosovitskiy et al., “An Image is Worth 16x16 Words: Transformers for Image Recognition at Scale,” Jun. 03, 2021, arXiv: arXiv:2010.11929. doi: 10.48550/arXiv.2010.11929. \
[2] J. Devlin, M.-W. Chang, K. Lee, and K. Toutanova, “BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding,” May 24, 2019, arXiv: arXiv:1810.04805. doi: 10.48550/arXiv.1810.04805. \
[3] A. Vaswani et al., “Attention Is All You Need,” Aug. 02, 2023, arXiv: arXiv:1706.03762. Accessed: Nov. 18, 2024. [Online]. Available: http://arxiv.org/abs/1706.03762
