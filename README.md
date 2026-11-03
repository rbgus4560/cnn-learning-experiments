# CNN 이미지 분류 및 학습 조건 비교

CNN의 구성과 학습·평가 과정을 이해하기 위해 Keras와 PyTorch로 이미지 분류 모델을 구성하고, 프레임워크·옵티마이저·모델 구조에 따른 결과를 비교한 학습 프로젝트입니다.

**최고 정확도 달성보다, 각 구성 요소의 역할을 이해하고 실험 결과를 읽고 설명하는 데 목적을 두었습니다.**

## 학습 및 구현 범위

| 구분 | 내용 |
|---|---|
| CNN 구성 | Convolution, Batch Normalization, ReLU, Pooling, GAP/Flatten, Fully Connected, Dropout |
| 프레임워크 | Keras의 compile/fit과 PyTorch의 forward·loss·backward·optimizer.step 비교 |
| 학습 설정 | Adam·SGD, 학습률, Epoch, Batch Size, Seed 설정 |
| 모델 구조 | Dropout 비율, Spatial Dropout, FC 크기 변경 |
| 평가 | Accuracy, Precision, Recall, F1, Confusion Matrix, 학습·검증 곡선 |
| 모델 선택 | 마지막 Epoch 모델과 검증 손실 최소 체크포인트 비교 |

## 실험 구성

| 데이터 | 구성 | 실험 수 |
|---|---|---:|
| Fashion-MNIST | Keras/PyTorch × Adam/SGD | 4 |
| Furniture | Keras/PyTorch × Adam/SGD × 구조 4단계 | 16 |
| Furniture 원인 분석 | 기본 모델 / BatchNorm 제거 / 학습률 감소 | 3 |

가구 분류의 구조 변경은 `Baseline → Dropout → SpatialDropout → FC64` 순서로 이전 단계에 변경을 누적합니다. 각 단계는 해당 변경만의 효과를 독립적으로 입증하는 실험과 구분합니다.

## 데이터와 처리 흐름

`데이터 입력 → 크기 조정·정규화 → CNN 특징 추출 → 클래스 예측 → 손실 계산·가중치 갱신 → 검증·테스트 → 결과 저장`

- Fashion-MNIST: 의류 이미지 10종. 공식 Train 60,000장을 Train 48,000 / Validation 12,000으로 분리하고 공식 Test 10,000장으로 평가합니다.
- Furniture: `almirah / chair / fridge / table / tv` 5종. `train / valid / test` 폴더를 읽으며 128×128 RGB 입력을 사용합니다.
- 학습·검증·테스트 코드와 저장 결과를 포함합니다. 데이터셋 원본 및 모델 가중치는 포함하지 않습니다.

## 먼저 볼 자료

1. [Keras Fashion-MNIST](notebooks/comparison/01_FashionMNIST_Keras_Adam.ipynb) / [PyTorch Fashion-MNIST](notebooks/comparison/03_FashionMNIST_PyTorch_Adam.ipynb): 두 프레임워크의 학습 코드 비교
2. [가구 기본 모델](notebooks/comparison/05_Furniture_Keras_Adam_Baseline.ipynb) → [FC64 단계](notebooks/comparison/08_Furniture_Keras_Adam_FC64.ipynb): 구조 변경 과정
3. [20개 실험 결과 표](results/comparison_summary.csv) / [통합 분석 코드](notebooks/analysis/compare_20_experiments.ipynb)
4. [검증 변동 원인 분석](notebooks/analysis/furniture_cause_analysis.ipynb) / [실험 해석](docs/experiments.md)

## 학습 결과 예시

![학습 정확도 곡선](results/comparison/FashionMNIST_Keras_Adam/accuracy_curve.png)

![가구 분류 혼동 행렬](results/comparison/Furniture_Keras_Adam_FC64/final_confusion_matrix.png)

그래프·CSV는 기존 실행에서 저장한 기록입니다. 저장소 정리 중 학습을 다시 실행하지 않았습니다. 수치만으로 특정 프레임워크나 옵티마이저가 항상 우수하다고 결론 내리지 않습니다.

## 실행 방법

Google Colab에서 `notebooks/comparison`의 노트북 하나를 열고 셀을 위에서 아래로 실행합니다.

- Fashion-MNIST는 노트북의 데이터 로더를 사용합니다.
- Furniture는 Google Drive의 `MyDrive/dataset/furniture.zip`이 필요합니다. 압축 내부에는 `train / valid / test`와 각 클래스 폴더가 있어야 합니다. 코드 주석 일부의 `val` 표기와 달리 실제 코드는 `valid`를 확인합니다.
- 노트북은 Google Drive 연결, 로컬 압축 해제 폴더 초기화, 모델·결과 저장 및 ZIP 다운로드를 포함합니다. 실행 전 경로와 초기화 구간을 확인합니다.
- [환경·데이터 준비](docs/setup.md)에서 의존성과 폴더 구조를 확인할 수 있습니다.

## 자료 구성

```text
notebooks/comparison/  # 발표용 비교 실험 20개
notebooks/analysis/    # 통합 분석 및 원인 분석
results/comparison/   # 20개 실험의 CSV·그래프
results/cause_analysis/ # 원인 분석 3개 결과
docs/                 # 실행 환경·실험 해석·자료 출처
```

## 확인 범위

기존 자료에는 초기 과일·가구 실험과 발표용 재실험이 함께 존재합니다. 이 저장소는 조건을 정리한 20개 비교 실험과 별도 원인 분석 3개를 중심으로 구성하여 서로 다른 실행 결과를 섞지 않았습니다. 노트북 실행 출력은 정리본에서 제거하고 저장된 결과를 별도 폴더에 보존했습니다.

가구 실험의 높은 테스트 수치가 실제 환경의 일반화 성능을 보장하지는 않습니다. 원본 이미지의 중복 여부와 데이터 분할 간 유사성은 이번 정리에서 재검증하지 않았습니다. 반복 Seed에 대한 통계적 검증도 포함하지 않습니다.

학습 자료를 통해 CNN 구현·학습·평가·실험 비교를 설명하기 위한 저장소이며 배포 서비스나 성능 보장 제품을 의미하지 않습니다.
