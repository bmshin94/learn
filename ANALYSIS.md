# 📚 learn 레포 분석 & 활용 정리

> 이 문서는 `learn` 레포를 처음부터 뜯어보며 정리한 기록이야.
> **무엇인지 → 어떻게 쓰는지 → 어떻게 돈을 버는지 → 어떻게 만드는지** 순서로 되어 있어. ✨

**작성일:** 2026-09-01

---

## 🔗 관련 링크 모음

| 구분 | 주소 |
|---|---|
| 📁 **이 레포 (내 것)** | https://github.com/bmshin94/learn |
| 🌱 **원본 레포** | https://github.com/amosblomqvist/learn |
| 📹 **출처 영상** | https://www.youtube.com/watch?v=kzcI5F4tGiU |
| 🛠️ **pi (필요한 에이전트 툴)** | https://github.com/earendil-works/pi |
| 🔌 **pi-interactive-subagents** | https://github.com/amosblomqvist/pi-interactive-subagents |
| 📖 **pi 퀵스타트 문서** | https://github.com/earendil-works/pi/blob/main/packages/coding-agent/docs/quickstart.md |
| 📦 **pi npm 패키지** | https://www.npmjs.com/package/@earendil-works/pi-coding-agent |

---

# 1부. 이게 뭐야? 🤔

## 한 줄 요약

> **"AI를 과외선생님으로 바꿔주는 설정 파일 모음"**

실행되는 프로그램이 아니라, **AI한테 주는 사용설명서 뭉치**야.
`CLAUDE.md`에 페르소나를 적으면 AI가 그렇게 행동하는 것과 똑같은 원리인데,
내용이 "캐릭터 되기"가 아니라 **"제대로 가르치기"**인 거지.

