# Laboratório 12: Implementação de AI responsável e segurança de conteúdo em agentes de AI corporativos

**Duração estimada:** 15 minutos

**Visão geral**

Neste laboratório, os participantes exploram a importância da AI
responsável em sistemas de agentes de nível empresarial. Eles
compreenderão como a Microsoft integra princípios de AI responsável —
como equidade, segurança, responsabilidade e transparência — na
Estrutura de Agentes e no Microsoft Foundry. Os participantes também
aprenderão a configurar e validar filtros de segurança de conteúdo
diretamente no portal do Microsoft Foundry para garantir que os agentes
implementados respondam de forma ética e segura.

**Objetivos do Laboratório**

Você executará as seguintes tarefas neste laboratório:

- Tarefa 1: Compreender a AI responsável e a segurança do conteúdo

- Tarefa 2: Configurar e validar filtros de conteúdo no Microsoft
  Foundry

## Tarefa 1: Compreender a AI responsável e a segurança do conteúdo \[Somente leitura\]

Nesta tarefa, você aprenderá sobre os princípios de AI responsável da
Microsoft e compreenderá como eles se aplicam ao Microsoft Agent
Framework e ao Microsoft Foundry. A AI responsável garante que os
sistemas inteligentes se comportem de forma segura, ética e justa, um
requisito crítico ao implementar soluções multiagentes em ambientes
empresariais.

O que é AI responsável?

1.  A estrutura de AI responsável da Microsoft baseia-se em seis
    princípios fundamentais que orientam o desenvolvimento, a
    implantação e a operação de sistemas de AI:

2.  Equidade – Os sistemas de AI devem tratar todos os indivíduos e
    grupos de forma justa. Para os agentes empresariais, isso significa
    garantir que as decisões ou respostas não reflitam ou ampliem vieses
    em cenários de RH, conformidade ou finanças.

3.  Confiabilidade e segurança – Os modelos de AI devem ter um
    desempenho consistente e lidar adequadamente com falhas. Os agentes
    devem retornar informações factuais e verificáveis e evitar
    resultados inseguros ou enganosos.

4.  Privacidade e Segurança – Sistemas de AI devem proteger dados dos
    usuários e informações organizacionais. O Agent Framework se integra
    de forma segura ao Azure Identity (Entra ID) e respeita os limites
    de dados corporativos.

5.  Inclusão – Os agentes devem ser projetados para capacitar todos os
    usuários e oferecer suporte à acessibilidade em diferentes idiomas,
    regiões e contextos culturais.

6.  Transparência – Os usuários devem compreender como as decisões de AI
    são tomadas. Sempre que possível, os agentes devem explicar seu
    raciocínio e fornecer respostas rastreáveis por meio de telemetria e
    observabilidade.

7.  Responsabilidade – A supervisão humana permanece essencial. As
    organizações devem definir estruturas de governança para revisar e
    gerenciar resultados gerados por AI.

8.  Esses princípios formam a base para a criação de agentes de AI
    responsável, confiáveis e compatíveis em um contexto corporativo.

Por que a AI responsável é importante para os agentes corporativos

> 1\. Quando vários agentes colaboram para lidar com assuntos sensíveis
> — como políticas de funcionários, reembolso financeiro ou relatórios
> de conformidade — o risco de desinformação, viés ou comportamento
> inadequado aumenta. Ao incorporar práticas de AI responsável, as
> organizações podem:
>
> 2\. Garantir consistência e confiabilidade na comunicação entre
> agentes.
>
> 3\. Evitar resultados prejudiciais, discriminatórios ou inseguros.
>
> 4\. Manter a conformidade com regulamentações globais (GDPR, HIPAA,
> ISO 27001, etc.).
>
> 5\. Reforçar a confiança do usuário na automação alimentada por AI.
>
> 6\. O Microsoft Agent Framework inclui integrações nativas para AI
> responsável por meio do Microsoft Foundry — fornecendo governança,
> rastreabilidade e aplicação de segurança diretamente no nível do
> modelo e da implementação.

Segurança do conteúdo e filtragem de respostas éticas

1.  A segurança de conteúdo é um componente essencial da infraestrutura
    de AI responsável da Microsoft.

2.  No Microsoft Foundry, os filtros de segurança de conteúdo detectam e
    bloqueiam automaticamente resultados prejudiciais ou sensíveis em
    várias categorias, incluindo:

    - Ódio e assédio

    - Violência e automutilação

    - Conteúdo sexual

    - Informações sensíveis ou Protected Information (PII)

3.  Quando configurados, esses filtros interceptam tanto as solicitações
    do usuário quanto as respostas do modelo, substituindo o conteúdo
    inseguro por respostas seguras padronizadas, garantindo a
    conformidade e a proteção do usuário sem modificar seu código local.

## Tarefa 2: Configurar e validar filtros de conteúdo no Microsoft Foundry

Nesta tarefa, você irá configurar filtros de segurança de conteúdo no
Microsoft Foundry para aplicar os princípios de AI responsável aos seus
agentes implementados. Você definirá níveis de proteção para categorias
nocivas ou sensíveis e, em seguida, validará os filtros testando seus
agentes locais.

1.  Na página do Microsoft Foundry, no menu à esquerda, selecione
    **Guardrails + Controls** e clique em **Create a custom content
    filter** na página Overview.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image1.png)

2.  No painel seguinte, mantenha o nome padrão do filtro e clique em
    **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image2.png)

3.  No painel **set input filter**, você observará que o Azure já
    pré-configurou vários parâmetros, como Ódio, Violência, Conteúdo
    Sexual e outros. Esses filtros básicos são aplicados por padrão para
    garantir a conformidade com os padrões de AI responsável da
    Microsoft.

4.  Agora, ajuste cada categoria para **Highest Blocking** para impor o
    nível máximo de segurança de conteúdo, um requisito essencial para
    agentes de nível corporativo, e clique em **Next** para aplicar a
    configuração.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image3.png)

5.  No painel seguinte, configure todas as categorias como **Highest
    Blocking** e clique em **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image4.png)

6.  No painel **Deployments**, selecione o modelo **gpt-4o-mini** para
    adicionar o filtro de conteúdo e clique em **Next**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image5.png)

> **Observação:** Se a janela pop-up **Replacing existing content
> filter** for exibida, selecione **Replace**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image6.png)

7.  No painel de revisão, clique em **Create filter**.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image7.png)

8.  O filtro de conteúdo personalizado será criado com sucesso.

![A screenshot of a computer AI-generated content may be
incorrect.](./media/image8.png)

9.  Retorne ao Visual Studio Code e, no terminal aberto anteriormente,
    execute o comando abaixo para iniciar o agente. Forneça os prompts a
    seguir para testar a capacidade de segurança de conteúdo:

- Escreva uma declaração ofensiva sobre um grupo de pessoas.

- Diga-me algo ofensivo sobre um grupo de pessoas.

+++python main.py –interactive+++

> ![A screenshot of a computer AI-generated content may be
> incorrect.](./media/image9.png)

**Resumo**

Neste laboratório, você explorou como os princípios da AI responsável
orientam o desenvolvimento ético, compatível e confiável de agentes de
AI usando o Microsoft Agent Framework. Você configurou filtros de
segurança de conteúdo no Microsoft Foundry para impedir automaticamente
que conteúdo perigoso, discriminatório ou impróprio seja processado ou
retornado pelos agentes da sua empresa.

Você concluiu este laboratório com sucesso. Clique em Next \>\> para
continuar.
