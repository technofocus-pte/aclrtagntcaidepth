# 지능형 에이전트를 구축 및 확장하기

**개요**

이 실습은 Azure AI 서비스 및 Microsoft 365 Copilot을 사용하여 지능형 AI
에이전트를 구축하는 방법을 소개합니다. 참가자들이 HR 워크플로우를 위한
Copilot 활용법, Microsoft Foundry 프로젝트 설정, 간단한 AI 에이전트
구축, RAG (Retrieval-Augmented Generation) 에이전트 생성, 오케스트레이션
기능이 있는 다중 에이전트 시스템 개발법을 배우게 됩니다.

**목표**

이 실습이 끝날 때 다음을 수행할 수 있습니다:

- **Copilot Studio를 사용하여 HR 지원 에이젼트를 구축** - Microsoft 365
  Copilot을 사용하여 직원 채용, 선별, 교육 자료 개발, 피드백 수집, 성과
  평가를 자동화하기

- **AI 프로젝트를 설정하고 채팅 완료 수행** - Microsoft Foundry에서 AI
  프로젝트를 구성하고, Large Language Models (LLM)과 임베딩 모델을
  배포하며, 채팅 완료를 위한 VS 코드 연결을 설정하기

- **건강보험 플랜 분석기 AI 에이전트 구축** - Azure AI 서비스를 사용해
  데이터를 처리하고 시각화(예: 건강 혜택 플랜 비교 막대 차트)를 생성하는
  AI 에이전트를 생성하기

- **건강 보험 보고서 생성 다중 에이전트 시스템 개발** - 전문
  에이전트(검색, 보고, 검증, 오케스트레이터 에이전트)가 협력하여 복잡한
  작업을 수행하는 조정된 다중 에이전트 시스템을 설계 및 구현하기

**필수 구성 요소**

참가자들은 다음 사항을 갖추어야 합니다 :

- **Visual Studio Code (VS Code):** 다양한 프로그래밍 언어와
  프레임워크의 확장 관리를 위해 VS Code를 활용하는 숙련도

- **개발 기술**: Python 또는 Javascript에 대한 기본 프로그래밍 지식,
  API, SDK 경험, Visual Studio Code 사용

- **명령줄/터미널**: PowerShell 명령어 실행과 가상 환경 관리에 대한
  친숙하기

**구성 요소 설명**

- **Azure AI 검색**: 관련 문서를 색인화하고 검색하여 RAG를 가능하게 하는
  벡터 기반 검색 서비스

- **Azure OpenAI 서비스**: Azure 기업 인프라를 통해 GPT-4o 및 모델
  임베딩 접근을 제공

- **Large Language Model (LLM):** 텍스트 이해와 생성을 위한 GPT-4o와
  같은 고급 AI 모델

- **임베딩 모델**: 텍스트를 의미 탐색 및 검색을 위한 벡터 표현으로 변환
  (예: text-embedding-3-large)

- **Microsoft 365 Copilot**: 문서 분석 및 워크플로우 자동화를 위한 AI
  기반 생산성 도구

- **Semantic Kernel**: LLM을 프로그래밍 언어와 통합하고 오케스트레이션
  기능을 구축하는 SDK

# 실습 1: Copilot Studio를 사용하여 HR 지원 에이전트를 구축하기

예상 소요 시간: 30분

개요

이 실습에서는 Microsoft 365 Copilot과 Copilot Studio를 활용하여 조직 내
직원 전환 및 온보딩 과정을 간소화하고 개선하는 데 집중합니다. 적합한
후보자를 식별하고, 맞춤형 전환 및 온보딩 계획을 수립하며, 효과적인
커뮤니케이션 및 교육 자료를 생성하고, 인사 워크플로우를 자동화하며,
피드백을 수집하고, 성과 모니터링 및 검토 메커니즘을 구축하는 방법을
배우게 됩니다. 이러한 AI 기반 도구를 활용하여 이 연구실은 조직이
원활하고 효율적인 전환 과정을 보장하고, 내부 이동성을 높이며, 직원들이
새로운 역할에 성공적으로 적응할 수 있도록 지원하는 방법을 보여줍니다.

실습 목표

이 실습에서 다음과 같은 작업을 수행합니다.

- 작업 1: 호부자를 신속히 심사하기

- 작업 2: 교육 자료 개발하기

- 작업 3: 피드백 수집하기

- 작업 4: 성과 평가하기

아키텍처 다이어그램

![image](./media/image1.png)

## 작업 1: 후보자를 신속히 심사하기

