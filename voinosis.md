# Voinosis (2023.12 - 2024.04)

## Task 1: Interspeech (IEEE) Emotion Recognition
Focus: Nature Condition Emotion Recognition challenge.

Contributions
- Integrated teammates' modules for end-to-end training
- Maximized compute usage with parallel programming
- Added NLP features using Whisper for speech-to-text
- Built a sentiment lexicon approach to count category words (anger, POS, etc.)

Results
- 2nd place in valence score
- 4th place overall

Repositories (one-line roles)
- InterspeechRecognition2025: competition workspace with feature extraction and ensemble notebooks
- is2025_llm_promt: LLM prompt tests on STT text for downstream analysis

## Task 2: VAD Model Testing
Built a component to visualize how noise inclusion degrades VAD performance and report the metric clearly.

Repositories (one-line roles)
- snr_mix_test: generates synthetic noisy audio at multiple SNR levels to probe VAD robustness
- vad_preprocess: SGVAD-based preprocessing with SNR tests and TextGrid parsing for evaluation
- vad_performance: VAD performance evaluation that builds TextGrid labels from Whisper outputs
- noise_module_test: quick experiments around noise/VAD module setup and benchmarking
