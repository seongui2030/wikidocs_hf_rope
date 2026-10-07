# 14. 웹 재생을 위한 H.264 MP4 인코딩

## 1. 학습 목표
* OpenCV에서 기본 출력된 영상 코덱(`mp4v`)이 웹 브라우저에서 제대로 재생되지 않는 원인을 알아봅니다.
* MoviePy를 이용하여 웹 표준 비디오 코덱인 H.264(`libx264`)로 인코딩을 수행합니다.

## 2. 비디오 코덱 인코딩 원리
* **웹 호환성 문제**: Chrome, Safari 등 모바일/웹 환경은 `mp4v` 재생을 지원하지 않아 검은 화면으로 노출되는 문제가 존재합니다.
* **H.264 코덱 변환**: `VideoFileClip`으로 읽어온 비디오를 `codec="libx264"` 옵션을 적용해 웹 인코딩합니다.

## 3. 소스코드 예시

```python
from moviepy.editor import VideoFileClip

def convert_to_h264_web_video(input_path, output_path):
    clip = VideoFileClip(input_path)
    # H.264 웹 표준 코덱으로 저장
    clip.write_videofile(output_path, codec="libx264", audio=False)
    clip.close()
    return output_path
```

## 4. 확인 문제
- Gradio 및 웹 브라우저에서 검은 화면 이슈 없이 완벽히 호환 재생되는 영상 인코딩 코덱 이름은 무엇인가요?

    - **정답**: libx264 (H.264)