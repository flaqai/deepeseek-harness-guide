# DeepSeek Harness 사용 안내서

[English](USAGE.md) · [简体中文 전체판](USAGE_zh.md)

이 문서는 한국어 빠른 안내서입니다. DeepSeek Harness는 개발자 프리뷰 단계이므로 실제로 사용하는 커밋과 공식 문서를 기준으로 확인하세요.

## 빠른 시작

```bash
npx @deepseek-ai/dsh web
dsh --profile web --dump-config
```

`http://127.0.0.1:3080`을 열어 모델 서비스를 설정하고 일회용 작업 공간에서 안전한 작업부터 실행합니다. 두 번째 명령은 Profile, Bundle, Patch가 조합한 최종 플러그인 트리를 보여 줍니다.

## 모듈 분류

- Runtime 구성: Context, Service, Fiber, Effect, Event, Loader.
- Agent 실행: 모델 어댑터, Prompt, Agent Loop, 도구, 정책, 승인, 샌드박스.
- 상태: Session Event, 메모리, 압축, 재생.
- UI: Host, Remote API, Web Client, 데스크톱, TUI, 모바일.
- 생태계: 워크플로, 브라우저, 비전, 외부 연동, 테마, 개발 도구.

## 안전한 설치

```bash
dsh plugin --profile demo add <package-or-git-spec>
dsh --profile demo --dump-config
```

Git 커밋을 고정하고 라이선스, 설치 스크립트, 네트워크, 파일, 하위 프로세스, 자격 증명, 데이터 보존 정책을 검토하세요. 일회용 Profile에서 시작, 거부, 시간 초과, 언로드, 재시작, 롤백을 시험합니다.

## 실용 Skills

[`skills/`](skills/)에는 저장소 탐색, 플러그인 스캐폴드, 도구 개발, 플러그인 검토용 Agent Skill 네 개가 있습니다. Skill은 개발 작업을 안내하며 DSH Runtime 플러그인이 아닙니다.

전체 절차와 문제 해결, 릴리스 체크리스트는 [English handbook](USAGE.md)을 참고하세요.
