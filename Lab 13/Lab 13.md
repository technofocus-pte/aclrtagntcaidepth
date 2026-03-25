# 실습 13: Human-in-the-Loop AI를 이용한 기업 사기 탐지 구현하기

**예상 소요 시간**: 60분

**개요**

당신은 Contoso Ltd.의 AI 엔지니어로, Human-in-the-Loop (HITL) AI
워크플로우 구현을 담당하고 있습니다. 이 실습에서는 Contoso 사기 탐지 및
대응 워크플로우를 탐구하는데, AI 에이전트가 의심스러운 활동을 분석하고
고위험 행동을 실시간 React + FastAPI 대시보드를 통해 모니터링 및
상호작용을 위해 인간 분석가에게 전달하는 방식입니다.

실습 목표

이 실습에서 다음과 같은 작업을 수행하게 됩니다.

- 작업 1: Azure 에이전트 프레임워크를 활용한 Human-in-the-Loop AI
  워크플로우 구현하기

## 작업 0: 코드를 설정하기 

1.  C:\Labfiles\Day 3에서 **OpenAIWorkshop-Framework** 파일을
    추출하세요.

2.  LabVM 데스크톱에서 **Visual Studio Code**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  **File** **(1)**을 선택하고 **OpenAIWorkshop-Framework** 폴더를
    열려면 **Open Folder** **(2)**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  C:\Labfiles\Day 3\\**OpenAIWorkshop-Framework** 경로로 이동하고
    **OpenAIWorkshop-Framework**를 선택하고 **Select Folder**를
    클릭하세요.

5.  **Yes, I trust the authors**를 선택하세요.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image3.png)

6.  **ellipsis(...)** **(1)**을 클릭하고 **Terminal** **(2)**를 클릭하고
    **New Terminal** **(3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  아래 명령어를 입력하여 **applications** 디렉터리로 이동하고
    **pyproject.toml / uv.lock** 파일에서 필요한 모든 의존성을
    설치하세요.

> cd agentic_ai/applications
>
> uv sync

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

**참고:** 오류가 발생하면 아래에 주어진 명령어를 실행하세요

> +++pip install uv+++

+++uv sync+++

8.  명령은 완료하는 데 5분에서 10분 정도 걸릴 수 있습니다. **그동안 작업
    1을 진행할 수 있습니다**.

## 작업 1: Azure 에이전트 프레임워크를 활용한 Human-in-the-Loop AI 워크플로우 구현하기

이 실습에서는 Contoso의 사기 탐지 시스템을 위한 Human-in-the-Loop (HITL)
워크플로우를 구현하게 됩니다. 다중 에이전트 사기 탐지를 실행하고, 고위험
경고를 검토하며, 인간의 의사결정을 내리고, React + FastAPI 대시보드를
사용해 실시간으로 워크플로우를 시각화할 수 있습니다.

1.  Visual Studio Code에서 **agentic_ai (1) \> workflow (2)\>
    fraud_detection (3)**을 확장하고 **fraud_detection_workflow.py
    (4)**를 선택하세요. 코드를 보세요 **(5)**.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image6.png)

2.  **fraud_detection** **(1)**에서 **.env.sample** **(2)**를 우클릭하고
    **Rename** **(3)**을 선택하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

3.  .env로 이름을 바꾸고 클릭하면 파일을 여세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image8.png)

4.  AZURE_OPENAI_API_KEY **(1)**와 AZURE_OPENAI_ENDPOINT **(2)** 값을
    이전 실습에서 복사한 실제 값으로 교체하세요.

5.  AZURE_OPENAI_CHAT_DEPLOYMENT을 **gpt-4o-mini (3)**로 추가하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

- **Microsoft Foundry** 포털로 이동하고 **Overview** **(1)**을 선택하고
  **Azure OpenAI** **(2)**를 선택하세요. **Azure OpenAI
  key** **(3)** 및 **Azure OpenAI endpoint** **(4)**를 복사하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

