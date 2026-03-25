# Microsoft Foundry 및 에이전트 프레임워크를 활용한 확장 가능한 AI 에이전트 설계하기

**개요**

이 실습에서는 3일간 진행되어 Microsoft Foundry와 Microsoft Agent
Framework를 활용한 확장 가능한 AI 에이전트를 설계하고 구축했습니다.
참가자들은 Microsoft Foundry 포털을 통해 첫 AI 에이전트를 생성하는
것부터 시작하며, 기업 정책 문서를 업로드하고 Azure AI Search에 입력하여
검색 가능한 지식 베이스를 준비하는 방법을 배웁니다. 워크숍은 이후
Microsoft Agent Framework SDK를 사용하여 다중 에이전트 시스템 구축으로
진행되며, 여러 전문 에이전트가 Agent-to-Agent (A2A) 통신 패턴을 통해
협력합니다. 학습자들은 Model Context Protocol (MCP)을 사용하여 외부
도구와 데이터 소스를 통합하여 에이전트 역량을 확장하며, 지식 검색을 위한
Azure AI Search와 티켓 관리를 위한 Freshdesk와 같은 외부 API와
연결됩니다. 교육은 에이전트를 Microsoft Foundry Agent Service에 배포하는
단계로 발전하며, 상태 관리와 기업급 신뢰성을 갖춘 지속적인 클라우드
호스팅 솔루션으로 구현합니다. 마지막으로, 참가자들은 중앙 조정이
이루어진 오케스트레이션 다중 에이전트 시스템과 사용자 의도와 도메인
전문성에 따라 전문 에이전트 간 대화가 원활하게 전환되는 핸드오프 기반
시스템 등 고급 워크플로우 패턴을 구현할 예정입니다.

**목표**

이 실습이 끝날 때 다음을 수행할 수 있습니다:

- **AI 프로젝트를 설정하고 VS Code에서 채팅 완료를 수행:** Microsoft
  Foundry 프로젝트를 생성하고, GPT-4 및 모델을 임베딩하며, Visual Studio
  Code와 안전한 연결을 구축하여 생산성 준비가 된 AI 개발 환경을
  구성하세요. 채팅 완료 호출을 실행하여 설정 검증을 수행하여, 로컬 개발
  환경과 Azure AI 서비스 간의 원활한 통합 및 적절한 인증 및 프로젝트
  구성을 보장합니다.

- **건강 보험 플랜 분석기 AI 에이전트 구축:** 건강보험 데이터를 분석하고
  시각화하는 지능형 AI 에이전트를 개발하세요. 복잡한 건강보험 플랜
  정보를 처리하고 비교 막대 차트를 자동으로 생성하는 에이전트를 생성하여
  데이터 해석, 자연어 이해, 코드 실행, 의사결정 지원을 위한 자동 시각화
  생성 등 핵심 AI 에이전트 기능을 시연합니다.

- **다중 에이전트 협업 시스템 개발:** 전문 AI 에이전트들이 협력하여 건강
  보험 문서를 분석하고 포괄적인 보고서를 생성하는 첨단 다중 에이전트
  아키텍처를 설계하고 구현합니다. Azure AI Search를 이용한 지능형 문서
  검색용 Search Agent, 상세한 분석 보고서를 생성하는 Report Agent, 준수
  및 정확성을 보장하는 Validation Agent, 그리고 에이전트 간 통신 및
  워크플로우 조정을 관리하는 Orchestrator Agent를 구축하여
  엔터프라이즈급 에이전트 협업 패턴을 보여줄 것입니다.

**필수 구성 요소**

참가자들은 다음 사항을 갖추어야 합니다:

- **Azure 및 클라우드 경험** – Azure Portal, Resource Groups, 및 Azure
  AI 서비스에 대한 익숙

- **프로그래밍 기술** – 기본 Python 지식 (async/await, 환경 변수, API
  호출)

- **AI 개념** – LLM, 임베딩, RAG (Retrieval-Augmented Generation), 및
  프롬프트 엔지니어링에 대한 이해

- **개발 도구** – Visual Studio Code, 터미널 사용 및 Git 숙련도

- **에이전트 프레이워크 인식** – 에이전트 아키텍처, 도구 및
  오케스트레이션 패턴에 대한 기본 지식

구성 요소 설명

