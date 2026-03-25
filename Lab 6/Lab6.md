# 실습 6: Agent-to-Agent (A2A) 통신을 통한 다중 에이전트 시스템 개발하기

**예상 소요 시간**: 30분

**개요**

이 실습에서는 Microsoft Agent Framework를 사용해 멀티 에이전트 시스템을
구축하게 됩니다. 각각 다른 에이전트 역할(플래너, 인사, 컴플라이언스)을
정의하고, 이를 배포하며, 한 에이전트가 다른 에이전트에게 전화할 수
있도록 A2A(Agent-to-Agent) 통신을 설정해야 합니다. 사용자 쿼리가
에이전트 네트워크를 통해 위임되는 시나리오를 테스트한 후, 올바른
라우팅을 확인하기 위해 트레이스와 로그를 점검합니다.

Microsoft Agent Framework SDK는 다른 에이전트와 협력하고 행동하며 협업할
수 있는 지능형 모듈형 에이전트를 구축하는 새로운 공식 개발 키트입니다.
이 방법은 다음과 같이 제공합니다:

- 통합 에이전트 아키텍처 – AutoGen, Semantic Kernel, 및 단편화된
  오케스트레이터 대체

- Microsoft Foundry에 대한 내장 지원 – 에이전트를 Foundry의 에이전트
  서비스에 직접 배포

- MCP (Model Context Protocol)을 통한 금형 – 데이터, API, 시스템과의
  표준화된 통합

- 내장 A2A 통신 – 에이전트는 다른 에이전트를 자율적 협력자로 호출 가능

이 SDK는 신뢰성, 관측 가능성, 거버넌스가 처음부터 내장된 엔터프라이즈급
운영 대상 에이전트 시스템을 지원하도록 설계되었습니다.

실습 목표

이 실습에서 다음과 같은 작업을 수행할 것입니다.

- 작업 1: 사전 구성된 VS Code 프로젝트를 열기

- 작업 2: 플래너 에이전트를 생성하기

- 작업 3: HR 및 규정 준수 작업자 에이전트를 생성하기

- 작업 4: A2A 라우팅 로직 (에이전트 그래프/워크플로우)를 정의하기

- 작업 5: 다중 에이전트 대화 테스트 및 로그 검사하기

## 작업 1: 사전 구성된 VS Code 프로젝트를 열기

이 작업에서는 사전 설정된 폴더 구조를 검토하여 에이전트 정의,
워크플로우, 도구가 어디에 조직되어 있는지 이해하게 됩니다. 이 과정은
Microsoft Agent Framework SDK를 사용하여 시스템을 확장할 수 있도록
준비시킵니다.

1.  LabVM Desktop에서 **Visual Studio Code**를 선택하세요.

