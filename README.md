# 분할매수 플래너 (Stockfolio)

개인용 분할매수 계획 관리 + 시장 모니터링 PWA. 백엔드 없이 브라우저 단독 동작.

> ⚠ **본 도구는 투자 자문이 아닙니다.** 정보 제공 목적으로만 사용하세요. 자세한 내용은 하단 면책 조항 참조.

---

## ✨ 5개 탭 구성

| 탭 | 기능 |
|---|---|
| **분할계획** | 본주/레버 트랙 구분, 균등/피라미드/완만/수동 분배, 매수 시뮬레이션, 진행 추적, 피보나치 18레벨 지지선 + 레버 추정가 컬럼 |
| **차트** | 등록 종목 칩 클릭 → TradingView 임베드 차트, 지표 토글 (MA/BB/일목/RSI/MACD/거래량), "TradingView에서 열기 ↗" 버튼, 펀더멘털·외부분석 링크 |
| **지수** | 공포탐욕지수 5년 트렌드 (CNN + Alternative.me), 주요 지수 10개 (VIX/S&P/NASDAQ/러셀/SOX/채권/BTC/ETH + 나스닥 위켄드 IG), Finviz 시장 맵, ARK 일일 거래 |
| **관심종목** | 미보유 종목 워치리스트 (최대 30개), 칩 클릭 시 펀더멘털·외부분석 즉시 표시, TradingView 차트 직링크, Stock Screener |
| **메모** | 자유 텍스트 메모 (자동 저장) |

### 모든 데이터는 브라우저에 저장
- localStorage 사용 (서버/백엔드 없음)
- 분할계획·관심종목·메모·피보나치·마지막 탭까지 자동 저장
- 페이지 새로고침해도 유지
- "내보내기 / 가져오기" 버튼으로 JSON 백업
- **앱 닫고 다시 열어도 보던 탭 그대로** 복원

### 피보나치 지지선 — 18레벨
- 정통 피보나치 되돌림 (전고점/전저점/현재가 입력)
- 18단계 비율: -0.618 / 100 / 88.6 / 78.6 / **70.7 minor** / 61.8 / **55.0 minor** / 50 / **44.0 minor** / 38.2 / **31.8 minor** / 23.6 / 14.6 / 0 + 확장 (1.128 / 1.272 / 1.414 / 1.618)
- minor 비율은 들여쓰기 + MINOR 라벨로 시각 구분
- 현재가 바로 아래 지지선은 ⭐ 표시 + 초록 강조
- 4종 정보 한눈에: 레벨 / 본주 가격 / 전고점 대비 % / 현재가 대비 %
- **레버 ETF 추정가 컬럼**: 레버 배수 + 레버 현재가 입력 시 각 피보 레벨의 레버 추정가 표시

### PWA — 앱처럼 설치 가능
- 단일 파일 `index.html` (모든 자원 인라인)
- manifest data URI 인라인 → 별도 파일 없음
- 모바일에서 "홈 화면에 추가" 가능

---

## 🚀 배포 방법

### GitHub Pages에 무료 호스팅

1. 이 리포에 4개 파일을 push:
   - `index.html`
   - `README.md`
   - `LICENSE`
   - `.gitignore` (점 포함!)
2. 리포 페이지 → **Settings → Pages**
3. Source: **`main` branch**, Folder: **`/ (root)`** → Save
4. 1~2분 대기 후 `https://goldenboykr.github.io/{repo-name}/` 에서 접속

### 모바일 PWA 설치
1. 위 URL을 모바일 브라우저(Safari/Chrome)에서 열기
2. 메뉴 → **"홈 화면에 추가"** 또는 **"앱 설치"**
3. 아이콘 등록 → 일반 앱처럼 실행

### ⚠ 업데이트 후 새 버전이 안 보일 때
- **모바일 앱**: 앱 종료 후 재실행
- **PC 브라우저**: `Ctrl + Shift + R` (강제 새로고침)
- **GitHub Pages CDN 지연**: push 후 5~10분 기다리기
- **URL에 `?v=2` 쿼리 추가**: 캐시 우회 (다음번엔 `?v=3`)

별도 service worker가 없어서 브라우저 기본 캐시 정책만 따릅니다.

---

## 🛠 기술 스택

- **순수 HTML + CSS + Vanilla JavaScript** — 빌드 시스템·번들러·의존성 없음
- **단일 파일** — `index.html` 하나로 모든 게 끝남 (~180KB)
- **외부 자원**: Google Fonts, TradingView 임베드 위젯, 공포탐욕 데이터 API

