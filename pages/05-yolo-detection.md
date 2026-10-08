## 1. 학습 목표
* YOLOv8 경량화 모델(`yolov8n.pt`)의 특징과 역할을 이해합니다.
* 영상 프레임 내부의 여러 물체 중 사람(Person) 감지 클래스만 필터링하는 기법을 배웁니다.

## 2. YOLOv8 모델 개요
* **경량화 모델**: Nano 버전(`yolov8n.pt`)을 사용하여 속도와 정확도의 균형을 맞춰 실시간 탐지에 적합합니다.
* **사람 클래스**: YOLO의 COCO 데이터셋 기본 인덱스 중 `0`번 또는 `names[cls_id] == 'person'` 식별자를 사용합니다.

## 3. 소스코드 예시

```python
from ultralytics import YOLO

# YOLOv8 Nano 모델 로드
yolo_model = YOLO('yolov8n.pt')

def detect_person(frame):
    results = yolo_model(frame, verbose=False)[0]
    person_boxes = []
    
    for box in results.boxes:
        cls_id = int(box.cls[0])
        if yolo_model.names[cls_id] == 'person':
            x1, y1, x2, y2 = map(int, box.xyxy[0])
            person_boxes.append((x1, y1, x2, y2))
            
    return person_boxes
```
## 4. 확인 문제
- YOLO 모델에서 사람(Person) 객체를 탐지했을 때 얻게 되는 4개의 위치 좌표값은 무엇을 나타내나요?

    - **정답**: 바운딩 박스(Bounding Box)의 좌상단 및 우하단 좌표 $(x_1, y_1, x_2, y_2)$
