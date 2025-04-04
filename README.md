# 한국어 Small Language Model 구축 보고서

## 서론
최근 인공지능 분야에서 생성형 AI와 거대 언어 모델(LLM)의 발전은 놀라운 성과를 보여주고 있습니다. 그러나 LLM은 높은 컴퓨팅 자원 요구량과 특정 도메인에 대한 전문성 부족이라는 한계를 지니고 있습니다. 이에 따라, 자원 제약적인 환경에서 효율적으로 작동하며 특정 목적에 최적화된 **Small Language Model (SLM)**에 대한 관심이 증가하고 있습니다. 특히 **한국어 SLM**은 한국어의 고유한 언어적 특성을 반영하여, 한국어 데이터에 특화된 성능을 제공할 수 있다는 점에서 그 중요성이 부각됩니다.

본 보고서는 처음부터 한국어 SLM을 구축하고자 하는 사용자를 위해 **단계별 가이드라인**을 제시합니다. 모델 구축의 전 과정, 필요한 자원, 예상되는 결과, 그리고 성능 최적화 방법에 대해 상세히 다루며, 특히 **C, Rust, Go**와 같은 시스템 프로그래밍 언어를 활용한 최적화 전략과 그 효과를 심층적으로 분석합니다. SLM은 특정 한국어 자연어 처리(NLP) 작업에서 LLM보다 높은 효율성과 정확성을 제공할 수 있으며, 이는 컴퓨팅 자원 효율성뿐만 아니라 도메인 특화 성능 측면에서도 큰 이점을 갖습니다.

---

## Small Language Model 구축의 기본 단계

### 1. 데이터 수집 및 전처리
- **데이터 수집**  
  - 고품질 한국어 텍스트 데이터를 수집합니다.  
  - 예시 데이터셋:  
    - **위키백과 한국어**: 일반적인 대규모 텍스트 데이터  
    - **CC-100**: 광범위한 웹 크롤링 데이터  
    - **NamuWiki**: 특정 주제 중심의 텍스트  
    - **청와대 국민청원**: 공식적이고 특정 형식의 데이터  
    - **KorNLI, KorSTS, KLUE**: 특정 NLP 작업용 데이터셋  
  - 모델의 목표와 용도에 맞는 데이터셋을 신중히 선택해야 합니다.

- **데이터 전처리**  
  - 한국어 특화 전처리 과정이 필수적입니다.  
  - 주요 단계:  
    - HTML 태그, 특수 문자, URL, 이메일 주소 제거  
    - 형태소 분석 (Mecab, KoNLPy 활용)  
    - 토큰화 (BPE, SentencePiece 등)  
    - 불용어 제거 및 어간/표제어 추출  
  - 한국어는 띄어쓰기가 모호하고 조사가 중요하므로, 적절한 토큰화 전략이 모델 성능에 큰 영향을 미칩니다.

### 2. 모델 아키텍처 설계
- **아키텍처 선택**  
  - **Transformer**: Self-attention 메커니즘으로 문맥 이해에 강력  
  - **LSTM/GRU**: 계산량이 적어 SLM에 적합하나 긴 문맥 학습에 한계  
  - **Knowledge Distillation**: DistilBERT, GPT-2 등 사전 학습된 모델 활용  
  - 한국어의 자유로운 어순과 의미 관계를 반영한 설계가 중요합니다.

- **고려 사항**  
  - 모델 크기, 속도, 한국어 문법 이해를 돕는 토큰화 방식 (예: Sub-character level, Bidirectional BPE)

### 3. 모델 학습
- **학습 환경 설정**  
  - 도구: Python, TensorFlow/PyTorch, Hugging Face Transformers  
  - 데이터 로딩: Hugging Face Datasets 활용  

- **학습 과정**  
  - **사전 학습 (Pre-training)**: 대규모 unlabeled 데이터로 언어 패턴 학습  
  - **Fine-tuning**: 특정 작업에 맞춘 추가 학습  
  - 하이퍼파라미터 튜닝 (학습률, 배치 크기 등)이 성능 최적화에 필수

