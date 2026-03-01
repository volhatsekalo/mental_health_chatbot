# Master's Thesis Project: A Comparative Study of Fine-Tuning Methods: LoRA vs Partial Fine-Tuning of Polish Large Language Model using Psychological dataset

## Overview

This project focuses on comparing **LoRA vs Partial Fine-tuning** in fitting Large Language Model Bielik to a psychological domain, creating **a model that provides mental health support to users** in a friendly and supportive tone. The goal is to design a system that follow **Cognitive Behavioral Therapy principles**. The chatbot will simulate human behavior and cognitive processes, aiming to help users manage their mental health through gentle suggestions and information.

### Features

- **Friendly Tone**: The model is designed to engage in a friendly manner, ensuring a **comfortable interaction**.
- **Mental Health Tips**: Users can receive advice and suggestions on **managing stress, anxiety, and other mental health issues**.
- **Cognitive Behavioral Therapy based**: the model is trained on Cactus dataset, that was **approved by the professionals**

### Technologies Used

- **Base Model**: Polish LLM ``Bielik`` (for the core model architecture and pre-trained weights)
- **AI Framework**: ``PyTorch`` (for model development and fine-tuning)
  
## The Challenge: "Shortcut Learning" in the Original Dataset

Initial experiments using the original translated Cactus dataset revealed a critical issue. The model exhibited **Shortcut Learning** — it learned the superficial structure of a therapeutic response (e.g., *always saying "I understand this is hard" and immediately asking a generic question*) without actually processing the patient's unique context.

## Data Augmentation: The Hybrid Dataset

To address this limitation, I implemented a **Synthetic Data Augmentation** strategy. I enriched the original dataset to create a "Hybrid Dataset" that enforces **Contextual Grounding** and **Cognitive Scaffolding**. The new, augmented responses follow a strict, safe therapeutic protocol:

1. **Validation & Reflective Listening:** Acknowledging the patient's specific situation.
2. **Cognitive Reframing & Normalization:** Providing low-risk psychoeducational support (e.g., normalizing the experience to reduce distress and validate emotions).
3. **Guided Inquiry:** Seamlessly transitioning to a verified, exploratory question from the oryginal dataset.

## Experimental Setup & Evaluation

I compared the base Bielik model against two parameter-efficient fine-tuning methods across both datasets:
- **Partial Fine-Tuning:** Unfreezing and training only the last 8 layers of the model.
- **LoRA (Low-Rank Adaptation):** Injecting trainable rank decomposition matrices into the attention layers while freezing the pre-trained backbone.

Evaluation was conducted using **Semantic Similarity** (via SBERT `all-mpnet-base-v2`) against a hold-out test set of 50 varied dialogues. I measured both the **Mean Score** (overall quality and adherence to the therapeutic persona) and the **Standard Deviation** (stability and predictability).

## Results: LoRA and High-Quality Data Win

The results demonstrated a massive leap in performance when combining LoRA with the augmented Hybrid dataset.

| Model / Method | Dataset | Mean Semantic Similarity | Standard Deviation (STD) |
| :--- | :--- | :--- | :--- |
| Bielik (Base) | Hybrid | 0.6226 | 0.1227 |
| Partial FT (8 layers) | Original | 0.5694 | 0.1735 |
| LoRA | Original | 0.6179 | 0.1412 |
| Partial FT (8 layers) | Hybrid | 0.7363 | 0.1119 |
| **LoRA (Ours)** | **Hybrid** | **0.8100** | **0.0842** |

### Key Findings:

1. **Data Quality is the Bottleneck:** Moving from the Original to the Hybrid dataset yielded a ~20% performance jump for the LoRA model. High-quality, structurally sound data proved to be crucial for domain adaptation.
2. **LoRA Provides Superior Stability:** In supportive mental health applications, predictability is vital. LoRA achieved the lowest Standard Deviation (0.0842), meaning it rarely "breaks character" or hallucinates, unlike Partial FT which showed dangerous performance dips.
3. **Zero-Shot Persona Adherence:** Unlike the base model, which requires extensive in-context history to sound empathetic, the LoRA-tuned model internalizes the therapeutic persona from the very first interaction (Cold Start).

## Qualitative Example: The "Cold Start" Problem

Here is an example of how the models react to a user's opening statement without any prior conversation history. This highlights the risk of relying on a base model versus a fine-tuned therapeutic agent.

**User:** *"I just feel like everyone thinks I'm a loser because I work at McDonald's. It's been weighing on me for a while."*

* ❌ **Base Bielik Model:** *Proceeds to generate a generic, assistant-like bullet-point list about the benefits of working in fast food (e.g., "1. Stability of employment, 2. Flexible hours"), completely missing the user's emotional distress and failing to validate their feelings.*
* ✅ **LoRA Model (Hybrid Dataset):** *"It's completely normal to feel that way when you feel judged by others based on societal stereotypes. Working in fast food can be tough when people have certain expectations. Can you tell me more about what makes you feel like they view you as a loser?"* *(Notice the structure: Validation -> Contextual Normalization -> Exploration)*

## Prototype

The initial design of the application has been developed using Figma. Below are some screenshots of the prototype:

**Login page**
![image](https://github.com/user-attachments/assets/0fa6b350-9fb7-44d4-a5a7-44a21ac2a6f5)

**Chat page**
![image](https://github.com/user-attachments/assets/33cde4c1-71ef-452c-bdab-ff06e62f3664)


> Note: The design is still in progress and will evolve as the project progresses.

## Future Work
Implementing Reinforcement Learning with Human Feedback (RLHF) for improving chatbot responses.

Enhancing the safety and accuracy using safety fine-tuning.
