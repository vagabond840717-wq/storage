# HANA STAY — 비품 창고 (storage)

창고에 남은 비품 개수를 관리하고, 호실별 메모·특이사항을 기록하는 모바일 웹앱.

비품은 방마다 두는 것이 아니라 **창고 한 곳에 보관**하고 청소하면서 채워 넣는다.
그래서 방별 재고는 추적하지 않고 **창고에 몇 개 남았는가**만 본다.

## 화면

1. **비품 창고 (홈)** — 품목별 남은 개수. 기준 개수 아래로 떨어지면 주황, 다 떨어지면 빨강.
   맨 위 상자에 지금 사야 할 품목 이름이 모여 나온다 (따로 저장하지 않고 계산해서 보여줌).
   스크롤을 내리면 호실 목록.
2. **호실별 메모** — 방을 눌러 들어가서 메모를 적고, 눌러서 고치고 지운다.

## 저장 위치

| 무엇 | 어디 |
|---|---|
| 비품·메모 | Cloudflare KV 키 `storage_data` — 주차앱 Worker 의 범용 `/kv` 사용 |
| 폴백 | `localStorage['hana_storage']` |
| 호실 목록 | `ical-proxy` 의 `GET /rooms` — **읽기 전용** |
| 호실 폴백 | `localStorage['hana_storage_rooms']` |

이 앱은 **자기 Worker 가 없다.** 주차앱 Worker(`parking.vagabond1984.workers.dev`)의
`/kv?key=` 가 범용 key-value 창고라 키만 다르게 쓴다. `worker.js` 는 수정하지 않았다.

## 배포

GitHub Pages (이 저장소 루트 `index.html`). push 하면 바로 반영된다.
PWA 가 아니라 서비스워커 캐시 문제가 없다 — 새로고침이면 새 버전이 뜬다.

전체 규칙: `vagabond840717-wq/hana-stay` 저장소의 CLAUDE.md 참고.
