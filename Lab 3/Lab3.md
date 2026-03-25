# 실습 3: 건강 보험 설계 분석 AI 에이전트 구축하기

**개요**

이 실습에서는 건강보험 플랜 데이터를 처리하고 분석하기 위해 특별히
설계된 건강보험 플랜 분석 AI 에이전트를 구축하게 됩니다. 이 지능형
에이전트는 플랜 세부 정보를 해석하고, 보장 옵션을 분석하며, 의사결정을
지원하는 시각적 표현을 생성하여 다양한 건강 보험 플랜 간 비교를
자동화합니다. Microsoft Foundry와 Azure AI 서비스를 활용해 보험 플랜 간
차이를 명확히 보여주는 비교 막대 차트를 생성하여 사용자가 옵션을
이해하고 가장 적합한 건강 보험을 선택할 수 있도록 합니다.

**실습 목표**

이 실습에서 다음 작업에서 수행할 것입니다.

- 작업 1: 간단한 AI 에이전트를 생성하기

## 작업 1: 간단한 AI 에이전트를 생성하기

이 작업에서는 Azure AI 서비스를 사용해 다양한 건겅 보험 플랜을 비교하는
막대 차트를 생성하는 간단한 AI 에이전트를 구축하여 분석 및 시각화를
수행합니다.

1.  **Lab 2 - Create A Simple AI Agent.ipynb** 파일을 여세요. 이 **Lab
    2 - Create A Simple AI Agent.ipynb** 노트북은 데이터를 처리하고
    다양한 건강 보험 플랜을 비교하는 막대 차트를 생성하는 간단한 AI
    에이전트를 생성하는 방법을 안내합니다

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  오른쪽 상단에 있는 **Select kernel** 설정을 선택하세요. 목록에서
    **venv (Python 3.x.x)**를 선택하세요.

![A blue and red rectangle with white text AI-generated content may be
incorrect.](./media/image2.png)

3.  아래 셀을 실행하여 Azure AI 프로젝트 작업에 필요한 라이브러리를
    가져오고 환경 변수를 로드하세요. 이 설정은 Azure AI 서비스와의
    안전한 인증 및 상호작용을 가능하게 합니다.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image3.png)

4.  아래 셀을 실행하여 Microsoft Foundry 프로젝트에 연결하고 배포된
    **gpt-4o** 모델에 접근하세요 . 이 방법은 프로젝트 연결 문자열과
    Azure 자격 증명을 사용하여 안전한 연결을 설정합니다.

![A screen shot of a computer AI-generated content may be
incorrect.](./media/image4.png)

5.  이 셀을 실행하면 Microsoft Foundry를 사용해 다양한 건강 보험 플랜을
    비교하는 막대 차트를 생성하는 **simple AI agent**를 생성하세요 . 이
    스크립트는 AI 에이전트를 초기화하고, 건강 보험 데이터를 포함한
    프롬프트를 보내며, 막대 차트를 요청합니다. 에이전트는 요청을
    처리하고, 차트를 생성하며, 이미지 파일을 저장한 후 에이전트를
    삭제하여 정리합니다.

![A screen shot of a computer program AI-generated content may be
incorrect.](./media/image5.png)

6.  마지막으로 출력을 관찰하세요.

![A screenshot of a computer program AI-generated content may be
incorrect.](./media/image6.png)

**요약**

이 실습에서는 Microsoft Foundry와 Azure AI 서비스를 활용해 건강보험 플랜
분석 AI 에이전트를 성공적으로 구축하여 건강 보험 플랜의 분석 및 비교를
자동화했습니다. Microsoft Foundry 프로젝트에 연결하는 방법, 배포된
GPT-4o 모델에 접근하는 방법, 복잡한 건강 보험 계획을 처리하는 지능형
에이전트를 생성하는 방법을 배웠습니다. 상담원은 플랜 세부사항을
해석하고, 보장 옵션을 분석하며, 보험 플랜 간 차이를 시각화하기 위해
자동으로 비교 막대 차트를 생성했습니다. 이 실습 경험은 AI 에이전트가
데이터 분석을 간소화하고, 의사결정 과정을 지원하며, 사용자가 건강 보험
옵션을 쉽게 이해하고 비교할 수 있도록 돕는 방법을 보여주었습니다.

이 실습을 성공적으로 완료했습니다. 계속하려면 Next \>\>를 클릭하세요.
