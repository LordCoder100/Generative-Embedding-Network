# Generative Embedding Network (LinguaGEN)

**LinguaGEN** is an experimental **Generative Embedding Network** — a research prototype exploring how embeddings can be **compressed, generated, and generalized** beyond the limitations of fixed-vocabulary models.  

At the current stage, this repository represents a **baseline proof-of-concept**:  
LinguaGEN is trained as a **semantic projector** that compresses high-dimensional embeddings (1536-d OpenAI vectors) into a compact **256-dimensional latent space** while preserving their semantic relationships.  

Although it does not yet *generate embeddings from scratch*, the model demonstrates that a **bidirectional Bilinear+GLU encoder** can approximate semantic structure with high cosine similarity, opening the path towards a truly generative embedding framework.  

---

## Motivation

Classical approaches like **Word2Vec** or **GloVe** produce static embeddings.  
Modern large-scale APIs (e.g., OpenAI embeddings) provide strong semantic vectors but are **expensive, high-dimensional (1536-d)** and often **black-box dependent**.

LinguaGEN aims to explore an alternative:  
- **Compression**: reduce embeddings to lightweight 256-d vectors while keeping semantics intact.  
- **Bidirectionality**: inspired by BERT, but without attention — leveraging a simpler Bilinear+GLU architecture.  
- **Generative capacity**: future iterations aim to move beyond projection and into fully **self-sufficient embedding generation**.  

---

## Current Status (Baseline)

- ✅ **Baseline dependency**: training supervised on OpenAI embeddings as teacher signals.  
- ✅ **Compression**: 1536 → 256 dimensions.  
- ✅ **Loss function**: cosine similarity, ensuring semantic preservation.  
- ✅ **Generalization**: early validation shows promising alignment even for unseen tokens.  
- ❌ **True generation**: the model still depends on baseline vectors during training.  
- ❌ **Contextual embeddings**: token-level embeddings only, no sentence-level context yet.  

---

## Architecture

The core design introduces a **bidirectional encoder** without attention:  

- **Forward Encoder** — processes input embeddings in natural order.  
- **Backward Encoder** — processes embeddings in reversed order.  
- **Bilinear + GLU Block** — replaces self-attention with lightweight gated bilinear interactions.  
- **Projection Head** — compresses outputs into 256-d latent space.  

Training setup:  
- **Loss**: `1 - cosine_similarity(pred, baseline)`  
- **Optimizer**: AdamW  
- **Scheduler**: CosineAnnealingLR  
- **Logging**: TensorBoard  

---

## Why It Matters

This project demonstrates that:  
1. Even a lightweight encoder can **learn semantic geometry** from large teacher embeddings.  
2. **Cosine similarity loss** is robust enough to transfer semantic structure.  
3. A **generative embedding model** is feasible: starting as a projector, evolving into an independent generator.  

The vision is to move beyond simple projection towards:  
- **Self-supervised objectives** (skip-gram, contrastive learning, SimCSE-style).  
- **Entity pooling & contextualization**.  
- **Standalone embeddings** independent of external APIs.  

---

## Roadmap

**Stage 1 (Baseline PoC)** — ✅ Done  
- Train projector on OpenAI embeddings.  
- Show cosine similarity preservation.  
- Validate on unseen tokens.  

**Stage 2 (Independence)** — 🚧 Work in Progress  
- Replace baseline inputs with raw tokens.  
- Introduce learnable embeddings (`nn.Embedding`).  
- Train with contrastive/self-supervised losses.  

**Stage 3 (Generative Network)** — 🔮 Future  
- Context-aware embeddings (sentence/paragraph).  
- On-the-fly embeddings for OOV words.  
- Multimodal extension (text + images).  

---

## Early Results

- **Compression**: embeddings reduced from 1536 to 256 dimensions.  
- **Cosine similarity**: preserved >0.95 on validation tokens.  
- **Proof-of-concept**: demonstrated feasibility of bidirectional Bilinear+GLU encoder as a generative baseline.  

TensorBoard screenshots show stable training with convergence even in early epochs.  

---

## Disclaimer

This repository represents an **early research prototype**.  
LinguaGEN is currently a **semantic projector baseline**, not yet a full generative model.  
Future work will focus on independence from baseline embeddings and on building true generative embedding capacity.  

---

## Vision

The long-term goal is a **Generative Embedding Network** capable of:  
- Creating embeddings for any new token without relying on pre-trained APIs.  
- Preserving semantic similarity in a compressed, efficient latent space.  
- Serving as a foundation for **NER, POS-tagging, translation, and reasoning tasks**.  

This PoC is just the **first step** in that direction.

---

## Author

Created by [Mykhailo Lapshyn](https://www.linkedin.com/in/mykhailo-lapshyn-2a3702309)  

📬 Feel free to connect on [![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue)](https://www.linkedin.com/in/mykhailo-lapshyn-2a3702309) if you’d like to discuss AI, embeddings, or research collaboration.

## Slogan

**TAKE UNREAL AND MAKE IT REALITY 😏**
