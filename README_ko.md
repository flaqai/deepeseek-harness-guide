# DeepSeek Harness 가이드

[English](README.md) | [简体中文](README_zh.md) | [繁體中文](README_tw.md) | [日本語](README_ja.md) | [한국어](README_ko.md) | [Deutsch](README_de.md) | [Español](README_es.md) | [Français](README_fr.md) | [Italiano](README_it.md) | [Português](README_pt.md) | [Русский](README_ru.md) | [العربية](README_ar.md) | [Bahasa Indonesia](README_id.md) | [ไทย](README_th.md) | [Tiếng Việt](README_vi.md)

> [DeepSeek Harness](https://github.com/deepseek-ai/deepseek-harness)를 이해하고 확장하며 플러그인을 개발하기 위한 커뮤니티 다국어 가이드입니다.

DeepSeek Harness(`dsh`)는 DeepSeek AI가 공개한 오픈 소스 에이전트 하네스입니다. 핵심 철학은 **“모든 것은 플러그인”**입니다. 모델 어댑터, 도구, 에이전트 루프, 세션 저장소, 권한, 샌드박스, 텔레메트리와 UI를 설정으로 조합하거나 교체할 수 있습니다.

> [!IMPORTANT]
> 이 저장소는 독립적인 커뮤니티 가이드이며 DeepSeek 공식 저장소가 아닙니다. DeepSeek Harness는 현재 개발자 프리뷰 단계이므로 호환성을 깨는 변경이 생길 수 있습니다. 세부 구현은 [공식 저장소](https://github.com/deepseek-ai/deepseek-harness)와 [공식 문서](https://deepseek-harness.github.io/deepseek-harness/)에서 확인하세요.

## Harness가 하는 일

모델만으로는 저장소 읽기, 명령 실행, 도구 호출, 승인 요청, 세션 보존, 오류 복구를 할 수 없습니다. Harness는 이러한 실행 환경을 제공하고 사용자, 모델, 도구, 애플리케이션 상태 사이의 루프를 조정합니다.

DeepSeek Harness는 [Cordis](https://github.com/cordiverse/cordis)를 기반으로 합니다. 플러그인은 공유 Context에 Service, 타입이 지정된 Event, 되돌릴 수 있는 Effect를 제공합니다. 따라서 애플리케이션 전체를 포크하지 않고 모델, 도구, 샌드박스, 저장소 또는 서브에이전트를 바꿀 수 있습니다.

## 핵심 개념

| 개념 | 설명 |
| --- | --- |
| Plugin | Cordis Context에 마운트되는 TypeScript 모듈, 객체 또는 서비스 클래스. |
| Bundle | `dsh.bundle`을 통해 구성 레이어를 배포하는 npm 패키지. |
| Profile | Bundle과 로컬 플러그인 의존성을 모은 실행 가능한 구성. |
| Patch | 구성 행을 삽입하거나 교체하는 YAML 오버레이. |
| Service / Event | 교체 가능한 기능과 에이전트 흐름을 관찰·가로채는 확장 지점. |

에이전트 루프 자체도 교체 가능합니다. 기본 루프는 프롬프트와 도구 스키마를 조립하고, 모델 응답을 스트리밍하며, 도구를 실행하고, 세션 이벤트를 기록합니다.

## 빠른 시작

```bash
npx @deepseek-ai/dsh web
```

Web UI는 기본적으로 `http://127.0.0.1:3080`에서 열립니다. **Settings → Models**에서 모델 자격 증명을 추가한 뒤 워크스페이스를 선택하세요.

## 이 가이드의 범위

- Cordis, 플러그인 수명 주기, 의존성 주입, 되돌릴 수 있는 Effect.
- 도구, 모델, 샌드박스, 저장소, 서브에이전트, Web UI 플러그인.
- Bundle, Profile, `cordis.patch.yml`, 테스트, 배포, 보안.
- 계획 중인 Agent Skills: `dsh-repository-explorer`, `dsh-plugin-scaffold`, `dsh-tool-builder`, `dsh-plugin-review`.

여기서 **Skill**은 AI 코딩 에이전트를 위한 재사용 가능한 작업 절차이며, DeepSeek Harness 런타임 **Plugin**과는 다릅니다. 위 Skills는 아직 배포되지 않았습니다.

## 공식 자료

- [소스 코드](https://github.com/deepseek-ai/deepseek-harness)
- [아키텍처](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)
- [첫 플러그인 튜토리얼](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/index.md)
- [플러그인 패키징](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/user/develop/basic/publish.md)

## 라이선스

[MIT](LICENSE)
