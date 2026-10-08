## 1. 학습 목표
* 입력 영역(종목, 학번, 이름, 영상 업로드)과 출력 영역(분석 영상, 진단 텍스트, 음성)을 결합한 통합 대시보드를 구축합니다.

## 2. 대시보드 컴포넌트 구조표

| 컴포넌트 구획 | UI 요소 명칭 | 역할 및 기능 |
| :--- | :--- | :--- |
| **입력 영역** | `gr.Radio` / `gr.Textbox` | 분석 종목 선택 및 학생 정보(학번, 이름) 입력 |
| **입력 영역** | `gr.Video` / `gr.Number` | 원본 운동 영상 업로드 및 분석 시간 구간 지정 |
| **실행 버튼** | `gr.Button` | AI 분석 파이프라인 가동 이벤트 트리거 |
| **출력 영역** | `gr.Video` / `gr.Textbox` / `gr.Audio` | 결과 비디오, 텍스트 진단서, 1.5배속 MP3 음성 오디오 재생 |

## 3. 소스코드 예시

```python
import gradio as gr

with gr.Blocks(title="AI 동반 코치(Co-Coach) 코코") as demo:
    gr.Markdown("# 🏃‍♂️ AI 스켈레톤 렌즈와 실시간 보이스 코칭")
    
    selected_sport = gr.Radio(["줄넘기", "축구", "달리기", "멀리뛰기"], value="줄넘기", label="운동 종목 선택")
    student_id_input = gr.Textbox(label="학번")
    student_name_input = gr.Textbox(label="이름")
    video_input = gr.Video(label="운동 영상 업로드")
    
    btn_analyze = gr.Button("🔍 AI 자세 분석 실행", variant="primary")
    
    output_video = gr.Video(label="분석 결과 영상 (Skeleton Overlay)")
    output_feedback = gr.Textbox(label="피드백 진단 결과")
    output_audio = gr.Audio(label="1.5배속 음성 피드백")
```

## 4. 확인 문제
- 백엔드에서 생성된 1.5배속 MP3 음성 파일을 웹 대시보드 상에서 바로 들어볼 수 있도록 제공하는 Gradio 출력 컴포넌트는 무엇인가요?

    - **정답**: gr.Audio
