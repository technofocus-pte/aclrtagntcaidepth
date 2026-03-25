# 실습 4: 건강 보험 보고서 생성 다중 에이전트 시스템 개발하기

**개요**

이 실습에서는 포괄적인 건강 보험 보고서 자동 생성을 위해 특별히 설계된
지능형 다중 에이전트 시스템을 개발하게 됩니다. 이 시스템은 4개의 전문 AI
에이전트가 합력하여 상세한 건강보험 문서를 검색, 분석, 생성 및 검증하는
협력을 활용합니다. 다중 에이전트 아키텍처는 자율 에이전트들이 협력하여
단일 에이전트가 효과적으로 처리하기 어려운 복잡한 작업을 수행할 수
있음을 보여줍니다.

이 4개의 AI 에이전트를 구축할 것입니다:

- **검색 에이전트** – 이 에이전트는 특정 건강 보험 정책에 관한 정보를
  위해 Azure AI 검색 인덱스를 검색합니다.

- **보고서 에이전트** – 이 에이전트는 검색 에이전트로부터 반화된 정보를
  바탕으로 건강 보험 정책에 대한 상세 보고서를 생성합니다.

- **검증 에이전트** – 이 에이전트는 생성된 보고서가 지정된 요구사항을
  충족하는지 검증합니다. 저희 경우에는 보고서에 보장 제외 사항에 관한
  정보가 포함되는지 확인하는 것이 중요합니다.

- **오케스트레이터 에이전트** – 이 에이전트는 검색 에이전트, 보고
  에이전트, 검증 에이전트 간의 통신을 관리하는 오케스트레이터 역할을
  합니다.

![A diagram of a company AI-generated content may be
incorrect.](./media/image1.png)

오케스트레이션은 다중 에이전트 시스템의 핵심 요소로, 우리가 생성하는
에이전트들이 목표를 달성하기 위해 서로 소통할 수 있어야 하기 때문입니다.

Azure AI 에이전트 서비스를 사용해 검색, 보고, 검증 에이전트를 생성할
예정입니다. 하지만 Orchestrator Agent를 생성하기 위해서는 Semantic
Kernel을 사용할 것입니다. Semantic Kernel 라이브러리는 멀티 에이전트
시스템을 오케스트레이션하기 위한 기본 기능을 제공합니다.

**실습 목표**

이 실습에서 다음과 같은 작업을 수행할 것입니다.

- 작업 1: Azure AI Search 인덱스를 생성하기

- 작업 2: 검색, 보고서 및 검증 에이전트를 생성하기

## 작업 1: Azure AI Search 인덱스를 생성하기

이 작업에서는 **Azure AI Search index**를 생성하여 건강보험 계획 문서의
벡터화된 표현을 저장하여, AI 기반 검색 및 분석을 위한 효율적인 검색을
가능하게 합니다.

1.  **Azure portal**로 이동하고 **AI Search (1)**을 검색하고
    서비스에서 **AI Search (2)** 리소스를 선택하세요.

![](./media/image2.png)

2.  이 모드는 AI Foundry로 이동하며 **AI Search** (1) 내에서
    **Create**(2)를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

3.  **Create a Search service** 창에서 다음 정보를 입력하고 **Review +
    Create** (4)를 클릭하세요

    - Subscription : **Leave default subscription**

    - Resource Group : Select **AgenticAI (1)**

    - Service Name : **my-search-service- (2)**

    - Location : **(3)**

![](./media/image4.png)

4.  **Review + Create**에서 **Create**를 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

5.  배포가 완료될 때까지 기다렸다가 **Go to resource**를 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  왼쪽 메뉴에서 **Settings** 아래의 **Keys (1)**로 이동하세요. **API
    Access control**에서 **Both(2)**를 선택하세요.

![](./media/image7.png)

7.  **Are you sure want to update the API Access Control for this serach
    service**에 **Yes**를 선택하세요.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image8.png)

