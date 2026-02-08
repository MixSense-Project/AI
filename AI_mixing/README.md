# MixSense AI Mixing (ML + DSP)

MixSense의 **AI DJ Mixing 엔진**입니다.  
두 곡(스템 기반)을 입력받아, **전이(transition) 구간을 계획(ML)**하고 **스템 레벨에서 실제로 레이어를 섞어(render)** 2~3분 길이의 “하나의 믹스 트랙”을 생성합니다.

- Input: 2 tracks (stems in zip)
- Output: mixed audio (`.wav`) + explainable plan (`*_plan.json`)
- Core: **ML-based transition planner + DSP stem renderer**

---

## What this module does

### 1) Audio analysis (feature extraction)
각 트랙에서 다음 특징을 추출합니다.

- Tempo / BPM 안정성
- Key 추정 + confidence
- Energy profile (구간별 에너지 곡선)
- Stem availability (drums / bass / harmony / fx / mix 등)

> 이 특징들은 “어떤 전이 전략이 안전하고 설득력 있는가”를 결정하는 입력이 됩니다.

### 2) ML decides a mixing strategy (explainable)
두 트랙의 pair-feature를 만들고, 학습된 모델이 전이 전략을 고릅니다.

- `strategy_clf.joblib` : 전이 전략 분류 (예: FULL_TAKEOVER / DRUM_SWAP / HARMONY_BLEND 등)
- `transition_reg.joblib` : 전이 시점/강도(duck/peak 등) 회귀

결과는 `plan.json`으로 저장되어 프론트에서 그대로 “왜 이렇게 섞였는지” 설명 UI에 사용 가능합니다.

### 3) Rendering (stem-level mixing)
선택된 전략을 따라 stem bus를 구성하고,
- **드럼/베이스/하모니 레이어를 교체하거나 병치**
- 전이 구간에서는 ducking + band-limit(HPF 등) + fade를 조합
- 마지막은 **end fade-out**로 자연스럽게 종료

---

## Why this is NOT just a crossfade 

단순 crossfade는 “A를 줄이고 B를 키우는” 2채널 볼륨 보간입니다.  
MixSense는 그보다 한 단계 위로, **곡을 ‘스템/파트 단위로 분해된 요소’로 보고 실제로 재조합**합니다.

### Crossfade (baseline)
- A 전체 볼륨 ↓, B 전체 볼륨 ↑
- 전이 품질은 주로 “볼륨 곡선”에만 의존
- 두 곡이 겹치는 동안 **리듬/하모니 충돌**이 나도 해결 불가

### MixSense Mixing (our approach)
- 드럼/베이스/하모니 등 **stem bus 단위로 교체/병치**
- 전이 구간에서 “무엇을 남기고, 무엇을 바꿀지”를 **전략(ML)**로 결정
- plan.json으로 “왜 이 선택을 했는지”를 설명 가능 (key/bpm/energy 근거)

즉, 결과물은 **단순 페이드가 아니라 ‘레이어링 기반 재편곡’에 가까운 믹스**입니다.

---

## Quickstart

### 0) Install
```bash
cd AI/AI_mixing
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
