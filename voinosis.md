# Voinosis (2023.12 - 2024.04)

## 작업 1: Interspeech (IEEE) 감정 인식
주제: Nature Condition Emotion Recognition 챌린지

기여
- 팀원들이 만든 모듈을 통합해 end-to-end 학습 파이프라인을 구성했습니다.
- 병렬 프로그래밍으로 연산 자원 활용도를 극대화했습니다.
- Whisper 기반 speech-to-text를 활용해 NLP 피처를 추가했습니다.
- 감정 카테고리 단어(분노, 긍정 등)를 세는 sentiment lexicon 방식을 구축했습니다.

결과
- valence score 2위
- 최종 종합 4위

관련 저장소
- InterspeechRecognition2025: feature extraction과 ensemble notebook이 포함된 대회 작업 공간
- is2025_llm_promt: STT 텍스트 기반 downstream 분석을 위한 LLM 프롬프트 실험

## 작업 2: VAD 모델 테스트
노이즈가 포함될 때 VAD 성능이 어떻게 저하되는지 시각화하고, 지표를 명확히 보여주는 컴포넌트를 만들었습니다.

관련 저장소
- snr_mix_test: 여러 SNR 조건의 합성 noisy audio를 생성해 VAD 강건성을 점검
- vad_preprocess: SGVAD 기반 전처리, SNR 테스트, TextGrid 파싱 평가
- vad_performance: Whisper 출력으로 TextGrid 라벨을 구성해 VAD 성능 평가
- noise_module_test: noise/VAD 모듈 구성과 벤치마킹을 위한 빠른 실험
