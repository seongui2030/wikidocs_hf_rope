# 17. 앱 실행 및 Hugging Face/Colab 배포

## 1. 학습 목표
* 작성된 웹 애플리케이션을 구동하고 `share=True` 옵션을 이용하여 외부 라이브 접속 링크를 생성합니다.
* Hugging Face Spaces 환경으로 배포하는 절차를 이해합니다.

## 2. 앱 구동 및 외부 공유
* **`demo.launch(share=True)`**: 로컬 및 Google Colab 환경에서 72시간 동안 접속 가능한 임시 라이브 URL(`https://xxx.gradio.live`)을 발급합니다.
* **Hugging Face Spaces 배포**: `app.py` 스크립트를 리포지토리에 커밋하여 영구적인 웹 서비스로 전환합니다.

## 3. 소스코드 예시

```python
if __name__ == "__main__":
    # 라이브 외부 공유 옵션 활성화 후 구동
    demo.launch(share=True)
```

## 4. 확인 문제
- Google Colab 또는 로컬 컴퓨터에서 웹 앱을 실행할 때 스마트폰이나 외부 PC에서 접근 가능하도록 라이브 URL을 만드는 옵션은 무엇인가요?

    - **정답**: share=True