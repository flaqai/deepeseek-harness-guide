# DeepSeek Harness 기술 가이드

[English](GUIDE.md) | [简体中文](GUIDE_zh.md) | [繁體中文](GUIDE_tw.md) | [日本語](GUIDE_ja.md) | [한국어](GUIDE_ko.md) | [Deutsch](GUIDE_de.md) | [Español](GUIDE_es.md) | [Français](GUIDE_fr.md) | [Italiano](GUIDE_it.md) | [Português](GUIDE_pt.md) | [Русский](GUIDE_ru.md) | [العربية](GUIDE_ar.md) | [Bahasa Indonesia](GUIDE_id.md) | [ไทย](GUIDE_th.md) | [Tiếng Việt](GUIDE_vi.md)

이 문서는 [중국어 아키텍처 분석 글](https://mp.weixin.qq.com/s/Kf87hcNdSmY4ODWI4UZ8cg)을 참고하고 [공식 소스](https://github.com/deepseek-ai/deepseek-harness)와 [공식 아키텍처 문서](https://github.com/deepseek-ai/deepseek-harness/blob/master/docs/architecture.md)로 교차 검증했습니다.

> DeepSeek Harness는 Developer Preview 단계입니다. 글은 고정된 Commit을 분석하므로 패키지 이름, Preset, 내부 API가 변경될 수 있습니다.

## 핵심 모델

DSH는 두 시스템을 함께 유지합니다.

- **런타임 플러그인 그래프**: 현재 기능, 기능이 보이는 Scope, Fiber가 소유한 수명 주기를 표현합니다.
- **Append-only Session Event Stream**: Agent가 수행한 영속적 사실을 기록하고 모델 기록, UI, Resume, Fork로 투영합니다.

Agent Loop는 그래프에서 모델, Prompt, 도구, 정책을 가져오고 결과를 이벤트 스트림에 씁니다.

## 구성 파이프라인

`Profile → Bundles → Profile Patch → Home Patch → --patch`

뒤쪽 레이어는 ID로 구성 행 전체를 교체하거나 새 행을 삽입합니다. 첫 진단은 다음 명령으로 시작합니다.

```bash
dsh --profile web --dump-config
```

## Cordis 런타임

| 요소 | 역할 |
| --- | --- |
| Context | Service 가시성, 상속, 격리 Realm을 결정합니다. |
| Service | Definition, Provider, Consumer를 안정된 계약으로 연결합니다. |
| Fiber | 구성, 의존성, 상태, Disposer를 가진 실제 Plugin 인스턴스입니다. |
| Effect | 리소스 획득과 정리를 Fiber에 귀속합니다. |
| Event | 알림, 결정, Waterfall Middleware로 흐름을 확장합니다. |
| Loader | 구성 Entry를 갱신·언로드 가능한 플러그인 트리로 만듭니다. |

`inject`는 Context 의존성 계약이지 OS 권한이 아닙니다. `ctx.effect()`는 구조화된 정리를 제공하지만 외부 트랜잭션을 자동으로 되돌리지는 않습니다.

## Agent와 Session

Turn은 0개 이상의 Step을 포함하고, Step은 보통 모델 요청과 관련 도구 실행을 포함합니다. Session Event는 경계, 메시지, Chunk, Tool Call, Result를 기록합니다. `deriveMessages()`는 로그에서 모델이 볼 기록을 투영합니다.

완전한 기록과 매 요청마다 전체를 다시 보내는 것은 다릅니다. Compaction은 원본 Event를 보존하면서 오래된 Surface를 가릴 수 있습니다. 재생 가능한 로그도 외부 부작용의 안전한 재실행을 보장하지 않습니다.

## 캐시와 보안

동적 그래프 자체는 Prefix Cache를 무효화하지 않습니다. 도구, Prompt, 모델, 기록 등 모델이 보는 Surface가 바뀔 때 무효화됩니다. 순서를 안정적으로 유지하고 변동 데이터를 분리하세요.

서드파티 Plugin은 고권한 동일 프로세스 코드입니다. 설치 스크립트, Node API, 네트워크, 자격 증명, 파일 범위, 하위 프로세스, 텔레메트리, 정리를 검토하고 Commit을 고정하세요.

## 개발 체크리스트

- Loop 변경 전에 기존 Service / Event Seam을 찾습니다.
- `inject`로 의존성을 선언하고 Schema로 구성을 검증합니다.
- Listener, Timer, Service, Handle에 소유권과 Cleanup을 둡니다.
- 상태가 Host, Agent Scope, Session Log 중 어디에 속하는지 결정합니다.
- Provider 교체, 갱신, Unload, Resume, Fork, Compaction을 테스트합니다.
- Bundle로 패키징하고 `--dump-config`로 최종 트리를 검증합니다.

전체 내용은 [영문판](GUIDE.md) 또는 [중문판](GUIDE_zh.md)을 참고하세요.

