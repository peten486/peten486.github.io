# HA Schema Changelog

HA 앱이 Instagram 게시물을 파싱할 때 사용하는 원격 스키마의 변경 이력입니다.
Instagram 측 HTML/JSON 포맷이 바뀔 때마다 앱 업데이트 없이 이 파일을 고쳐서 대응합니다.

## 📌 현재 버전

| 항목 | 값 |
|---|---|
| **Live schema** | [`schema.json`](/ha/schema.json) |
| **Schema version** | `1` |
| **Schema variant** | `full` |
| **Min app version** | `1.0.0` |
| **Last updated** | 2026-04-21 |
| **Status** | 🟢 정상 동작 |

## 📋 Changelog

### v1 — 2026-04-21 (초기 버전)

- 최초 배포. 기존 앱 하드코딩 값 그대로 이관.
- 주요 필드:
  - `embed_html.username_marker`: `"username":"`
  - `embed_html.carousel_root_marker`: `edge_sidecar_to_children`
  - `embed_html.video_url_key`: `video_url`
  - `private_api.endpoint_template`: `/api/v1/media/{media_id}/info/`

<!-- 다음 변경 시 아래 양식으로 추가
### v2 — 2026-XX-XX
- **변경 이유**: Instagram이 `video_url` 키를 `playback_url`로 리네이밍
- **감지 방법**: 사용자 제보 / 자동 모니터링 실패 알림
- **변경 필드**:
  - `embed_html.video_url_key`: `video_url` → `playback_url`
- **영향 범위**: v1 스키마 사용자(앱 1.0.0 ~ 1.2.x)는 자동 복구됨
- **롤백**: [`schema-v1.json`](/ha/schema-v1.json)
-->

## 🔀 호환성 매트릭스

| App 버전 | 지원 Schema | 비고 |
|---|---|---|
| `1.0.0` ~ 현재 | v1 | 기본 번들 |

## 📁 파일 구조

- [`/ha/schema.json`](/ha/schema.json) — 앱이 실시간 로드 (항상 최신)
- [`/ha/schema-v1.json`](/ha/schema-v1.json) — v1 스냅샷 (호환성 폴백용)

## 🚨 긴급 롤백 절차

새 스키마 배포 후 파싱이 깨졌다면:

1. `/ha/schema.json` 을 직전 커밋으로 `git revert`
2. GitHub Pages 배포 완료 대기 (~1분)
3. 앱은 다음 fetch 사이클(최대 5분)에 자동 복구

## 🔍 Instagram 포맷 변경 감지 방법

- **수동**: 에러 보고 증가 관찰 → 실제 URL 테스트 → 스키마 업데이트
- **자동(예정)**: CI에서 샘플 URL 정기 파싱 → 실패 시 Slack/이메일 알림

## 💬 피드백

- Bug: [Issues](https://github.com/peten486/HA/issues)
- 스키마 수정 PR 환영

---

## 중요 원칙

이 페이지는 앱이 직접 파싱하지 않습니다. 앱은 `schema.json` 만 읽고, 이 블로그 페이지는 사람(본인/기여자)이 변경 이력을 추적하는 용도입니다. 두 파일을 분리하면:

- 앱은 JSON만 파싱 → 단순/안정
- 사람은 Markdown으로 편하게 기록
- Apple 심사 시 "블로그에서 코드 다운로드" 오해 없음 (앱이 읽는 건 데이터 JSON 하나뿐)

## 권장 운영 습관

1. `schema.json` 수정 시 같은 PR에 changelog도 추가 — 잊기 쉬움
2. `version` 번호는 breaking change에만 올리기 — 필드 추가는 같은 버전 유지
3. `v1`, `v2` 스냅샷 파일 유지 — 롤백/구버전 앱 호환

---

*이 페이지는 사람용 기록이며, 앱이 직접 파싱하지 않습니다. 앱은 `/ha/schema.json` 만 읽습니다.*
