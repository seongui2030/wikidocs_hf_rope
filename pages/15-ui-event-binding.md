# 15. Gradio UI와 AI 분석 엔진 함수 연결

## 1. 학습 목표
* 이벤트 기반 프로그래밍(Event Driven Programming)의 개념을 이해합니다.
* Gradio UI의 분석 실행 버튼 클릭 시 백엔드 파이프라인 함수가 작동하도록 매핑합니다.

## 2. 이벤트 바인딩 매커니즘
* **`btn.click()`**: 사용자 버튼 클릭 이벤트를 감지하여 지정 함수(`fn`)를 작동시킵니다.
* **`inputs` & `outputs`**: 화면의 입력 요소 값들을 순서대로 함수 파라미터에 넘기고, 함수 반환 결과를 출력 UI 요소에 바인딩합니다.

## 3. 소스코드 예시

```python
# Gradio UI 요소와 백엔드 함수(analyze_sports_posture) 바인딩
btn_analyze.click(
    fn=analyze_sports_posture,
    inputs=[
        selected_sport, 
        student_id_input, 
        student_name_input, 
        video_input, 
        start_time_input, 
        end_time_input
    ],
    outputs=[
        output_video, 
        output_feedback, 
        output_audio
    ]
)
```

## 4. 확인 문제
- Gradio의 btn.click()에서 실행할 파이썬 AI 함수를 등록할 때 지정하는 매개변수 키워드는 무엇인가요?

    - **정답**: fn