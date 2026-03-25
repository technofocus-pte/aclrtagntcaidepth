# 실습 8: Microsoft Foundry에 에이전트 배포 및 런타임 관리

**예상 소요 시간**: 15분

**개요**

이 실습에서는 Microsoft Agent Framework SDK를 사용해 개발한 멀티
에이전트 시스템을 Microsoft Foundry Agent Service에 배포하게 됩니다.
구성된 에이전트를 관리형 런타임 환경에 게시할 것입니다.

지금까지 Chat Response Agent를 생성했으니, 이는 다음을 의미합니다:

- 단일 턴, 상태 없는 상호작용을 처리하며 사용자 입력에 즉시 응답합니다.

- 영구 백엔드 없이 애플리케이션이나 SDK 내에서 로컬로 실행됩니다.

- 각 요청은 독립적으로 처리되며, 메모리나 장기적인 맥락은 유지되지
  않습니다.

- 빠른 채팅 경험이나 완전 배포 전 핵심 로지 테스트에 이상적입니다.

이제 Microsoft Foundry에서 이를 Persistent Agent로 업데이트하는데, 이는
다음을 의미합니다:

- 이 서비스는 Foundry 환경 내에서 관리되고 장기 지속되는 서비스로
  운영됩니다.

- 세션 전반에 걸쳐 상태와 맥락을 유지하여 연속성과 학습을 가능하게
  합니다.

- MCP 및 A2A 프로토콜을 사용하는 외부 도구 및 기타 에이전트와의 통합을
  지원합니다.

- 기업 규모의 신뢰성, 모니터링, 컴플라이언스에 최적화되어 있습니다.

**실습 목표**

이 실습에서 다음 작업을 수행할 것입니다.

- 작업 1: Microsoft Foundry Agent Service로 에이전트를 배포하기

## 작업 1: Microsoft Foundry Agent Service로 에이전트를 배포하기

이 작업에서는 기존 에이전트를 persistent agent로 업데이트하고, 각
에이전트를 독립 실행형 모델로 Microsoft Foundry Agent Service에
게시합니다.

1.  이제 코드 파일을 업데이트하여 Microsoft Foundry 에이전트 서비스에
    에이전트를 등록하는 persistant agent 시스템을 지원해야 합니다.

2.  왼쪽 메뉴의 Visual Studio Code 창에서 AI Foundry 프로젝트 키를
    업데이트하려면 .env 파일을 선택하세요.

3.  다음 변수들을 파일에 추가하세요.

> \# Azure AI Project Configuration
>
> AZURE_AI_PROJECT_ENDPOINT=**\<Microsoft Foundry endpoint\>**
>
> AZURE_AI_MODEL_DEPLOYMENT_NAME=gpt-4o-mini
>
> Overview 페이지에서 Microsoft Foundry 프로젝트 엔드포인트를 찾고 그
> 값으로 **\<Microsoft Foundry endpoint\>**를 교체하세요.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

![](./media/image2.png)

4.  업데이트되면 파일은 다음과 비슷하게 보일 것입니다.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  이제 에이전트들을 하나씩 업데이트해야 합니다. **Agents**의 탐색기
    메뉴에서 **compliance_agent.py**를 선택하세요. 내용을 코드
    스니펫으로 교체하세요.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_compliance_agent():