2.  Visual Studio Code가 열리면 **File** **(1)**을 클릭하고 코드 파일
    폴더를 열기 위해 **Open Folder** **(2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  열려 있는 폴더 창에서 C:\Labfiles\Day
    2\Enterprise-Agent-Code-Files으로 이동하고 **select folder**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  열면 팝업 창이 열리니 **Yes, I trust authors** 옵션을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  엔터프라이즈 에이전트의 폴더 구조를 검토하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **.env.example (1)** 파일을 우클릭하고 파일 이름을 바꾸려면 **Rename
    (2)**를 선택하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

7.  완료 후에는 **.env.example** --\> **.env**에서 이 환경 파일을 이
    에이전트에 대해 활성화하도록 이름 변경하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

8.  .env 파일의 내용을 아래 내용으로 교체하세요.

> AZURE_OPENAI_ENDPOINT=https://agentic-
> @lab.LabInstance.Id.cognitiveservices.azure.com/
>
> AZURE_OPENAI_API_KEY=**\<Replace with Azure OpenAI key\>**
>
> AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME=gpt-4o-mini
>
> AZURE_OPENAI_API_VERSION=2025-03-01-preview

Microsoft Foundry Overview 페이지에서 API Key를 복사하고 env 파일에서
자리 표시자 **\<Replace with Azure OpenAI key\>** 로 교체하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

9.  완료되면 **File** **(1)**을 선택하고 파일을 저장하려면
    **Save** **(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image9.png)

## 작업 2: 플래너 에이전트를 생성하기

이 작업에서는 사용자 문의를 해석하고 어떤 Planner Agent (전문
에이전트)에게 작업을 위임할지 결정하는 플래너 에이전트를 정의할
것입니다. 에이전트는 역할별 지침과 함께 에이전트 프레임워크 SDK를 사용해
구성해야 합니다.

1.  목록에서 에이전트 폴더의 **planner_agent.py**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  다음 Python 코드를 추가하고 플래너 에이전트를 구성하세요.

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_planner_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="PlannerAgent",
>
> instructions=(
>
> "You are an intelligent routing agent. Analyze user queries and route
> them to the appropriate specialist. "
>
> "Available specialists:\n"
>
> "- HR: Employee policies, leave, benefits, working hours, performance,
> hiring\n"
>
> "- FINANCE: Reimbursements, expenses, budgets, travel costs, meal
> allowances, equipment purchases\n"
>
> "- COMPLIANCE: GDPR, data privacy, regulatory requirements, legal
> compliance, audits\n\n"
>
> "Return exactly one word: HR, FINANCE, or COMPLIANCE. "
>
> "Consider keywords like: money, cost, budget, reimburse, expense,
> payment, allowance → FINANCE\n"
>
> "Keywords like: leave, sick, vacation, policy, employee, benefits →
> HR\n"
>
> "Keywords like: GDPR, privacy, compliance, legal, audit, regulation →
> COMPLIANCE"
>
> ),
>
> )
>
> async def classify_target(planner_agent, user_query: str) -\> str:
>
> result = await planner_agent.run(
>
> "Analyze and route this query:\n\n"
>
> f"User query: {user_query}\n\n"
>
> "Return exactly one word: HR, FINANCE, or COMPLIANCE."
>
> )
>
> \# Extract the text content from the AgentRunResponse object
>
> text = str(result).strip().lower()
>
> \# Advanced classification with fallback logic
>
> if "finance" in text or "financial" in text:
>
> return "FINANCE"
>
> elif "hr" in text or "human" in text:
>
> return "HR"
>
> elif "compliance" in text or "legal" in text:
>
> return "COMPLIANCE"
>
> else:
>
> \# Fallback keyword analysis if agent response is unclear
>
> query_lower = user_query.lower()
>
> finance_keywords = \["reimburs", "expense", "cost", "budget", "money",
> "payment", "allowance", "travel", "meal", "flight", "hotel"\]
>
> hr_keywords = \["leave", "sick", "vacation", "employee", "benefit",
> "policy", "hire", "performance", "work"\]
>
> compliance_keywords = \["gdpr", "privacy", "compliance", "legal",
> "audit", "regulation", "data protection"\]
>
> finance_score = sum(1 for keyword in finance_keywords if keyword in
> query_lower)
>
> hr_score = sum(1 for keyword in hr_keywords if keyword in query_lower)
>
> compliance_score = sum(1 for keyword in compliance_keywords if keyword
> in query_lower)
>
> if finance_score \> hr_score and finance_score \> compliance_score:
>
> return "FINANCE"
>
> elif hr_score \> compliance_score:
>
> return "HR"
>
> else:
>
> return "COMPLIANCE"

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image11.png)

> **플래너 에이전트의 목적:**

- 이 에이전트는 사용자 문의를 분석하고 어떤 전문 에이전트(HR, 재무,
  컴플라이언스)가 응답을 처리할지 결정하도록 설계되었습니다.

> **AzureOpenAIResponsesClient를 사용하여 에이전트 생성:**

- build_planner_agent() 함수는 Agent Framework SDK를 사용하여 Planner를
  초기화하며, 환경 변수에서 로드된 API 기반 자격 증명을 사용합니다.

> **LLM 가이드 라우팅 (기본 로직):**

