# 🔍 OCR Model — 메뉴판 텍스트 인식

> **관광 음식메뉴판에 특화된 한국어 문자 인식 모델 (Docker 컨테이너)**

| 기술 스택 | 역할 |
|-----------|------|
| **PyTorch** | 딥러닝 프레임워크 |
| **TPS-VGG-BiLSTM-Attn** | 4단계 텍스트 인식 파이프라인 |
| **Docker** | 격리된 추론 환경 |
| **AI Hub 데이터** | 관광 음식메뉴판 사전학습 모델 |

---

## 🧠 모델 아키텍처

```
입력 이미지 (크롭된 텍스트 영역)
    ↓
TPS (Thin Plate Spline)     ← 기하학적 왜곡 정규화
    ↓
VGG (Feature Extraction)    ← 시각적 특징 추출
    ↓
BiLSTM (Sequence Modeling)  ← 문자 시퀀스 학습
    ↓
Attention (Prediction)      ← 최종 문자열 예측
    ↓
출력: "제육볶음"
```

## ⚙️ 추론 실행

```bash
python text_recognition.py \
  --Transformation TPS \
  --FeatureExtraction VGG \
  --SequenceModeling BiLSTM \
  --Prediction Attn \
  --image_folder data/real_testset/ \
  --saved_model saved_models/TPS-VGG-BiLSTM-Attn-final/best_accuracy.pth \
  --batch_size 64 --workers 16 --imgH 64 --imgW 300
```

## 🐳 Docker 설정

```bash
# 공유 메모리 부족으로 인한 OOM 방지
docker run --shm-size=8GB ...
```

## 📦 출력

- 인식된 한국어 텍스트를 `.xlsx` 형태로 출력하여 다음 단계(번역 컨테이너)로 전달됩니다.
