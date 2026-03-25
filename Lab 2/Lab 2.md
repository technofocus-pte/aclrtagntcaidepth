# 실습 2: AI 프로젝트를 설정하고 VS Code에서 채팅 완료를 수행하기

**개요**

이 실습에서는 Microsoft Foundry에서 AI 프로젝트를 생성하고 구성하며,
Large Language Model (LLM)과 임베딩 모델을 배포하고, 프로젝트를 Visual
Studio Code와 연결하여 AI 에이전트 구축에 필요한 완전한 개발 환경을
준비하게 됩니다. 그 후 코드에서 간단한 채팅 완료를 실행하여 환경을
올바르게 구성하고 AI 기반 애플리케이션 개발에 준비가 되었는지 검증할
것입니다.

실습 목표

이 실습에서 다음과 같은 작업을 수행합니다.

- 작업 1: Microsoft Foundry에서 AI 프로젝트를 설정하기

- 작업 2: LLM 및 임베딩 모델을 배포하기

- 작업 3: 의존성을 설치하고, 가상 환경을 생성하고 환경 변수 파일을
  생성하기

## 작업 1: Microsoft Foundry에서 AI 프로젝트를 설정하기

이 작업에서는 Microsoft Foundry 내에서 AI 프로젝트를 생성하고 구성하게
됩니다. 이는 필요한 자원 구축, 프로젝트 매개변수 정의, AI 모델 배포 환경
준비 등을 포함합니다. 이 작업이 끝날 때쯤이면 완전히 초기화된 AI
프로젝트가 완성되어 추가 개발과 실습의 토대가 될 것입니다.

1.  Azure Portal 페이지에서 포털의 상단의 Search 자원
    상자에서 **Microsoft Foundry (1)**을 입력한 후 Services
    항목에서 **Microsoft Foundry (2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  In the left navigation pane for the **Use with Foundry**,
    select **AI Hubs (1)**. On the AI Hubs page, click on **Create
    (2)** and select **Hub (3)** from the drop-down.

![](./media/image2.png)

3.  On the **Create an Azure AI hub** pane enter the following details
    under **Basics** **(1)** :

    - Subscription : **Leave default subscription** **(2)**

    - Resource Group : **AgenticAI** **(3)**

    - Region : **East US2** (4)

    - Name : ** <+++ai-foundry-hub@lab.LabInstance.Id>+++ (5)**

    - Connect AI Services incl. OpenAI : Click on **Create
      New** **(6)**.

    - Connect AI Services incl. OpenAI : Provide a
      name **<+++my-ai-service@lab.LabInstance.Id>+++ (7)**.

    - Click on **Save** **(8)**, followed by **Next:Storage** **(9)**

> ![](./media/image3.png)

4.  Click on **Review + Create** tab followed by **Create.**

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

![](./media/image5.png)

5.  Wait for the deployment to complete and then click on **Go to
    resource**.

![](./media/image6.png)

6.  On the Overview pane, click on **Launch Azure AI Foundry**. This
    will navigate you to the Microsoft Foundry portal.

![](./media/image7.png)

7.  Scroll down and click on **+ New project** on the Hub Overview.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  Provide the project name
    as **<+++ai-foundry-project@lab.LabInstance.Id>+++ ,** then click
    on **Create (2)**.

![](./media/image9.png)

9.  Once the project is created, scroll down and copy the **Project
    connection string**, then paste them into Notepad or a secure
    location, as they will be required for upcoming tasks.

![A screenshot of a project AI-generated content may be
incorrect.](./media/image10.png)

## 작업 2: LLM 및 임베딩 모델을 배포하기

이 작업에서는 Microsoft Foundry 프로젝트 내에 large language model
(LLM)과 임베딩 모델을 배포하게 됩니다. 이 모델들은 향후 실습에서 AI 기반
응용 및 벡터 기반 검색 기능에 사용될 예정입니다.

1.  **Microsoft Foundry project**에서 **My assets** **(1)** 섹션으로
    이동하고 **Models + endpoints** **(2)**를 선택하세요. **Deploy
    model** **(3)**을 클릭하고 계속하려면 **Deploy base
    model** **(4)**를 선택하세요.

![](./media/image11.png)

2.  **Select a model** 창에서 **gpt-4o** **(1)**을 검색하고
    **gpt-4o** **(2)**를 선택하고 **Confirm** **(3)**을 선택하세요

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image12.png)

3.  **Deploy model gpt-4o** 창에서 **Customize**를 선택하세요.

![](./media/image13.png)

- Deployment Name: **gpt-4o** **(1)**

- Deployment type: **Global Standard** **(2)**

- Change the **Model version to 2024-08-06 (Default)** **(3)**

- Change the Tokens per Minute Rate Limit to **200K** **(4)**

