# 실습 9: Azure AI 프레임워크를 사용하여 단일 및 멀티 에이전트 워크플로우를 구현하기

**예상 소요 시간**: 45분

**개요**

당신은 Contoso Ltd.의 AI 엔지니어로, Azure AI 프레임워크를 활용해 지능형
에이전트 워크플로우를 개발하는 임무를 맡고 있습니다. 이 실험실에서는
MCP를 사용하는 외부 도구와 통합되는 단일 에이전트 시스템을 생성하고,
여러 전문 에이전트가 사용자 의도에 따라 동적으로 협업하거나 작업을
인수하는 다중 에이전트 워크플로우를 설계할 것입니다.

실습 목표

이 실습에서 다음과 같은 작업을 수행할 것입니다.

- 작업 1: Azure OpenAI 채팅 에이전트를 구축 및 테스트하기

- 작업 2: 도구 통합을 통한 단일 에이전트 워크플로우 생성하기

- 작업 3: 다중 에이전트 워크플로우 설계

  - 작업 3.1: 다중 에이전트 워크플로우를 조정하기

  - 작업 3.2: 핸드오프 패턴 다중 에이전트 시스템

## 작업 0: 실습 환경 설정

1.  C:\Labfiles\Day 2에서 **OpenAIWorkshop-Framework** 파일을
    추출하세요.

2.  LabVM 데스크톱에서 **Visual Studio Code**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

3.  **File** **(1)**을 선택하고 **OpenAIWorkshop-Framework** 폴더를
    열려면 **Open Folder** **(2)**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

4.  C:\Labfiles\Day 2\\**OpenAIWorkshop-Framework** 경로로 이동하고
    **OpenAIWorkshop-Framework**를 선택하고 **Select Folder**를
    클릭하세요.

5.  **Yes, I trust the authors**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

6.  **ellipsis(...)** **(1)**을 클릭하고 **Terminal** **(2)**를 클릭하고
    **New Terminal** **(3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  아래 명령을 입력하면 **applications **디렉터리로 이동하고
    **requirements.txt** 파일에서 필요한 모든 의존성을 설치하세요.

> cd agentic_ai/applications
>
> pip install -r requirements.txt

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image5.png)

8.  명령은 완료하는 데 5분에서 10분 정도 걸릴 수 있습니다.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

## 작업 1: Azure OpenAI 채팅 에이전트를 구축 및 테스트하기

이 작업에서는 Visual Studio Code로 간단한 Azure OpenAI 채팅 에이전트를
구축하고 테스트합니다. 환경 변수를 설정하고, 에이전트를 배포된 모델에
연결하며, 다양한 프롬프트에 따라 동적 응답을 생성하는 방식을 관찰할
것입니다.

1.  **Visual Studio Code**로 이동하세요.

2.  pip install -r 명령어가 성공적으로 완료되었는지 requirements.txt
    확인하세요. 아직 실행 중이라면 끝날 때까지 기다리세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image6.png)

3.  **Explorer**에서 **agentic_ai** **(1)
    \>** **applications** **(2)**를 확장하세요. .env.sample **(3)**을
    우클릭하고 **Rename** **(4)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

4.  파일 이름을 .env로 바꾸고 클릭하면 파일을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

5.  AZURE_OPENAI_API_KEY **(1)**와 AZURE_OPENAI_ENDPOINT **(2)** 값을
    실제 값으로 대체하세요. Microsoft Foundry **Overview** 페이지에서
    가져오세요.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

6.  AZURE_OPENAI_CHAT_DEPLOYMENT를 **gpt-4o-mini (3)**로 추가하세요

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image10.png)

