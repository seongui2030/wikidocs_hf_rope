---

marp: true
theme: default
paginate: true
header: "AI 체육 자세 분석 및 피드백 시스템"
footer: "정보 & 체육 융합 수업 프로젝트"
style: |
  section {
    font-family: 'Pretendard', 'Gothic', sans-serif;
    background-color: #f8fafc;
    padding: 40px 60px;
  }
  h1 { color: #0f172a; font-size: 2.1em; }
  h2 { color: #2563eb; font-size: 1.4em; border-bottom: 2px solid #cbd5e1; padding-bottom: 8px; }
  h3 { color: #475569; font-size: 1.1em; }
  p, li { color: #334155; font-size: 0.9em; line-height: 1.6; }
  strong { color: #1e40af; }
  code { background-color: #e2e8f0; color: #0f172a; padding: 2px 6px; border-radius: 4px; font-size: 0.85em; }
  pre { background-color: #d4d4d4; color: #d4d4d4; border-radius: 8px; padding: 12px; font-size: 0.78em; border: 2px solid #3c3c3c; border-radius: 6px; }
  table { font-size: 0.82em; width: 100%; margin-top: 10px; }
  th { background-color: #2563eb; color: white; }
  blockquote { border-left: 5px solid #2563eb; background-color: #eff6ff; padding: 8px 12px; margin-top: 10px; }

---

## 📘 체육시간에 쓴 인공지능 : YOLO와 MediaPipe로 구축하는 피트니스 AI
### 🎯 단원 학습 목표
* **AI와 체육의 만남**: 인공지능 기술이 스포츠 분석 및 자세 교정에 어떻게 활용되는지 이해합니다.
* **컴퓨터 비전 기초**: 영상 속에서 사람의 위치를 찾는 YOLOv8과 몸의 관절 좌표를 추적하는 MediaPipe Pose 기술의 원리를 배웁니다.
* **수학적 자세 진단**: 관절 좌표 간의 각도를 계산하여 올바른 운동 자세인지 판단하는 알고리즘을 직접 코딩합니다.
* **대시보드 앱 구현**: Gradio 라이브러리를 활용해 누구나 쉽게 영상을 업로드하고 음성 피드백(gTTS)을 받을 수 있는 AI 체육 앱을 완성합니다.

---

## 🏃‍♂️ Slide 01. 교재 및 체육 AI 소개

### 컴퓨터 비전 기술로 체육 수업 동작을 자동 진단하는 대시보드 구축

* **프로젝트 목적**: 영상 속 관절 변화를 분석하여 실시간 자세 피드백 제공
* **핵심 기술 요소**:
  1. **컴퓨터 비전**: 영상 속 인물과 관절을 탐지
  2. **수학적 모델링**: 관절 위치 기반 꺾임 각도 산출
  3. **멀티미디어 피드백**: 시각 자막 및 음성(TTS) 안내

---

## 🛠️ Slide 02. Python 개발 환경 및 필수 라이브러리

프로젝트에 사용되는 6대 핵심 라이브러리의 역할과 기능을 파악합니다.

| 라이브러리 | 역할 및 핵심 기능 |
| :--- | :--- |
| **`cv2` (OpenCV)** | 비디오 프레임 읽기, 도형/스켈레톤 시각화, 웹용 H.264 인코딩 |
| **`numpy`** | 관절 좌표 행렬 계산 및 삼각함수 기반 꺾임 각도 측정 |
| **`mediapipe`** | 몸의 33개 관절 위치(Landmark) 3D 좌표 추적 |
| **`ultralytics`** | YOLOv8 모델 기반 사람(Person) Bounding Box 탐지 |
| **`gradio`** | 파이썬 기반 인터랙티브 웹 UI 레이아웃 구현 |

---

## 🎨 Slide 03. Gradio 기초 및 UI 레이아웃 설계

Gradio `Blocks`를 활용하여 사용자 입력값과 출력 창이 배치될 **대시보드 화면**을 구성합니다.

```python
import gradio as gr

with gr.Blocks(title="피트니스 AI 대시보드") as demo:
    gr.Markdown("# 🏃‍♂️ AI 체육 자세 분석 시스템")
    
    with gr.Row():
        selected_sport = gr.Radio(["줄넘기", "배구", "축구"], value="줄넘기", label="종목 선택")
        student_id = gr.Textbox(label="학번")
        student_name = gr.Textbox(label="이름")
        
    video_input = gr.Video(label="운동 영상 업로드")
    btn_analyze = gr.Button("🔍 AI 자세 분석 실행", variant="primary")
```

---

## ✂️ Slide 04. 영상 업로드 및 구간 Cut (Crop) 처리

분석 효율을 올리기 위해 전체 영상 중 필요한 구간(0~5초)만 자르는 법을 배웁니다.

- 시간 기준 프레임 변환: 프레임 번호 = 시간(초) × FPS
- 시작 위치 이동: cap.set(cv2.CAP_PROP_POS_FRAMES, start_frame)

```python
import cv2

cap = cv2.VideoCapture("input_sample.mp4")
fps = cap.get(cv2.CAP_PROP_FPS) or 30.0

# 0초부터 5초까지 지정 구간 프레임 환산
start_frame = int(0.0 * fps)
end_frame = int(5.0 * fps)

# 해당 시작 프레임 위치로 이동
cap.set(cv2.CAP_PROP_POS_FRAMES, start_frame)
```
| 💡 핵심 포인트: 필요한 시간 영역만 슬라이싱하면 AI 처리 속도가 크게 향상됩니다.

---

## 🔍 Slide 05. YOLOv8 객체 탐지: 사람(Person) 감지

영상 내 여러 배경 요소 중 분석 대상이 되는 사람 영역만 분리합니다.

- 사용 모델: lightweight 인공지능 모델 yolov8n.pt (Nano 버전)
- 클래스 필터링: person (클래스 ID = 0) 탐지 박스 추출

```python
from ultralytics import YOLO

# YOLOv8 Light 모델 로드
yolo_model = YOLO('yolov8n.pt')

# 프레임 단위 객체 탐지 수행
results = yolo_model(frame, verbose=False)[0]

for box in results.boxes:
    cls_id = int(box.cls[0])
    if yolo_model.names[cls_id] == 'person':
        x1, y1, x2, y2 = map(int, box.xyxy[0]) # 사람 박스 좌표
```

---

## 🦴 Slide 06. MediaPipe Pose Landmarker와 관절 좌표

사람 신체의 33개 주요 관절 좌표(x, y, z)를 실시간 추출합니다.

- MediaPipe Pose: 머리부터 발끝까지 주요 관절 인덱스 제공 (예: 팔꿈치=13/14, 무릎=25/26)
- 좌표 정규화: 0~1 사이 값으로 제공되므로 영상 가로·세로 크기를 곱해 정수 좌표로 변환

```python
import mediapipe as mp
from mediapipe.tasks.python import vision

# PoseLandmarker 모델 설정 및 생성
base_options = mp.tasks.python.BaseOptions(model_asset_path='pose_landmarker.task')
options = vision.PoseLandmarkerOptions(base_options=base_options)
pose_detector = vision.PoseLandmarker.create_from_options(options)

# 관절 점 위치 추출
detection_result = pose_detector.detect(mp_image)
```

---

## 📐 Slide 07. 삼각함수를 활용한 관절 각도 계산

세 관절 A, B, C가 이루는 꺾임 사이각 $\theta$를 아크탄젠트(arctan2)로 산출합니다.
- 개념: 두 직선 벡터의 기울기 차이를 이용하여 0~180도 사이 각도로 변환

```python
import numpy as np

def calculate_angle(a, b, c):
    a, b, c = np.array(a), np.array(b), np.array(c)
    
    # 두 벡터 사이의 각도(라디안) 구하기
    radians = np.arctan2(c[1]-b[1], c[0]-b[0]) - np.arctan2(a[1]-b[1], a[0]-b[0])
    angle = np.abs(radians * 180.0 / np.pi)
    
    # 180도 이하 사이각으로 보정
    if angle > 180.0:
        angle = 360.0 - angle
    return angle

```

---

## 📦 Slide 08. YOLO Bounding Box 시각화

YOLOv8으로 탐지된 사람 위치 테두리에 직사각형 박스와 라벨 오버레이를 적용합니다.

```python
import cv2

# 감지 영역 바운딩 박스 그리기 (주황색)
cv2.rectangle(frame, (x1, y1), (x2, y2), (255, 120, 0), 2)

# 라벨 텍스트 표기
cv2.putText(
    frame, "Person", (x1, y1 - 10),
    cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 120, 0), 2
)
```

---

## 🦴 Slide 09. MediaPipe 스켈레톤(Skeleton) 시각화

추출된 관절 지점을 선으로 연결하여 신체 뼈대 라인을 직관적으로 표현합니다.

```python
import cv2

# 어깨 -> 팔꿈치 -> 손목 관절 연결선 그리기
cv2.line(frame, tuple(l_shoulder), tuple(l_elbow), (255, 0, 0), 3)
cv2.line(frame, tuple(l_elbow), tuple(l_wrist), (255, 0, 0), 3)

# 주요 관절점 표기
cv2.circle(frame, tuple(l_elbow), 6, (0, 0, 255), -1)

```

---

## 📊 Slide 10. 실시간 분석 정보 자막 오버레이

영상 상단에 학번과 실시간 관절 각도 데이터를 자막으로 상시 노출합니다.

```python
import cv2

# 상단 자막용 반투명 검은색 배경 박스
cv2.rectangle(frame, (10, 10), (480, 85), (0, 0, 0), -1)

# 학번 및 각도 데이터 출력
info_text = f"Student: {student_id} {student_name}"
angle_text = f"Elbow Angle: {int(elbow_angle)}deg"

cv2.putText(frame, info_text, (20, 35), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (255, 255, 255), 2)
cv2.putText(frame, angle_text, (20, 65), cv2.FONT_HERSHEY_SIMPLEX, 0.6, (0, 255, 255), 2)
```

---

## 💡 Slide 11. 종목별 자세 자동 진단 알고리즘

측정된 각도 평균값을 기반으로 if-else 조건문을 이용해 자세 상태를 자동 진단합니다.

```python
feedback_msgs = []

# 팔꿈치 각도 조건판단
if 80 <= avg_elbow <= 120:
    feedback_msgs.append("✅ 팔꿈치 각도: 양호합니다.")
else:
    feedback_msgs.append("⚠️ 팔꿈치 각도: 주의가 필요합니다.")

# 상체 기울기 조건판단
if avg_trunk < 15:
    feedback_msgs.append("✅ 상체 기울임: 양호합니다.")
else:
    feedback_msgs.append("🚨 상체 기울임: 경고! 상체가 앞으로 많이 쏠렸습니다.")
```

---

## 🔊 Slide 12. gTTS를 활용한 한국어 음성 피드백

진단된 텍스트 결과를 바탕으로 자동 음성 피드백(MP3) 파일을 생성합니다.
- gTTS (Google Text-to-Speech): 파이썬 텍스트를 자연스러운 한국어 음성 파일로 변환

```python
from gtts import gTTS

feedback_text = "팔꿈치 각도는 양호하나 상체 기울임에 주의하세요."
tts = gTTS(text=feedback_text, lang='ko')
tts.save("feedback_result.mp3")
```

---

## ⚡ Slide 13. MoviePy 기반 음성 1.5배속 변환

피드백 전달 시간을 단축하기 위해 생성된 음성 배속을 1.5배속으로 조절합니다.

- MoviePy: 오디오 및 비디오 편집 작업을 처리하는 파이썬 라이브러리

```python
from moviepy.editor import AudioFileClip

# 음성 파일 로드 및 1.5배속 적용
audio_clip = AudioFileClip("feedback_result.mp3")
fast_audio = audio_clip.speedx(factor=1.5)
fast_audio.write_audiofile("fast_feedback.mp3")
```

---

## 🎬 Slide 14. 웹 재생을 위한 H.264 MP4 인코딩

OpenCV 기본 출력 영상은 웹에서 재생되지 않을 수 있으므로 H.264 코덱으로 변환합니다.

- 코덱 문제 해결: Gradio 등 웹 브라우저는 libx264 인코딩 MP4 파일만 호환 가능

```python
from moviepy.editor import VideoFileClip

# 웹 호환 인코더로 변환
clip = VideoFileClip("temp_raw.mp4")
clip.write_videofile("web_output.mp4", codec="libx264", audio=False)
```

---

## 🔗 Slide 15. Gradio UI와 AI 분석 엔진 함수 연결

웹 UI의 버튼 클릭 이벤트와 작성한 AI 비디오 분석 파이프라인 함수를 하나로 묶어줍니다.

```python
# 버튼 클릭 시 analyze_sports_posture 함수 실행
btn_analyze.click(
    fn=analyze_sports_posture,
    inputs=[selected_sport, student_id, student_name, video_input],
    outputs=[output_video, output_feedback, output_audio]
)
```

---

## 🖥️ Slide 16. Gradio 대시보드 화면 통합 완성

입력 컴포넌트와 분석 결과(비디오, 텍스트, 음성) 출력창을 하나의 깔끔한 웹 레이아웃으로 완성합니다.

```plaintext
┌──────────────────────────────────────────────────────────┐
│ [입력] 종목 선택 / 학번 / 이름 / 비디오 업로드 / [분석 버튼] 
├──────────────────────────────────────────────────────────┤
│ [출력] 1. 분석 결과 비디오 (YOLO+MediaPipe+자막 오버레이)   
│        2. 종합 평가 진단 텍스트                            
│        3. 1.5배속 음성 피드백 오디오 플레이어               
└──────────────────────────────────────────────────────────┘
```

---

## 🚀 Slide 17. 앱 실행 및 Share 외부 공유 링크 생성

로컬 환경뿐만 아니라 외부 단말기에서도 접근 가능한 라이브 공유 링크를 활성화합니다.

- 공유 옵션: share=True 설정 시 72시간 동안 유효한 임시 웹 URL이 발급됨

```python
if __name__ == "__main__":
    demo.launch(share=True)
```
| 🌐 생성된 https://xxx.gradio.live 링크를 스마트폰이나 태블릿에 입력하면 모바일에서도 AI 대시보드 테스트가 가능합니다.

---

## ❓ Slide 18. 자주 발생하는 오류 및 트러블슈팅

프로젝트 제작 중 학생들이 빈번히 겪는 에러 원인 및 해결 방법입니다.

발생 에러,원인 및 해결 조치 방법

FileNotFoundError,"모델 파일(yolov8n.pt, pose_landmarker.task) 경로 확인"

비디오 재생 불가,MoviePy를 이용해 코덱을 libx264로 변환했는지 확인

IncompleteInputError,"파이썬 멀티라인 문자열("""""") 따옴표 닫힘 여부 점검"
Gradio 오디오 미출력,gTTS 파일 저장 경로와 Gradio Output 컴포넌트 연결 확인