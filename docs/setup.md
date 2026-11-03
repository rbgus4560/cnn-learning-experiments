# 실행 환경과 데이터 준비

기존 코드는 Google Colab 기준입니다. Python, TensorFlow/Keras, PyTorch, torchvision, NumPy, pandas, matplotlib, scikit-learn, Pillow를 사용합니다. 정확한 기존 패키지 버전은 확정하지 않았으며 requirements.txt는 버전 고정 없는 참고 목록입니다. CUDA 환경에 맞는 PyTorch 설치는 해당 런타임에 맞춰 진행합니다.

```text
furniture.zip
└─ (선택적인 상위 폴더)
   ├─ train/{almirah,chair,fridge,table,tv}/
   ├─ valid/{almirah,chair,fridge,table,tv}/
   └─ test/{almirah,chair,fridge,table,tv}/
```

데이터셋의 원본 배포 주소·라이선스는 현재 자료에서 확정하지 않았으므로 이미지는 재배포하지 않습니다. 원본 데이터를 별도로 준비해야 합니다. 원인 분석 노트북과 통합 분석 노트북은 각자의 설정 셀에 지정된 Google Drive 경로를 확인해 실행합니다.

기존 노트북은 지정한 Colab 작업 폴더를 삭제한 뒤 압축을 푸는 구간이 있습니다. 개인 파일이 있는 경로로 변경하지 마세요. 로컬 실행을 위한 자동 변환은 수행하지 않았습니다.
