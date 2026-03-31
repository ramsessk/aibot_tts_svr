# TTS Server supertonic

## 개요

TTS Server Supertonic은 텍스트를 음성으로 변환하는 서비스입니다. Supertonic TTS 엔진을 기반으로 한 HTTP 서버로, RESTful API를 통해 텍스트 입력을 받아 음성 파일을 생성하고 반환합니다.

Supertonic TTS 엔진은 : https://github.com/supertone-inc/supertonic#
이 레포에는 submodule 로 포함되어 있다.

여기 코드도 모두 AI가 만든 것임.

## 디렉토리 구조

```text
~/mybin/
├── tts-server-supertonic      # 메인 서버 실행 스크립트
└── supertonic/
    ├── py
    |   └──infereence.py 여기에 있는 것으로 가정함. 
    |
    ├─-assets/ 모델데이타가 있어야 함(https://huggingface.co/Supertone/supertonic-2)
    └── [추가 모듈 및 설정 파일들]
```

Git submodules을 유지하기 위해 inference.py 위치를 supertonic/으로 옮겼다.
huggingface에 있는 model을 clone 하기전에에 반드시
```shell
apt install git-lfs
git lfs install
```

설치 방법

필수 요구사항

• Python 3.x
• Supertonic TTS 엔진
• 필수 Python 패키지 (요구사항은 서버 스크립트를 참조)

설치 단계

1. 저장소를 클론하거나 파일을 복사합니다:
```shell

git clone [repository-url]
# 또는
cp -r ~/mybin/tts-server-supertonic ~/mybin/supertonic/ [목적지]
```

2. 필요한 의존성을 설치합니다:
```shell

# requirements.txt가 있는 경우
pip install -r requirements.txt
```

사용 방법

서버 실행

```
~/mybin/tts-server-supertonic
```

서버는 기본적으로 로컬 호스트의 특정 포트에서 실행됩니다. 포트 번호는 서버 스크립트에서 확인할 수 있습니다.

API 엔드포인트

텍스트를 음성으로 변환

```
POST /tts
Content-Type: application/json

{
  "text": "변환할 텍스트",
  "language": "ko",  // 선택 사항, 기본값: 한국어
  "voice": "default" // 선택 사항, 음성 종류
}
```
응답:

• 성공 시: 음성 파일 (WAV 형식 또는 설정에 따른 형식)
• 오류 시: 적절한 HTTP 상태 코드와 오류 메시지

예제 사용법

```shell

curl -X POST http://localhost:8000/tts \
  -H "Content-Type: application/json" \
  -d '{"text": "안녕하세요, 제주도의 오늘 날씨는 맑음입니다."}' \
  -o output.wav
```

테스트 방법

기본 기능 테스트

1. 서버를 시작합니다:
```
~/mybin/tts-server-supertonic &
```

2. 간단한 텍스트로 테스트합니다:
```shell

curl -X POST http://localhost:8000/tts \
  -H "Content-Type: application/json" \
  -d '{"text": "테스트입니다."}' \
  -o test_output.wav
```

3. 생성된 파일을 재생하여 정상 동작 여부를 확인합니다:
# Linux의 경우
```
aplay test_output.wav
```

# 또는 다른 오디오 재생 도구 사용
오류 상황 테스트

• 빈 스트 전송
• 지원하지 않는 언어 코드 전송
• 매우 긴 텍스트 전송

설정 방법

서버 스크립트(tts-server-supertonic)의 상단 부분을 수정하여 다음과 같은 설정을 변경할 수 있습니다:

• 호스트 및 포트 번호
• 기본 언어 및 음성 설정
• 오디오 출력 형식 및 품질
• 타임아웃 및 버퍼 크기

문제 해결

일반적인 문제점

1. 서버가 시작되지 않음
  • 의존성이 제대로 설치되지 않았는지 확인
  • 포트가 다른 프로세스에 의해 사용 중인지 확인
2. 음성 생성 실패
  • 텍스트 인코딩 문제 확인 (UTF-8 권장)
  • Supertonic 엔진이 올바르게 설치되어 있는지 확인
3. 오디오 재생 문제
  • 생성된 파일의 형식 확인
  • 시스템의 오디오 재생 도구 호환성 확인

라이선스

[라이선스 정보가 있는 경우 여기에 추가]

기여 방법

1. 이 저장소를 포크합니다
2. 기능 브랜치를 생성합니다 (git checkout -b feature/amazing-feature)
3. 변경사항을 커밋합니다 (git commit -m 'Add some amazing feature')
4. 브랜치에 푸시합니다 (git push origin feature/amazing-feature)
5. 풀 리퀘스트를 엽니다

문의 사항

문제가 있거나 질문이 있는 경우, 저장소의 이슈 기능을 이용해 주세요.
")]

