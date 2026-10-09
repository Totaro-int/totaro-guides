# sLLM이란? LLM과의 차이, 구축 방법 4단계, 비용 · 사례 — 특화 경량 언어 모델 가이드

> 이 글은 토타로(TOTARO) 홈페이지의 가이드 원문을 옮긴 것입니다. 원문과 최신 갱신본: https://www.totaro.co.kr/guide/sllm-guide

sLLM(small Large Language Model, 소형 언어 모델)은 수십억 파라미터 이하의 경량 언어 모델을 특정 도메인 데이터로 추가 학습(fine-tuning)해 전문 업무에 맞춘 모델입니다. 범용 거대 모델(LLM)보다 비용과 지연이 낮고 사내 서버에서 운영할 수 있어 의료 · 금융처럼 데이터가 외부로 나갈 수 없는 업무에 적합합니다. 공개 연구는 데이터 품질이 좋으면 작은 모델도 전문 과제를 수행할 수 있고, 반복적인 에이전트 업무에는 소형 모델이 더 적합하다고 보고합니다.

### LLM과 sLLM 비교
'SLM'과 'sLLM'은 같은 대상을 가리키는 표기 차이입니다. 국내에서는 sLLM, 해외 논문에서는 SLM(Small Language Model)으로 많이 씁니다.
항목 | LLM(범용 거대 모델) | sLLM(특화 경량 모델)
규모 | 수백억~수조 파라미터 | 수억~수십억 파라미터
지식 | 범용, 최신성은 학습 시점까지 | 도메인 특화, 용어 · 서식을 학습
운영 위치 | 주로 클라우드 API | 사내 서버 · 로컬 가능
비용 · 지연 | 호출량에 비례, 지연 큼 | 고정 인프라, 지연 작음
데이터 보안 | 외부 전송 필요 | 데이터가 밖으로 나가지 않음
적합 업무 | 일반 질의, 다양한 과제 | 판독문 · 보고서 생성 등 반복되는 전문 과제

### sLLM 구축 4단계

1. 학습 데이터 구축 — 도메인 문서(예: CT 판독문)를 입력 · 출력 쌍으로 정리합니다. 양보다 품질과 서식 일관성이 결과를 좌우합니다.
2. 베이스 모델 선택 — 공개 경량 모델 중 언어(한국어) 성능과 라이선스를 확인해 고릅니다.
3. LoRA · QLoRA fine-tuning — 전체 파라미터 대신 저랭크 어댑터만 학습해 GPU 메모리와 시간을 줄입니다. QLoRA는 양자화된 모델에 적용해 더 적은 자원으로 학습합니다. Unsloth 같은 도구로 속도를 높입니다.
4. 평가 · 서빙 — 전문가가 결과물을 평가하고, Ollama 같은 로컬 런타임으로 사내 환경에 서빙합니다. 대량 동시 요청에는 vLLM 같은 고성능 서빙 엔진을 검토합니다.

### 비전 AI와의 결합
이미지 판정이 필요한 업무는 비전 모델을 함께 씁니다. YOLO 계열 객체 검출 모델은 차량 · 제품 이미지의 품질 판정처럼 실시간성이 필요한 현장에 쓰이고, 그 결과를 언어 모델이 보고서로 정리하는 구조가 가능합니다.

### 실 운영 사례 — CT 영상판독문 sLLM
대학병원 스타트업 MEDIVU와 진행한 CT 영상판독문 특화 sLLM은 16주가 걸렸고, 전문가 만족도 87%를 기록했습니다. 범용 모델이 놓치는 전문 용어와 서식을 학습한 경량 모델을 fine-tuning하고 로컬 런타임으로 서빙했습니다. 자세한 문제 · 해결 · 기술 스택은 포트폴리오 상세에 있습니다.

**자주 묻는 질문**
Q. sLLM과 LLM의 차이는 무엇인가요?
A. 규모와 용도입니다. LLM은 수백억 파라미터 이상의 범용 모델로 클라우드 API로 쓰고, sLLM은 수십억 이하의 경량 모델을 도메인 데이터로 추가 학습해 사내에서 운영합니다. 전문 용어 · 서식이 반복되는 업무와 데이터 보안이 중요한 업무에 sLLM이 맞습니다.

Q. sLLM 구축 방법은요?
A. 학습 데이터 구축 → 베이스 모델 선택 → LoRA · QLoRA fine-tuning → 전문가 평가와 로컬 서빙 4단계입니다. 데이터 품질이 결과를 좌우합니다.

Q. sLLM과 vLLM은 다른 건가요?
A. 다릅니다. sLLM은 모델의 종류(경량 특화 모델)이고, vLLM은 모델을 빠르게 서빙하기 위한 추론 엔진 소프트웨어입니다. sLLM을 vLLM으로 서빙할 수 있습니다.

Q. sLLM 종류에는 무엇이 있나요?
A. 공개 경량 모델 계열이 여럿 있으며, 한국어 성능과 라이선스(상업 이용 가능 여부)를 기준으로 고릅니다. 특정 모델이 정답은 아니고, 평가셋으로 과제에서 직접 비교하는 것이 확실합니다.

Q. sLLM 구축 비용은 무엇이 결정하나요?
A. 학습 데이터 구축 공수, GPU 학습 · 서빙 환경, 전문가 평가 반복 횟수입니다. LoRA · QLoRA로 학습 자원을 줄이고 로컬 서빙으로 운영비를 고정할 수 있습니다.

Q. 비전 AI란 무엇인가요?
A. 이미지나 영상에서 객체를 찾고 판정하는 인공지능입니다. 제조 · 물류 현장의 품질 검사, 차량 상태 판정처럼 사람이 눈으로 하던 판정을 자동화합니다. YOLO 계열 실시간 객체 검출이 대표적입니다.

**참고 자료**
[1] Ashish Vaswani et al. (2017). Attention Is All You Need (arXiv:1706.03762). https://arxiv.org/abs/1706.03762
[2] Edward J. Hu et al. (2021). LoRA: Low-Rank Adaptation of Large Language Models (arXiv:2106.09685). https://arxiv.org/abs/2106.09685
[3] Tim Dettmers et al. (2023). QLoRA: Efficient Finetuning of Quantized LLMs (arXiv:2305.14314). https://arxiv.org/abs/2305.14314
[4] Suriya Gunasekar et al. (2023). Textbooks Are All You Need (arXiv:2306.11644). https://arxiv.org/abs/2306.11644
[5] Peter Belcak et al. (2025). Small Language Models are the Future of Agentic AI (arXiv:2506.02153). https://arxiv.org/abs/2506.02153
[6] Unsloth AI (2025). Unsloth 공식 문서. https://docs.unsloth.ai/
[7] Ollama (2025). Ollama. https://ollama.com/
[8] Joseph Redmon et al. (2015). You Only Look Once: Unified, Real-Time Object Detection (arXiv:1506.02640). https://arxiv.org/abs/1506.02640

---
작성: 윤태준 (TOTARO CEO & Lead Developer) · 원문: https://www.totaro.co.kr/guide/sllm-guide
토타로는 정부지원사업 과제와 외주 프로젝트를 기획부터 제작, 실 운영 안착까지 한 팀이 진행하는 0→1 개발 파트너입니다. 30분 무료 진단: https://www.totaro.co.kr/#contact