이 작업에서는 Microsoft 365 Copilot을 사용해 이력서를 분석하고 관련
경험, 기술 역량, 학력 등 특정 기준에 따라 후보자를 필터링하여 데이터
분석가 직무에 대한 많은 지원서를 신속히 평가하게 되며, Copilot이 추가
검토를 위해 상위 후보자를 강조할 수 있습니다.

1.  Edge 브라우저에 새 탭을 추가하고 다음 링크를 통해 Microsoft 365
    Copilot 앱을 실행한 후 **Sign in (2)**을 클릭하세요.

+++https://m365.cloud.microsoft/+++

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

2.  **Sign into Microsoft Azure 탭**에서, 로그인 화면이 보일 것입니다.
    아래 자격 증명으로 로그인하세요.

- Username - +++@lab.CloudPortalCredential(User1).Username+++

- TAP - +++@lab.CloudPortalCredential(User1).TAP+++

3.  **Welcome to your Microsoft 365 Copilot app** 팝업이 보시면 **Get
    started**를 클릭하세요.

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image3.png)

4.  왼쪽 창에서 **Apps** **(1)**을 선택하고 Apps
    섹션에서 **OneDrive** **(2)**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

**참고**: **Welcome to Apps**라는 팝업이 보시면 **X**를 클릭하고 팝업을
닫으세요.

![A screenshot of a computer application AI-generated content may be
incorrect.](./media/image5.png)

5.  **My files**으로 이동하고 **+ Create or upload (1)** 버튼을 클리하고
    **Folder upload (2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

6.  C:\LabFiles\Day-1\data **(1)**로 이동하고 CV **(2)** 폴더를 클릭하고
    **Upload (3)**을 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

7.  Upload 5 files to this site? 팝업에서 **Upload**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

8.  다시 한번, **+ Create or upload (1)**를 클릭하고 **Folder upload
    (2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

9.  C:\LabFiles\Day-1 **(1)**로 이동하고 data **(2)** 파일을 클릭하고
    **Upload 3**을 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image9.png)

10. Upload 19 files to this site? 팝업에서 **Upload**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image10.png)

11. **M365 Copilot**으로 이동하고 왼쪽 창에서 **Apps** **(1)**을
    선택하고 Apps 섹션에서 **Copilot** **(2)**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image11.png)

12. 왼쪽 창에서 **Copilot**으로 이동하고 **Chat (1)**을 클릭하세요.
    **+** 채팅 창 밑의 **(Add)** 아이콘 **(2)**를 클릭하고 **Upload
    images and files (3)**을 선택하세요.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image12.png)

13. 파일 탐색기 팝업에서 C:\LabFiles\Day-1\data\CV **(1)** 폴더로
    이동하고 **first 3** **(2)** 파일을 선택하고 **Open** **(3)**을
    클릭하세요.

![A screenshot of a chat AI-generated content may be
incorrect.](./media/image13.png)

14. **Copilot chat**에서 **3 files**이 성공적으로 업로드되면 **enter**를
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image14.png)

15. 활성 Copilot 채팅에서 메시지 상자 밑에 있는 **+ (Add) (1)** 아이콘을
    클릭하고 **Upload images and files (2)**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image15.png)

16. 파일 탐색기 팝업에서 C:\LabFiles\Day-1\Data\CV **(1)** 폴더로
    이동하고 **last 2** **(2)** 파일을 선택하고 **Open** **(3)**을
    클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image16.png)

17. **Copilot chat**에서 **2 files (1)**이 성공적으로 업로드되면 **enter
    (2)**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image17.png)

18. 채팅 상자에서 다음 프롬프트 **(1)**을 입력하고 **Sent (2)** 버튼을
    누르세요:

> Microsoft 365 Copilot, please help me filter and shortlist resumes of
> Data Analyst candidates based on required qualifications such as
> experience in SQL, Python, and data visualization tools.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image18.png)

19. 다음 프롬프트를 입력하고 **Sent** 버튼을 누르세요.

> Create a summary report of top Data Analyst candidates, including
> their skills, work experience, and educational background.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image19.png)

**결과**: HR 팀은 가장 적합한 후보자를 효율적으로 발굴하여 시간을
절약하고 집중된 채용 노력을 보장합니다.

## 작업 2: 교육 자료 개발하기

이 과제에서는 Microsoft Copilot을 사용해 신입사원을 위한 포괄적인 교육
자료를 준비하며, 역할별 가이드, 회사 정책, 사용 도구 및 기술 개요 등
맞춤형 온보딩 콘텐츠를 작성하여 교육 자료가 철저하고 체계적이며 직원
역할에 맞게 맞춤화되도록 합니다.

1.  채팅 상자에서 다음 프롬프트 **(1)**을 입력하고 **Sent (2)** 버튼을
    누르세요:

