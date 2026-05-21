# 분할매수 플래너 (Stockfolio)

개인용 분할매수 계획 관리 + 시장 모니터링 도구. **백엔드 없이 브라우저 단독으로 동작하는 PWA (Progressive Web App)**.

> ⚠ **본 도구는 투자 자문이 아닙니다.** 정보 제공 목적으로만 사용하세요. 자세한 내용은 [면책 조항](#-면책-조항-disclaimer) 참조.

---

## ✨ 주요 기능

### 5개 탭 구성

| 탭 | 기능 |
|---|---|
| **분할계획** | 종목별 분할매수 계획 (균등/가중치/수동 모드), 매수 시뮬레이션, 진행 추적, 피보나치 14레벨 지지선 |
| **차트** | 분할계획 등록 종목 → 칩 클릭 시 TradingView Advanced Chart, 지표 토글, "본 사이트에서 열기 ↗" 버튼으로 본인 계정 차트 연계 |
| **지수** | 공포탐욕지수 5년 트렌드 + 주요 지수 10개 (VIX, S&P, NASDAQ, 러셀, SOX, 채권 10/30년, BTC, ETH, 나스닥 위켄드 IG) + Finviz 시장 맵 + ARK 일일 거래 |
| **관심종목** | 미보유 종목 워치리스트 (최대 30개), 펀더멘털·외부분석 즉시 표시, TradingView 차트 직링크, Stock Screener |
| **메모** | 자유 텍스트 메모 + 레버리지 가격 계산기 |

### 모든 데이터는 사용자 브라우저에 저장
- localStorage 사용 (서버 없음, 백엔드 없음)
- 분할계획, 관심종목, 메모, 피보나치 입력값 모두 자동 저장
- 페이지 새로고침해도 유지
- "내보내기 / 가져오기" 버튼으로 JSON 백업 가능

---

## 🚀 배포 방법

### GitHub Pages에 무료 호스팅

1. 이 리포를 GitHub에 push (`index.html` 하나만 있으면 됨)
2. 리포 페이지 → **Settings → Pages**
3. Source: **`main` branch**, Folder: **`/ (root)`**
4. Save 후 1~2분 대기
5. `https://{username}.github.io/{repo-name}/` 에서 접속

### 모바일에서 PWA 설치
1. 위 URL을 모바일 브라우저(Safari, Chrome 등)에서 열기
2. 브라우저 메뉴 → **"홈 화면에 추가"** 또는 **"앱 설치"**
3. 앱 아이콘으로 등록됨 → 일반 앱처럼 실행 가능

### ⚠ 캐시 업데이트 시 주의
배포한 PWA를 사용자가 한 번 설치하면, 브라우저가 캐시를 잡기 때문에 새 버전이 자동 반영되지 않을 수 있습니다. 새 `index.html`을 push했는데 변경이 안 보이면:
- **모바일**: 앱을 닫았다가 다시 열기, 또는 브라우저 캐시 삭제
- **PC**: `Ctrl + Shift + R` (강제 새로고침) 또는 개발자 도구 → Network 탭 → "Disable cache" 체크

본 도구는 별도 service worker를 두지 않으므로 브라우저 기본 캐시 정책만 따릅니다. 그래서 보통 강제 새로고침 한 번이면 새 버전이 보입니다.

---

## 🛠 기술 스택

- **순수 HTML + CSS + Vanilla JavaScript** — 빌드 시스템 없음, 의존성 없음
- **단일 파일** — `index.html` 하나로 모든 게 끝남 (manifest, 아이콘도 data URI로 인라인)
- **외부 자원**:
  - Google Fonts (Noto Sans KR, JetBrains Mono)
  - TradingView 임베드 위젯 스크립트 (`s3.tradingview.com/external-embedding/`)
  - 공포탐욕 데이터: GitHub raw CSV + feargreedchart.com + alternative.me API

---

## 📊 데이터 출처

본 도구는 외부 사이트의 데이터를 **사용자 브라우저에서 직접 호출**하거나 **링크로 안내**합니다. 데이터를 우리 서버에 저장하거나 재배포하지 않습니다.

### 임베드 (위젯)
- **[TradingView](https://www.tradingview.com)** — Advanced Chart, Single Quote 위젯. 각 위젯에 TradingView attribution이 자동 표시되며 라이선스에 따라 유지합니다.

### API 호출
- **[CNN Fear & Greed Index](https://www.cnn.com/markets/fear-and-greed)** — 미국 주식 공포탐욕지수 (CNN Business)
- **[Alternative.me Crypto F&G API](https://alternative.me/crypto/fear-and-greed-index/)** — 암호화폐 공포탐욕지수
- **[GitHub: whit3rabbit/fear-greed-data](https://github.com/whit3rabbit/fear-greed-data)** — CNN F&G 일별 히스토리 CSV (주 1회 금요일 갱신)
- **[feargreedchart.com](https://feargreedchart.com)** — CNN F&G 실시간 보충 데이터

### 외부 링크 (사용자 브라우저가 이동)
- **[FRED (St. Louis Fed)](https://fred.stlouisfed.org)** — 국채 yield (DGS10, DGS30) via TradingView 위젯
- **[OANDA](https://www.oanda.com)** (TradingView 경유) — 지수 CFD 24시간 가격
- **[Zacks](https://www.zacks.com)** — Zacks Rank 등급
- **[TipRanks](https://www.tipranks.com)** — 애널리스트 컨센서스
- **[Finviz](https://finviz.com)** — 시장 맵, 종목 정보
- **[Barchart](https://www.barchart.com)** — MAX PAIN (옵션)
- **[Fintel](https://fintel.io)** — 공매도 정보 (한국어 페이지)
- **[IG](https://www.ig.com)** — 나스닥 위켄드 가격
- **[ARK Invest (Cathie's ARK)](https://cathiesark.com)** — 일일 ETF 거래
- **[CBOE](https://www.cboe.com)** — VIX (TradingView 경유)

---

## ⚖ 면책 조항 (Disclaimer)

### 투자 자문 아님
- 본 도구는 **순전히 정보 제공 목적**의 개인용 도구입니다.
- 표시되는 모든 가격·지수·재무 데이터·차트·신호는 **매수·매도 의사결정의 근거가 될 수 없습니다.**
- 데이터의 정확성, 실시간성, 완전성에 대해 **어떠한 보증도 하지 않습니다.**
- 본 도구 사용으로 인한 직접·간접·결과적 손실에 대해 작성자는 **어떠한 책임도 지지 않습니다.**
- 투자 결정은 반드시 본인의 판단과 책임 하에, 필요시 면허받은 금융 전문가의 자문을 받아 진행하세요.

### 저작권 및 라이선스
- 외부 데이터·차트·로고·브랜드의 저작권은 **각 출처에 귀속**됩니다.
- 본 도구는 사용자가 그 출처로 접근하도록 돕는 **인터페이스(클라이언트)** 일 뿐, 데이터를 재배포·캐싱·저장하지 않습니다.
- TradingView 위젯은 TradingView의 [Widget Terms of Use](https://www.tradingview.com/policies/)에 따라 attribution을 유지하며 사용합니다.
- 본 코드 자체(HTML/CSS/JS)는 개인 학습·관리 목적의 오픈 소스로 자유롭게 fork/수정 가능합니다.

### 사용 범위 제한
- **개인용·비상업 사용만 권장.** 본인 또는 비공개 소규모 그룹의 사용은 안전합니다.
- **광고 게재·유료 판매·재라이선스 등 상업 이용은 금지**됩니다. 일부 외부 서비스(TradingView 임베드 위젯, alternative.me 무료 API 등)의 무료 사용 정책에 위배될 수 있습니다.
- 본 코드를 fork해서 다른 누군가가 상업적으로 사용했을 때 발생하는 문제에 대해 원작자는 책임지지 않습니다.

### 데이터 정확성 한계 (솔직히 짚어드리는 부분)
- **CNN F&G**: GitHub 데이터셋은 매주 금요일에만 갱신. 평일에는 fallback API로 보충하지만 일별 EOD 수준입니다. 1시간 단위 실시간은 CNN 본 사이트 링크로 확인하세요.
- **TradingView 위젯**: 일부 심볼(예: 정규 지수 SPX, NDX)은 위젯에서 라이선스 차단되어 OANDA CFD로 대체. CFD는 실제 지수와 약간 차이가 있습니다.
- **OANDA CFD**: 24시간 거래되는 CFD라 본장이 닫혀도 가격이 움직이지만, 정규 거래소의 진짜 종가와는 약간 다를 수 있습니다.
- **FRED 국채 yield**: 일별 EOD 데이터. 실시간 X.
- **IG 위켄드 가격**: 브로커가 만든 합성 CFD. 실제 시장이 닫혀있는 토~일 가격이라 정확한 신호가 아니라 sentiment 참고용입니다.

---

## 🤝 기여

본 도구는 개인 학습 목적으로 만들어졌습니다. 버그 제보·개선 제안은 GitHub Issues로 환영합니다.

본 코드는 자유롭게 fork·수정 가능하며, 상업 이용은 위 [사용 범위 제한](#사용-범위-제한)을 참조하세요.

---

## 📝 빌드 정보

- 빌드 날짜: 2026-05-21
- 단일 파일: `index.html` (~180KB)
- 모든 자원 인라인 — 외부 의존성 없이 단독 실행 가능
