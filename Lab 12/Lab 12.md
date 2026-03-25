# 실습 12: 기업용 AI 에이전트에 Responsible AI 및 콘텐츠 안전 구현하기

**예상 소요 시간**: 15분

**개요**

이 실습에서 참가자들은 엔터프라이즈급 에이전트 시스템에서 Responsible
AI의 중요성을 탐구합니다. 그들은 공정성, 안전, 책임성, 투명성 등
Responsible AI 원칙을 에이전트 프레임워크와 Microsoft Foundry에 통합하는
방식을 이해하게 될 것입니다. 참가자들은 또한 Microsoft Foundry 포털에서
콘텐츠 안전 필터를 직접 설정하고 검증하는 방법을 배워, 배포된 에이전트가
윤리적이고 안전하게 응답하도록 보장합니다.

**실습 목표**

이 실습에서 다음과 같은 작업을 수행하게 됩니다.

- 작업 1: Responsible AI 및 콘텐츠 안전 이해하기

- 작업 2: Microsoft Foundry의 콘텐츠 필터를 구성 및 검증하기

## 작업 1: Responsible AI 및 콘텐츠 안전 이해하기 \[읽기 – 전용\]

이 과제에서는 Microsoft의 Responsible AI 원칙과 그것이 Microsoft
에이전트 프레임워크 및 Microsoft Foundry에 어떻게 적용되는지 배우게
됩니다. Responsible AI는 지능형 시스템이 안전하고 윤리적이며 공정하게
동작하도록 보장하며, 이는 기업 환경에서 다중 에이전트 솔루션을 배포할 때
매우 중요한 요구사항입니다.

Responsible AI란?

1.  Microsoft의 Responsible AI 프레임워크는 AI 시스템의 개발, 배포 및
    운영을 안내하는 여섯 가지 기본 원칙에 기반합니다:

2.  공정성 – AI 시스템은 모든 개인과 집단을 공평하게 대해야 합니다. 기업
    에이전트의 경우, 이는 인사, 컴플라이언스, 재무 사용 사례에서
    의사결정이나 대응이 편향을 반영하거나 증폭하지 않도록 하는 것을
    의미합니다.

3.  신뢰성과 안전성 – AI 모델은 일관되게 작동하고 고장 시에도 우아하게
    처리해야 합니다. 에이전트는 사실적이고 검증 가능한 정보를 반환하고
    안전하지 않거나 오해의 소지가 있는 출력물을 피해야 합니다.

4.  프라이버시 및 보안 – AI 시스템은 사용자 데이터와 조직 정보를
    보호해야 합니다. 에이전트 프레임워크는 Azure Identity(Entra ID)와
    안전하게 통합되며 엔터프라이즈 데이터 경계를 존중합니다.

5.  포용성 – 에이전트는 모든 사용자를 권한 부여하고 언어, 지리, 배경을
    넘어 접근성을 지원하도록 설계되어야 합니다.

6.  투명성 – 사용자는 AI 의사결정이 어떻게 이루어지는지 이해해야 합니다.
    에이전트는 가능한 경우 자신의 이유를 설명하고, 텔레메트리와 관찰
    가능성을 통해 추적 가능한 응답을 제공해야 합니다.

7.  책임성 – 인간의 감독이 여전히 핵심입니다. 조직은 AI 기반 결과를
    검토하고 관리할 거버넌스 구조를 정의해야 합니다.

8.  이 원칙들은 기업 환경에서 신뢰할 수 있고 준수하는 AI 에이전트를
    구축하는 기반이 됩니다.

기업 에이전트에서 Responsible AI가 중요한 이유

1.  여러 에이전트가 협력하여 직원 정책, 재정 보상, 준수 보고 등 민감한
    주제를 다룰 경우 허위 정보, 편향, 부적절한 행동 위험이 증가합니다.
    Responsible AI 실천을 내재화함으로써 조직은 다음과 같은 역할을 할 수
    있습니다:

2.  에이전트 간 통신의 일관성과 신뢰성을 보장합니다.

