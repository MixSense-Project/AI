# MixSense - AI_mixing (MVP2, Option 1)

입력은 **zip만** 받고, 내부에서 **bus를 자동 생성/캐시**한 뒤 bus 기반으로 믹싱합니다.

- duration: 150s
- transitions: 4~6 목표(기본 5), 품질 기준 k 자동 감소
- Style B: Backbone(A) 유지 + Inject(B) 침투
- Ending: 마지막 20~30초 Inject(B) 비중 증가
- Output: WAV 48kHz / 24-bit (PCM_24)

## Run
```bash
pip install -r requirements.txt
python scripts/run_demo.py --a "data/inputs/zips/Sauce 142bpm.zip" --b "data/inputs/zips/Newleaf 140bpm.zip" --out outputs
```
