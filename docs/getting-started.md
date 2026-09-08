# 시작하기 (5분)

풀문 경제 API를 내 봇/대시보드에서 읽기까지의 전체 과정.

## 0. 전제 이해 (30초)

경제의 단일 진실 원천은 마크 경제서버의 PostgreSQL이다. 쓰는 주체는 코인브릿지
봇과 마크 플러그인 둘뿐이고, **외부 개발자는 읽기만 한다**. 지급·차감·송금
엔드포인트는 존재하지 않는다 — 호출하면 405가 돌아온다.

## 1. 키 발급 (운영자에게 요청)

봇 이름을 정해서 풀문 네트워크 디스코드 운영진에게 알려주면 bearer 키를 준다.

- 스코프는 없다. 키 하나 = 읽기 전부.
- 키가 새면 읽기가 샌다 — 그 이상은 아무것도 못 한다(설계상).
- 운영자는 키를 `ECONOMY_API_CLIENTS` 레지스트리에 등록한다.

## 2. 클라이언트 복사 (30초)

[fullmoon-sdk](https://github.com/Fullmoon-OSS/fullmoon-sdk)의
`economyClient.js`를 프로젝트에 복사한다. 의존성 0, Node 18+, ESM. 이 파일 하나가
SDK 전부다.

```js
import { EconomyClient } from './economyClient.js';
const eco = new EconomyClient({ key: process.env.ECONOMY_API_KEY });
```

기본 baseUrl은 `https://api.fullmoon.ink/economy`다. 어느 호스트에서 돌리든 이
주소 하나면 된다.

## 3. 첫 조회

```bash
# 인증 없이 접속 확인
curl -s https://api.fullmoon.ink/economy/v1/health
# → { "ok": true, "service": "economy-api", "readOnly": true }

# 키로 잔액 조회
curl -s -H "Authorization: Bearer $ECONOMY_API_KEY" \
     https://api.fullmoon.ink/economy/v1/overview
```

```js
const acc = await eco.getAccount(userId);          // { balance, linked, mcUsername } | null
const top = await eco.getLeaderboard(10);          // [{ rank, discordId, balance }]
const wal = await eco.getWalletByMc('SteveMan');   // MC 사용자명 기반 (런처용)
```

계정이 아직 없으면 `null`이 돌아온다 — 예외가 아니다. 유저가 활동하면 자동으로
생긴다.

## 4. 예제 붙이기

- [`examples/balance-and-ranking.js`](https://github.com/Fullmoon-OSS/fullmoon-sdk/blob/main/examples/balance-and-ranking.js) —
  디스코드 봇 `/잔액` `/랭킹` 커맨드 골격
- [`examples/dashboard-poller.js`](https://github.com/Fullmoon-OSS/fullmoon-sdk/blob/main/examples/dashboard-poller.js) —
  대시보드 집계 5종 폴링

## 5. 알아둘 에러 처리

| 상황 | 클라이언트 동작 |
|---|---|
| `404` 계정 없음 | `null` / 빈 배열 — 정상 업무 결과 |
| `401` 키 문제 | **예외** — 운영자에게 문의 |
| `429` 레이트리밋 | **예외** — 백오프 후 재시도 (앱 기본 60req/10s, nginx 10req/s burst 40) |
| `405` 쓰기 시도 | API가 막는다 — 설계상 쓰기는 없다 |
| `getConfigValue()` 실패 | **예외 없음** — fallback을 돌려준다(설정은 부가 정보). 운영 문제를 보려면 `getConfigMap()` |

## 6. 통합 등록 (선택)

만든 것을 [통합 카탈로그](https://github.com/Fullmoon-OSS/fullmoon-sdk/blob/main/INTEGRATIONS.md)에
PR 한 장으로 등록할 수 있다.

## 다음 읽을거리

- [정책](./policies.md) — 3가지 철칙과 그 근거
- [API 레퍼런스](https://github.com/Fullmoon-OSS/fullmoon-economy-api#엔드포인트)