### 4. 모델 평가
- **평가 지표**  
  - Perplexity, Accuracy, F1 score, BLEU, ROUGE  
  - 텍스트 생성 품질은 Human Evaluation 병행 추천  

- **벤치마크 데이터셋**  
  - KLUE, KMMLU, CLIcK, HAE_RAE_BENCH 등 한국어 데이터셋 활용

### 5. 저장 공간 및 컴퓨팅 성능 요구 사항
- **저장 공간**  
  - 데이터셋 및 모델 크기에 따라 수십 GB 이상 필요  

- **컴퓨팅 성능**  
  - GPU 권장, 모델 크기에 따라 자원 요구량 변동  
  - 소규모 SLM은 CPU로도 학습 가능 (예: DistilBERT)

---

## Small Language Model의 기대 결과
- **텍스트 생성**  
  - 특정 도메인에서 고품질 텍스트 생성 가능  
  - Fine-tuning으로 LLM 수준 성능 달성 가능  

- **NLP 작업 수행**  
  - 텍스트 분류, 감성 분석, 질의 응답, 번역 등에서 효율적이고 정확한 결과  
  - 도메인 특화 시 LLM보다 우수한 성능  

- **성능 비교**  
  - 특정 작업에서 LLM과 유사하거나 더 나은 성능  
  - 자원 효율성과 특화성이 강점

---

## C, Rust, Go를 통한 최적화
- **모델 양자화**  
  - 추론 속도 2배~8배 향상 (예: GPTQ, 4비트 양자화)  

- **가지치기 및 희소성**  
  - 모델 크기 감소, 추론 속도 증가 (예: Sparse Llama)  

- **추론 라이브러리**  
  - **C/C++**: llama.cpp, ONNX Runtime, OpenVINO, mlpack  
  - **Rust**: llm, rllama, lm.rs, burn  
  - **Go**: Llama.go, gomlx, langchaingo, fun  

- **학습 성능 최적화**  
  - 분산 학습 (DeepSpeed 등), PEFT (LoRA), 병렬 데이터 로딩

---

## 최적화를 통한 성능 향상 기대치
- **추론 속도**: 1.2배~8배 향상  
- **학습 시간 단축**: LoRA, DEMs로 최대 91% 비용 절감  
- **모델 압축**: 저장 공간 및 메모리 사용량 감소

---

## 한국어 Small Language Model 구축 진행 순서 및 목차
1. **환경 설정**  
   - Python, TensorFlow/PyTorch, Hugging Face 설치  

2. **데이터 수집 및 전처리**  
   - 한국어 데이터셋 선정 및 전처리  

3. **모델 아키텍처 선택**  
   - Transformer, LSTM, GRU 중 결정  

4. **모델 학습**  
   - 사전 학습 및 Fine-tuning 수행  

5. **모델 평가**  
   - 평가 지표 및 벤치마크 활용  

6. **최적화**  
   - 양자화, 가지치기, 시스템 언어 활용  

7. **배포**  
   - 로컬 또는 서버 환경에 모델 배포  

---

## 결론
한국어 SLM 구축은 데이터 준비, 모델 설계, 학습, 평가, 최적화, 배포 등 여러 단계를 거치는 복잡한 과정입니다. 각 단계에서 한국어의 언어적 특성을 고려한 신중한 접근이 필요하며, **C, Rust, Go**를 활용한 최적화는 성능을 크게 향상시킬 수 있습니다. 본 보고서는 한국어 SLM 구축에 대한 포괄적인 가이드라인을 제공하며, 실제 프로젝트 진행에 실질적인 도움을 줄 것입니다.

--- 

이 README.md는 보고서 내용을 간결하고 체계적으로 정리하여, 사용자가 한국어 SLM 구축 과정을 쉽게 이해하고 따라 할 수 있도록 구성되었습니다.