>
> """Build or reuse persistent compliance agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-ComplianceAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a senior compliance and legal specialist."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> "When provided CONTEXT, prefer it as the primary source. "
>
> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a compliance request\\, \\open a support ticket\\),
> output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: user@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself.\n\n"
>
> "Always provide factual, well-researched answers with relevant legal
> citations. "
>
> "Include practical implementation steps and potential risks. Use
> formal, professional tone."
>
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a senior compliance and legal specialist."
>
> )

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image4.png)

> **Azure AI Project Client와 통합:**

- AIProjectClient는 Microsoft Foundry 프로젝트 엔드포인트에 직접
  연결되어, 스크립트가 Foundry 내에서 영구적으로 호스팅되는 에이전트를
  나열, 검색 또는 생성할 수 있습니다.

> **에이전트 재사용 로직:**

- 새 에이전트를 생성하기 전에 코드는 먼저
  "Enterprise-ComplianceAgent"라는 이름의 기존 에이전트를 확인합니다.

- 발견되면 기존 에이전트를 Foundry가 관리하는 고유 agent_id을 통해
  연결하여 재사용합니다.

> **Persistent Agent 생성:**

- 에이전트가 존재하지 않는다면, project_client.agents.create_agent()를
  통해 생성됩니다.

- 에이전트는 모델, 이름, 상세 명령어 집합과 함께 Foundry에 등록되어 있어
  세션 간에 영구적으로 접근 가능합니다.

> **ChatAgent Wrapping:**

- 생성되거나 복구된 후, 영구 Foundry 에이전트는 AzureAIAgentClient를
  이용한 ChatAgent 인스턴스에 랩됩니다.

- 이를 통해 호스트 에이전트와 프로그래밍 통신이 가능하며, Microsoft
  Foundry 내에서 상태, 정책, 모니터링 기능을 유지할 수 있습니다.

6.  완료하면 **File (1)**을 선택하고 파일을 저장하려면 **Save (2)**를
    클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

7.  **finance_agent.py** 파일을 선택하고 아래에 제시된 코드 스니펫으로
    내용을 대체하여 persistent finance agent를 구성하세요.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_finance_agent():
>
> """Build or reuse persistent finance agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-FinanceAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a finance and reimbursement specialist."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> "specific amounts, policies, and actionable guidance.\n\n"
>
> "When provided CONTEXT, prefer it as the primary source. "
>
> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a reimbursement request\\, \\open a support
> ticket\\), output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: user@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself."
>
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are a finance and reimbursement specialist."
>
> )

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

> **Microsoft Foundry로 Persistent Agent관리:**

- AIProjectClient는 Microsoft Foundry 프로젝트에 연결되어, 스크립트가
  로컬 대신 Foundry 환경 내에 존재하는 영구 에이전트를 나열, 찾거나
  생성할 수 있게 합니다.

> **기존 에이전트의 재사용 가능성:**

- 새로운 에이전트를 생성하기 전에 이 기능은 기존의
  "엔터프라이즈-파이낸스에이전트"가 이미 존재하는지 확인합니다.

- 발견되면 Foundry가 관리하는 ID를 통해 ChatAgent를 초기화하여 배포된
  에이전트를 재사용하여 중복 배포를 방지합니다.

> **자동 에이전트 생성 (누락 시):**

- 에이전트를 찾지 못하면 Foundry에서
  project_client.agents.create_agent()를 사용해 새로운 영구 에이전트를
  생성합니다.

- 모델 배포 이름, 고유 에이전트 이름, 그리고 재무 및 환급에 초점을 맞춘
  도메인별 지침으로 등록합니다.

> **통신을 위한 AzureAIAgentClient와 통합:**

- 생성된 또는 재사용된 에이전트는 AzureAIAgentClient를 이용한
  ChatAgent에 랩핑됩니다.

- 인증, 모델 라우팅, 배포된 Foundry 에이전트와의 지속적인 통신을
  처리합니다.

8.  완료하면 **File (1)**을 선택하고 파일을 저장하려면 **Save (2)**를
    클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

9.  이제 **hr_agent.py** 파일을 선택하고 코드를 다음 코드로 교체하세요.
    이 코드는 상태 없는 채팅 에이전트를 persistent agent로 변환합니다.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_hr_agent():