---

## 📊 데이터 출처

본 도구는 외부 데이터를 **사용자 브라우저에서 직접 호출**하거나 **링크로 안내**할 뿐, 데이터를 우리 서버에 저장/재배포하지 않습니다.

### 임베드 (위젯)
- **TradingView** — Advanced Chart, Single Quote 위젯 (Widget Terms of Use에 따라 attribution 유지)

### API 호출
- **CNN Fear & Greed Index** (via GitHub: whit3rabbit/fear-greed-data + feargreedchart.com)
- **Alternative.me Crypto F&G API** (무료, 무인증)

### 외부 링크 (사용자 브라우저가 이동)
- **TradingView** Financials / Stock Screener
- **FRED (St. Louis Fed)** — 국채 yield (DGS10, DGS30)
- **OANDA** (TradingView 경유) — 지수 CFD 24h 가격
- **Zacks** — Zacks Rank
- **TipRanks** — 애널리스트 컨센서스
- **Finviz** — 시장 맵, 종목 정보
- **Barchart** — MAX PAIN (옵션)
- **Fintel** — 공매도 정보 (한국어 페이지)
- **IG** — 나스닥 위켄드
- **ARK Invest (Cathie's ARK)** — 일일 ETF 거래
- **CBOE** — VIX (TradingView 경유)

---

## ⚖ 면책 조항 (Disclaimer)

### 투자 자문 아님
- 본 도구는 **순전히 정보 제공 목적**의 개인용 도구입니다.
- 표시되는 가격·지수·재무 데이터·차트·신호는 **매수·매도 의사결정의 근거가 될 수 없습니다.**
- 데이터의 정확성·실시간성·완전성에 대해 **어떠한 보증도 하지 않습니다.**
- 본 도구 사용으로 인한 직접·간접·결과적 손실에 대해 작성자는 **어떠한 책임도 지지 않습니다.**
- 투자 결정은 본인 판단·책임 하에, 필요시 면허받은 금융 전문가의 자문을 받아 진행하세요.

### 저작권 및 라이선스
- 외부 데이터·차트·로고·브랜드의 저작권은 **각 출처에 귀속**됩니다.
- 본 도구는 사용자가 그 출처로 접근하도록 돕는 **인터페이스(클라이언트)** 일 뿐, 데이터를 재배포·캐싱·저장하지 않습니다.
- TradingView 위젯은 [Widget Terms of Use](https://www.tradingview.com/policies/)에 따라 attribution을 유지하며 사용합니다.
- 본 코드 자체는 MIT 라이선스 — 자유롭게 fork/수정 가능.

### 사용 범위 제한
- **개인용·비상업 사용만 권장.** 본인 또는 비공개 소규모 그룹 사용은 안전합니다.
- **광고·유료 판매·재라이선스 등 상업 이용 금지** — 일부 외부 서비스(TradingView 임베드 위젯, alternative.me 무료 API 등)의 무료 사용 정책 위배 소지.
- Fork 이후 누군가가 상업적으로 사용했을 때 발생하는 문제에 대해 원작자는 책임지지 않습니다.

### 데이터 정확성 한계 (솔직히 짚어드리는 부분)
- **CNN F&G**: GitHub 데이터셋은 매주 금요일 갱신. 평일에는 fallback API로 보충. 1시간 단위 실시간은 CNN 본 사이트 링크로.
- **TradingView 위젯**: 일부 심볼 (SPX, NDX, SOX)은 위젯에서 라이선스 차단 → OANDA CFD 또는 SOXX ETF로 대체. 정규 지수와 약간 차이.
- **OANDA CFD**: 24h 거래되는 CFD라 본장 마감 후에도 가격 움직임. 정규 거래소 종가와 약간 다를 수 있음.
- **FRED 국채 yield**: 일별 EOD. 실시간 X.
- **IG 위켄드 가격**: 브로커 합성 CFD. 토~일 신호는 sentiment 참고용.
- **피보나치 레버 추정가**: 일일 변동률 × 배수 단순 계산. 실제 레버 ETF는 변동성 손실로 더 떨어지는 경향. **참고용 추정치**.

---

## 📝 빌드 정보

- 단일 파일: `index.html` (~180KB)
- 외부 의존성: TradingView 위젯 스크립트, Google Fonts (둘 다 CDN)
- 데이터 자원: 모두 인라인 (manifest, 아이콘, CSS, JS 전부 한 파일)
