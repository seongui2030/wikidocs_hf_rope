# 10. 실시간 분석 정보 자막 오버레이

## 1. 학습 목표
* 영상 화면 상단에 학생 정보 및 실시간 각도 데이터를 가독성 높은 반투명 자막 패널로 그립니다.

## 2. 자막 오버레이 기법
* **반투명 배경 박스**: `cv2.rectangle`에 채움 옵션을 주어 상단에 검은색 배경 패널을 배치합니다.
* **실시간 정보 출력**: `cv2.putText`를 통해 학생 정보, 현재 측정된 관절 각도(팔꿈치/무릎 등)를 직관적으로 표현합니다.

## 3. 소스코드 예시

```python
import cv2

def overlay_caption(frame, student_id, student_name, elbow_deg, knee_deg):
    # 반투명 상단 검은색 패널 배경
    cv2.rectangle(frame, (10, 10), (500, 85), (0, 0, 0), -1)
    
    # 자막 텍스트 구성 및 그리기
    info_str = f"Student: {student_id} {student_name}"
    angle_str = f"Elbow: {int(elbow_deg)} deg | Knee: {int(knee_deg)} deg"
    
    cv2.putText(frame, info_str, (20, 35), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 255, 255), 2)
    cv2.putText(frame, angle_str, (20, 65), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (0, 255, 255), 2)
    return frame
```

## 4. 확인 문제
- 화면 자막의 가독성을 높이기 위해 자막 텍스트 하단에 검은색 배경 패널을 그릴 때 사용하는 OpenCV 함수는 무엇인가요?

    - **정답**: cv2.rectangle