>
> """Build or reuse persistent HR agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-HRAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an expert HR policy specialist."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> "When provided CONTEXT, prefer it as the primary source. "
>
> "If the user asks to create a ticket (phrases like \\create a
> ticket\\, \\submit a leave request\\, \\open a support ticket\\),
> output a structured block starting with:\n"
>
> "CREATE_TICKET\n"
>
> "Subject: \<one-line subject\>\n"
>
> "Body: \<detailed description\>\n"
>
> "Tags: tag1,tag2 (optional)\n"
>
> "Email: user@example.com (optional)\n"
>
> "Name: John Doe (optional)\n"
>
> "Return only the CREATE_TICKET block when requesting a ticket; do not
> call any APIs yourself.\n\n"
>
> "Provide specific, actionable guidance with policy references where
> applicable. "
>
> "Be empathetic and professional in your responses."
>
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an expert HR policy specialist."
>
> )

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image7.png)

> 이번 업데이트는 HR 에이전트를 Microsoft Foundry 내에서 영구적이고
> 클라우드 호스팅된 에이전트로 전환합니다. AIProjectClient를 사용해
> Foundry 프로젝트에 연결하거나, 배포된 기존 "Enterprise-HRAgent"를
> 재사용하거나, 전문 HR 도메인 지침이 포함된 새로운 프로젝트를
> 생성합니다. 배포 후에는 AzureAIAgentClient를 통해 연결된 ChatAgent에
> 감싸여, Foundry 환경 내에서 상태 기반, 재사용 가능, 중앙 관리 HR
> 자동화를 가능하게 합니다.

10. 완료하면 **File (1)**을 선택하고 파일을 저장하려면 **Save (2)**를
    클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

11. **planner_agent.py** 파일을 선택한 후, 아래에 제시된 코드 스니펫으로
    내용을 대체하여 영구 오케스트레이터를 설정하세요.

> import os
>
> import asyncio
>
> from azure.ai.projects.aio import AIProjectClient
>
> from agent_framework import ChatAgent
>
> from agent_framework.azure import AzureAIAgentClient
>
> from azure.identity.aio import AzureCliCredential
>
> async def build_planner_agent():
>
> """Build or reuse persistent planner agent"""
>
> credential = AzureCliCredential()
>
> async with AIProjectClient(
>
> endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> credential=credential
>
> ) as project_client:
>
> \# Try to find existing agent first
>
> agent_name = "Enterprise-PlannerAgent"
>
> try:
>
> agents = project_client.agents.list_agents()
>
> async for agent in agents:
>
> if agent.name == agent_name:
>
> \# Return existing persistent agent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an intelligent routing agent."
>
> )
>
> except Exception:
>
> pass \# Continue to create new agent if listing fails
>
> \# Create new persistent agent if not found
>
> created_agent = await project_client.agents.create_agent(
>
> model=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME"),
>
> name=agent_name,
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
> )
>
> )
>
> \# Return persistent agent wrapped in ChatAgent
>
> return ChatAgent(
>
> chat_client=AzureAIAgentClient(
>
> async_credential=credential,
>
> agent_id=created_agent.id,
>
> project_endpoint=os.getenv("AZURE_AI_PROJECT_ENDPOINT"),
>
> model_deployment_name=os.getenv("AZURE_AI_MODEL_DEPLOYMENT_NAME")
>
> ),
>
> instructions="You are an intelligent routing agent."
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
>
> ![A screen shot of a computer program AI-generated content may be
> incorrect.](./media/image8.png)

AIProjectClient를 통해 연결되거나, 이미 배포된 기존
"Enterprise-PlannerAgent"를 재사용하거나, HR, 재무, 컴플라이언스
카테고리로 쿼리를 분류하는 라우팅 로직을 가진 새로운 에이전트를
생성합니다.

12. 완료하면 **File (1)**을 선택하고 파일을 저장하려면 **Save (2)**를
    클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

13. 이제 루트에서 **main.py** 를 선택하고 코드를 주어진 스니펫으로
    교체하세요. 모든 코드 파일에는 Python이 들여쓰기에 민감하므로 코드를
    신중히 교체하세요.

> import asyncio
>
> import time
>
> import logging
>
> import re
>
> from typing import Dict, Any
>
> from utils.env import load_env
>
> from azure.identity.aio import AzureCliCredential
>
> from agents.planner_agent import build_planner_agent, classify_target
>
> from agents.hr_agent import build_hr_agent
>
> from agents.compliance_agent import build_compliance_agent
>
> from agents.finance_agent import build_finance_agent
>
> from tools.azure_search_tool import AzureSearchTool
>
> from tools.freshdesk_tool import FreshdeskTool
>
> \# Configure logging
>
> logging.basicConfig(level=logging.INFO, format='%(asctime)s -
> %(levelname)s - %(message)s')
>
> def parse_create_ticket_block(response_text: str) -\> Dict\[str,
> Any\]:
>
> """
>
> Parse CREATE_TICKET block from agent response.
>
> """
>
> if "CREATE_TICKET" not in response_text:
>
> return None
>
> \# Extract the CREATE_TICKET block
>
> lines = response_text.split('\n')
>
> ticket_start = -1
>
> for i, line in enumerate(lines):
>
> if line.strip() == "CREATE_TICKET":
>
> ticket_start = i
>
> break
>
> if ticket_start == -1:
>
> return None
>
> \# Parse ticket details
>
> ticket_data = {
>
> "subject": "",
>
> "body": "",
>
> "tags": \[\],
>
> "email": "system@enterprise.com",
>
> "name": "Enterprise System User"
>
> }
>
> \# Process lines after CREATE_TICKET
>
> for line in lines\[ticket_start + 1:\]:
>
> line = line.strip()
>
> if not line:
>
> continue
>
> if line.startswith("Subject:"):
>
> ticket_data\["subject"\] = line\[8:\].strip()
>
> elif line.startswith("Body:"):
>
> ticket_data\["body"\] = line\[5:\].strip()
>
> elif line.startswith("Tags:"):
>
> tags_str = line\[5:\].strip()
>
> if tags_str:
>
> ticket_data\["tags"\] = \[tag.strip() for tag in tags_str.split(',')\]
>
> elif line.startswith("Email:"):
>
> ticket_data\["email"\] = line\[6:\].strip()
>
> elif line.startswith("Name:"):
>
> ticket_data\["name"\] = line\[5:\].strip()
>
> return ticket_data
>
> async def run_multi_agent_with_user_info(query: str, agents:
> Dict\[str, Any\], user_name: str = None) -\> Dict\[str, Any\]:
>
> """
>
> Enhanced multi-agent system with CREATE_TICKET pattern support and
> user name handling.
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
> \# Step 2: Retrieve relevant context using Azure Search
>
> logging.info("Retrieving context from knowledge base...")
>
> context = await agents\["search_tool"\].search(query, top=3)
>
> \# Step 3: Create enriched prompt with context
>
> enriched_prompt = f"""
>
> Context from Knowledge Base:
>
> {context}
>
> ---
>
> User Question: {query}
>
> Please provide a comprehensive answer based on the context above. If
> no relevant context is found, provide your best guidance based on your
> training.
>
> """
>
> \# Step 4: Get agent response
>
> agent_key = target.lower()
>
> agent_name = f"{target}Agent"
>
> if agent_key in agents:
>
> logging.info(f"Processing with {agent_name}...")
>
> answer = await agents\[agent_key\].run(enriched_prompt)
>
> else:
>
> \# Fallback to HR if routing unclear
>
> logging.warning(f"Unknown target '{target}', falling back to HR")
>
> answer = await agents\["hr"\].run(enriched_prompt)
>
> target = "HR"
>
> agent_name = "HRAgent"
>
> answer_text = str(answer)
>
> \# Step 5: Check for CREATE_TICKET pattern in response
>
> ticket_info = None
>
> ticket_created = False
>
> ticket_data = parse_create_ticket_block(answer_text)
>
> if ticket_data and "freshdesk_tool" in agents:
>
> logging.info("CREATE_TICKET pattern detected - creating Freshdesk
> ticket")
>
> \# Use provided user name if available
>
> if user_name:
>
> ticket_data\["name"\] = user_name
>
> logging.info(f"Using provided user name: {user_name}")
>
> try:
>
> \# Create ticket using parsed data
>
> ticket_result = await agents\["freshdesk_tool"\].create_ticket(
>
> subject=ticket_data\["subject"\] or f"{target} Request:
> {query\[:60\]}...",
>
> description=ticket_data\["body"\] or f"Request: {query}\n\nAgent
> Response:\n{answer_text}",
>
> tags=ticket_data\["tags"\] or \[target.lower(), "agent-system"\],
>
> requester={
>
> "name": ticket_data\["name"\],
>
> "email": ticket_data\["email"\]
>
> }
>
> )
>
> if ticket_result.get("success"):
>
> ticket_info = ticket_result
>
> ticket_created = True
>
> ticket_id = ticket_result.get("ticket", {}).get("id")
>
> ticket_url = ticket_result.get("ticket", {}).get("url")
>
> \# Replace CREATE_TICKET block with success message
>
> if "CREATE_TICKET" in answer_text:
>
> \# Remove the CREATE_TICKET block and replace with success message
>
> lines = answer_text.split('\n')
>
> filtered_lines = \[\]
>
> skip_ticket_block = False
>
> for line in lines:
>
> if line.strip() == "CREATE_TICKET":
>
> skip_ticket_block = True
>
> \# Add success message with user name
>
> success_msg = f"""
>
> 🎫 \*\*Support Ticket Created Successfully\*\*
>
> - Ticket ID: \#{ticket_id}
>
> - Subject: {ticket_data\["subject"\]}
>
> - Requester: {ticket_data\["name"\]}
>
> - Status: Open
>
> - URL: {ticket_url}
>
> Your request has been submitted to our {target} team. You will receive
> updates via email.
>
> """
>
> filtered_lines.append(success_msg)
>
> continue
>
> elif skip_ticket_block and (line.startswith("Subject:") or
> line.startswith("Body:") or
>
> line.startswith("Tags:") or line.startswith("Email:") or
>
> line.startswith("Name:")):
>
> continue
>
> else:
>
> skip_ticket_block = False
>
> filtered_lines.append(line)
>
> answer_text = '\n'.join(filtered_lines)
>
> else:
>
> answer_text += f"\n\n⚠️ \*\*Note\*\*: Could not create support ticket:
> {ticket_result.get('error', 'Unknown error')}"
>
> except Exception as e:
>
> logging.error(f"Failed to create Freshdesk ticket: {e}")
>
> answer_text += f"\n\n⚠️ \*\*Note\*\*: Ticket creation failed:
> {str(e)}"
>
> \# Step 6: Process response
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
> "answer": answer_text,
>
> "context_retrieved": len(context) \> 100, \# Simple check if context
> was found
>
> "ticket_created": ticket_created,
>
> "ticket_info": ticket_info,
>
> "response_time": round(response_time, 2),
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": True,
>
> "user_name": user_name
>
> }
>
> except Exception as e:
>
> logging.error(f"Multi-agent processing error: {e}")
>
> return {
>
> "query": query,
>
> "routed_to": "Error",
>
> "agent_name": "ErrorHandler",
>
> "answer": f"I encountered an error processing your request: {str(e)}.
> Please try again.",
>
> "context_retrieved": False,
>
> "ticket_created": False,
>
> "ticket_info": None,
>
> "response_time": 0,
>
> "timestamp": time.strftime("%Y-%m-%d %H:%M:%S"),
>
> "success": False,
>
> "user_name": user_name
>
> }
>
> async def run_multi_agent(query: str, agents: Dict\[str, Any\]) -\>
> Dict\[str, Any\]:
>
> """
>
> Wrapper for multi-agent system with no user name.
>
> """
>
> return await run_multi_agent_with_user_info(query, agents, None)
>
> def format_response(result: Dict\[str, Any\]) -\> str:
>
> """Format the agent response for display."""
>
> status_icon = "✅" if result\["success"\] else "❌"
>
> context_icon = "📚" if result.get("context_retrieved") else "📭"
>
> ticket_icon = "🎫" if result.get("ticket_created") else ""
>
> formatted = f"""
>
> {status_icon} Agent Response Summary:
>
> ┌─ Routed to: {result\['routed_to'\]} ({result\['agent_name'\]})
>
> ├─ Response time: {result\['response_time'\]}s
>
> ├─ Context retrieved: {context_icon} {'Yes' if
> result.get('context_retrieved') else 'No'}
>
> ├─ Ticket created: {ticket_icon} {'Yes' if
> result.get('ticket_created') else 'No'}
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
> \# Add ticket details if available
>
> if result.get("ticket_info") and
> result\["ticket_info"\].get("success"):
>
> ticket = result\["ticket_info"\]\["ticket"\]
>
> formatted += f"""
>
> 🎫 Ticket Details:
>
> ├─ ID: \#{ticket\['id'\]}
>
> ├─ Status: {ticket\['status'\]}
>
> ├─ Priority: {ticket\['priority'\]}
>
> └─ URL: {ticket\['url'\]}
>
> """
>
> return formatted
>
> async def interactive_ticket_creation(agents: Dict\[str, Any\],
> base_query: str) -\> Dict\[str, Any\]:
>
> """
>
> Simple interactive ticket creation.
>
> """
>
> print("\n🎫 \*\*Manual Ticket Creation\*\*")
>
> print("I'll help you create a support ticket manually.\n")
>
> try:
>
> \# Get basic ticket details
>
> subject = input(f"📝 Ticket Subject: ").strip() or f"Manual Request:
> {base_query\[:60\]}..."
>
> print("\n📄 \*\*Ticket Description\*\* (press Enter twice when
> done):")
>
> description_lines = \[f"Original Request: {base_query}", ""\]
>
> while True:
>
> line = input(" ").strip()
>
> if not line:
>
> break
>
> description_lines.append(line)
>
> description = "\n".join(description_lines)
>
> \# Create the ticket directly
>
> print(f"\n🚀 Creating ticket: '{subject}'...")
>
> ticket_result = await agents\["freshdesk_tool"\].create_ticket(
>
> subject=subject,
>
> description=description,
>
> tags=\["manual", "interactive"\],
>
> requester={
>
> "name": "Enterprise System User",
>
> "email": "system@enterprise.com"
>
> }
>
> )
>
> if ticket_result.get("success"):
>
> ticket_info = ticket_result.get("ticket", {})
>
> print(f"""
>
> ✅ \*\*Ticket Created Successfully!\*\*
>
> 🎫 Ticket Details:
>
> • ID: \#{ticket_info.get('id')}
>
> • Subject: {subject}
>
> • Status: Open
>
> • URL: {ticket_info.get('url')}
>
> 📧 You will receive email updates about your ticket status.
>
> """)
>
> return {
>
> "success": True,
>
> "ticket_created": True,
>
> "ticket_info": ticket_result
>
> }
>
> else:
>
> print(f"❌ \*\*Failed to create ticket\*\*:
> {ticket_result.get('error', 'Unknown error')}")
>
> return {"success": False, "ticket_created": False}
>
> except KeyboardInterrupt:
>
> print("\n🚫 Ticket creation cancelled.")
>
> return {"success": False, "ticket_created": False}
>
> except Exception as e:
>
> print(f"❌ \*\*Error during ticket creation\*\*: {str(e)}")
>
> return {"success": False, "ticket_created": False}
>
> async def run_interactive_mode(agents: Dict\[str, Any\]):
>
> """Interactive mode for real-time queries with enhanced ticket
> creation."""
>
> print("\n🤖 Enterprise Agent System - Interactive Mode")
>
> print("Available agents: HR, Finance, Compliance")
>
> print("Type 'quit' to exit, 'help' for commands, 'ticket' for
> interactive ticket creation\n")
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
> - Ask any question about HR, Finance, or Compliance
>
> - 'ticket' - Interactive ticket creation mode
>
> - 'quit' or 'exit' - Exit the system
>
> - 'help' - Show this help message
>
> 🎯 Example questions:
>
> - "What's the travel reimbursement limit for meals?"
>
> - "I need to create a ticket for sick leave"
>
> - "Can you help me submit a reimbursement request?"
>
> - "How many vacation days do employees get?"
>
> - "Do we need GDPR compliance for EU customers?"
>
> 🎫 Ticket Creation:
>
> - Use 'ticket' command for guided ticket creation
>
> - Or include phrases like "create ticket", "submit request" in your
> question
>
> - For LEAVE and REIMBURSEMENT requests, you'll be prompted for your
> name
>
> """)
>
> continue
>
> elif query.lower() == 'ticket':
>
> if "freshdesk_tool" not in agents:
>
> print("❌ Ticket creation is not available (Freshdesk tool not
> configured)")
>
> continue
>
> base_query = input("📝 Describe what you need help with: ").strip()
>
> if base_query:
>
> await interactive_ticket_creation(agents, base_query)
>
> continue
>
> elif not query:
>
> continue
>
> \# Check if this is a leave or reimbursement request that needs user
> name
>
> query_lower = query.lower()
>
> is_leave_request = any(word in query_lower for word in \["leave",
> "vacation", "sick", "time off", "pto", "holiday"\])
>
> is_reimbursement_request = any(word in query_lower for word in
> \["reimburse", "expense", "travel", "receipt", "reimbursement"\])
>
> wants_ticket = any(keyword in query_lower for keyword in \["create
> ticket", "submit ticket", "file ticket", "raise ticket",
>
> "create request", "submit request", "file request", "need help with",
>
> "open ticket", "new ticket", "support ticket", "help ticket"\])
>
> user_name = None
>
> if (is_leave_request or is_reimbursement_request) and wants_ticket:
>
> print("\n👤 For leave and reimbursement requests, I need to collect
> some information:")
>
> user_name = input("Please enter your name: ").strip()
>
> if not user_name:
>
> print("❌ Name is required for this type of request. Please try
> again.")
>
> continue
>
> print(f"✅ Thank you, {user_name}! Processing your request...")
>
> print("\n🤔 Processing your query...")
>
> result = await run_multi_agent_with_user_info(query, agents,
> user_name)
>
> print(format_response(result))
>
> print() \# Add spacing between queries
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
> print()
>
> async def run_batch_tests(agents: Dict\[str, Any\]):
>
> """Run focused test with ticket creation."""
>
> test_queries = \[
>
> "What is the company's policy on remote work and flexible hours?"
>
> \]
>
> print("🧪 Running focused batch tests with grounded data
> integration...\n")
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
> await asyncio.sleep(1.0) \# Longer delay for tool operations
>
> async def main():
>
> """Main application entry point with enhanced features and tool
> integration."""
>
> print("🚀 Initializing Enterprise Agent System with Persistent Azure
> AI Foundry Agents...")
>
> try:
>
> \# Load environment and build persistent agents
>
> load_env()
>
> logging.info("Building persistent Azure AI Foundry agent network...")
>
> \# Build core persistent agents using Azure AI Foundry
>
> planner_agent_client = await build_planner_agent()
>
> hr_agent_client = await build_hr_agent()
>
> compliance_agent_client = await build_compliance_agent()
>
> finance_agent_client = await build_finance_agent()
>
> async with (
>
> planner_agent_client as planner_agent,
>
> hr_agent_client as hr_agent,
>
> compliance_agent_client as compliance_agent,
>
> finance_agent_client as finance_agent
>
> ):
>
> agents = {
>
> "planner": planner_agent,
>
> "hr": hr_agent,
>
> "compliance": compliance_agent,
>
> "finance": finance_agent
>
> }
>
> \# Initialize and attach tools
>
> logging.info("Initializing tools...")
>
> try:
>
> search_tool = AzureSearchTool()
>
> agents\["search_tool"\] = search_tool
>
> \# Test search tool
>
> health = await search_tool.health_check()
>
> if health\["status"\] == "healthy":
>
> logging.info("✅ Azure Search tool initialized successfully")
>
> else:
>
> logging.warning(f"⚠️ Azure Search tool health check failed: {health}")
>
> except Exception as e:
>
> logging.error(f"Failed to initialize Azure Search tool: {e}")
>
> \# Create mock search tool for testing
>
> class MockSearchTool:
>
> async def search(self, query, top=3):
>
> return f"📭 Mock search results for: {query}\n(Azure Search tool not
> configured)"
>
> agents\["search_tool"\] = MockSearchTool()
>
> \# Initialize Freshdesk tool for ticket creation
>
> try:
>
> freshdesk_tool = FreshdeskTool()
>
> agents\["freshdesk_tool"\] = freshdesk_tool
>
> logging.info("✅ Freshdesk tool initialized successfully")
>
> except Exception as e:
>
> logging.warning(f"⚠️ Freshdesk tool initialization failed: {e}")
>
> \# System will work without Freshdesk, just won't create tickets
>
> logging.info("✅ All Azure AI Foundry agents and tools initialized")
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
> logging.error(f"Azure AI Foundry agent system initialization failed:
> {e}")
>
> print(f"❌ Failed to start Azure AI Foundry system: {e}")
>
> \# Try to run with minimal configuration
>
> logging.info("Attempting to run with minimal Azure AI Foundry
> configuration...")
>
> try:
>
> planner_agent_client = await build_planner_agent()
>
> hr_agent_client = await build_hr_agent()
>
> compliance_agent_client = await build_compliance_agent()
>
> finance_agent_client = await build_finance_agent()
>
> async with (
>
> planner_agent_client as planner_agent,
>
> hr_agent_client as hr_agent,
>
> compliance_agent_client as compliance_agent,
>
> finance_agent_client as finance_agent
>
> ):
>
> minimal_agents = {
>
> "planner": planner_agent,
>
> "hr": hr_agent,
>
> "compliance": compliance_agent,
>
> "finance": finance_agent,
>
> "search_tool": type('MockSearch', (), {'search': lambda self, q,
> top=3: f"Mock search for: {q}"})()
>
> }
>
> await run_batch_tests(minimal_agents)
>
> except Exception as minimal_error:
>
> print(f"❌ Even minimal Azure AI Foundry configuration failed:
> {minimal_error}")
>
> if \_\_name\_\_ == "\_\_main\_\_":
>
> asyncio.run(main())

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image9.png)

14. 완료하면 **File (1)**을 선택하고 파일을 저장하려면 **Save (2)**를
    클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image5.png)

15. 에이전트들은 영속성 구성으로 성공적으로 업데이트됩니다. 이제
    Microsoft Foundry 포털에서 에이전트 생성 여부를 확인하기 위해
    에이전트를 실행하세요.

16. 매뉴를 확장하려면 상단 메뉴에서 **... (1)** 옵션을 선택하세요.
    **Terminal (2)**를 선택하고 **New Terminal (3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

17. **VS Code** Terminal에서 Azure CLI 로그인 명령어를 실행하세요:

+++az login+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image11.png)

18. **Sign in** 창에서 **Work or school account**를 선택하고
    **Continue**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

19. **Sign into Microsoft** 탭에서 다음 자격 증명으로 로그인하세요.

- 사용자 이름 - <+++@lab.CloudPortalCredential(User1).Username>+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

20. 로그인 옵션이 나오면 **No, this app only**를 선택해 다른 데스크톱
    앱을 연결하지 않고 계속 진행하세요.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image13.png)

21. 구독과 테넌트를 선택하라는 메시지가 나오면 **1**번 입력 하고
    **Enter** 버튼을 눌러 계속하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

22. 검색 도구의 작동 방식을 시험하려면 아래 명령어를 실행하세요.

+++python main.py+++

![](./media/image15.png)

23. 이전에 열렸던 Azure 포털을 열고, 리소스 그룹으로 이동한 후 리소스
    목록에서 **agent- **AI foundry 리소스를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

24. 다음 창에서 **Go to Foundry portal**을 클릭하세요. 이제 Microsoft
    Foundry 포털로 이동하게 됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

25. Microsoft Foundry 포털로 이동하면 왼쪽 메뉴에서 **Agents (1)**를
    선택하세요. 모든 에이전트가 Microsoft Foundry 포털에 등록된 것을 볼
    수 있습니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

> Microsoft Agent Framework 사용의 일환으로, 에이전트들은 로컬 또는
> 클라우드 호스팅 환경 내에서 작동하도록 설계되었으며, Microsoft Foundry
> 포털 UI가 아닌 SDK를 통해 프로그래밍적으로 관리됩니다. 배포 후 이
> 에이전트들은 Foundry가 관리하는 환경 내에서 지속되며 서비스로 계속
> 실행됩니다.
>
> 다음 연습 이후부터는 로컬에서 관찰 가능성, 모니터링, 추적 기능을
> 구성하는 작업을 계속하게 되며, 이를 통해 클라우드에서 작동하는
> 에이전트들의 동작을 시각화, 분석, 관리할 수 있습니다.

**요약**

이 실습에서 로컬에서 구축한 멀티 에이전트 시스템을 Microsoft Foundry
Agent Service에 성공적으로 배포했습니다.

이 실습을 성공적으로 완료했습니다. 계속하려면 Next \>\>를 클릭하세요.