7.  **File (1)**을 선택하고 **Save(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

8.  **application** **(1)** 폴더를 우클릭하고 **New file** **(2)**을
    통해 간단한 에이전트를 구성할 새 파일을 생성하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

9.  에이전트 파일을 +++simple_agent_test.py+++로 정하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

10. 다음 코드를 복사해서 파일에 붙여넣으세요.

> import asyncio
>
> import os
>
> from dotenv import load_dotenv
>
> from agent_framework.azure import AzureOpenAIChatClient
>
> from azure.identity import AzureCliCredential
>
> \# Load .env file (same folder or specify full path)
>
> load_dotenv(dotenv_path=".env")
>
> \# Retrieve values from .env
>
> endpoint = os.getenv("AZURE_OPENAI_ENDPOINT")
>
> deployment_name = os.getenv("AZURE_OPENAI_CHAT_DEPLOYMENT")
>
> api_version = os.getenv("AZURE_OPENAI_API_VERSION")
>
> print("Using Azure OpenAI endpoint:", endpoint)
>
> print("Deployment name:", deployment_name)
>
> print("API version:", api_version)
>
> \# ✅ Correct parameter name is deployment_name (not deployment)
>
> agent = AzureOpenAIChatClient(
>
> api_key=os.getenv("AZURE_OPENAI_API_KEY"),
>
> endpoint=endpoint,
>
> deployment_name=deployment_name,
>
> api_version=api_version
>
> ).create_agent(
>
> instructions="You are a helpful and funny assistant who tells short
> jokes.",
>
> name="Joker"
>
> )
>
> async def main():
>
> result = await agent.run("Tell me a joke about the cloud.")
>
> print("\nAgent response:\n", result.text)
>
> asyncio.run(main())

![A computer screen shot of a program AI-generated content may be
incorrect.](./media/image14.png)

11. **File** **(1)**을 선택하고 **Save** **(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

12. simple_agent_test.py **(1)**을 우클릭하고 **Open in Integrated
    Terminal** **(2)**를 선택하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image15.png)

13. 아래에 제시된 명령어를 실행하여 에이전트를 실행하고 출력을 관찰하여
    에이전트가 어떻게 작동하는지 이해하세요.

+++python simple_agent_test.py+++

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image16.png)

14. 에이전트가 어떻게 반응하는지 관찰하기 위해 명령어를 수정해 봅시다.
    Tell my a joke about the Earth'(**1)** (31행)로 안내한 후 파일을
    **저장**하세요. 그 다음 (**2)** 아래 명령을 실행하고 에이전트의 응답
    **(3)**를 검토하세요.

+++python simple_agent_test.py+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image17.png)

15. 이는 에이전트의 반응이 제공된 지시에 따라 어떻게 달라지는지를
    보여주며, 다양한 프롬프트에 적응하는 능력을 강조합니다.

## 작업 2: 도구 통합을 통한 단일 에이전트 워크플로우 생성하기

이 작업에서는 MCP (Model Context Protocol)을 사용하는 외부 도구와
통합되는 단일 에이전트 워크플로우를 구축하고 테스트하게 됩니다. 환경
변수를 설정하고, MCP 서버, 백엔드, 프론트엔드를 로컬에서 실행하며,
에이전트가 MCP 도구를 활용해 사용자 쿼리를 처리하고 지능적이고 맥락 인식
있는 응답을 어떻게 전달하는지 관찰하게 됩니다.

1.  Visual Studio Code에서 **agents (1) \> agent_framework (2) \>
    single_agent (3)**을 펼치고 MCPStreamableHTTPTool 도구가 통합된 단일
    에이전트 워크플로우를 확인하세요 **(4)**.

    - MCPStreamableHTTPTool은 에이전트가 MCP 서버를 통해 외부 HTTP 기반
      서비스를 호출하고 도구 출력을 대화에 포함시킬 수 있게 합니다.

    - ChatAgent로 전달되어 지시와 사용자 안내에 따라 자동으로
      사용되었습니다

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image18.png)

2.  코드를 살펴보면서 어떻게 통합되는지 이해하세요:

    - In the \_maybe_create_tools method:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- 이렇게 하면 MCP 서버에 연결된 스트리밍 가능한 HTTP 도구가 생성됩니다.

- 이 시스템은 에이전트가 워크플로우의 일부로 외부 서비스(MCP를 통해)에
  HTTP 호출을 할 수 있게 합니다.

&nbsp;

- 초기화 시 이 도구는 ChatAgent에 전달됩니다:

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

- The agent can then use this tool whenever a user prompt triggers a
  tool call.

- Streaming support with WebSocket: When a tool/function is called
  during a streamed conversation, it broadcasts the tool name and turn
  via \_chat_async_streaming.

3.  .env 파일 **(1)**로 이동하고 Environment 변수를variable to
    your .env 파일로 추가하고 **(2)**를 실행하려면 **Single agent
    workflow**를 할당하세요:

+++AGENT_MODULE=agents.agent_framework.single_agent+++

