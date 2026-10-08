## 1. 학습 목표
* 개발 및 배포 과정에서 자주 발생하는 대표적인 예외(Error) 유형과 해결 방법을 파악합니다.
* 예외 처리 구문(`try-except`)을 적용하여 시스템의 안정성을 극대화합니다.

## 2. 주요 오류 발생 원인 및 조치 방법 표

| 발생 에러 명칭 | 원인 | 해결 조치 방법 |
| :--- | :--- | :--- |
| **`FileNotFoundError`** | AI 모델 파일(`yolov8n.pt`, `pose_landmarker.task`) 경로 미존재 | 파일 다운로드 URL 및 현재 작업 경로를 재확인 |
| **비디오 재생 불가 (검은 화면)** | OpenCV 기본 출력 코덱(`mp4v`)과의 웹 호환성 부족 | MoviePy를 이용하여 코덱을 `libx264`로 인코딩 |
| **`IncompleteInputError`** | 멀티라인 따옴표(`"""`) 또는 괄호 미닫힘 | 파이썬 문법 괄호 및 따옴표 닫힘 여부를 점검 |
| **Gradio 오디오 미출력** | gTTS 파일 경로와 Gradio Output 연결 누락 | 생성된 오디오 파일 경로가 `gr.Audio` 반환값과 일치하는지 확인 |

## 3. 예외 처리 적용 예시

```python
from moviepy.editor import AudioFileClip

def safe_audio_speedup(temp_mp3, output_path, speed=1.5):
    try:
        audio_clip = AudioFileClip(temp_mp3)
        fast_audio = audio_clip.speedx(factor=speed)
        fast_audio.write_audiofile(output_path, verbose=False, logger=None)
        audio_clip.close()
    except Exception as e:
        print(f"오디오 변환 중 오류 발생: {e}. 원본 음성 파일로 대체합니다.")
        output_path = temp_mp3
    return output_path
```

## 4. 확인 문제
- 예상치 못한 시스템 에러가 발생하더라도 프로그램 전체가 중단되지 않도록 보호할 때 사용하는 파이썬의 구문 구조는 무엇인가요?

    - **정답**: try-except 구문