- **Connect and Deploy** **(5)**를 클릭하세요

![](./media/image14.png)

4.  **Model + Endpoints** **(1)**을 클릭하고 배포된
    **gpt-4o** **(2)** 모델을 볼 수 있습니다.

![](./media/image15.png)

5.  **Azure Portal**로 이동하고 **Open AI** **(1)**을 검색하고 **Azure
    Open AI** **(2)** 자원을 선택하세요.

![](./media/image16.png)

6.  **Microsoft Foundry | Azure OpenAI** 페이지에서 **+
    Create** **(1)**을 선택하고 Azure OpenAI 리소스를 생성하려면 **Azure
    OpenAI** **(2)**를 선택하세요 .

![](./media/image17.png)

7.  **Create Azure OpenAI** 페이지에서 다음 설정을 입력하고
    **Next** **(6)**을 클릭하세요:

[TABLE]

> ![](./media/image18.png)

8.  Review + submit 탭이 나타날 때 까지 **Next**를 클릭하세요.

9.  **Review + submit** 페이지에서 **Create**를 클릭하세요.

![](./media/image19.png)

10. 배포가 성공될 때 기다리고 **Go to resource**를 선택하세요.

![](./media/image20.png)

11. **my-openai-service** 리소스 페이지에서 **Go to Foundry portal**을
    선택하세요.

![](./media/image21.png)

12. AI Foundry 프로젝트에서 **Shared resources** 섹션으로 이동하고
    **Deployments** **(1)**을 선택하세요. **Deploy model** **(2)**를
    클릭하고 계속하려면 **Deploy base model** **(3)**을 선택하세요.

![](./media/image22.png)

**참고**: 이후 실습에서 사용할 Azure AI Search의 가져오기 및 벡터화
wizard는 아직 AI Foundry 프로젝트 내에서 텍스트 임베딩 모델을 지원하지
않습니다. 이 때문에 Azure OpenAI 서비스를 생성하고 텍스트 임베딩 모델을
배포해야 합니다. 이 텍스트 임베딩 모델은 나중에 벡터 인덱스를 생성할 때
사용할 예정입니다.

13. **Select a model** 창에서 **text-embedding-3-large** **(1)**을
    검색하고 **text-embedding-3-large** **(2)**를
    선택하고 **Confirm** **(3)**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image23.png)

14. **Deploy model text-embedding-3-large** 창에서,

    - Deployment type: Select **Standard (1)**

    - Tokens per Minutes Rate Limit: **120K (2)**

    - 모델을 배포하려면 **Deploy (3)**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

15. **Deployment (1)**을 클릭하면 배포된 **text-embedding-3-large
    (2)** 모델을 볼 수 있습니다.

![](./media/image25.png)

## 작업 3: 의존성을 설치하고, 가상 환경을 생성하고 환경 변수 파일을 생성하기

이 작업에서는 필요한 의존성을 설치하고, 가상 환경을 설정하며, 환경 변수
파일을 생성해야 합니다. 이를 통해 통제된 개발 환경을 보장하고 AI
프로젝트의 구성 설정을 안전하게 관리할 수 있습니다.

1.  **Lab VM**에서 **Visual Studio Code**를 실행하세요.

2.  **File** **(1)**을 클릭하고 **Open Folder**를 선택하세요.

![](./media/image26.png)

3.  C:\LabFiles\Day-1 \\**(1)**로 이동하고
    **azure-ai-agents-labs** **(2)** 폴더를 선택하고 **Select
    folder** **(3)**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image27.png)

4.  **Yes, I Trust the authors**를 클릭하세요,

![](./media/image28.png)

5.  **elipses(...)** **(1)**을 클릭하고 **Terminal** **(2)**를 클릭하고
    **New Terminal** **(3)**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image29.png)

6.  azure-AI-agents-labs 프로젝트 디렉터리에 있는지 확인하세요. 아래의
    PowerShell 명령어를 실행하여 가상 환경을 생성하고 활성화하세요:

7.  python -m venv venv

+++venv/Scripts/activate+++

![](./media/image30.png)

8.  다음 powershell 명령어를 실행하세요. 이렇게 하면 필요한 모든
    패키지가 설치됩니다:

9.  pip install -r requirements.txt

+++pip install azure-ai-ml azure-identity+++

![A screen shot of a computer code AI-generated content may be
incorrect.](./media/image31.png)

10. 아래 powerShell 명령어를 실행하여 pip을 최신 버전으로 설치하거나
    업그레이드하세요.

+++python.exe -m pip install --upgrade pip+++

![](./media/image32.png)

11. 아래 명령을 실행하여 Azure 계정에 로그인하세요.

+++az login+++

12. 승인할 사용자 계정을 선택하세요.

13. 승인이 완료되면 Visual Studio 코드로 다시 이동하세요.