- 플래너 에이전트가 쿼리 내 키워드와 맥락에 따라 HR, FINANCE, 또는
  COMPLIANCE 단 한 단어를 반환하도록 지시받습니다.

> **의사 결정용 classify_target():**

- 이 함수는 먼저 wait_run() 호출을 사용하여 플래너에게 어떤 전문가를
  선택할지 묻습니다. 응답이 불명확할 경우, 대체 키워드 기반 분석을
  적용합니다.

> **하이브리드 AI + 휴리스틱 전략:**

- 이 설계는 모델 추론과 수동 키워드 점수 매기를 결합하여 신뢰할 수 있는
  라우팅을 보장하여, AI 출력이 모호할 때도 플래너를 견고하게 생성합니다.

3.  완료되면 **File** **(1)**을 선택하고 파일을 저장하려면
    **Save** **(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 작업 3: 작업자 에이전트를 생성하기

이 작업에서는 HR, 재무, 컴플라이언스 지식을 담당하는 도메인 특화
에이전트를 개발하게 됩니다. 각 에이전트는 A2A 통신을 통한 발견 및 위임을
가능하게 하기 위해 에이전트 등록부에 등록됩니다.

1.  목록에서 agent 폴더 아래에서 **hr_agent.py**을 선택하고 다음 Python
    코드를 추가하여 HR 에이전트를 설정하세요. HR 에이전트를 구성하기
    위해 다음 Python 코드를 추가하세요.

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_hr_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="HRAgent",
>
> instructions=(
>
> "You are an expert HR policy specialist with deep knowledge of
> employment law and best practices. "
>
> "Answer questions about:\n"
>
> "- Leave policies (sick, vacation, parental, bereavement)\n"
>
> "- Employee benefits (health insurance, retirement, wellness
> programs)\n"
>
> "- Performance management and reviews\n"
>
> "- Hiring, onboarding, and termination procedures\n"
>
> "- Working hours, overtime, and flexible work arrangements\n"
>
> "- Employee relations and conflict resolution\n"
>
> "- Training and development programs\n\n"
>
> "Provide specific, actionable guidance with policy references where
> applicable. "
>
> "Be empathetic and professional in your responses."
>
> ),
>
> )

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image12.png)

> **HR에이전트의 목적:**

- 이 에이전트는 직원 복지, 휴가 구조, 복리후생, 직장 절차 등 다양한
  질문에 답변할 수 있도록 훈련받은 전담 인사 정책 전문가로 활동합니다.

> **Azure 응답 클라우언트를 사용하여 에이전트 초기화:**

- build_hr_agent() 함수는 Azure OpenAIResponsesClient를 사용하여
  에이전트를 초기화하며, API 키와 환경 변수에 저장된 엔드포인트 값을
  통해 인증합니다.

> **도메인별 특화:**

- 지침 섹션에서는 휴가 유형, 복리후생, 온보딩, 직원 관계, 성과 관리 등
  HR 담당자의 업무 범위를 명확히 정의하며, HR 관련 문의에만 응답하도록
  보장합니다.

> **전문적이고 공감적인 어조:**

- 이 에이전트는 실제 HR 커뮤니케이션 기준을 모방하여 정확하고 전문적이며
  공감 어린 지침을 제공하여 내부 조직 보조원에게 이상적입니다.

> **다중 에이전트 협업 기초:**

- 구축되면 이 HR 에이전트는 플래너 에이전트에 의해 호출되어, HR 관련
  쿼리가 감지될 때 다중 에이전트 워크플로우에서 자동 위임이 가능합니다.

2.  완료되면 **File** **(1)**을 선택하고 파일을 저장하려면
    **Save** **(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

3.  목록에서 agent 폴더 아래의 **finance_agent.py**을 선택하고 다음
    Python 코드를 추가하여 컴플라이언스 에이전트를 구성하세요. 다음
    Python 코드를 추가하여 금융 에이전트를 구성하세요.

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_finance_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="FinanceAgent",
>
> instructions=(
>
> "You are a finance and reimbursement specialist. Answer questions
> about "
>
> "expense policies, reimbursement limits, budget approvals, travel
> expenses, "
>
> "meal allowances, equipment purchases, and financial procedures.
> Provide "
>
> "specific amounts, policies, and actionable guidance."
>
> ),
>
> )

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image13.png)