3.  해롭거나 차별적이거나 안전하지 않은 출력물을 방지합니다.

4.  글로벌 규정 (GDPR, HIPAA, ISO 27001 등)을 준수합니다.

5.  AI 기반 자동화에 대한 사용자 신뢰를 강화하세요.

6.  Microsoft Agent Framework는 Microsoft Foundry를 통한 Responsible
    AI를 위한 네이티브 통합을 포함하며, 모델 및 배포 수준에서 직접
    거버넌스, 추적성, 안전 집행을 제공합니다.

콘텐츠 안전 및 윤리적 응답 필터링

1.  콘텐츠 안전은 Microsoft의 Responsible AI 인프라의 핵심 요소입니다.

2.  Microsoft Foundry에서는 콘텐츠 안전 필터가 여러 카테고리에서
    유해하거나 민감한 출력을 자동으로 감지하고 차단합니다.:

    - 증오와 괴롭힘

    - 폭력과 자해

    - 성적 내용

    - 민감 또는 Protected Information (PII)

3.  이 필터들은 설정 시 사용자 프롬프트와 모델 응답을 모두 가로채
    안전하지 않은 콘텐츠를 표준화된 안전한 응답으로 대체하여 지역 코드를
    변경하지 않고도 준수와 사용자 보호를 보장합니다.

## 작업 2: Microsoft Foundry의 콘텐츠 필터를 구성 및 검증하기

이 작업에서는 Microsoft Foundry에서 콘텐츠 안전 필터를 설정하여 배포된
에이전트에 Responsible AI 원칙을 강제합니다. 유해하거나 민감한
카테고리에 대한 보호 수준을 설정한 후, 지역 에이전트를 테스트하여 필터를
검증할 것입니다.

1.  왼쪽 메뉴의 Microsoft Foundry 페이지에서 **Guardrails + Controls**을
    선택하고 개요에서 **Create a custom content filter**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  다음 창에서 필터 기본 이름을 그대로 두고**Next**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  **set input filter**창에서는 Azure가 이미 증오, 폭력, 성적 콘텐츠 등
    여러 매개변수를 미리 설정해 두었다는 것을 알 수 있습니다. 이 기본
    필터들은 기본적으로 적용되어 Microsoft의 Responsible AI 기준 준수를
    보장합니다.

4.  이제 각 카테고리를 최대 콘텐츠 안전성을 강제하기 위해 **Highest
    Blocking**으로 조정한 후 **Next**를 클릭하여 설정을 적용하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  다음 창에서 모든 카토고리를 **Highest Blocking**로 구성하고
    **Next**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  **Deployments** 창에서 내용 필터를 추가하려면 **gpt-40-mini** 모델을
    클릭하세요. **Next**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

> **참고:** **Replacing existing content filter** 팝업 창이 열리면
> **Replace**를 선택하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

7.  Review 창에서 **Create filter**를 클릭하세요.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

8.  이제 맞춤형 콘텐츠 필터가 성공적으로 생성되었습니다.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

9.  이전에 열렸던 터미널에서 Visual Studio Code 창으로 돌아가 다음
    명령을 실행해 에이전트를 시작하세요. 아래 안내문을 제공해 콘텐츠
    필터링 기능을 확인하세요.

- 한 집단에 대해 불쾌한 진술을 작성하세요.

- 어떤 사람들에 대해 증오스러운 점을 말해 주세요.

+++python main.py –interactive+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

**요약**

이 연구실에서는 Responsible AI 원칙이 Microsoft Agent 프레임워크를
활용한 윤리적이고 준수하며 신뢰할 수 있는 AI 에이전트 개발을 어떻게
안내하는지 탐구하셨습니다. Microsoft Foundry에서 콘텐츠 안전 필터를
설정하여 기업 에이전트가 처리하거나 반환하는 것을 자동으로 방지했습니다.

이 실습을 성공적으로 완료했습니다. 계속하려면 Next \>\>를 클릭하세요.