- DISABLE_AUTH=true **(3)** 환경 변수를 추가하면 애플리케이션 내 인증을
  비활성화하는 데 사용됩니다. 이는 현지에서 더 쉽게 개발하고 테스트할 수
  있게 해줍니다.

> +++DISABLE_AUTH=true+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

4.  **File (1)**을 선택하고 **Save(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

5.  이제 **MCP server, backend,** 및 **React frontend**를 시작해 전체
    에이전트 환경을 로컬에서 실행하고, UI가 에이전트 및 도구와
    상호작용할 수 있게 됩니다.

6.  Visual Studio Code 창에서 **ellipses (...) (1)**을 클릭하고
    **Terminal (2)**를 클릭하고 **New Terminal (3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

7.  이전 단계가 완료될 때까지 기다렸다가 다음 단계로 넘어가세요.

8.  **MCP 서버 시작 (터미널 1)**: (MCP 디렉터리는 프로젝트 루트 레벨에
    있습니다)

    - 아래 명령을 실행해 **MCP server**를 실행하면, 에이전트가 도구로
      호출할 수 있는 API를 노출합니다. 서버는
      <http://localhost:8000>에서 실행됩니다)

> cd mcp
>
> uv run python mcp_service.py
>
> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image21.png)
>
> 참고: 오류가 발생하면 아래에 주어진 명령어를 실행하세요:

+++pip install uv+++

+++uv run python mcp_service.py+++

9.  명령어를 실행하고 새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **Backend 시작 (터미널 2)**:

    - 아래 명령을 실행해 에이전트 워크플로우, 세션 관리, API
      엔드포인트를 호스팅하는 백엔드 서버를 시작하세요.

> cd agentic_ai/applications
>
> uv run python backend.py

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image22.png)

- 지역 운영: [http://localhost:7000](http://localhost:7000/).

- 이것이 프론트엔드가 통신하는 핵심 애플리케이션 로직입니다.
  **Connection is open**있는지 확인하세요.

> ![A screen shot of a computer AI-generated content may be
> incorrect.](./media/image23.png)

11. 명령어를 실행하고, 새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

12. **React Frontend 시작 (터미널 3)**:

    - 아래에 제시된 명령어를 입력하여 react-frontend 디렉터리로
      이동하세요.

> +++cd agentic_ai/applications/react-frontend+++

- 아래 명령어를 입력하면 에이전트 UI의 **React frontend**를 실행하세요.
  에이전트와 실시간으로 상호작용하고 그들의 응답을 확인할 수 있는 사용자
  인터페이스를 제공합니다.

> +++npm start+++

- 컴필링에는 시간이 좀 걸릴 수 있습니다. 경고는 무시하고 완료될 때까지
  기다리세요. Once the **webpack compiled successfully** 표시되면
  에이전트 애플리케이션은 <http://localhost:3000>에 로컬로 실행합니다.

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image24.png)

13. 세 터미널이 모두 실행되면 에이전트 애플리케이션이 브라우저에서
    실행되며, 이를 통해 에이전트와 상호작용하고 기능을 테스트할 수
    있습니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

**참고**: 세 터미널 모두 작동 중인지 확인하세요. 만약 그 중 누군가가
멈추면, 해당 명령을 다시 실행하세요. 세 가지 모두 활성화되어 있지 않으면
연결 오류가 발생할 수 있습니다.

14. 채팅 **(1)**에 다음 프롬프트를 보내고 **(2)**를 보세요:

+++Customer 251, what's my billing summary?+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

**참고**: 세 터미널 모두 작동 중인지 확인하세요. 만약 그 중 누군가가
멈추면, 해당 명령을 다시 실행하세요. 세 가지 모두 활성화되어 있지 않으면
연결 오류가 발생할 수 있습니다.

15. 출력을 보기: 프롬프트를 해석한 것은 ChatAgent(self.\_agent)였으며,
    아마 도 **MCP tool**라고 불렸을 가능성이 있으며 출력을 생성했습니다.

    - 에이전트는 귀하의 요청을 **Customer 251**에 대한 청구 문의로
      해석했습니다.

    - **MCP** **tool**을 사용해 구조화된 청구 데이터를 가져왔습니다.

    - 에이전트는 의도한 대로 작동하며, 도구 출력과 AI 추론을 동적으로
      통합하여 사용자 특화된 질문에 답합니다.

