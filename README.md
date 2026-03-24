## 📄 Paper Review

### BoostTrack: Boosting the Similarity Measure and Detection Confidence for Improved Multiple Object Tracking

---

## 1. Background

### ByteTrack

기존 다중 객체 추적(MOT)에서 널리 사용되는 ByteTrack은  
**Detection Confidence(검출 신뢰도)**를 기반으로 객체를 추적하는 방식이다.

#### 🔹 핵심 아이디어
- 높은 신뢰도의 detection을 우선적으로 사용하여 track과 매칭
- 낮은 신뢰도의 detection도 활용하여 누락된 객체 보완

#### 🔹 두 단계 연관 과정 (Two-stage Association)

1. **High-confidence matching**
   - 높은 신뢰도 detection과 기존 tracklet 매칭

2. **Low-confidence matching**
   - 남은 tracklet과 낮은 신뢰도 detection 추가 매칭

---

## 2. Problem

ByteTrack은 효과적이지만 다음과 같은 한계를 가진다:

### ❗ 주요 문제점

1. **Detection Confidence 의존성**
   - 신뢰도가 낮거나 잘못된 경우 성능 저하

2. **IoU 기반 유사도 한계**
   - 객체 간 겹침이 많을 경우 잘못된 매칭 발생

3. **Multi-stage 구조의 문제**
   - Low-confidence 단계에서 ID Switch(IDSW) 발생 가능

4. **실시간성 저하 가능성**
   - 두 단계 매칭으로 인한 연산 증가

---

## 3. Proposed Method: BoostTrack

BoostTrack은 ByteTrack의 한계를 해결하기 위해  
**유사도 행렬 개선 + 탐지 신뢰도 보정**을 결합한 방법이다.

---

## 4. Method

### 4.1 Similarity Matrix Boosting

#### 1) Detection-Tracklet Confidence (DTC) Boost
- 신뢰도가 높은 detection과 tracklet을 우선적으로 매칭
- 신뢰도 기반으로 유사도 가중치 조정

---

#### 2) Mahalanobis Distance (MhD) Boost
- Kalman Filter의 공분산을 활용한 거리 기반 유사도
- Softmax 정규화를 통해 확률 형태로 변환

👉 효과:
- ambiguity(모호한 매칭) 감소
- 여러 후보 중 더 적절한 매칭 선택 가능

---

#### 3) Shape Similarity Boost
- IoU 외에 객체 크기(shape) 정보 추가

👉 효과:
- 크기가 다른 객체 간 잘못된 ID 연결 방지
- 크기가 유사한 경우 매칭 강화

---

### 4.2 Detection Confidence Boosting

#### 1) Detecting Likely Objects (DLO)
- IoU 기반으로 기존 tracklet과 겹치는 detection의 신뢰도 증가

👉 핵심:
- 가려진 객체(occlusion)도 유지 가능

---

#### 2) Detecting “Unlikely” Objects (DUO)
- 기존 tracklet과 멀리 떨어진 detection은 새로운 객체로 판단

👉 방법:
- Mahalanobis distance 사용
- 일정 threshold 이상이면 새로운 객체로 간주

---

## 5. Key Idea Summary

### ✔ Similarity Boost (3가지)
- DTC Boost
- Mahalanobis Distance Boost
- Shape Similarity Boost

### ✔ Detection Confidence Boost (2가지)
- DLO (Likely Objects)
- DUO (Unlikely Objects)

---

## 6. Result

- 기존 ByteTrack 대비 더 안정적인 tracking 성능
- ID Switch 감소
- 의미 있는 detection 유지

👉 핵심 성과:
- **One-stage association 구조 유지**
- 실시간 성능 확보
- MOT17 / MOT20에서 경쟁력 있는 성능 달성

---

## 7. Conclusion

BoostTrack은 단순히 detection을 추가 활용하는 수준을 넘어,  
**유사도와 신뢰도를 동시에 개선**하여 추적 성능을 향상시킨 방법이다.

특히,
- Multi-stage 구조의 한계를 보완하면서
- 실시간성과 정확도를 동시에 확보했다는 점에서 의미가 있다.

---

## 8. My Insight

- 기존 MOT 문제에서 “유사도 정의”가 성능에 핵심적임을 확인
- 단순 IoU 기반 접근은 실제 환경에서 한계가 존재
- 거리 기반 + 신뢰도 기반 결합이 효과적인 방향

