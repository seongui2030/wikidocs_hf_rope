## 1. 학습 목표
* 체육 AI 자세 분석 시스템 구축에 필요한 6대 핵심 파이썬 라이브러리의 역할을 이해합니다.
* 개발 환경을 구성하고 필수 라이브러리를 정상적으로 설치 및 임포트(Import)합니다.

## 2. 라이브러리 소개 및 역할
* **OpenCV (`cv2`)**: 비디오 프레임을 읽고 프레임 상에 바운딩 박스나 관절 스켈레톤 라인을 시각화하며 비디오 파일로 저장합니다.
* **NumPy (`numpy`)**: 관절 좌표 데이터를 행렬 형태로 관리하고 삼각함수 기반의 관절 각도를 고속으로 계산합니다.
* **MediaPipe (`mediapipe`)**: 구글에서 개발한 비전 AI 기술로 신체 33개 관절 점(Landmark) 위치를 정밀 추적합니다.
* **Ultralytics (`ultralytics`)**: YOLOv8 모델을 가동하여 영상 내에서 분석 대상이 되는 사람(Person) 영역을 객체 탐지합니다.
* **gTTS (`gtts`) & MoviePy (`moviepy`)**: 진단 결과 텍스트를 한국어 음성(MP3)으로 변환하고 1.5배속으로 조절 및 웹 호환 영상(H.264)으로 인코딩합니다.
* **Gradio (`gradio`)**: 복잡한 웹 개발 과정 없이 파이썬 코드로 대시보드 사용자 인터페이스(UI)를 구성합니다.

## 3. 설치 및 임포트 소스코드

```python
# 필수 패키지 설치 명령어 (가상환경 또는 Google Colab 터미널)
# %pip install -q "numpy<2" "opencv-python<5" mediapipe ultralytics gradio gtts moviepy

import cv2                   # 영상 및 이미지 처리
import numpy as np           # 수학 및 배열 계산
import mediapipe as mp       # 관절 추적 AI
from ultralytics import YOLO  # 사람 탐지 AI
from gTTS import gTTS        # 텍스트 -> 음성 변환
from moviepy.editor import AudioFileClip, VideoFileClip # 음성/영상 편집
import gradio as gr          # 파이썬 웹 UI 생성
```

## 4. 확인 문제
- 영상에서 객체 중 사람(Person)의 영역을 찾아주는 객체 탐지 인공지능 라이브러리는 무엇인가요?

    - **정답**: Ultralytics (YOLO)
