# 06. MediaPipe Pose Landmarker와 관절 좌표

## 1. 학습 목표
* MediaPipe Pose 모델을 활용하여 인체의 33개 관절 랜드마크(Landmark) 위치를 좌표로 추출하는 원리를 익힙니다.
* 정규화된 관절 좌표를 영상 크기에 맞추어 Pixel 좌표로 변환합니다.

## 2. 관절 좌표 탐지 원리
* **33개 관절 인덱스**: 어깨(11, 12), 팔꿈치(13, 14), 손목(15, 16), 골반(23, 24), 무릎(25, 26), 발목(27, 28) 등 인체 주요 부위 제공
* **좌표 정규화 변환**: MediaPipe가 출력하는 $0.0 \sim 1.0$ 사이 비율 좌표에 영상의 가로($W$)와 세로($H$) 길이를 곱하여 정수 좌표 $(x, y)$로 환산합니다.

## 3. 소스코드 예시

```python
import mediapipe as mp
from mediapipe.tasks.python import vision

# MediaPipe Pose Task 모델 설정 및 생성
base_options = mp.tasks.python.BaseOptions(model_asset_path='pose_landmarker.task')
options = vision.PoseLandmarkerOptions(base_options=base_options)
pose_detector = vision.PoseLandmarker.create_from_options(options)

def extract_landmarks(frame, pose_detector):
    rgb_frame = cv2.cvtColor(frame, cv2.COLOR_BGR2RGB)
    mp_image = mp.Image(image_format=mp.ImageFormat.SRGB, data=rgb_frame)
    detection_result = pose_detector.detect(mp_image)
    return detection_result
```

## 4. 확인 문제
- MediaPipe Pose 모델이 탐지하는 인체 관절 점(Landmark)의 총 개수는 몇 개인가요?

    - **정답**: 33개