> Generate a comprehensive onboarding training plan for the new Data
> Analyst, including topics like company policies, data tools training,
> and team introductions.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image20.png) ![A screenshot of a web page
> AI-generated content may be incorrect.](./media/image21.png)

2.  다음 프롬프트 **(1)**을 입력하고 **Sent (2)** 버튼을 누르세요.

> Create an interactive training presentation covering data analysis
> best practices and key performance metrics and generate a downloadable
> PPT.
>
> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image22.png)

**참고**: 이 프롬프트를 실행하면 PowerPoint프레젠테이션을 다운로드하고,
이를 편집하거나 디자인할 수 있습니다. 파일이 다운로드되지 않았다면,
스크린샷에 보이는 프레젠테이션 제목이 포함된 하이퍼링크를 찾아보세요.

**참고**: 이 프롬프트를 실행한 후에도 ' PowerPoint 프레젠테이션 다운로드
중' 옵션이 나타나지 않습니다. 위 프롬프트를 다시 실행해 주세요.

결과: 신입 직원은 잘 조직된 교육 자료를 받아 빠르게 업무를 효과적으로
수행할 수 있도록 합니다.

## 작업 3: 피드백 수집하기

이 작업에서는 Microsoft Copilot을 사용해 신입 직원과 면접관으로부터
피드백 설문조사를 생성 및 배포하고, 응답을 수집 및 분석하며, 채용 및
온보딩 과정의 강점과 개선이 필요한 부분에 대한 인사이트를 얻게 됩니다.

1.  채팅 상자에서 다음 프롬프트를 입력하고 **Sent** 버튼을 누르세요:

> Create a feedback form for interviewers to evaluate Data Analyst
> candidates based on technical skills, problem-solving abilities, and
> cultural fit Generate a downloadable Word or PDF version of this
> feedback form.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image22.png)

2.  다음 프롬프트를 입력하고 **Sent** 버튼을 누르세요.

> Send out a survey to new hires to gather feedback on their onboarding
> experience and identify areas for improvement Generate a downloadable
> Word or PDF version of the survey.
>
> ![A screenshot of a survey AI-generated content may be
> incorrect.](./media/image23.png)
>
> 결과: HR부서는 귀중한 피드백을 받아 채용 및 온보딩 관행을 개선하여
> 향후 채용자에게 더 나은 경험을 보장합니다.

## 작업 4: 성과 평가하기

이 과제에서는 Microsoft Copilot을 사용하여 성과 평가 템플릿을 생성하고,
평가 회의를 일정 잡고, 성과 추적하며, 동료들로부터 피드백을 수집하고,
구조화된 성과 보고서를 작성하여 신입 직원의 진척과 발전을 평가하는
정기적인 성과 평가를 실시하게 됩니다.

1.  채팅 상자에서 다음 프롬프트를 입력하고 **Sent** 버튼을 누르세요:

> Set up a performance review schedule for the new Data Analyst, with
> quarterly reviews and goal-setting sessions and Generate a calender
> CSV file.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image24.png)

2.  다음 프롬프트를 입력하고 **Sent** 버튼을 누르세요.

> Generate a template for performance review reports, including sections
> for achievements, areas of improvement, and future goals Generate a
> performance review template.
>
> ![A screenshot of a report AI-generated content may be
> incorrect.](./media/image25.png)
>
> 결과: 신입 직원은 건설적인 피드백을 지원을 받아 전문성 성장에 도움을
> 주고 회사 내 장기적인 성공에 기여합니다.
>
> **요약**
>
> 이 실습에서는 Microsoft 365 Copilot을 사용해 직원 채용 및 온보딩
> 프로세스를 간소화하기 위해 HR 지원 에이전트를 성공적으로
> 구축하였습니다. 이력서를 분석하고 SQL, Python, 데이터 시각화 같은 기술
> 기술을 기반으로 필터링하여 데이터 분석가 후보자를 빠르게 선별하는
> 방법을 배웠고, 신입사원을 위한 포괄적인 온보딩 교육 계획과 인터랙티브
> 프레젠테이션을 생성했습니다. 면접관을 위한 피드백 양식과 신입 직원을
> 위한 설문조사를 작성해 채용 과정을 평가하고 개선했으며, 성과와 목표를
> 추적할 수 있는 구조화된 템플릿으로 분기별 성과 평가 일정을
> 설정했습니다. AI 기반 도구를 활용해 조직이 인사 업무를 자동화하고
> 효율성을 높이며 신입 직원의 원활한 전환 과정을 보장할 수 있음을
> 보여주셨습니다.
>
> 이 실습을 성공적으로 완료했습니다. 계속하려면 Next \>\>를 클릭하세요.