원본은 [How I Use AI to Learn Things](https://www.youtube.com/watch?v=kzcI5F4tGiU) 영상에서 소개된
개인용 학습 시스템이고, 파일은 총 19개뿐이야.

## 폴더 구조

```
learn/                      ← pi에서는 이 폴더를 `.pi`로 씀
├── skills/
│   ├── teach/SKILL.md       ⭐ 핵심! 교육 철학 + 수업 진행 절차 (200줄)
│   └── visualize/SKILL.md      "말보다 그림이 나을 때만" 다이어그램
├── agents/
│   ├── researcher.md           웹 검색 → 사실 검증 (환각 방지)
│   ├── mermaid-maker.md        관계도·플로우차트 그리기
│   └── svg-maker.md            좌표·기하학 그림 그리기
├── extensions/
│   ├── quiz.ts          (1062줄) 채점되는 객관식 퀴즈 UI
│   ├── ask-user-question.ts (669줄) 팝업으로 질문하기
│   ├── md-log.ts         (443줄) 수업 내용 → 옵시디언 마크다운 자동 기록
│   └── visual-tools/            머메이드/SVG → PNG 렌더링 도구
├── assets/thumbnail.png
├── README.md
└── CLAUDE.md               ← 원본에는 없음 (내가 추가한 페르소나 파일)
```

## 구성 요소 정리

| 폴더 | 정체 | 하는 일 |
|---|---|---|
| `skills/teach/` | 🏆 **핵심 중의 핵심** | 교육 철학 + 수업 3단계 절차 |
| `skills/visualize/` | 시각화 규칙 | 그림이 진짜 필요할 때만 그리게 함 |
| `agents/` | 심부름꾼 AI 3명 | 검색봇 1 + 그림봇 2 |
| `extensions/` | 실제 TypeScript 코드 | 퀴즈 UI, 팝업, 자동 기록, 렌더링 |

---

# 2부. 핵심 철학 🧠

`skills/teach/SKILL.md`에 담긴 두 가지 원칙. **이게 이 레포의 전부**라고 봐도 돼.

## 원칙 1️⃣ — 무조건 참인 것부터 시작해라

> 뇌는 "나중에 뒤집힐지도 모르는 사실"은 확실하게 저장하지 않는다.

나중에 더 근본적인 무언가가 이걸 반박하면 비싼 업데이트를 해야 하니까,
뇌가 **위험을 감지하고 대충 저장**해버려. 그래서 안 외워지는 거야.

그래서 **예외가 하나도 없는 확실한 사실**부터 박아넣고 그 위에 쌓아올려.

- 예시: *"컴퓨터 간의 모든 통신은 패킷 전송으로 이루어진다"*
- 특히 강한 형태 두 가지:
  - **전칭 명제** — "모든 X는 Y다" / "어떤 X도 Y가 아니다"
  - **진짜 정의** — 속성 나열 말고 실제 정의일 때만

> ⚠️ 용어 구분: **무조건적 진리**(단서 없이 받아들일 수 있는 사실)와
> **공리**(아무것도로부터 유도되지 않는 사실)는 다르다. "공리"를 남발하지 말 것.

## 원칙 2️⃣ — "내가 어떻게 이걸 스스로 발견할 수 있었을까?"

> 사실이 '원래 그런 것'처럼 느껴지면 뇌가 거부한다.

그래서 결론을 던지지 않고, **동기가 있는 발견의 여정**으로 풀어:

1. 애초에 우리가 왜 이걸 하고 있지? (핵심 문제 제시)
2. 왜 하필 이 방법을 떠올렸지? (중간 단계도 전부 동기 부여)
3. 그래서 이게 나왔다

3Blue1Brown(Grant Sanderson) 스타일이 레퍼런스야.

### 소크라테스식 vs 강의식

- **소크라테스식** — 문제를 던지고 스스로 답하게 함. 힘들지만 각인 효과가 큼. **기본값**
- **강의식** — 직접 서술. 차가운 추론으로는 도달 불가능한 주제거나, 학습자가 지쳐 있을 때

## 🎯 최종 목표: "클릭(the click)"

> 흩어진 사실 더미가 몇 개의 생성 원리로 **압축(collapse)**되는 순간.
> 정보량은 같은데 움직이는 부품이 훨씬 적어지는 그 느낌.

- 연결된 지식 > 분리된 지식
- 의존성 그래프 > 외딴 노드들
- 이해 > 암기

---

# 3부. 수업 진행 방식 🎬

## 3단계 프로세스: 탐색 → 계획 → 수업

### Phase 1 — 탐색 (절대 건너뛰지 않음)

**1a. 실력의 "경계선" 찾기 → `quiz` 사용**

목표는 실력 점검이 아니라 **지도 그리기**. 아는 것과 모르는 것의 **경계**를 찾는 것.

| 규칙 | 내용 |
|---|---|
| 🚫 다 맞췄다 = 끝? | **아니다.** 문제가 쉬웠던 것. 바닥만 확인했고 천장은 못 찾음 → 더 어렵게 |
| 🔍 이진 탐색 | 맞추면 난이도를 **확** 올리고, 틀리면 좁혀 들어감 |
| 🚫 하나 틀렸다 = 끝? | **아니다.** 실수인지, 좁은 구멍인지, 체계적 오개념인지 주변을 더 파봄 |
| 🕸️ 모든 갈래 | 수업이 딛고 설 모든 선수 지식 갈래를 각각 확인 |

> **경계는 위아래로 감싸야(bracket) 확정된다.** 맞춘 것(바닥) + 틀린 것(천장), 둘 다 필요.

**1b. 학습 목표 파악 → `ask_user_question` 사용**

"LLM을 이해하고 싶어"는 열 가지 뜻이 될 수 있음. 구체화될 때까지 파고들기.
정답이 없는 질문이므로 `quiz`가 아니라 `ask_user_question`.

### Phase 2 — 계획 (여기서 깊게 생각)

1. `researcher` 서브에이전트로 주제 지형 파악 (진짜 제1원리가 뭔지 확인)
2. 무조건적 진리 후보 정리 → 이미 아는 것 확인 → 목표까지의 발견 경로 설계
3. **뿌리 스트레스 테스트** — "이거 진짜 근본인가, 아니면 위장한 정리인가?"
4. 📊 **계획을 채팅으로 제시** — 산문 설명 + 머메이드 의존성 그래프(DAG)
5. 🛑 **승인 대기** — 사용자가 OK 해야 Phase 3 시작

### Phase 3 — 수업 (루프)

**모든 노드마다** (무조건적 진리든 유도된 단계든 **똑같이**) 다음 4단계:

```
1. 동기부여  → 왜 지금 이게 필요한가?
2. 확립      → 사실은 그대로 진술 / 유도는 동기 있는 전개로
3. 연결      → 기존 노드와의 의존 관계를 명시적으로
4. 퀴즈확인  → 진짜 박혔는지 확인. 틀리면 멈추고 수리
```

> 기초를 앞에 몰아넣고 확인을 그만두면 안 됨. **노드마다 매번** 이 루프를 돈다.

## 🚨 정확성은 타협 불가

> 자신 없는 사실·이름·날짜·공식·정의가 나오면 **말하기 전에 멈추고** `researcher`로 확인할 것.
> 확신에 차서 내뱉은 환각 하나가 선생에 대한 신뢰를 통째로 망가뜨린다.
> 확인 결과 내용이 바뀌면 얼버무리지 말고 **명확히 정정**할 것.

## 📝 퀴즈 선택지 작성법 (구성 절차)

사후 검사로는 부족하니까, **애초에 균등하게 만들어지도록** 구성:

1. **모든 선택지는 이유 없는 맨 주장** — 정답에만 "왜냐하면"이 붙으면 즉시 들킴. 이유는 `explanation`에만
2. **정답을 먼저 쓰고, 그걸 변형해서 오답을 만든다** — 같은 뼈대·같은 입자도·같은 어투
3. 각 오답은 **실제로 할 법한 오해**여야 함 (뭘 골랐는지가 진단 정보가 되도록)
4. **비대칭 볼드 금지** — 정답에만 핵심어를 굵게 하면 바로 들킴

> 읽어보고 내용을 몰라도 정답이 보이면 → 패치하지 말고 **처음부터 다시**

## 🖼️ 시각화 규칙 (`skills/visualize/`)

- 그림은 **말로 못 담는 것**(구조·방향·관계·기하)을 보여줄 때만
- 관계도(노드-엣지) → `mermaid-maker` / 좌표·도형 → `svg-maker`
- 요소 5~7개 넘으면 자르기. **"이거 지워도 뜻이 통하나?"**
- 그림봇은 **렌더링한 PNG를 직접 눈으로 보고** 맞는지 확인한 뒤에야 반환 (틀린 그림 방지)

---

# 4부. 설치 및 사용법 🛠️

> ⚠️ **중요: 이건 Claude Code용이 아니라 [`pi`](https://github.com/earendil-works/pi)용이야.**

## 준비물

| 준비물 | 확인 | 이유 |
|---|---|---|
| Node.js 22.19+ | `node -v` | pi가 Node로 동작 |
| tmux | `tmux -V` | 서브에이전트가 tmux 패널에서 실행됨 |
| 옵시디언 | — | 수업 기록을 예쁘게 보기 |
| Chrome | — | 머메이드 다이어그램 렌더링 |
| rsvg-convert | `rsvg-convert -v` | SVG 렌더링 (`brew install librsvg`) |

> macOS 기준. Windows는 tmux가 없어 서브에이전트 사용 불가 (WSL이면 가능).

## 1단계 · pi 설치

```bash
npm install -g --ignore-scripts @earendil-works/pi-coding-agent
pi --version
```

## 2단계 · 로그인

```bash
pi
```
```
/login          # Claude Pro/Max, ChatGPT Plus/Pro, GitHub Copilot 중 선택
```

또는 환경변수:
```bash
export ANTHROPIC_API_KEY=sk-ant-...
pi
```

## 3단계 · 공부방 만들고 `.pi`로 클론

**폴더 이름이 `.pi`여야 한다.** pi가 그 이름으로 자동 인식하기 때문.

```bash
mkdir ~/study && cd ~/study
git clone https://github.com/bmshin94/learn .pi
```

결과 구조:
```
~/study/                ← 여기서 pi 실행 (옵시디언 볼트로도 사용)
└── .pi/
    ├── skills/         ← 자동 인식 ✅
    ├── agents/         ← 자동 인식 ✅
    └── extensions/     ← 자동 인식 ✅
```

> 📌 pi 공식 문서 기준 자동 로드 경로
> - 스킬: `.pi/skills/` (전역: `~/.pi/agent/skills/`)
> - 확장: `.pi/extensions/*.ts`, `.pi/extensions/*/index.ts` (전역: `~/.pi/agent/extensions/`)
> - ⚠️ 프로젝트를 **신뢰(trust)** 해야 확장이 활성화됨

## 4단계 · 서브에이전트 확장 설치

```bash
mkdir -p ~/.pi/agent/extensions
cd ~/.pi/agent/extensions
git clone https://github.com/amosblomqvist/pi-interactive-subagents
cd pi-interactive-subagents
cp config.json.example config.json
npm install
```

## 5단계 · 시각화 도구 설치

```bash
cd ~/study/.pi/extensions/visual-tools
npm install --omit=dev
```

> 🚨 `--omit=dev` 필수! `package.json`의 devDependencies에 원작자 컴퓨터 절대경로
> (`file:/Users/amos/.nvm/...`)가 박혀 있어서 그냥 설치하면 실패한다.

## 6단계 · 실행

```bash
cd ~/study
tmux new -A -s pi 'pi'
```

## 7단계 · 사용

```
/md-log ~/study/오늘_공부.md      # 마크다운 기록 시작 (옵시디언으로 열어두기)
```
```
트랜스포머가 어떻게 동작하는지 이해하고 싶어
```

`teach` 스킬 설명에 *"설명하거나 가르칠 때는 언제나 이걸 써라"*고 되어 있어 자동 발동.
강제로 부르려면 `/skill:teach <주제>`.

### 주요 명령어

| 명령어 | 하는 일 |
|---|---|
| `/md-log <경로>` | 마크다운 기록 시작 |
| `/md-unlog` | 기록 중단 |
| `/skill:teach` | 수업 강제 시작 |
| `/model` | 모델 변경 |
| `/reload` | 설정 수정 후 새로고침 |
| `/new` `/resume` | 새 세션 / 이어하기 |

## 🩹 트러블슈팅

| 증상 | 해결 |
|---|---|
| 퀴즈 팝업이 안 뜸 | 프로젝트 미신뢰 → 재시작 후 trust, 또는 `/reload` |
| 서브에이전트 안 뜸 | tmux 밖에서 실행함 → `tmux new -A -s pi 'pi'` |
| 다이어그램 렌더 실패 | `extensions/visual-tools/tools/_common.ts`의 `CHROME_CANDIDATES`에 크롬 경로 추가 |
| SVG 렌더 실패 | `brew install librsvg` (또는 imagemagick) |
| `npm install` 실패 | `--omit=dev` 누락 |
| 검색봇 에러 | `agents/researcher.md`의 `model: openrouter/z-ai/glm-5.3`을 보유 모델로 변경 |
| `safe_bash` 관련 에러 | `pi-interactive-subagents` 전용 도구. 다른 구현이면 해당 줄 제거 |

## 🅱️ 대안: Claude Code로 옮기기

핵심인 "가르치는 방법"은 도구와 무관한 그냥 글이라, 그것만 옮겨도 90%는 챙길 수 있음.

```bash
mkdir -p .claude/skills/teach
cp .pi/skills/teach/SKILL.md .claude/skills/teach/SKILL.md
```

| 원본 | 대체 |
|---|---|
| `quiz` 도구 | `AskUserQuestion` (정답·해설은 답변 후 제시) |
| `ask_user_question` | `AskUserQuestion` |
| `researcher` 서브에이전트 | 웹 검색 직접 수행 |
| 옵시디언 `![[...]]` 임베드 | 아티팩트 또는 머메이드 직접 렌더 |

---

# 5부. 수익화 아이디어 💰

## ⚠️ 먼저: 라이선스 확인

```
❌ LICENSE 파일 없음 → 기본값 "All rights reserved"
```

| | 가능? |
|---|---|
| 개인적으로 사용 | ✅ |
| 코드 그대로 상업 서비스에 투입 | ❌ 위험 |
| **아이디어·방법론을 참고해 새로 작성** | ✅ **문제 없음** |

> 아이디어와 방법론은 저작권 대상이 아님. "무조건 참인 것부터", "스스로 발견하게 한다"는
> 소크라테스·파인만·3Blue1Brown 계보의 것. **내 문장으로 새로 쓰면 됨.**
> 원작자에게 이슈로 MIT 라이선스 추가를 요청하는 방법도 있음.

## 진짜 자산이 뭐냐

AI 튜터 시장은 레드오션. 하지만 이 레포엔 흔치 않은 게 3개 있음:

1. ⭐⭐⭐ **"모르는 지점 찾기" 루프** — 대부분의 AI 튜터는 진단 없이 처방부터 함
2. ⭐⭐ **계획 승인 게이트** — 사람이 OK 해야 진행
3. ⭐⭐ **"모르겠어요" 버튼** — 찍은 것과 모르는 것을 구분해 기록

> **결론: 팔 것은 "AI 튜터"가 아니라 "지식 진단 엔진"이다.**

## 아이디어 평가표

| # | 아이디어 | 난이도 | 수익성 | 차별화 | 현실성 |
|---|---|---|---|---|---|
| 1 | **개발자 온보딩 튜터 (B2B)** 🏆 | 🔥🔥🔥 | 💰💰💰💰 | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ |
| 2 | 면접 대비 개념 진단 (B2C) | 🔥🔥 | 💰💰 | ⭐⭐⭐ | ⭐⭐⭐ |
| 3 | 강사용 퀴즈 생성 툴 | 🔥🔥 | 💰💰💰 | ⭐⭐ | ⭐⭐⭐ |
| 4 | 유료 스킬팩 판매 | 🔥 | 💰 | ⭐ | ⭐⭐⭐⭐⭐ |
| 5 | 콘텐츠 → 강의 | 🔥 | 💰💰 | ⭐⭐ | ⭐⭐⭐⭐ |

### 1️⃣ 개발자 온보딩 튜터 (B2B) — 최우선 추천

- **문제:** 새 코드베이스 파악에 평균 2~3개월. 그동안 인건비는 계속 나감
- **해결:** 사내 레포 기준 퀴즈로 이해도 진단 → 모르는 부분만 순서대로 교육 → 매니저에게 리포트
- **왜 돈이 되나:** 개인은 월 2만원도 아까워하지만, 회사는 **온보딩 1주 단축 = 수백만원**
- **시작:** 회사 한 곳 무료 파일럿 → 온보딩 기간 실측 → 그 숫자가 세일즈 자료

### 4️⃣ 유료 스킬팩 — 가장 빠른 검증

- Gumroad에 "학습 스킬팩" $19
- 제작 하루, **1주일이면 "돈 낼 사람이 있나"에 답이 나옴**
- 팔리면 1번으로 확장, 안 팔리면 빨리 접기

## 추천 순서

```
[1~2주]  4번으로 검증      →  "돈 내는 사람이 있는가?"
             ↓
[1~3개월] 1번 무료 파일럿   →  온보딩 기간 실측 데이터 확보
             ↓
[이후]    그 데이터로 B2B 세일즈
```

## 🚫 피해야 할 것

| 하지 말 것 | 이유 |
|---|---|
| "AI 과외 서비스" 통짜 제작 | 무료 챗봇과 정면승부 — 못 이김 |
| 원본 코드 그대로 상업화 | 라이선스 없음 = 법적 리스크 |
| 전 과목 지원 | 좁게 시작해야 이김 |
| 6개월 개발 후 출시 | 파는 게 먼저 |

---

# 6부. PHP로 만들 수 있나? 🐘

## 결론: 가능. 오히려 잘 맞음 ✅

AI 부분은 **HTTP API 호출 한 방**이라 언어 무관.
그리고 실제 코드 비중을 보면:

```
회원가입/로그인       ▓▓▓▓▓▓▓▓░░
퀴즈 기록 저장/조회   ▓▓▓▓▓▓▓▓▓░
학습 진도 관리        ▓▓▓▓▓▓▓░░░
결제/구독             ▓▓▓▓▓▓░░░░
리포트 화면           ▓▓▓▓▓░░░░░
🤖 AI 호출            ▓░░░░░░░░░  ← 이만큼
```

**90%가 CRUD** → 라라벨의 안방.

## 공식 PHP SDK

```bash
composer require "anthropic-ai/sdk:^0.7"
```

> ⚠️ v0.4 이하는 파라미터 방식이 달라 명명 인자 사용 시
> `Unknown named parameter $model` 에러. 스트리밍은 v0.5.0+ 필요.

## 추천 스택

```
Laravel 11 + Livewire/Inertia   ← 화면
anthropic-ai/sdk                ← AI 호출
MySQL/PostgreSQL + Redis(큐)    ← 저장소
```

라라벨을 고른 이유: 인증·결제(Cashier)·구독 기본 탑재 + 큐가 튼튼함.

## 코드 예시

### 클라이언트

```php
use Anthropic\Client;

$client = new Client(apiKey: getenv('ANTHROPIC_API_KEY'));
```

### 핵심: 퀴즈 생성 (JSON 강제)

```php
<?php

namespace App\Services;

use Anthropic\Client;

class QuizGenerator
{
    public function __construct(private Client $client) {}

    public function generate(string $topic, string $learnerLevel): array
    {
        $message = $this->client->messages->create(
            model: 'claude-opus-5',
            maxTokens: 16000,
            thinking: ['type' => 'adaptive'],
            system: [
                [
                    'type' => 'text',
                    'text' => file_get_contents(resource_path('prompts/teach.md')),
                    'cacheControl' => ['type' => 'ephemeral'],  // 💰 비용 절감
                ],
            ],
            messages: [[
                'role' => 'user',
                'content' => "주제: {$topic}\n학습자 수준: {$learnerLevel}\n"
                           . "이 사람의 '이해의 경계'를 찾는 진단 문제 1개를 만들어줘.",
            ]],
            outputConfig: [
                'format' => [
                    'type' => 'json_schema',
                    'schema' => [
                        'type' => 'object',
                        'properties' => [
                            'question'      => ['type' => 'string'],
                            'options'       => ['type' => 'array', 'items' => ['type' => 'string']],
                            'correct_index' => ['type' => 'integer'],
                            'explanation'   => ['type' => 'string'],
                            'tests_concept' => ['type' => 'string'],
                            'difficulty'    => ['type' => 'integer'],
                        ],
                        'required' => [
                            'question', 'options', 'correct_index',
                            'explanation', 'tests_concept', 'difficulty',
                        ],
                        'additionalProperties' => false,
                    ],
                ],
            ],
        );

        foreach ($message->content as $block) {
            if ($block->type === 'text') {
                return json_decode($block->text, true);
            }
        }

        throw new \RuntimeException('퀴즈 생성 실패');
    }
}
```

> 💡 `cacheControl`이 핵심. 교육 철학 프롬프트는 매 요청 동일하므로 캐싱하면 입력 비용이 크게 줄어든다.
> 적중 확인은 `$message->usage->cacheReadInputTokens`.

### 컨트롤러

```php
public function nextQuestion(Request $request, QuizGenerator $generator)
{
    $session = LearningSession::findOrFail($request->session_id);

    $quiz = $generator->generate(
        topic: $session->topic,
        learnerLevel: $session->levelSummary(),
    );

    $q = $session->questions()->create($quiz);

    // 정답·해설은 응답에서 제외 (프론트에서 훔쳐보기 방지)
    return response()->json([
        'id'       => $q->id,
        'question' => $q->question,
        'options'  => $q->options,
    ]);
}
```

### 스트리밍

```php
use Anthropic\Messages\RawContentBlockDeltaEvent;
use Anthropic\Messages\TextDelta;

$stream = $client->messages->createStream(
    model: 'claude-opus-5',
    maxTokens: 64000,
    messages: $history,
);

foreach ($stream as $event) {
    if ($event instanceof RawContentBlockDeltaEvent
        && $event->delta instanceof TextDelta) {
        echo "data: " . json_encode(['t' => $event->delta->text]) . "\n\n";
        ob_flush();
        flush();
    }
}
```

## ⚠️ PHP에서 조심할 것 3가지

### 1. 스트리밍 = PHP의 최대 약점

PHP-FPM은 요청 하나당 워커 하나를 점유함.
AI 응답 30초 = 워커가 30초 묶임 → 동시 접속 50명이면 워커 50개 필요.

| 해결책 | 설명 | 추천도 |
|---|---|---|
| **큐 + 폴링** | 요청을 큐에 넣고 즉시 응답, 프론트가 주기적으로 확인 | ⭐⭐⭐⭐⭐ 초기엔 이것 |
| Laravel Octane | 워커 상주 방식 | ⭐⭐⭐ 나중에 |
| Node/Go 사이드카 | 스트리밍만 분리 | ⭐⭐ 규모 커지면 |

> 퀴즈 서비스는 요청/응답 구조라 **초기엔 스트리밍 없이 시작해도 충분함.**

### 2. 타임아웃 — 세 군데 전부 늘려야 함

```ini
; php.ini
max_execution_time = 300
```
```nginx
proxy_read_timeout 300s;
fastcgi_read_timeout 300s;
add_header X-Accel-Buffering no;   # SSE 쓸 경우 필수
```

### 3. AI 호출은 무조건 큐로

```php
// ❌ 사용자가 30초 대기
$quiz = $generator->generate(...);

// ✅ 즉시 응답, 백그라운드 처리
GenerateQuizJob::dispatch($session);
```

## 💰 비용 (Claude Opus 5 기준: 입력 $5 / 출력 $25 per 1M tokens)

| 항목 | 대략 |
|---|---|
| 퀴즈 1문제 생성 | 약 20~30원 |
| 세션 1회 (20문제 + 설명) | 약 500~800원 |
| 캐싱 적용 시 | 위 금액의 30~50% 절감 |

> 더 저렴한 Claude Sonnet 5(입력 $2 / 출력 $10)도 있음.
> 다만 **"뭘 모르는지 진단"은 서비스의 핵심 품질**이라 여기는 Opus 5 권장.
> 단순 채점·요약 같은 부수 작업은 내려도 됨. 실제 품질 비교 후 결정할 것.

## 🗄️ DB 스키마 초안

```php
// learning_sessions
$table->id();
$table->foreignId('user_id');
$table->string('topic');
$table->json('level_map');         // { "포인터": 3, "메모리": 1 }
$table->enum('phase', ['probe','plan','teach']);
$table->timestamps();

// questions
$table->id();
$table->foreignId('learning_session_id');
$table->text('question');
$table->json('options');
$table->tinyInteger('correct_index');
$table->text('explanation');
$table->string('tests_concept');    // ⭐ 어떤 개념을 테스트했나
$table->tinyInteger('difficulty');  // ⭐ 난이도 (이진 탐색용)
$table->tinyInteger('answered_index')->nullable();
$table->boolean('dont_know')->default(false);  // ⭐ "모르겠어요"
$table->timestamps();
```

> ⭐ 표시한 3개가 핵심 자산.
> `tests_concept` + `difficulty` + `dont_know`가 쌓이면 그게 **지식 진단 데이터**이고,
> B2B에 팔 리포트의 원천이 된다.

---

# 7부. 다음 할 일 ✅

- [ ] `teach.md` 프롬프트를 **내 문장으로 새로 작성** (라이선스 회피 + 내 학습 스타일 반영)
- [ ] 최소 프로토타입: 한 파일로 "퀴즈 생성 → 채점"까지 동작 확인
- [ ] Gumroad 스킬팩으로 시장 검증 (1~2주)
- [ ] 라라벨 프로젝트 스캐폴딩 (마이그레이션 + `QuizGenerator` + Job)
- [ ] 회사 한 곳 무료 온보딩 파일럿 → 기간 실측

---

## 💬 마지막 한마디

이 레포의 진짜 가치는 사업 아이템이 아니라,
**"AI를 이렇게까지 정교하게 부릴 수 있구나"**를 보여준 데 있다.

`teach/SKILL.md` 200줄은 그냥 글인데, 그게 AI의 행동을 완전히 바꿔놓는다.
이 감각을 익힌 개발자와 아닌 개발자는 앞으로 생산성이 몇 배로 갈릴 것.

그게 어떤 사이드 프로젝트보다 값질 수도 있다. 🔥