- **Microsoft Foundry**: Microsoft Foundry는 엔터프라이즈 AI 에이전트를
  개발, 배포 및 관리하는 클라우드 플랫폼입니다. 관리형 에이전트 서비스
  런타임 중앙 집중식 프로젝트 관리, 애플리케이션 인사이트 모니터링을
  제공하여 에이전트 수명주기 전반에 걸쳐 엔터프라이즈급 신뢰성, 보안,
  관측 가능성을 보장합니다.

- **Microsoft Agent Framework SDK**: AutoGen과 Semantic Kernel을
  대체하는 지능형 모듈형 에이전트를 구축하는 공식 Python SDK입니다. 이
  시스템은 네이티브 Agent-to-Agent 통신, Model Context Protocol 통합,
  Microsoft Foundry 지원을 제공하여 표준화된 도구 사용과 함께 본용 생산
  준비가 된 엔터프라이즈 에이전트 시스템을 가능하게 합니다.

- **Azure AI Search**: Retrieval-Augmented Generation (검색 증강 생성)
  워크플로우를 가능하게 하는 벡터 기반 검색 엔진입니다. 이 도구는 벡터
  유사성과 키워드 검색, 의미론적 순위 부여를 통한 관련성 향상, 문서
  인덱싱 기능을 결합한 하이브리드 검색을 제공하여 에이전트가 기업 지식
  소스로부터 근거 있고 사실에 근거한 정확한 응답을 제공할 수 있도록
  보장합니다.

- **Model Context Protocol (MCP)**: 에이전트가 외부 지식과 도구에
  안전하게 접근할 수 있도록 하는 표준화된 인터페이스입니다. MCP는
  엔터프라이즈 데이터 소스, Freshdesk와 같은 외부 API, 구조화된 스키마를
  갖춘 맞춤형 도구와 연결되어 신뢰할 수 있고 감사 가능한 상호작용을
  보장하고 확장 가능한 엔터프라이즈 AI 시스템의 기반을 형성합니다.

- **채팅 응답 에이전트**: 로컬 개발 및 테스트를 위한 단일 턴, 상태 없는
  에이전트 모델입니다. 이 장치는 맥락을 유지하지 않고 독립적으로 요청을
  처리하며, 로컬 환경에서 실행되고 즉시 응답합니다. 영구 에이전트로
  프로덕션으로 나아가기 전에 코어 로직 프로토타이핑과 동작 검증에
  이상적입니다.

- **지속 에이전트**: Microsoft Foundry의 클라우드 호스팅 장기 서비스가
  대화 전반에 걸쳐 상태를 유지하는 방식입니다. MCP를 통한 외부 도구
  통합, Agent-to-Agent 협업, 내장 모니터링과 함께 엔터프라이즈 규모의
  신뢰성을 지원하여, 상태 기반 다중 턴 대화 경험이 필요한 생산
  애플리케이션의 기반을 제공합니다.

- **플래너 에이전트**: 사용자 쿼리를 분석하여 적절한 전문 에이전트로
  라우팅하는 지능형 오케스트레이터입니다. AI 추론과 키워드 휴리스틱을
  활용해 HR, 재무, 컴플라이언스 등 다양한 영역에서 쿼리를 분류하여
  최적의 업무 분배를 보장하고 중앙 조정 지점 역할을 합니다.

- **작업자 에이전트**: HR, 재무, 컴플라이언스 등 특정 분야에 전문성을
  가진 도메인 전문가들. 각 에이전트는 도메인별 지침, 전문 도구, 관련
  지식 소스를 가지고 있습니다. 이들은 A2A 통신을 통해 플래너 에이전트와
  협력하며, 복잡한 도메인 특화 문의에 대해 권위 있고 정확한 답변을
  제공합니다.

- **Azure OpenAI**: 보안 API 엔드포인트를 통해 고급 LLM에 접근할 수 있는
  엔터프라이즈급 서비스입니다. 채팅 완료, 임베딩 모델, 콘텐츠 필터링,
  준수 기능을 제공합니다. Microsoft Foundry와 원활하게 통합되어
  에이전트가 GPT-4를 활용하면서도 데이터 프라이버시와 거버넌스 통제를
  유지할 수 있습니다.

# 실습 5: Microsoft Foundry를 이용한 검색 증강 AI 에이전트 구축하기

**개요**

이 실험실에서는 Microsoft Foundry 포털을 사용해 첫 AI 에이전트를 만들어
보게 됩니다. 먼저 기업 정책 문서를 업로드하여 Azure AI Search에 입력하여
지식 베이스를 준비하는 것부터 시작해야 합니다. 그 후 Microsoft Agent
Framework를 사용하여 retrieval-augmented generation (RAG)을 활성화하도록
에이전트를 구성합니다. 마지막으로, 에이전트의 응답을 테스트하고 실행
로그를 분석하여 정보를 어떻게 검색하고 처리하는지 관찰하게 됩니다.

