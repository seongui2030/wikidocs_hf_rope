# 08. YOLO Bounding Box 시각화

## 1. 학습 목표
* OpenCV 함수를 이용하여 탐지된 사람 객체의 테두리에 사각형 박스와 식별 텍스트 라벨을 그립니다.

## 2. 시각화 주요 함수
* **`cv2.rectangle`**: 감지된 좌상단 $(x_1, y_1)$ 및 우하단 $(x_2, y_2)$ 위치에 사각형 테두리를 그립니다.
* **`cv2.putText`**: 바운딩 박스 상단에 객체명("Person")을 텍스트로 오버레이합니다.

## 3. 소스코드 예시

```python
import cv2

def draw_yolo_boxes(frame, yolo_results, yolo_model):
    for box in yolo_results.boxes:
        cls_id = int(box.cls[0])
        if yolo_model.names[cls_id] == 'person':
            x1, y1, x2, y2 = map(int, box.xyxy[0])
            # 주황색 테두리 사각형 그리기
            cv2.rectangle(frame, (x1, y1), (x2, y2), (255, 120, 0), 2)
            # 텍스트 라벨 표시
            cv2.putText(frame, "Person", (x1, y1 - 10), 
                        cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 120, 0), 2)
    return frame
```

## 4. 확인 문제
- OpenCV에서 cv2.rectangle에 전달되는 색상 인자 (255, 120, 0)의 기본 순서는 무엇인가요?

    - **정답**: BGR (Blue, Green, Red)