16. 테스트가 끝난 후에는 VS Code로 돌아가 실행 중인 모든 터미널 세션을
    종료하세요. 이로 인해 다가오는 다중 에이전트 워크플로우가 간섭 없이
    실행되도록 보장합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

## 작업 3: 다중 에이전트 워크플로우 설계

이 작업에서는 다양한 조정 패턴을 보여주는 고급 다중 에이전트
워크플로우를 설계하고 구현하게 됩니다. 중앙 관리자를 통해 여러 전문
에이전트를 조율하여 복잡한 쿼리를 협력적으로 처리하는 것부터 시작하고,
사용자 의도에 따라 도메인별 에이전트 간에 제어권이 동적으로 이동하는
핸드오프 기반 시스템을 탐색하게 됩니다.

### 작업 3.1: 다중 에이전트 워크플로우를 조정하기

이 작업에서는 중앙 조정자가 여러 전문 에이전트를 조율하여 복잡한 사용자
쿼리를 협력적으로 처리하고 정확하고 도구 기반 응답을 생성하는 다중
에이전트 워크플로우를 조율하게 됩니다.

1.  **agent (1) \> agent_framework (2) \> multi_agent (3) \>
    magentic_group (4)**로 이동하고 코드를 보세요 **(5)**.

    - 이 코드는 **multi-agent orchestration** 프레임워크를 대표하는데,
      이는 중앙 조정자의 지도 아래 여러 전문 에이전트가 협력하는
      시스템을 정의하기 때문입니다.

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image28.png)

- \_create_participants 여러 전문 에이전트(CRM/청구, 제품/프로모션,
  보안/인증)를 초기화합니다.

- 각 에이전트는 가집니다:

  - 특정 도메인과 도구 세트가 있습니다.

  - 오직 오케스트레이터와만 통신하고, 사용자와는 직접 통신하지 않습니다.

  - 사실적이고 도구에 기반한 답변을 제공합니다.

- 다음은 이 멀티 에이전트 워크플로우에서 사용되는 에이전트들입니다

  - **CRM 및 청구 에이전트** – 사실에 기반한 도구를 기반으로 고객 계정,
    구독, 청구, 청구서, 결제 및 관련 문의를 처리합니다.

  - **제품 및 프로모션 에이전트** – 구조화된 출처를 사용하여 제품
    가용성, 프로모션, 할인, 자격 및 조건을 제공합니다.

  - **보안 및 인증 에이전트** – 보안 사고, 인증 문제, 계정 잠금 및 위험
    완화 지침을 로그와 도구를 활용해 관리합니다.

2.  .env 파일 **(1)**로 이동해 단일 에이전트 변수**(2)**를 주석 처리 한
    뒤, 아래 명령어를 입력해 **Orchestrating Multi-Agent** 변수**(3)**을
    추가하세요.

+++AGENT_MODULE=agents.agent_framework.multi_agent.magentic_group+++

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image29.png)