6.  **File** **(1)**을 선택하고 **Save** **(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

7.  Visual Studio Code 창에서 **ellipsis(...)** **(1)**을
    클릭하고 **Terminal** **(2)**를 클릭하고 **New Terminal** **(3)**을
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  다음 명령어를 실행하세요.

> cd mcp
>
> uv run python mcp_service.py

9.  명령어를 실행하고 새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. 아래에 제시된 명령러를 입력하여 명령줄로 워크플로우를 실행하세요.

> cd agentic_ai/workflow/fraud_detection
>
> uv run python fraud_detection_workflow.py
>
> ![A black screen with white text AI-generated content may be
> incorrect.](./media/image12.png)

**참고**: 명령은 완료하는 데 5분에서 10분 정도 걸릴 수 있습니다. 끝날
때까지 기다리세요.

11. 예시에는 세 가지 샘플 경고가 포함되어 있습니다:

    - **경고 1: 다국 로그인** (고중증도)

    - alert_id: "ALERT-001"

    - customer_id: 1

    - alert_type: "multi_country_login"

    - 설명: "Login attempts from USA and Russia within 2 hours."

심각성: "상"

- **경고 2: 데이터 스파이크** (중등 중증도)

- alert_id: "ALERT-002"

- customer_id: 2

- alert_type: "data_spike"

- 설명: "Data usage increased by 500% in the last 24 hours."

심각성: "중"

- **경고 3: 비정상적인 혐의** (고강도)

- alert_id: "ALERT-003"

- customer_id: 3

- alert_type: "unusual_charges"

- 설명: "Three large purchases totaling $5,000 in 10 minutes."

심각도: "상"

12. 실행이 성공하면 터미널을 아래와 같이 볼 수 있습니다. 위험 심각도에
    따라 행동을 선택하세요. 위험 심각도≥0.6이 필요하다면 인간 검토가
    필요합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. 위험 심각도가 높으므로 고객 계정을 잠그기 위해 2를 입력할 수
    있습니다 **(1)**

    - 분석가 노트: 세 가지 분석 모두 고위험이 확인되었습니다. 즉각적인
      조치: 무단 접근을 막기 위해 계정을 잠가세요. **(2)**

    - 분석가 ID 입력(기본값: analyst_cli): **Enter (3)**를 누르세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. 워크플로우가 완료되면 다음과 같은 결과물을 받게 됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. 명령이 성공하면, **delete all the existing running terminal
    sessions**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

## Contoso 사기 탐지 및 대응 워크플로우를 위한 실시간 워크플로우 시각화 UI

실시간 워크플로우 시각화 UI를 사용하여 Contoso 사기 탐지 및 대응
워크플로우를 모니터링하고 상호작용하게 됩니다. 모든 서비스(MCP 서버,
백엔드, 프론트엔드)를 시작하고, 샘플 알림을 선택하며, 실시간 워크플로우
실행을 관찰하고, 고위험 사기 경고를 검토하며, 분석가 결정을 제출하고,
이벤트 스트림을 실시간으로 모니터링합니다.

1.  새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

2.  모든 서비스 시작하세요 (3개 터미널):

    - 터미널 1 - MCP Server:

> cd mcp
>
> uv run mcp_service.py

- 터미널 2 - FastAPI Backend:

> cd agentic_ai/workflow/fraud_detection
>
> uv run --prerelease allow backend.py
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image17.png)

- 터미널3 - React Frontend:

> cd agentic_ai/workflow/fraud_detection/ui
>
> npm run dev
>
> **참고**: 오류가 발생하면 +++npm install+++ 명령을 실행한 후 +++npm
> run dev+++ 명령을 다시 실행하세요.
>
> ![A computer screen with white text AI-generated content may be
> incorrect.](./media/image18.png)

- **Ctrl +** http://localhost:3000 클릭하면 브라우저에서 애플리케이션을
  여세요

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

3.  실시간 워크플로우 시각화 UI 보세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  **Select Alerts**드롭다운에서 샘플 알림을 볼 수 있습니다 .

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

**참고**: 두 번째 단말기(backend.py)에서 연결이 열린 후에야 드롭다운에서
알림을 볼 수 있습니다. 연결이 열려 있는지 확인하세요.

5.  **경고 선택**: 3가지 샘플 경고 (ALERT-001, ALERT-002, ALERT-003)
    **(1) 중에서 선택하세요**

    - 처리를 시작하려면 **Start Workflow (2)**를 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

6.  **실시간 업데이트 시청**: 노드는 실행 시 색상이 변합니다

- 🔵 파란색 = 달리기

- 🟢 녹색 = 완료

- ⚪ 회색 = 대기 상태

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

7.  **분석가 리뷰**: 고위험 사기가 발견되면 심사 위원회가 구성됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

8.  **결정 제출**: 액션을 선택하고 노트를 추가하세요

    - 결정: 심각성이 높으면 **Lock Account (1)**

    - 분석가 노트: 세 가지 분석 모두 확인된 고위험 항목을 입력합니다.
      즉각적인 조치: 무단 접근을 막기 위해 계정을 잠가세요. **(2)**

    - SUBMIT WORKFLOW **(3)** 선택

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

9.  **모니터 이벤트**: 오른쪽 패널은 전체 이벤트 스트림을 보여줍니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**요약**

이 실습에서는 Azure Agent 프레임워크를 활용해 사기 탐지를 위한
human-in-the-loop (HITL) 워크플로우를 구현하셨습니다. AI 에이전트가
의심스러운 활동을 분석하고, 고위험 사례를 인간 분석가에게 연결하며,
실시간 React + FastAPI 대시보드와 상호작용하여 워크플로우 실행을
모니터링하고 의사결정을 제출하는 방식을 탐구하셨습니다.
