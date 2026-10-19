# CLAUDE.md — 크리에이터 소재 대시보드 (oy-creator-dash_1)

이 저장소에서 작업하는 Claude가 지킬 규칙이다. 세션 시작 시 자동으로 읽힌다.

## 이 페이지가 쓰이는 곳

- `public/index.html` 이 **USE 광고 대시보드**(https://use-ad-dashboard.pages.dev)의 **🎬 UGC 성과 페이지 `/ugc/`** 로 그대로 들어간다.
- USE 대시보드 자동 동기화(매일 **09:00 · 17:00 KST**)가 이 저장소 `main`의 `public/index.html` 최신본을 받아 함께 배포한다.
  - push → 다음 동기화 때 라이브 반영. 급하면 USE 대시보드 담당에게 수동 동기화 요청.
  - 받기 실패·파일 이상(10KB 미만·`<html` 없음)이면 직전 사본이 유지된다 — 깨진 파일이 라이브로 나가지 않는다.
- USE 쪽은 받은 뒤 디자인 토큰을 덧씌운다(변수 `--surface`·`--ink`·`--ink-2` 개명 + `tokens.css`·`ugc_theme.css` 연결). 아래 디자인 규칙을 지키면 그 처리 없이도 같은 모습이 된다.

## 파일 규칙

| 항목 | 규칙 |
|:---|:---|
| 수정 범위 | `public/index.html` 한 파일. 경로·파일명을 바꾸지 말 것 |
| 단일 파일 | 외부 JS·CSS를 **상대경로 파일로 추가하지 말 것** — USE는 이 파일 하나만 가져간다. 필요하면 인라인 |
| 루트 `index.html` | 배포에 쓰이지 않는 옛 사본 — `public/`과 맞추거나 삭제 |
| 데이터 | HTML 안에 내장 가능. 자동 갱신 커밋은 그대로 유지 |

## 디자인 — USE 대시보드 기준

1. `<head>`에 USE 디자인 토큰과 폰트를 **절대 주소**로 불러온다(workers.dev에서 열어도 같은 모습):
   ```html
   <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/variable/pretendardvariable-dynamic-subset.min.css">
   <link rel="stylesheet" href="https://use-ad-dashboard.pages.dev/css/tokens.css">
   ```
2. **색·폰트·간격을 하드코딩하지 말 것** — 토큰(`var(--…)`)만 쓴다.
   - 배경·면: `--bg` `--surface` `--surface-sub` · 테두리: `--border` `--border-soft`
   - 글자: `--ink` `--ink-2` `--ink-3` · 강조: `--primary` `--primary-bg` · 브랜드: `--brand`
   - 신호: `--sig-green` `--sig-yellow` `--sig-red` (각 `-bg`) · 없음: `--sig-na-bg`
   - 매체 색: `--ch-meta` `--ch-tiktok` `--ch-google` `--ch-criteo` `--ch-reddit` `--ch-snapchat`
   - 폰트: `--font` `--font-display` · 간격 `--sp-1`~`--sp-5` · 모서리 `--radius` `--radius-sm`
   - 전체 목록: https://use-ad-dashboard.pages.dev/css/tokens.css
3. **다크 모드**: USE 대시보드가 이 페이지의 `<html>`에 `data-theme="dark"`를 붙인다. 토큰만 쓰면 따로 작업할 것이 없다. 자체 다크 팔레트(`:root[data-theme="dark"]{…}`)를 만들지 말 것 — USE 다크 값과 어긋난다.
4. 같은 이름 변수를 새로 정의하지 말 것(`--surface`·`--ink` 등) — USE 토큰을 덮어쓴다.

## 표기 규칙 (USE 대시보드 공통)

- **금액은 전체 자릿수**: `₩11,521,614` — `1,152만`·`$11.5K` 같은 축약 금지
- **율에는 분자/분모 병기**: `CVR 5.1% (417/8,217)`
- ROAS는 자연수 %(`417%`) · CTR·CVR은 소수 1자리 · 비율 변화는 `%p`
- 표: 헤더 가운데 · 숫자 오른쪽 · 글자 왼쪽 정렬
- 인사이트는 **Good / Bad / Next** 3단, Next에는 할 일(→ 액션)
- 좋다·나쁘다 평가에는 **대조군**(같은 기간 평균 등)을 함께 둔다
- 결측은 0으로 쓰지 말고 `–` 또는 "미입력"
- 외국어 용어는 괄호로 한국어 병기

## 확인

- push 후 다음 동기화가 끝나면 https://use-ad-dashboard.pages.dev 사이드바 **└ 🎬 UGC 성과** 또는 https://use-ad-dashboard.pages.dev/ugc/ 에서 확인.
