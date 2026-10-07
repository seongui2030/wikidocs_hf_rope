# 12. gTTS를 활용한 한국어 음성 피드백

## 1. 학습 목표
* Google Text-to-Speech (`gTTS`) 라이브러리를 사용해 자세 진단 결과 문자열을 한국어 오디오 파일(.mp3)로 생성합니다.

## 2. 음성 합성 원리
* **`gTTS(text, lang='ko')`**: 파이썬 문자열 데이터를 한국어 구사 음성 객체로 변환합니다.
* **`save()`**: 전달된 임시 경로에 오디오 파일을 저장합니다.

## 3. 소스코드 예시

```python
from gtts import gTTS

def generate_tts_audio(text_message, output_filepath="temp_tts.mp3"):
    # 텍스트를 한국어 음성으로 변환
    tts = gTTS(text=text_message, lang='ko')
    tts.save(output_filepath)
    return output_filepath
```

## 4. 확인 문제
- gTTS 모듈 생성 시 한국어 음성을 지정하기 위해 설정하는 언어 매개변수 값은 무엇인가요?

    - **정답**: lang='ko'