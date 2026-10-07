# 07. 삼각함수를 활용한 관절 각도 계산

## 1. 학습 목표
* 세 좌표(A, B, C)가 이루는 사이각을 아크탄젠트(`np.arctan2`) 삼각함수로 구하는 공식을 배웁니다.
* 신체 관절(예: 팔꿈치, 무릎, 골반)의 꺾임 각도를 $0^\circ \sim 180^\circ$ 범위로 계산하는 함수를 구현합니다.

## 2. 삼각함수 각도 계산 원리
* **기울기각 연산**: `np.arctan2(y, x)`를 사용하여 중심점 $B$를 기준으로 벡터 $\vec{BA}$와 $\vec{BC}$의 기울기 각도를 계산합니다.
* **각도 보정**: 라디안을 도(Degree) 단위로 환산한 후, 사이각이 $180^\circ$를 초과할 경우 $360^\circ - \text{angle}$을 적용하여 최단 꺾임 각도를 구합니다.

## 3. 소스코드 예시

```python
import numpy as np

def calculate_angle(a, b, c):
    a, b, c = np.array(a), np.array(b), np.array(c)
    
    # b점을 중심으로 두 선분의 라디안 각도 차이 계산
    radians = np.arctan2(c[1] - b[1], c[0] - b[0]) - np.arctan2(a[1] - b[1], a[0] - b[0])
    angle = np.abs(radians * 180.0 / np.pi)
    
    # 0 ~ 180도 범주로 보정
    if angle > 180.0:
        angle = 360.0 - angle
    return angle
```

## 4. 확인 문제세 
- 관절 좌표 중 팔꿈치 꺾임 각도를 측정하고자 할 때, 가운데 중심점($B$) 역할을 하는 관절은 어디인가요?

    - **정답**: 팔꿈치(Elbow)