**실습 목표**

이 실습에서 다음과 같은 작업을 수행할 것입니다.

- 작업 1: Azure 리소스를 생성하기

- 작업 2: Microsoft Foundry에 AI 에이전트를 생성하기

- 작업 3: RAG을 위한 Azure AI Search를 연결하기

- 작업 4: 에이전트 실행 로그를 테스트 및 관찰하기

## 작업 1: Azure 리소스를 생성하기

이 작업에서는 이 실습을 수행하는 데 필요하는 모든 Azure 리소스를 생성할
것입니다.

### 적업 1.1: 스토리지 계정 생성하기

1.  다음 자격 증명을 사용하여 +++https://portal.azure.com+++로 Azure
    portal에 로그인하고 Storage account를 선택하세요.

- 사용자 이름 - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - <+++@lab.CloudPortalCredential(User1).TAP>+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image1.png)

2.  **Create**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  다음 정보를 입력하고 **Review + create**를 선택하세요. 다음 화면에서
    Create를 선택하세요.

- Storage account name - +++aistorage@lab.LabInstance.Id+++

- Preferred storage type – **Azure Blob Storage or Azure Data Lake
  Storage Gen2**를 선택하세요

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image3.png)
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image4.png)

4.  리소스가 생성하면 **Go to resource**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  **Upload**를 선택하고 새로운 컨테이너를 생성하려면 **Create new**를
    선태하세요. +++**datasets**+++로 이름을 정한 후 **OK**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

![A screenshot of a login box AI-generated content may be
incorrect.](./media/image7.png)

6.  **Browse for files**을 선택하고 **C:\Labfiles\Day 2**에서 정책
    파일을 선택하고 **Upload**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

![A screenshot of a upload box AI-generated content may be
incorrect.](./media/image9.png)

이제 Storage account가 성공적으로 생성되고 정책 문서와 로드되었습니다.

### 작업 1.2: Foundry 리소스를 생성하기

이 작업에서는 Microsoft Foundry에 접근하기 위해 필요한 Foundry 리소스를
생성할 것입니다.

1.  Azure 포털 (+++https://portal.azure.com+++)의 홈페이지에서
    **Foundry**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

2.  왼쪽 창에서 **Foundry**를 선택하고 Foundry 리소스를 생성하려면
    **Create**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

3.  다음 정보를 입력하고 **Review + create**를 선택하세요.

- Name – <+++agentic-@lab.LabInstance.Id>+++

- Default project name – <+++agentic-ai-project-@lab.LabInstance.Id>+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

4.  검증되면 **Create**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

5.  리소스가 생성해졌는디 확인하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

6.  [**agentic-ai-project-@lab.LabInstance.Id**](mailto:agentic-ai-project-@lab.LabInstance.Id)를
    열고 **Go to Foundry portal**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image16.png)

7.  Microsoft Foundry의 왼쪽 창에서 Models + endpoints를 선택하세요. +
    **Deploy model** -\> **Deploy base model**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

8.  +++gpt-4o-mini+++를 검색하고 선택하고 모델을 배포하려면 Confirm을
    클릭하세요.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image18.png)

9.  배포 창에서 **Deploy**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

10. 마찬가지로 +++text-embedding-ada-002+++를 검색하고 배포하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image21.png)

이 작업에서는 Foundry 리소스를 성공적으로 생성하고 채팅과 임베딩 모델을
배포하였습니다.

### 작업 1.3: 애플리케이션 인사이트를 생성하기

이 작업에서는 모니터링에 필요한 에플리케이션 인사이트 리소스를 생성할
것입니다.

1.  Azure portal의 홈페이지에서 **Subscriptions**을 선택하고 할당된
    구독을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

2.  왼쪽 창에서 **Resource providers**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

3.  +++Operational+++를 검색하고 **Microsoft.OperationalInsights** 옆에
    있는 3 점을 선택하고 **Register**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

4.  Microsoft Foundry의 왼쪽 창에서 **Monitoring**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image26.png)

5.  **Create New** -\>를 선택하고 이름을
    <+++agent-insights-@lab.LabInstance.Id>+++로 입력하고 **Create**를
    선택하세요.