8.  **Settings**에서 **Identity(1)**로 이동하세요. System-assigned에서
    Status를 **On(2)**로 설정하고 **Save(3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

9.  **Enable System assigned managed identity**에 **Yes**를 선택하세요.

![A close-up of a computer error AI-generated content may be
incorrect.](./media/image10.png)

10. Azure portal에서 **Storage accounts (1)**를 검색하고 서비스에서
    **Storage accounts (2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

11. **aifoundry**로 시작하는 저장 계정으로 선택하세요.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

12. **Access control (IAM) (1)**을 선택하고 **Add(2)**를 클릭하고 **Add
    role assignment**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image13.png)

13. **Job function roles**에서 **Storage Blob Data Reader (1)**를
    검색하고 **Storage Blob Data Reader (2)**를 선택하고 **Next (3)**을
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

14. **Add role assignment** 페이지에서

    - Members에서 **Managed identity(1)**을 선택하세요

    - **Members (2)**를 선택하세요

    - Managed identity: **search service(1)** **(3)**

    - **my-search-service-**(4) search service를 선택하세요.

    - **Select (5)**를 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

15. **Review + assign**을 두번 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

16. **Azure OpenAI**, **my-openai-service**로 이동하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

17. **Access control (IAM) (1)**을 선택하고 **Add(2)**를 클릭하고 **Add
    role assignment**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

18. **Job function roles**에서 **Cognitive Services OpenAI User (1)**을
    검색하고 **Cognitive Services OpenAI User (2)**를 선택하고 **Next
    (3)**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image19.png)

19. **Add role assignment** 페이지에서

    - Members에서 **Managed identity(1)**을 선택하세요

    - **Members (2)**를 선택하세요

    - Managed identity: **search service(1)** **(3)**

    - **my-search-service-**(4) search service를 선택하세요.

    - **Select (5)**를 클릭하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image20.png)

20. **Review + assign**을 두번 선택하세요.

![](./media/image21.png)

21. **Azure Portal**로 이동하고 **Storage account (1)**을 검색하고
    **Storage account (2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

22. **aifoundryhub**로 시작하는 스토리지 계정을 선택 하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

23. Data storage에서 **Containers (1)**을 클릭하고 **+ Container(2)**를
    선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

24. New Container 페이지에서 name을 **healthplan(1)**로 입력하고
    **Create (2)**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image25.png)

25. **healthplan** 컨테이너를 클릭하고 여세요.

![](./media/image26.png)

26. 파일을 업로드하려면 **upload (1)**을 클릭하고 **browse for files
    (2)**를 클릭하세요.

> ![](./media/image27.png)

27. C:\LabFiles\Day-1\azure-ai-agents-labs\data **(1)**로
    이동하고 업로드할 PDF를 **(2)** 모두 선택하고 **Open (3)**을
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image28.png)

28. **Upload**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

**참고:** 기존 컨테이너를 선택하라고 하면, 드롭다운에서 건강보험을
선택하세요.

29. **Azure AI search** 서비스로 이동하고 **my-search-service-**를
    선택하세요.

![](./media/image30.png)

30. **import data (new)**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image31.png)

31. **azure blob storage**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image32.png)

32. **RAG** 모델을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image33.png)

33. Configure your Azure Blob Storage에서 다음 정보를 입력하고
    **Next(5)**를 클릭하세요:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image34.png)

34. Vectorize your text에서 다음 정보를 입력하고 **Next (7)**를
    클릭하세요:

[TABLE]

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image35.png)

35. **Next**를 두번 클릭하세요.

36. **Objects name prefix**에 **health-plan (1)**을 입력하고 **Create
    (2)**를 클릭하세요.

![A screenshot of a computer screen AI-generated content may be
incorrect.](./media/image36.png)

**참고**: 검색 서비스에서 데이터를 인덱스에 업로드하는 데 5-10분이 걸릴
수 있습니다.

37. 팝업에서 **Start searching**을 클릭하세요.

![A screenshot of a computer error AI-generated content may be
incorrect.](./media/image37.png)

38. **ai-foundry-project-**의 **Overview** (1) 페이지로 이동하세요.
    **Open In management center**(2)를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image38.png)

39. **Connected resources** (1)을 선택하고 **New connection** (2)를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image39.png)

40. 검색바에 **Azure AI Search**(1)을 입력하고 **Azure AI Search**(2)를
    선택하세요.

![](./media/image40.png)

41. 계속하려면 **Add connection**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image41.png)

## 작업 2: 검색, 보고서 및 검증 에이전트를 생성하기

이 작업에서는 건강 보험 보고서를 검색, 생성, 검증하기 위한 검색, 보고,
검증 에이전트를 생성합니다. 이 에이전트들은 정확성과 요구사항 준수를
보장하기 위해 협력합니다. 각 요원은 보고서의 조회, 수집, 정확성 보장에
뚜렷한 역할을 수행합니다.

