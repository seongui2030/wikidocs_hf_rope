# 03. Gradio 기초 및 UI 레이아웃 설계

## 1. 학습 목표
* Gradio의 `Blocks` 레이아웃 구조를 이해하고 입출력 컴포넌트를 배치합니다.
* AI 체육 대시보드의 화면 인터페이스를 완성합니다.

## 2. Gradio UI 컴포넌트 구성
* **`gr.Radio`**: 운동 종목(줄넘기, 축구, 달리기, 멀리뛰기)을 단일 선택합니다.
* **`gr.Textbox`**: 학생의 학번 및 이름을 입력받고, 최종 진단 결과를 출력합니다.
* **`gr.Video`**: 원본 비디오 업로드 및 스켈레톤 시각화 결과 영상을 표시합니다.
* **`gr.Button`**: 클릭 이벤트를 발생시켜 AI 분석 프로세스를 실행합니다.

## 3. 소스코드 예시

```python
import gradio as gr

with gr.Blocks(title="AI 동반 코치(Co-Coach) 코코") as demo:
    gr.Markdown("# 🏃‍♂️ AI 스켈레톤 렌즈와 실시간 보이스 코칭")
    
    with gr.Row():
        selected_sport = gr.Radio(["줄넘기", "축구", "달리기", "멀리뛰기"], value="줄넘기", label="운동 종목 선택")
        student_id_input = gr.Textbox(label="학번", placeholder="예: 10101")
        student_name_input = gr.Textbox(label="이름", placeholder="예: 홍길동")
        
    with gr.Row():
        video_input = gr.Video(label="운동 영상 업로드")
        with gr.Column():
            start_time_input = gr.Number(value=0, label="분석 시작 시간(초)")
            end_time_input = gr.Number(value=10, label="분석 종료 시간(초)")
            
    btn_analyze = gr.Button("🔍 AI 자세 분석 실행", variant="primary")
```

## 4. 확인 문제

- 여러 옵션 중 단 하나의 운동 종목만 선택할 수 있도록 제공하는 Gradio UI 컴포넌트는 무엇인가요?

    - **정답**: gr.Radio