![A screenshot of a application AI-generated content may be
incorrect.](./media/image27.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

이 작업에서는 애플리케이션 인사이트 리소스를 생성했습니다.

### 작업 1.4: Search 리소스를 생성하기

AI 에이전트가 기업 질문에 정확히 답하기 전에, 신뢰할 수 있는 데이터
소스에 접근해야 합니다. Azure AI Search는 정책, 계약서, 매뉴얼과 같은
문서를 색인화하여 Retrieval-Augmented Generation (RAG)을 가능하게
합니다. 인덱스는 검색 가능한 카탈로그처럼 작용하여 콘텐츠를 조각으로
나누고 메타데이터를 추가하며, 대화 중에 에이전트가 올바른 정보를 검색할
수 있게 합니다.

이 작업에서는 Azure AI 검색을 사용해 업로드된 문서를 색인화하여 검색
가능한 지식 베이스를 생성할 것입니다.

1.  Azure 포털의 홈페이지에서 **Foundry**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image30.png)

2.  왼쪽 창에서 **AI Search**를 선택하고 **+ Create**를 선택하세요**.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

3.  다음 정보를 입력하고 **Review + create**를 선택하세요.

- Service name - +++ai-knowledge-@lab.LabInstance.Id+++

- Region - East US2

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image32.png)

4.  검증이 되면 **Create**를 선택하세요. 리소스가 생성하면 Go to
    resource를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image34.png)

5.  **Import data (new)**를 선택하세요.

![A screenshot of a search engine AI-generated content may be
incorrect.](./media/image35.png)

6.  **Choose data source** 아래의 **Azure Blob Storage**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image36.png)

7.  검색 기반 에이전트를 개발 중이라 다음 창에서 **RAG** 옵션을
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image37.png)

> 각 옵션의 목적은 다음과 같습니다 -

1.  **키워트 검색:** 정확한 키워드를 기반으로 한 전통적인 검색 경험에
    사용됩니다. 이 시스템은 테스트를 색인화하여 사용자가 AI 추론 없이
    키워트 매칭을 통해 정보를 찾을 수 있도록 합니다.

2.  **RAG (Retrieval-Augmented Generation):** 문서 검색과 AI 생성을
    결합합니다. 텍스트 (및 간단한 OCR 이미지)를 흡수하여 AI 에이전트가
    현실적이고 맥락 인식에 맞는 답변을 제공할 수 있습니다.

3.  **Multimodal RAG:** RAG를 확장하여 다이어그램, 표, 워크플로우,
    차트와 같은 복잡한 시각 콘텐츠를 처리할 수 있습니다. AI는 텍스트와
    시각적 요소를 모두 해석하여 더 풍부하고 통찰력 기반의 응답을
    가능하게 합니다.

&nbsp;

8.  **Storage account** 및 **datasets** **under Blob containe**r의
    <aistorage@lab.LabInstance.Id>를 선태하고 **Next**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

9.  다음 정보를 선택하고 **Next**를 선택하세요.

- Kind – Azure AI Foundry (Preview)

- Azure AI Foundry/Hub project –
  <agentic-ai-project-@lab.LabInstance.Id>

- Model deployment – text-embedding-002-ada

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

10. **Review and create** 화면이 나타날 떄까지 다음 화면에서 **Next**를
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image40.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

11. **Review and create** 화면에서 **Create**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

12. Create succeeded 대화상자에서 **Close**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

데이터세트를 Azure AI Search에 성공적으로 인상하고 검색 가능한 인덱스를
생성했습니다. 다음 작업에서는 AI 에이전트를 생성하고 이 인덱스를 지식
소스로 연결할 것입니다.

## 작업 2: Microsoft Foundry에 AI 에이전트를 생성하기

이 작업에서는 Microsoft Foundry에서 새로운 AI 에이전트를 생성하고,
Microsoft Agent Framework 인터페이스를 사용해 그 핵심 목적, 지침, 모델을
구성하게 됩니다.

1.  리소스 그룹으로 돌아가서 리소스 목록에서 **agentic-** foundry
    리소스를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image44.png)

2.  다음 창에서 **Go to Foundry portal**을 클릭하세요. 이제 Microsoft
    Foundry 포털로 이동하여 첫 번째 에이전트를 생성하게 됩니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

3.  Foundry Portal로 이동하면 왼쪽 메뉴에서 **Agents (1)**를 선택하면
    이미 미리 생성된 에이전트가 보입니다. 만약 생성되지 않았다면 **+ New
    agent (2)** 옵션을 클릭하여 생성하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image46.png)