1.  **Lab 4 - Develop A Mult-Agent System.ipynb** 파일을 여세요. 이
    **Lab 4 - Develop A Mult-Agent System.ipynb** 노트북은 검색, 보고,
    검증, 조정 에이전트를 포함한 다중 에이전트 시스템을 개발하여 건강
    보험 보고서를 생성하고 검증하는 방법을 안내합니다. 각 요원은
    보고서의 조회, 수집, 정확성 보장에 뚜렷한 역할을 수행합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image42.png)

2.  오른쪽 상단에 있는 **Select kernel (1)** 설정을 선택한 후 목록에서
    **venv (Python 3.x.x) (2)**를 선택 하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image43.png)

3.  이 셀을 실행하여 Azure AI Search, GPT-4o, Semantic Kernel을 통합한
    **multi-agent system**을 개발 하여 지능형 작업 실행을 지원합니다. 이
    구조는 여러 AI 에이전트가 정보를 검색하고, 응답을 생성하며, 복잡한
    쿼리를 처리하는 데 협력할 수 있게 합니다.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image44.png)

4.  이 셀을 실행하면 **Search Agent**를 생성하는데, 이 에이전트는
    GPT-4o를 사용해 Azure AI Search에서 건강 보험 세부 정보를
    조회합니다. 이 에이전트는 건강 보험 문서 내에서 구조화된 정보를
    효율적으로 검색할 수 있게 합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image45.png)

5.  이 셀을 실행하면 GPT-4o를 사용해 건강 보험에 대한 상세 보고서를
    생성하는 **Search Agent**를 생성합니다. 이 에이전트는 구조화된 통찰,
    보장 세부사항, 다양한 플랜에 대한 제외 사항을 제공하여 문서를
    향상시킵니다.

![](./media/image46.png)

6.  이 셀을 실행하면 **Validation Agent**를 생성하는데, 이는 Report
    Agent가 생성한 보고서가 품질 기준을 충족하는지 확인하며, 특히 보장
    제외 여부를 확인합니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image47.png)

7.  **다중 에이전트 시스템을 생성하기**: 아래 셀을 실행하면 VS Code
    상단에 건강 보험 플랜 이름을 입력하라는 채팅 상자가 뜨게 됩니다.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image48.png)

8.  기억하신다면, 저희는 두 개의 건강 보험을 검색 인덱스에
    업로드했습니다. 안내를 받으면 상단에 나타나는 박스에 다음 건강 보험
    중 하나를 입력하고 **Enter**를 눌러 멀티 에이전트 시스템 실행을
    시작하세요:

    - **Northwind Health Standard**

    - **Northwind Health Plus**1

![](./media/image49.png)

9.  상자가 상단에 나타나면 Exit을 입력하고 Enter 버튼을 눌러 실행 중인
    코드 블록을 멈추세요.

**참고**: 셀을 성공적으로 돌리면 다음과 같은 결과를 받게 됩니다.

> Orchestrator Agent is starting...
>
> Calling SearchAgent...
>
> SearchAgent completed successfully.
>
> Calling ReportAgent...
>
> ReportAgent completed successfully.
>
> Calling ValidationAgent...
>
> ValidationAgent completed successfully.
>
> The report for Northwind Plus has been generated. Please check the
> Northwind Plus Report.md file for the report.
>
> Orchestrator Agent is starting...

**요약**

이 실습에서 4개의 전문 AI 에이전트가 협력하여 포괄적인 건강 보험 보고서
생성을 자동화하는 지능형 다중 에이전트 시스템을 성공적으로
개발하셨습니다. 벡터화된 건강보험 문서를 저장하는 Azure AI 검색 인덱스를
생성하고, 정책 정보를 검색하는 검색 에이전트, 상세 문서를 생성하는
리포트 에이전트, 요구사항 준수를 보장하는 검증 에이전트, 그리고 모든
에이전트 간 통신을 관리하는 Semantic Kernel을 사용하는 오케스트레이터
에이전트를 생성했습니다. 실제 건강보험 플랜 데이터를 활용한 다중
에이전트 시스템을 운영함으로써, 자율 에이전트들이 단일 에이전트가 어려운
복잡한 작업을 효과적으로 협력하여 수행할 수 있음을 보여주었고, 실용적인
비즈니스 애플리케이션을 위한 엔터프라이즈급 에이전트 오케스트레이션
패턴을 보여주었습니다.

축하합니다! 실습을 성공적으로 완료했습니다.