> 전무 금융 역할:

- 이 에이전트는 환급 정책, 여행 예산, 수당, 구매 승인 등 모든 금융 관련
  주제를 처리하도록 설계되었습니다.

> **에이전트 프레임워크 SDK를 통한 초기화:**

- build_finance_agent() 함수는 AzureOpenAIResponsesClient를 사용하여
  안전한 환경 변수에서 API 키 인증을 활용해 에이전트를 생성합니다.

> **정책 중심 지침:**

- 에이전트의 지침은 명확히 재무 절차에 대한 책임을 제한하여 비용, 지급,
  예산, 기업 경비 규칙에 대한 정확한 답변을 보장합니다.

> **정밀도와 실행 가능한 출력:**

- 일반 목적 에이전트와 달리, 이 재무 도우미는 한도, 자격, 승인 흐름과
  같은 특정 정책 가치를 제공하도록 지시받아 직원들에게 실용적입니다.

> **플래너 대표 지원 (A2A):**

- 이 에이전트는 플래너가 금융 관련 키워드나 쿼리를 감지할 때 자동으로
  호출되어, 시스템 내 다중 에이전트 협업이 원활하게 이루어집니다.

4.  완료되면 **File** **(1)**을 선택하고 파일을 저장하려면
    **Save** **(2)**를 클릭하세요file.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

5.  목록에서 agent 폴더 아래의 **compliance_agent.py**을 선택하고 다음
    Python 코드를 추가하여 컴플라이언스 에이전트를 구성하세요.
    컴플라이언스 에이전트를 구성하기 위해 다음 Python 코드를 추가하세요.

> import os
>
> import asyncio
>
> from agent_framework.azure import AzureOpenAIResponsesClient \# type:
> ignore
>
> async def build_compliance_agent():
>
> client = AzureOpenAIResponsesClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=os.getenv("AZURE_OPENAI_ENDPOINT"),
>
> deployment_name=os.getenv("AZURE_OPENAI_RESPONSES_DEPLOYMENT_NAME"),
>
> api_version=os.getenv("AZURE_OPENAI_API_VERSION"),
>
> )
>
> return client.create_agent(
>
> name="ComplianceAgent",
>
> instructions=(
>
> "You are a senior compliance and legal specialist with expertise in
> multiple jurisdictions. "
>
> "Provide authoritative guidance on:\n"
>
> "- GDPR and data protection regulations (EU, UK, US state laws)\n"
>
> "- Privacy policies and data processing agreements\n"
>
> "- Regulatory compliance (SOX, HIPAA, PCI-DSS, ISO standards)\n"
>
> "- Risk assessment and audit requirements\n"
>
> "- Contract law and vendor agreements\n"
>
> "- Information security policies\n"
>
> "- Cross-border data transfers and adequacy decisions\n"
>
> "- Breach notification requirements\n\n"
>
> "Always provide factual, well-researched answers with relevant legal
> citations. "
>
> "Include practical implementation steps and potential risks. Use
> formal, professional tone."
>
> ),
>
> )

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image14.png)

> **에이전트의 목적:**

- 이 에이전트는 GDPR, 규제 체계, 계약법, 위험 평가 및 보안 기준과 관련된
  문의를 처리하는 전담 법률 및 준수 기관 역할을 합니다.

> **에이전트 초기화:**

- build_compliance_agent() 함수는 Azure OpenAIResponsesClient와 API 키
  인증을 사용하여 Microsoft Agent Framework SDK를 통해 컴플라이언스
  에이전트를 등록합니다.

> **지침서에 정의된 규제 전문성:**

