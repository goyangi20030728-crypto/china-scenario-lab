# 🏛️ China Scenario Lab

> **Multi-layered scenario analysis system for China's bond market, combining political, global, and market signals with probabilistic forecasting and self-evaluation.**

중국 채권시장을 정치·글로벌·시장 3개 레이어로 분석하고, 단일 예측이 아닌 **확률적 시나리오**를 생성하며, 모든 예측을 사후 검증하여 시스템의 한계를 투명하게 드러내는 메타인지 기반 분석 플랫폼.

---

## 🎯 Why This Project?

중국 채권시장은 전통적인 금융 모델로 예측하기 어려운 시장입니다:

- **정책 개입의 일상성**: PBOC와 당의 방향성이 시장을 좌우
- **데이터 신뢰성 이슈**: 공식 통계와 실제 경제활동의 괴리
- **글로벌 연계성**: 미중 경쟁, Fed 정책, 달러 흐름이 복합 작용
- **블랙스완 빈발**: 헝다, 비구이위안 등 예측 불가 이벤트

본 프로젝트는 이러한 **구조적 불확실성을 인정**하고, "하나의 정답"이 아닌 **"여러 가능성과 그 확률"**을 제시하는 것을 목표로 합니다.

---

## 🧠 Core Philosophy

### 1️⃣ Three-Layer Analysis
```
┌─────────────────────────────────────┐
│  ① Political Layer                   │
│    PBOC 보고서, 당 주요 회의,         │
│    정책 신호 텍스트 분석              │
├─────────────────────────────────────┤
│  ② Global Layer                      │
│    미 10Y 금리, Fed 정책, DXY,       │
│    미중 지정학 이벤트                 │
├─────────────────────────────────────┤
│  ③ Market Layer                      │
│    중국 10Y/2Y 국채, 국개채,          │
│    DR007, MLF, LPR, 거시지표         │
└─────────────────────────────────────┘
             ↓
      🤖 Scenario Engine
             ↓
┌─────────────────────────────────────┐
│  📊 3 Probabilistic Scenarios        │
│   예: 강세 40% / 횡보 35% / 약세 25% │
│   + 각 시나리오별 트리거와 인과사슬    │
└─────────────────────────────────────┘
             ↓
      ⏳ Time Passes...
             ↓
┌─────────────────────────────────────┐
│  🎯 Self-Evaluation                  │
│   Brier Score · Calibration Plot    │
│   Hit Rate · "왜 틀렸나" 사후분석     │
└─────────────────────────────────────┘
```

### 2️⃣ Probabilistic, Not Deterministic
단일 예측이 아닌 **확률 가중 시나리오** 제시. Ray Dalio의 인과사슬(causal chain) 접근 차용.

### 3️⃣ Honest Self-Evaluation
Philip Tetlock의 Superforecasting 방법론 기반. 모든 예측은 DB에 기록되고, 결과가 나오면 자동 채점됩니다.

---

## 🏗️ Architecture

```
┌──────────────┐    ┌──────────────┐    ┌──────────────┐
│   AkShare    │    │  FRED API    │    │  News/Policy │
│ (중국 시장)   │    │ (미국/글로벌) │    │   (텍스트)    │
└──────┬───────┘    └──────┬───────┘    └──────┬───────┘
       │                   │                    │
       └───────────────────┼────────────────────┘
                           ↓
                  ┌────────────────┐
                  │  ETL Pipeline  │
                  │   (Python)     │
                  └────────┬───────┘
                           ↓
                  ┌────────────────┐
                  │   SQLite DB    │
                  │  - market_data │
                  │  - events      │
                  │  - predictions │
                  │  - outcomes    │
                  └────────┬───────┘
                           ↓
            ┌──────────────┴──────────────┐
            ↓                             ↓
   ┌────────────────┐           ┌─────────────────┐
   │  XGBoost Model │           │   LLM Scenario  │
   │  (정량 확률)    │           │    Generator    │
   │                │           │ (Gemini/Claude) │
   └────────┬───────┘           └────────┬────────┘
            │                             │
            └──────────────┬──────────────┘
                           ↓
                  ┌────────────────┐
                  │  Streamlit UI  │
                  │   Dashboard    │
                  └────────────────┘
```

---

## 🛠️ Tech Stack

모든 구성요소는 **100% 무료**로 운영됩니다.

| 영역 | 도구 |
|------|------|
| 중국 시장 데이터 | [AkShare](https://akshare.akfamily.xyz/) |
| 글로벌 데이터 | [FRED API](https://fred.stlouisfed.org/docs/api/fred/), yfinance |
| 뉴스/이벤트 | GDELT, NewsAPI (free tier) |
| 저장소 | SQLite |
| 분석/모델링 | pandas, scikit-learn, XGBoost |
| 시나리오 생성 | Gemini API (free tier) |
| 번역 (중→영) | Gemini API |
| 대시보드 UI | Streamlit |
| 자동화 | GitHub Actions |
| 호스팅 | Streamlit Community Cloud / Hugging Face Spaces |

---

## 📅 Roadmap

- [ ] **v0** — 중국/미국 10Y 금리 비교 차트 (AkShare + FRED)
- [ ] **v1** — 3-레이어 데이터 수집 파이프라인 + 기본 대시보드
- [ ] **v2** — XGBoost 기반 확률 예측 모델
- [ ] **v3** — LLM 기반 3-시나리오 자동 생성 리포트
- [ ] **v4** — Brier Score / Calibration Plot 자가평가 시스템
- [ ] **v5** — "왜 틀렸나" 자동 사후분석 + 누적 학습 노트

---

## ⚠️ Disclaimer

본 프로젝트는 중국 금융 데이터의 구조적 한계(정책 개입, 통계 가공 가능성, 정보 비대칭)를 인지하고 제작되었습니다. 예측 모델은 **"시장의 숨겨진 진실을 찾는 도구"가 아니라, "공식 데이터와 정책 신호 사이의 괴리를 가시화하는 도구"**로 설계되었습니다. 투자 조언이 아니며, 순수 학습 및 연구 목적입니다.

---

## 📚 Inspirations

- Philip Tetlock, *Superforecasting: The Art and Science of Prediction*
- Ray Dalio, *Principles for Dealing with the Changing World Order*
- Baker, Bloom, Davis — Economic Policy Uncertainty Index
- Matteo Iacoviello — Geopolitical Risk Index

---

*Built with curiosity, skepticism, and vibe coding. 🐉*