3.  **File (1)**을 선택하고 **Save(2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  이제 단계를 따라 세 가지 핵심 구성 요소를 시작하여 전체 에이전트
    애플리케이션을 실행하세요:

5.  Visual Studio Code 창에서 **ellipses (...) (1)**을 클릭하고
    **Terminal (2)**를 클릭하고 **New Terminal (3)**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **MCP 서버 시작 (터미널 1)**: (MCP 디렉터리는 프로젝트 루트 레벨에
    있습니다)

    - 아래 명령을 실행해 **MCP server**를 실행하면, 에이전트가 도구로
      호출할 수 있는 API를 노출합니다. (서버는
      <http://localhost:8000>에서 실행됩니다)

> cd mcp
>
> uv run python mcp_service.py

7.  명령어를 실행하고 새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Backend 시작 (터미널 2)**:

    - 아래 명령을 실행해 에이전트 워크플로우, 세션 관리, API
      엔드포인트를 호스팅하는 백엔드 서버를 시작하세요.

> cd agentic_ai/applications
>
> uv run python backend.py

- 이것이 프론트엔드가 통신하는 핵심 애플리케이션 로직입니다.
  ** Connection is open** 있는지 확인하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  명령어를 실행하고 새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **React Frontend 시작 (터미널 3)**:

    - 아래에 제시된 명령어를 입력하여 react-frontend 디렉터리로
      이동하세요.

> +++cd agentic_ai/applications/react-frontend+++

- 아래 명령어를 입력하면 에이전트 UI의 **React frontend**를 실행하세요.
  에이전트와 실시간으로 상호작용하고 그들의 응답을 확인할 수 있는 사용자
  인터페이스를 제공합니다.

> +++npm start+++

- **webpack compiled successfully** 표시되면, 에이전트 애플리케이션은
  로컬에서 실행됩니다: [http://localhost:3000](http://localhost:3000/).

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 아래 프롬프트를 채팅창에 보내고 왼쪽 창에서 답변을 확인하세요:

+++Customer 251, what's my billing summary?+++

12. 오케스트레이터는 관리자나 라우터와 같습니다. 사용자 쿼리를 읽고 어떤
    전문 에이전트가 처리할지 결정합니다. 이 결정은 “billing”,
    “promotion”, “login” 같은 맥락과 키워드를 사용합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

13. 오케스트레이터는 작업을 도메인 에이전트에게 할당합니다.
    오케스트레이터는 쿼리를 이러한 내부 에이전트 중 하나로 전송합니다:

    - crm_billing – billing, invoices, payments

    - product_promotions – products, discounts, offers

    - security_authentication – security, login, account lockouts

14. 문의하신 "청구 요약"에 대해, 오케스트레이터가 **crm_billing**로
    라우팅합니다 .

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

- 도메인 에이전트는 연결된 도구를 사용합니다. 각 에이전트는 MCP 서버를
  통해 특정 도구(API)에 접근할 수 있습니다.

- 예를 들어, crm_billing get_customer_detail, get_billing_summary,
  get_invoice_payment 등을 호출할 수 있습니다.

- 에이전트는 적절한 도구를 호출하고, 구조화된 데이터를 가져오며, 사실에
  기반한 응답을 작성합니다.

15. 테스트가 끝난 후에는 VS Code로 돌아가 실행 중인 모든 터미널 세션을
    종료하세요. 이로 인해 다가오는 다중 에이전트 워크플로우가 간섭 없이
    실행되도록 보장합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

### 작업 3.2: 핸드오프 패턴 다중 에이전트 시스템

이 작업에서는 사용자 의도에 따라 청구, 프로모션, 보안 등 전문 상담원 간
대화가 원활하게 전환되어, 도메인 간 매끄럽고 상황 인식 있는 상호작용을
보장하는 핸드오프 기반 다중 에이전트 시스템을 탐구하게 됩니다.

- **작동 방법**

  - 사용자는 도메인 에이전트와 직접 상호작용합니다 — 예: CRM 및 청구
    에이전트.

  - 의도 분류기는 사용자의 새 메시지가 다른 도메인(예: 프로모션이나
    보안)에 속하는지 확인합니다.

  - 만약 그렇다면 시스템은 자동으로 대화를 적절한 전문 상담원에게
    전달("핸드오프")합니다.

  - 각 에이전트는 해당 도메인(청구, 프로모션, 보안)과 관련된 필터링된
    도구를 가지고 있습니다.

  - 인수인계는 원활하게 진행되며, 새로운 상담원이 대화 기록을 이해할 수
    있도록 문맥 전달이 이루어집니다.

1.  **agents (1) \> agent_framework (2) \> multi_agent (3) \>
    handoff_multi_domain_agent (4)**를 확장하고  Code **(5)**를 보세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

2.  .env 파일 **(1)**로 이동하고, Orchestrating Multi-Agent 변수
    **(2)**를 주석 처리 하고 아래 명령어를 입력하여 **Handoff Pattern
    Multi-Agent System** 변수 **(3)**을 추가하세요.

+++AGENT_MODULE=agents.agent_framework.multi_agent.handoff_multi_domain_agent+++

- 아래에 제시된 명령을 입력하여 핸드오프 중 이전 대화 컨텍스트가 얼마나
  전달되는지 제어합니다. -1은 이전 대화 턴을 모두 전송하는 것을
  의미합니다 **(4)**.

> +++HANDOFF_CONTEXT_TRANSFER_TURNS=-1+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

3.  **File (1)**을 선택하고 **Save (2)**를 클릭하세요.

![A screenshot of a computer menu AI-generated content may be
incorrect.](./media/image11.png)

4.  이제 단계를 따라 세 가지 핵심 구성 요소를 시작하여 전체 에이전트
    애플리케이션을 실행하세요:

5.  Visual Studio Code 창에서 **ellipses (...) (1)**을 클릭하고
    **Terminal (2)**를 클릭하고 **New Terminal (3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **MCP 서버 시작 (터미널 1)**: (MCP 디렉터리는 프로젝트 루트 레벨에
    있습니다)

    - 아래 명령을 실행해 **MCP server**를 실행하면, 에이전트가 도구로
      호출할 수 있는 API를 노출합니다. (서버는 계속 작동합니다
      [http://localhost:8000](http://localhost:8000/))

> cd mcp
>
> uv run python mcp_service.py

7.  명령어를 실행하고 새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

8.  **Backend 시작 (터미널 2)**:

    - 아래 명령을 실행해 에이전트 워크플로우, 세션 관리, API
      엔드포인트를 호스팅하는 백엔드 서버를 시작하세요.

> cd agentic_ai/applications
>
> uv run python backend.py

- 이것이 프론트엔드가 통신하는 핵심 애플리케이션 로직입니다.
  **Connection is open** 있는지 확인하세요.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image23.png)

9.  명령어를 실행하고 새 터미널을 여세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

10. **React Frontend 시작 (터미널 3)**:

    - 아래에 제시된 명령어를 입력하여 react-frontend 디렉터리로
      이동하세요.

> +++cd agentic_ai/applications/react-frontend+++

- 아래 명령어를 입력하면 에이전트 UI의 **React frontend**를 실행하세요.
  에이전트와 실시간으로 상호작용하고 그들의 응답을 확인할 수 있는 사용자
  인터페이스를 제공합니다.

> +++npm start+++

- **webpack compiled successfully**이 표시되면, 에이전트 애플리케이션은
  로컬에서 실행됩니다: <http://localhost:3000>

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

11. 아래 프롬프트를 채팅창에 보내고 왼쪽 창에서 답변을 확인하세요:

+++Customer 251, what's my billing summary?+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

- 여기서 의도 분류기는 crm_billing도메인으로 라우팅됩니다

- get_billing_summary 도구는 고객 251을 위해 호출됩니다

12. 청구 관련 계속 문의를 위해 다음과 같은 문의를 할 수 있습니다:

+++Yes, I would like to view the invoice details+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image35.png)

**참고**: 인보이스 정보를 확인할 수 없어서 답변이 오신다면, 참고된
번호가 청구서 ID가 아니라 청구서 ID입니다. 구독 ID를 제공해 주시거나,
특정 청구서에 대한 자세한 정보가 필요하시면 제가 정확히 도와드릴 수
있습니다. 다음 프롬프트를 제공하세요.

+++Yes, I would like to view the invoice details for customer 251+++

13. 이제 다른 도메인과 관련된 쿼리를 시도해 보며 핸드오프가 어떻게
    작동하는지 테스트해 봅시다.

14. 제품 및 프로모션과 관련된 다음 쿼리를 입력하고 응답을 확인하세요.

+++Are there any promotions available for my subscription plan+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image36.png)

- 이전 대화는 CRM 및 청구 전문가가 처리했기 때문에, 시스템은 도메인
  변경을 감지합니다. 그 대화를 제품 및 프로모션 전문가에게 넘기기로
  결정합니다.

- 시스템은 HANDOFF_CONTEXT_TRANSFER_TURNS 설정에 따라 이전 대화 맥락(예:
  우리가 논의하는 고객)을 새 에이전트에게 선택적으로 전송할 수 있습니다.

- 제품 및 프로모션 전문가는 프로모션, 계획, 제품 정보 (예:
  get_promotions, get_eligible_promotions)와 관련된 도구에만 접근할 수
  있습니다.

15. 테스트가 끝난 후에는 VS Code로 돌아가 실행 중인 모든 터미널 세션을
    종료하세요. 이로 인해 다가오는 다중 에이전트 워크플로우가 간섭 없이
    실행되도록 보장합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

**요약**

이 실습네서는 MCP를 활용해 외부 도구와 통합되는 단일 에이전트
워크플로우를 생성했고, 여러 전문 에이전트가 사용자 의도에 따라
협업하거나 대화를 넘기는 다중 에이전트 설계를 탐구했습니다. 환경 변수를
설정하고, 전체 에이전트 환경을 실행하며, 에이전트가 도메인 특정 쿼리에
지능적으로 어떻게 반응하는지 테스트했습니다.