- 지침은 글로벌 개인정보 보호 규정(GDPR, HIPAA, SOX), 감사 준비태세,
  법적 합의, 침해 프로토콜 등 명확한 준수 범위를 제공하여 높은 신뢰
  대응을 보장합니다.

> **톤과 출력 기대:**

- 이 에이전트는 법적 인용이나 실행 권고를 포함해 공식적이고 권위 있는
  어조로 답변을 전달하도록 구성되어 있습니다.

> **다중 에이전트 시스템에서의 역할:**

- A2A 위임 과정에서 플래너 에이전트는 법률 또는 준법 관련 문의를 이
  전문가에게 전달하여 기업 의사결정 워크플로우의 정확성과 거버넌스를
  유지합니다.

6.  완료되면 **File** **(1)**을 선택하고 파일을 저장하려면
    **Save** **(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 작업 4: A2A 라우팅 로직 (에이전트 그래프/워크플로우)를 정의하기

Agent-to-Agent (A2A)는 Microsoft Agent Framework의 핵심 기능으로, 한
에이전트가 다른 에이전트에게 자율적으로 작업을 위임할 수 있게 합니다.

이 작업에서는 에이전트 워크플로우를 사용하여 라우팅 로직을 구현하여
플래너가 쿼리 의도에 따라 인사 또는 컴플라이언스 에이전트에게 자율적으로
호출할 수 있게 됩니다. 이것이 진정한 다중 에이전트 협업을 확립합니다.

1.  목록에서 agent 폴더 아래의 **main.py** 을 선택하고 다음 Python
    코드를 추가하여 A2A 통신 흐름 에이전트를 구성하세요. 다음 Python
    코드를 추가하여 에이전트 라우팅 로직을 구성하세요.

> import asyncio
>
> import time
>
> import logging
>
> from typing import Dict, Any
>
> from utils.env import load_env
>
> from agents.planner_agent import build_planner_agent, classify_target
>
> from agents.hr_agent import build_hr_agent
>
> from agents.compliance_agent import build_compliance_agent
>
> from agents.finance_agent import build_finance_agent
>
> \# Configure logging
>
> logging.basicConfig(level=logging.INFO, format='%(asctime)s -
> %(levelname)s - %(message)s')
>
> async def run_multi_agent(query: str, agents: Dict\[str, Any\]) -\>
> Dict\[str, Any\]:
>
> """
>
> Advanced multi-agent system with routing, timing, and comprehensive
> response handling.
>
> """
>
> start_time = time.time()
>
> try:
>
> \# Step 1: Route the query
>
> logging.info(f"Routing query: {query\[:50\]}...")
>
> target = await classify_target(agents\["planner"\], query)
>
> logging.info(f"Query routed to: {target}")
>
> \# Step 2: Get response from appropriate agent
>
> agent_mapping = {
>
> "HR": ("hr", "HRAgent"),
>
> "FINANCE": ("finance", "FinanceAgent"),
>
> "COMPLIANCE": ("compliance", "ComplianceAgent")
>
> }
>
> if target in agent_mapping:
>
> agent_key, agent_name = agent_mapping\[target\]
>
> answer = await agents\[agent_key\].run(query)
>
> else:
>
> \# Fallback to HR if routing unclear
>
> logging.warning(f"Unknown target '{target}', falling back to HR")
>
> answer = await agents\["hr"\].run(query)
>
> target = "HR"
>
> agent_name = "HRAgent"
>
> \# Step 3: Process response
>
> response_time = time.time() - start_time
>
> return {
>
> "query": query,
>
> "routed_to": target,
>
> "agent_name": agent_name,
>
> "answer": str(answer),
>
> "response_time": round(response_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": True
>
> }
>
> except Exception as e:
>
> logging.error(f"Error processing query: {e}")
>
> return {
>
> "query": query,
>
> "routed_to": "ERROR",
>
> "agent_name": "ErrorHandler",
>
> "answer": f"I apologize, but I encountered an error processing your
> request: {str(e)}",
>
> "response_time": round(time.time() - start_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": False
>
> }
>
> def format_response(result: Dict\[str, Any\]) -\> str:
>
> """Format the agent response for display."""
>
> status_icon = "✅" if result\["success"\] else "❌"
>
> formatted = f"""
>
> {status_icon} Agent Response Summary:
>
> ┌─ Routed to: {result\['routed_to'\]} ({result\['agent_name'\]})
>
> ├─ Response time: {result\['response_time'\]}s
>
> ├─ Timestamp: {result\['timestamp'\]}
>
> └─ Status: {'Success' if result\['success'\] else 'Error'}
>
> 💬 Answer:
>
> {result\['answer'\]}
>
> """
>
> return formatted
>
> async def run_interactive_mode(agents: Dict\[str, Any\]):
>
> """Interactive mode for real-time queries."""
>
> print("\n🤖 Enterprise Agent System - Interactive Mode")
>
> print("Available agents: HR, Finance, Compliance")
>
> print("Type 'quit' to exit, 'help' for commands\n")
>
> while True:
>
> try:
>
> query = input("Enter your question: ").strip()
>
> if query.lower() in \['quit', 'exit', 'q'\]:
>
> print("👋 Goodbye!")
>
> break
>
> elif query.lower() == 'help':
>
> print("""
>
> 📋 Available Commands:
>
> \- Ask any question about HR, Finance, or Compliance
>
> \- 'quit' or 'exit' - Exit the system
>
> \- 'help' - Show this help message
>
> 🎯 Example questions:
>
> \- "What's the travel reimbursement limit for meals?"
>
> \- "How many vacation days do employees get?"
>
> \- "Do we need GDPR compliance for EU customers?"
>
> """)
>
> continue
>
> elif not query:
>
> continue
>
> result = await run_multi_agent(query, agents)
>
> print(format_response(result))
>
> except KeyboardInterrupt:
>
> print("\n👋 Goodbye!")
>
> break
>
> except Exception as e:
>
> logging.error(f"Interactive mode error: {e}")
>
> print(f"❌ Error: {e}")
>
> async def run_batch_tests(agents: Dict\[str, Any\]):
>
> """Run predefined test queries."""
>
> test_queries = \[
>
> "How much reimbursement is allowed for international flights?",
>
> "Is employee data protected under GDPR?",
>
> "How many sick leave days do employees get?"
>
> \]
>
> print("🧪 Running batch tests...\n")
>
> for i, query in enumerate(test_queries, 1):
>
> print(f"{'='\*80}")
>
> print(f"TEST {i}/{len(test_queries)}: {query}")
>
> print(f"{'='\*80}")
>
> result = await run_multi_agent(query, agents)
>
> print(format_response(result))
>
> \# Small delay between queries for better readability
>
> if i \< len(test_queries):
>
> await asyncio.sleep(0.5)
>
> async def main():
>
> """Main application entry point with enhanced features."""
>
> print("🚀 Initializing Enterprise Agent System...")
>
> try:
>
> \# Load environment and build agents
>
> load_env()
>
> logging.info("Building agent network...")
>
> agents = {
>
> "planner": await build_planner_agent(),
>
> "hr": await build_hr_agent(),
>
> "compliance": await build_compliance_agent(),
>
> "finance": await build_finance_agent()
>
> }
>
> logging.info("✅ All agents initialized successfully")
>
> \# Check if running interactively or in batch mode
>
> import sys
>
> if len(sys.argv) \> 1 and sys.argv\[1\] == "--interactive":
>
> await run_interactive_mode(agents)
>
> else:
>
> await run_batch_tests(agents)
>
> except Exception as e:
>
> logging.error(f"System initialization failed: {e}")
>
> print(f"❌ Failed to start system: {e}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

> **중앙 실행 엔진:**

- 이 스크립트는 핵심 조정자 역할을 하여 모든 에이전트(플래너, 인사,
  재무, 컴플라이언스)를 조정하고 Microsoft Agent Framework를 사용하여
  다중 에이전트 라우팅을 관리합니다.

> **에이전트 네트워크 초기화:**

- 환경 설정을 불러오고, 각 에이전트를 wait build\_\*\_agent()로 빌드한
  뒤, 쉽게 위임할 수 있도록 공유 사전에 등록합니다.

> **고급 A2A 라우팅:**

- run_multi_agent() 기능은 플래너를 통해 사용자 문의를 올바른 전문가에게
  전달한 후 전문 상담원의 응답을 기다립니다. 경로, 타이밍, 성공 상태,
  최종 답변을 모두 포착합니다.

> **다중 실행 모드:**

- 배치 모드: 미리 정의된 테스트 쿼리를 실행합니다.

- 인터랙티브 모드(--인터랙티브): 실시간 채팅을 가능하게 하여 실시간
  테스트와 탐색을 지원합니다.

> **생산 준비 가능 회복력:**

- 응답 형식, 타임스탬프, 오류 백업 메커니즘, 로깅을 포함하여 이후
  활동에서 관측 가능성, 텔레메트리, 에이전트 옵스의 탄탄한 기반을
  마련합니다.

2.  완료되면 **File** **(1)**을 선택하고 파일이 저장하려면
    **Save** **(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image8.png)

## 작업 5: 다중 에이전트 대화 테스트 및 로그 검사하기

이 작업에서는 멀티 에이전트 시스템을 통해 end-to-end 테스트 쿼리를
실행하고, Microsoft Foundry에서 로그와 텔레메트리를 이용해 에이전트
협업을 관찰하게 됩니다.

1.  플래너 에이전트와 워커 에이전트로 멀티 에이전트 시스템을 성공적으로
    설정하셨습니다. 이제 이 다중 에이전트 시스템의 작동 방식을 시험할
    것입니다.

> **참고:** 멀티 에이전트 시스템은 현재 LLM 기능을 갖추고 있지만, MCP
> 통합이나 데이터세트, Azure AI 검색 인덱스와 같은 외부 지식 소스 접근은
> 아직 없습니다. 이 단계에서 에이전트들은 질문에 답하기 위해 일반 모델
> 지능에만 의존합니다.

2.  상단 메뉴에서 **... (1)** 옵션을 선택하고 메뉴를 확장하세요.
    **Terminal (2)**를 선택하고 **New Terminal (3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

3.  터미널이 열려면 필요한 패키지를 설치하려면 +++pip install -r
    requirements.txt+++ 명령어를 실행하세요.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

4.  설치가 성공적으로 완료되면, 다음 명령을 실행하여 에이전트를 실행하고
    코드 파일에 제공된 테스트 프롬프트에 대한 응답을 검토하세요.

+++python main.py+++

> ![A computer screen shot of a program AI-generated content may be
> incorrect.](./media/image19.png)

**Routed to** 매개변수를 확인하고 에이전트가 요청을 어떻게 결정하고 해당
작업자 에이전트에게 라우팅하는지 점검하세요.

5.  이제 --interactive 플래그를 추가하여 인터랙티브 모드로 에이전트를
    다시 실행하세요. 이렇게 하면 질문을 입력하면 답변을 받을 수
    있습니다. 질문이 나오면 아래 프롬프트를 질문으로 제공하세요.

    - Command:

> +++python main.py –interactive+++

- Prompt:

> +++How much reimbursement is allowed for international flights?+++

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image20.png)

6.  응답을 받은 후 다음 프롬프트에서 q를 추가해 에이전트를 종료하거나
    중단하세요.

![A black screen with white text AI-generated content may be
incorrect.](./media/image21.png)

**요약**

이 실습에서는 Microsoft Agent Framework SDK를 사용해 세 가지
에이전트(Planner, HR, Compliance)를 정의하고 등록했습니다. 사용자 문의를
에이전트 간 통화를 위임하는 라우팅 워크플로우를 생성했습니다. 멀티
에이전트 시나리오를 테스트하고 로그 검사를 통해 올바른 메시지 라우팅과
실행 흐름을 확인했습니다.
