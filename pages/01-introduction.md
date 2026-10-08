## 1. 학습 목표
* 인공지능 기술(컴퓨터 비전)이 체육 수업의 운동 자세 분석 및 교정에 활용되는 원리를 이해합니다.
* 컴퓨터 비전, 삼각함수, 음성 합성(TTS), 파이썬 웹 UI 기술을 융합하여 나만의 피트니스 AI 코치를 완성합니다.

## 2. 준비물 및 개발 환경
* **언어**: Python 3.10+
* **핵심 라이브러리**: OpenCV, NumPy, MediaPipe, Ultralytics(YOLOv8), gTTS, MoviePy, Gradio
* **플랫폼**: Google Colab 또는 Hugging Face Spaces

## 3. 전체 시스템 구조
본 프로젝트는 **"AI 동반 코치(Co-Coach) 코코"** 시스템으로, 사용자가 운동 영상을 업로드하면 지정된 시간 구간 동안 YOLOv8로 사람을 감지하고, MediaPipe로 33개 관절 좌표를 추출 및 각도를 계산하여 자막 및 1.5배속 음성 피드백을 제공합니다.

## 4. 확인 문제
- 영상에서 인물의 33개 관절 위치(Landmark)를 추출하는 데 사용되는 대표적인 Google의 AI 라이브러리는 무엇인가요?
    - **정답**: MediaPipe
