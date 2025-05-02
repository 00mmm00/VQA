# Assistive Visual Question Answering for Visually Impaired Users

This project develops an Assistive Visual Question Answering (VQA) system to help visually impaired users understand their surroundings by answering questions about images. Using the VizWiz dataset, the system is implemented in two approaches: classification-based (inspired by the "Less Is More" paper, using PaliGemma-2) and generation-based (using CLIP-ViT-GPT2, SigLIP-GPT2, and ViT-GPT2). The goal is to enhance accessibility and independence for visually impaired individuals through efficient and robust VQA.

## Dataset

VizWiz: A dataset of ~31,000 image-question pairs collected from visually impaired users.

Combined train and val into a single file (~25k samples) which is then split into train, val, test (80-10-10 split).

Each question has 10 answers out of which only the top answer is selected, the final combined file has image path, question and answer.

Challenges: Noisy images (blurry, poor lighting), diverse questions (average length: 7.14 words), and a large answer space (6503 unique answers, with 4843 "unanswerable").

Chosen for its real-world relevance and alignment with the project’s accessibility goals.

## Approaches

1. Classification-Based VQA (PaliGemma-2 + Classifier)

    - Inspired by the "Less Is More" paper, this approach focuses on efficiency:
    - Feature Extraction: Extracted features (shape: (1, 512)) using PaliGemma-2’s last hidden state, on VizWiz to extract robust features from image-question pairs.
    - Classifier Training: Trained a lightweight classifier (VQAClassifier: 512→256→6503) on the features.
    - Inference: New image-question pair → Extract features → Predict answer with classifier.
    - Advantages: Efficient, reliable predictions within a fixed vocabulary.
    - Challenges: Limited to 6503 answers, cannot generate open-ended responses.

2. Generation-Based VQA (CLIP-ViT-GPT2, SigLIP-GPT2, ViT-GPT2)

   - This approach explores generative VQA for flexibility:
   - Models:
      - CLIP-ViT-GPT2: CLIP’s Vision Transformer + GPT-2.
      - SigLIP-GPT2: SigLIP (improved CLIP) + GPT-2.
      - ViT-GPT2: Vision Transformer + GPT-2.
   - Pipeline: Image → Vision encoder → Embedding → Concatenate with question → GPT-2 generates answer.
   - Training: Fine-tuned each model end-to-end on VizWiz.
   - Advantages: Can generate open-ended answers beyond a fixed vocabulary.
   - Challenges: Resource-intensive, struggles with "unanswerable" cases.

## Results

1. Classification-Based (PaliGemma-2 + Classifier):
    - Test accuracy: ~71% (VizWiz is challenging due to noisy data).
    - Reliable for fixed answers, especially "unanswerable" cases.

2. Generation-Based (CLIP-ViT/SigLIP/ViT + GPT-2):
    - Test(semantic) accuracy: ~56% (struggles with precise answers on noisy data).
    - More flexible but less accurate, especially for "unanswerable" cases.

3. Comparison:
    - Classification is more efficient and reliable for VizWiz.
    - Generation offers flexibility but requires more resources and fine-tuning.

## Future Work

Develop a hybrid approach: Classify "unanswerable" first, then generate if answerable.

Expand vocabulary: Use larger generative models (e.g., GPT-3) for broader answer coverage.