4.  새로 생성된 **에이전트**를 선택하면 오른쪽에 설정 창이 열립니다.
    다음 정보를 입력하세요.

[TABLE]

> ![A screenshot of a computer program AI-generated content may be
> incorrect.](./media/image47.png)

5.  Microsoft Foundry에서 에이전트를 성공적으로 생성하셨습니다.
    다음으로, 다가오는 작업에서 인덱싱된 데이터를 연결하여 지식을
    풍부하게 할 때입니다.

## 작업 3: RAG을 위한 Azure AI Search를 연결하기

이 작업에서는 지식 통합 패널을 통해 Azure AI Search를 에이전트와
통합하여 MCP (Model Context Protocol)를 통한 검색 증강 응답을 가능하게
합니다.

1.  같은 에이전트 설정 창에서 아래로 **Knowledge** 매개변수를 위해 **+
    Add**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image48.png)

2.  **Add knowledge** 창에서 AI Search 리소스에 인덱스가 준비되어 있으니
    **Azure AI Search**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image49.png)

3.  다음 창에서 **Azure AI Search resource connection** 옵션을 위헤
    **drop-down arrow (1)**를 클릭하고 **Connect other Azure AI Search
    resource (2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image50.png)

4.  다음 창에서 올바른 AI Search 리소스가 선택되었는지 확인하고 **Add
    connection**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image51.png)

5.  **Adding Azure AI Search** 단계에서 다음 정보를 구성하고 완료되면
    **Connect (5)**를 클릭하세요.

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image52.png)

6.  에이전트는 이제 Azure AI Search 인덱스를 활용해 대화 중 정확한
    정보를 검색할 수 있는 지식 기반으로 성공적으로 정보를 풍부하게 하고
    있습니다.

## 작업 4: 에이전트 실행 로그를 테스트 및 관찰하기

이 작업에서는 정책 관련 질문을 하고 구조화된 로그를 검토하여 도구 사용,
검색 호출, 근거 있는 응답을 검증하여 에이전트를 테스트하게 됩니다.

1.  에이전트를 테스트하기 전에 Application Insights를 연결하여 상세한
    로그와 추적 가시성을 활성화하세요.

2.  Microsoft Foundry 포털에서 왼쪽 메뉴에서 **Monitoring (1)**을
    선택하고 **agent-insights- (2)**를 선택하고 **Connect (3)**을
    클릭하세요.

![](./media/image53.png)

3.  완료 후 왼쪽 메뉴에서 **Agents (1)**을 선택한
    후 **EnterpriseAssistant (2)** 에이전트를 선택한 후 **Try in
    playground (3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image54.png)

4.  채팅 패널이 열리면 프롬프트를 입력할 수 있습니다. 에이전트는
    연결하신 문서와 데이터셋을 사용해 응답할 것입니다.

샘플 프롬프트 -

- +++What is the employee travel reimbursement policy?+++

- +++Summarize the contract approval rules and cite the document.+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image55.png)

5.  상담원이 질문에 응답하면, 상단 메뉴에서 **Thread logs**를 클릭하여
    현재 스레드의 로그와 트레이스를 확인하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image56.png)

6.  에이전트 로그에 상세한 개요 정보를 보여주는 이러한 지표, 추적 및
    평가를 탐색하고 검토하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image57.png)

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image58.png)

7.  이제 이전에 연결된 애플리케이션 인사이트가 있던
    **monitoring **창으로 이동해 **Resource usage** 탭을 선택하고 모든
    지표와 값을 검토하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image59.png)

8.  여러분은 엄선된 엔터프라이즈 데이터셋을 기반으로 한 RAG 기반
    에이전트를 성공적으로 구축하셨습니다. 다음으로, 다중 에이전트 협업을
    가능하게 하여 에이전트들이 위임하고 논리적이며 지능적으로 함께 일할
    수 있도록 할 것입니다.

**요약**

이 실습에서 Microsoft Foundry에서 첫 AI 에이전트를 성공적으로 만들고
인덱스된 지식 베이스와 연결하셨습니다. 문서를 업로드하고, Azure AI
Search에 입력한 후 Microsoft Agent Framework 통합을 통해 RAG를
활성화했습니다. 에이전트를 테스트하고 실행 로그를 검토함으로써,
에이전트가 어떻게 안정적인 정보를 수집하고 기업용 응답을 생성하는지 직접
경험할 수 있었습니다.
