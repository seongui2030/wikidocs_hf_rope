## 1. 학습 목표
* MoviePy의 `AudioFileClip` 및 `speedx` 모듈을 이용하여 음성 재생 속도를 1.5배속으로 빠른 피드백 파일로 제작합니다.

## 2. 배속 조절 원리
* **현장 활용성**: 피드백 청취 시간을 단축하여 수업 진행 속도를 개선합니다.
* **`speedx(factor=1.5)`**: 음높이 변형을 최소화하면서 오디오 재생 속도를 1.5배로 가속합니다.

## 3. 소스코드 예시

```python
from moviepy.editor import AudioFileClip

def speedup_audio(input_mp3, output_mp3, speed=1.5):
    audio_clip = AudioFileClip(input_mp3)
    # 1.5배속 처리
    fast_audio = audio_clip.speedx(factor=speed)
    fast_audio.write_audiofile(output_mp3, verbose=False, logger=None)
    audio_clip.close()
    return output_mp3
```

## 4. 확인 문제
- MoviePy에서 오디오 속도를 1.5배 빠르게 가속할 때 호출하는 메서드 명칭은 무엇인가요?

    - **정답**: speedx(factor=1.5)
