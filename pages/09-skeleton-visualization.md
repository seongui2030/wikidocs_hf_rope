# 09. MediaPipe 스켈레톤(Skeleton) 시각화

## 1. 학습 목표
* 추출된 관절 좌표 데이터를 바탕으로 각 관절 점과 신체 뼈대 연결선을 영상 프레임에 직접 시각화합니다.

## 2. 스켈레톤 그리기의 원리
* **`cv2.line`**: 어깨-팔꿈치-손목, 골반-무릎-발목 등 주요 신체 연결 부위에 뼈대 선을 그립니다.
* **`cv2.circle`**: 주요 33개 관절 위치에 원을 그려 지점을 명확히 시각화합니다.

## 3. 소스코드 예시

```python
import cv2

def draw_skeleton(frame, landmarks, width, height):
    # 예시: 왼쪽 팔 (어깨 11, 팔꿈치 13, 손목 15)
    l_shoulder = (int(landmarks[11].x * width), int(landmarks[11].y * height))
    l_elbow = (int(landmarks[13].x * width), int(landmarks[13].y * height))
    l_wrist = (int(landmarks[15].x * width), int(landmarks[15].y * height))
    
    # 관절 연결선 그리기 (파란색)
    cv2.line(frame, l_shoulder, l_elbow, (255, 0, 0), 3)
    cv2.line(frame, l_elbow, l_wrist, (255, 0, 0), 3)
    
    # 관절 점 그리기 (빨간색)
    cv2.circle(frame, l_elbow, 6, (0, 0, 255), -1)
    return frame
```

## 4. 확인 문제
- OpenCV에서 프레임 상에 채워진 원을 그릴 때 두께 인자(thickness)로 설정해야 하는 값은 무엇인가요?

    - **정답**: -1