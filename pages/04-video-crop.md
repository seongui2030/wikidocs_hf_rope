# 04. 영상 업로드 및 구간 Cut (Crop) 처리

## 1. 학습 목표
* OpenCV의 `VideoCapture`를 이용해 영상의 FPS 및 지정 시간 프레임을 계산하는 방법을 배웁니다.
* 연산 효율성을 위해 전체 비디오 중 필요한 시간 구간만 슬라이싱하여 자르는 원리를 이해합니다.

## 2. 시간 기준 프레임 환산 원리
* **프레임 번호 계산**: $\text{Frame} = \text{초(Second)} \times \text{FPS}$
* **위치 이동**: `cap.set(cv2.CAP_PROP_POS_FRAMES, start_frame)`을 호출하여 영상 재생 위치를 분석 시작 지점으로 즉시 이동시킵니다.

## 3. 소스코드 예시

```python
import cv2

def crop_video_interval(video_path, start_sec=0, end_sec=5):
    cap = cv2.VideoCapture(video_path)
    fps = cap.get(cv2.CAP_PROP_FPS) or 30.0
    
    start_frame = int(start_sec * fps)
    end_frame = int(end_sec * fps)
    
    # 분석 시작 프레임 위치로 재생 헤드 이동
    cap.set(cv2.CAP_PROP_POS_FRAMES, start_frame)
    
    current_frame = start_frame
    while cap.isOpened() and current_frame < end_frame:
        ret, frame = cap.read()
        if not ret:
            break
        # 프레임별 처리 수행
        current_frame += 1
        
    cap.release()
```
## 4. 확인 문제
- 초당 프레임 수가 30 FPS인 영상에서 3초 지점의 시작 프레임 번호는 얼마인가요?

    - **정답**: 90 프레임 ($3 \times 30$)