![](./media/image33.png)

14. Sample.env **파일을 열고** 필요한 환경 변수를 입력하세요.

![](./media/image34.png)

- Microsoft foundry 포털로 이동하고 My asset의 **Models + endpoints(1)**
  섹션에서 **gpt-4o** **(2)** 모델을 클릭하고 오른쪽 창에서
  **Endpoint**를 복사하고 노트패드에 **Target URI (1)** 및 **Key (2)**를
  복사하고 붙여넣으세요.

![](./media/image35.png)

![](./media/image36.png)

15. **Sample.env** 파일에서,

    - AIPROJECT_CONNECTION_STRING: 작업 1의 단계 9에 복사한 **Project
      connection string**값을 제공하세요

    - CHAT_MODEL_ENDPOINT: 이전 단계에서 복사한 **gpt-4o** 모델의
      **Target URI**를 제공하세요

    - CHAT_MODEL_API_KEY: 이전 단계에서 복사한 **gpt-4o**모델의 **Key**
      값을 제공하세요

    - CHAT_MODEL: **gpt-4o**

![](./media/image37.png)

16. **Sample.env** 파일에 변경 사항을 저장하세요.

17. 다음 powershell 명령어를 실행하세요. **.env** 파일이 생성됩니다:

+++cp sample.env .env+++

![](./media/image38.png)

18. **Lab 1 - Project Setup.ipynb** 파일을 여세요. **Lab 1 - Project
    Setup.ipynb** 노트북은 Microsoft Foundry에서 AI 프로젝트 설정, LLM
    배포 및 모델 임베딩, VS Code 연결 설정 과정을 안내합니다. 또한 설정
    확인을 위한 간단한 Chat Completion API 호출도 포함되어 있습니다. 이
    노트북을 실행하면 AI 기반 애플리케이션 개발에 적합한 환경이 올바르게
    구성되어 있음을 보장합니다.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image39.png)

19. 오른쪽 상단에 있는 **Select kernel (1)** 설정을 선택하고
    **Install/enable selected extensions (python+jupyter) (2)**를
    선택하세요.

![](./media/image40.png)

20. 필요한 의존성이 설치된 올바른 Python 인터프리터에서 Jupyter
    Notebook이 실행되는지 확인하려면 **Python environments**를
    선택하세요.

![](./media/image41.png)

21. 목록에서 **venv(Python 3.x.x)**를 선택하세요. 이 버전은 Microsoft
    Foundry SDK 및 기타 의존성 호환성을 위해 필요할 가능성이 큽니다.

![](./media/image42.png)

**참고:** **venv (Python 3.x.x)**가 목록에 나타나지 않을 경우, Visual
Studio 코드를 닫고 열어보세요.

22. 첫 번째 셀을 실행해서 Azure AI 서비스를 다루기 위해 필요한 Python
    라이브러리를 가져오세요.

![](./media/image43.png)

23. 아래 셀을 실행하여 환경 변수에서 프로젝트 연결 문자열과 모델 이름을
    가져오세요. 이 값들은 민감한 정보를 하드코딩하지 않고 Large Language
    Model (LLM)과 안전하게 상호작용하는 데 필요합니다.

![](./media/image44.png)

24. 아래 셀을 실행하여 연결 문자열을 사용해 Microsoft Foundry 프로젝트에
    연결하세요. 이로 인해 AIProjectClient와 안전한 연결이 구축되어
    프로젝트 자원과의 상호작용이 가능해집니다.

![](./media/image45.png)

25. 아래 셀을 실행하여 Microsoft Foundry 프로젝트를 통해 GPT-4o 모델과
    상호작용하세요. 이 코드는 채팅 클라이언트를 초기화하고, 곰인형에
    관한 농담 요청을 보내고, 응답을 출력합니다. 마지막으로 채팅 모델에서
    제공되는 출력을 확인해 보세요.

![](./media/image46.png)

> **참고:** 이 단계의 출력은 AI 모델에 의해 동적으로 생성되며 매번
> 달라질 수 있습니다.

**요약**

이 실습에서는 Microsoft Foundry에서 AI 프로젝트를 생성 및 구성하고,
GPT-4o Large Language Model 및 text-embedding-3-large embedding model을
배포하며, Visual Studio Code와의 보안 연결을 구축하여 완전한 AI 개발
환경을 성공적으로 구축했습니다. 필요한 의존성을 설치하고, 가상 환경을
생성하고, 민감한 정보를 안전하게 관리할 수 있도록 환경 변수를
설정했습니다. 마지막으로, 간단한 채팅 완성 API 호출을 실행해 환경이
올바르게 구성되어 AI 기반 애플리케이션 개발에 적합한지 확인했습니다.

이 실습을 성공적으로 완료했습니다. 계속하려면 Next \>\>를 클릭하세요.
