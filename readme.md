# Understanding JEPA From Scratch

This repository documents my journey of understanding **representation learning**, **BYOL**, and eventually **JEPA (Joint Embedding Predictive Architecture)** from first principles.

The goal of this project is not to reproduce the original papers immediately, but to build intuition step by step and understand **why modern self-supervised learning works**.

---

## My Understanding of the Problem

Traditional supervised learning requires labels:

```text
Image → Label
```

For example:

```text
Dog Image → Dog
Car Image → Car
```

But collecting labels is expensive.

The question becomes:

> Can a model learn useful representations without labels?

This is where **self-supervised learning** comes in.

Instead of predicting labels, the model learns to understand the structure of the data itself.

---

# What Is a Representation?

An image is originally just pixels.

```text
[3, 32, 32]
```

Pixels alone are not very useful.

An encoder transforms the image into a compact representation:

```text
Image
   ↓
Encoder
   ↓
Representation
```

Example:

```text
[3, 32, 32]
      ↓
Encoder
      ↓
[128]
```

The representation contains semantic information about the image:

* shapes
* textures
* objects
* patterns

instead of raw pixel values.

This concept is called **Representation Learning**.

---

# Why Not Predict Pixels?

Predicting future pixels is difficult.

A small change in:

* lighting
* shadows
* camera angle
* background

can completely change the pixel values.

Modern approaches instead predict representations.

```text
Image
   ↓
Representation
   ↓
Future Representation
```

Representations capture meaning rather than exact pixels.

This idea is one of the key motivations behind JEPA.

---

# Project Structure

## 01 - Representation Learning (`initial.ipynb`)

The first notebook focuses on the core concept:

```text
Image
   ↓
Encoder
   ↓
Representation
```

Topics covered:

* Loading CIFAR-10
* Building a simple CNN encoder
* Converting images into embeddings
* Understanding representation vectors
* Visualizing and inspecting outputs

Goal:

Understand what an encoder does before introducing self-supervised learning.

---

## 02 - BYOL Toy Version (`JEPA V1.ipynb`)

This notebook introduces the basic components used in modern self-supervised learning.

Architecture:

```text
Image 1
   ↓
Online Encoder
   ↓
Predictor
   ↓
Prediction

Image 2
   ↓
Target Encoder
   ↓
Target Representation
```

Components:

* Online Encoder
* Predictor
* Target Encoder
* Loss Function
* Training Loop

Goal:

Understand how one network can learn to predict another representation without labels.

This is a simplified educational version.

---

## 03 - Real BYOL (`JEPA V2 (BYOL-style).ipynb` and `JEPA V3 (Real BYOL).ipynb`)

The toy version uses different images.

Real BYOL uses:

```text
Same Image
     ↓
Two Different Augmentations
```

Example:

```text
Original Dog

View 1:
- Crop
- Flip

View 2:
- Crop
- Color Jitter
```

The model learns:

```text
Representation(View 1)
      ≈
Representation(View 2)
```

even though the pixels are different.

Key concepts:

* Data Augmentation
* Online Encoder
* Target Encoder
* Predictor Network
* EMA / Momentum Updates
* Representation Consistency

Goal:

Learn invariant representations from multiple views of the same image.

---

## 04 - JEPA Toy Version (Work In Progress)

After understanding BYOL, the next step is JEPA.

Instead of:

```text
View A → View B
```

JEPA predicts hidden parts of the image in representation space.

Idea:

```text
Image
 ├── Context
 └── Target
```

The model receives the context and predicts the representation of the target region.

```text
Context
   ↓
Encoder
   ↓
Predictor
   ↓
Target Representation
```

Notice:

The model predicts embeddings, not pixels.

This is one of the biggest conceptual differences compared to reconstruction-based methods.

---

## 05 - Real JEPA (Currently Working On)

Planned features:

* Masking Strategy
* Multiple Target Blocks
* Vision Transformer (ViT)
* Context Encoder
* Target Encoder
* Representation Prediction
* JEPA Training Objective

Goal:

Move closer to the architecture described in the JEPA paper.

---

# Learning Path

```text
Representation Learning
        ↓
BYOL Toy Version
        ↓
Real BYOL
        ↓
JEPA Toy Version
        ↓
Real JEPA
```

Each notebook builds on concepts from the previous one.

The objective is to understand *why* these methods work rather than simply reproducing paper code.

---

# Current Status

* ✅ Representation Learning
* ✅ BYOL Toy Version
* ✅ Real BYOL
* 🚧 JEPA Toy Version
* 🚧 Real JEPA (V3)

---

# References

* BYOL (Bootstrap Your Own Latent)
* JEPA (Joint Embedding Predictive Architecture)
* Self-Supervised Learning
* Representation Learning

---

## Note

This repository represents my personal learning journey.

The focus is on building intuition from scratch through small experiments and progressively more realistic implementations.

I am currently working on **JEPA V3**, where I plan to introduce masking strategies, multiple target blocks, and Vision Transformers to better match the original architecture.
