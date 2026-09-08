# Fullmoon 개발자 허브 (fullmoon-docs)

풀문(Fullmoon) 네트워크를 외부 개발자가 확장하기 위한 문서 허브예요. 하나의
PostgreSQL 원장을 여러 봇·대시보드·클라이언트가 함께 읽는 구조를 오픈소스로
공개하고 있어요.

## 레포 지도

| 레포 | 내용 |
|---|---|
| [fullmoon-sdk](https://github.com/Fullmoon-OSS/fullmoon-sdk) | 공식 경제 API 클라이언트 (의존성 0) + 통합 카탈로그 데이터예요 |
| [fullmoon-economy-api](https://github.com/Fullmoon-OSS/fullmoon-economy-api) | 읽기 전용 경제 HTTP API 서버 소스예요 |
| [fullmoon-market](https://github.com/Fullmoon-OSS/fullmoon-market) | 통합 카탈로그 사이트 — [market.fullmoon.ink](https://market.fullmoon.ink) |
| [fullmoon-client](https://github.com/RedHatOnTop/fullmoon-client) | 풀문 전용 마인크래프트 클라이언트예요 (GPL-3.0) |
| **fullmoon-docs** | 이 문서 — 시작 가이드, 정책이에요 |

## 시작하기

[docs/getting-started.md](./docs/getting-started.md) — 5분이면 첫 조회까지 갈 수
있어요: 키 발급 → 클라이언트 복사 → 잔액 표시.

## 정책

[docs/policies.md](./docs/policies.md) — 왜 읽기 전용인가요, 3가지 철칙, 키
발급·유출 대응 절차가 담겨 있어요.

## 통합 등록 (플러그인 마켓)

SDK·API 위에 만든 봇·대시보드·도구는
[market.fullmoon.ink](https://market.fullmoon.ink) 카탈로그에 등록할 수 있어요.
등록 절차는
[fullmoon-sdk의 INTEGRATIONS.md](https://github.com/Fullmoon-OSS/fullmoon-sdk/blob/main/INTEGRATIONS.md)에
있어요 — PR 한 장이면 돼요.

## 공개 엔드포인트

```
https://api.fullmoon.ink/economy/v1/health
```

## 라이선스

[MIT](./LICENSE)예요.
