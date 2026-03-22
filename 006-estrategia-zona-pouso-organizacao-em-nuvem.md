# 006. Estratégia de Zona de Pouso e Organização em Nuvem

Data: 2026-03-22

Autor: Dickson Souza em nome do Grupo 1

## Status

Proposto

## Contexto

A modernização do Sistema Acadêmico Legado envolve uma abordagem incremental, usando o padrão Strangler Fig (figueira estranguladora), proposto como estratégia geral de modernização no ADR-001. Em termos de tecnologia de hospedagem também involve simultaneamente a migração de servidores on premises para o uso de computação em nuvem. Ambos aspectos (abordagem adotada e migração de hospedagem) apresentam benefícios, como a maior confiança na assertividade da modernização, por um lado, e a possibilidade de escalar a infraestrutura quando a demanda exigir, por outro, mas trazem riscos de fragmentação do ecossistema de aplicações e recursos de infraestrutura durante esse processo. Esses riscos podem tanto ser de baixa reprodutibilidade na criação dos ambientes e recursos de nuvem quanto da exposição desnecessária à ameaças de segurança e imprevisibilidade de custos.

As plataformas de computação em nuvem, de forma geral, e a Azure e AWS, em particular, por serem os provedores já selecionados para esse projeto de modernização, oferecem recomendações para contrabalancear essa flexibilidade oferecida pela nuvem com a governança necessária para o sucesso da modernização.

Essas recomendações são usualmente materializadas sob a forma de `landing zones` (zonas de pouso / aterrisagem em tradução literal). De origem militar, representando uma região onde aeronaves poderiam pousar, o termo representa o local onde os recursos usados por uma aplicação, computação, armazenamento, redes, firewalls, dentre outros, serão hospedados. A zona de pouso é assim o local devidamente preparado para a operacionalização de um aplicação no ambiente de computação em nuvem.

A documentação da Azure descreve assim o que é uma `landing zone`:

> An Azure landing zone is the standardized and recommended approach for all organizations utilizing Azure. It provides a consistent way to set up and manage your Azure environment at scale. It ensures consistency across your organization by aligning with key requirements for security, compliance, and operational efficiency through platform and application landing zones. They provide a well-architected foundation aligned with core design principles across eight design areas. (Disponível em https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)

Enquanto que a AWS usa as seguintes descrições para uma `landing zone`:

> A landing zone is a well-architected, multi-account AWS environment that is scalable and secure. This is a starting point from which your organization can quickly launch and deploy workloads and applications with confidence in your security and infrastructure environment. Building a landing zone involves technical and business decisions to be made across account structure, networking, security, and access management in accordance with your organization’s growth and business goals for the future. (Disponível em https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html)

## Decisão

O primeiro aspecto a ser decidido na estratégia de zonas de pouso é a divisão de recursos em contas. Essa divisão garante a segregação de recursos para redução do raio de impacto, em caso de ataques de agentes mal-intencionados, e a possibilidade de controle refinado de acessos.

Na Azure, essa divisão é feita por meio de `subscriptions`, vinculados a um `tenant` único do Microsoft Entra, que é também o responsável por centralizar os grupos de gerenciamento (`Management groups`) e políticas (`Azure policies`).

Na AWS, as organizações AWS (`AWS Organizations`) permitem a gestão das diferentes contas em unidades organizacionais. Uma primeira conta precisa ser definida como conta de gestão.

Independente do provedor de nuvem selecionado, as seguintes diretrizes devem ser seguidas:

1. Cada aplicação terá suas contas / `subscriptions`;
2. As contas serão divididas por ambiente: Desenvolvimento, Homologação e Produção;
   1. Nos casos em que o escopo não exigir (times reduzidos ou baixa criticidade), o ambiente de homologação pode ser fundido com o ambiente de Desenvolvimento para otimizar custos de nuvem.
   2. Ou podem ser instanciados quando necessários e desligados/destruídos/descomissionados quando inativos
3. No escopo das contas, os recursos serão segmentados conforme a possibilidade oferecida pelo provedor:
   1. Na Azure, recursos similares (ambiente, monitoramento, gestão) serão agrupados em diferentes grupos de recursos (`resource groups`) sob uma dada conta;
      1. Recursos sob a mesma conta podem ser comunicar e interagir diretamente; não existe restrição para comunicação / descoberta entre diferentes grupos de recursos sob a mesma conta.
   2. Na AWS, existe o equivalente sob o nome de AWS Resource groups que possui funcionalidades similares às da Azure.

O acesso a cada uma das contas deve ser controlado de forma a conceder permissões somente aos desenvolvedores e líderes técnicos diretamente envolvidos no ciclo de desenvolvimento, incluindo a equipe de plataforma, se porventura vier a existir, e aos administradores de sistema da TI, que são, em última instância, os responsáveis pelo bom funcionamento da infraestrutura provisionada para o Sistema Acadêmico Modernizado da universidade.

Nenhum recurso deve ser exposto diretamente à internet sem as devidas medidas de segurança descritas a seguir. O acesso deve ser intermediado via um Web Application Firewall do provedor de nuvem e as APIs/microserviços devem ser expostas via um serviço de gerenciamento de API / load balancer / proxy reverso. Essa definição estará nas políticas de cada conta criada (via `Azure policies` ou `AWS Managed policies`), assegurando aderência às regras mais estritas de segurança desde a criação do recurso. Bancos de dados, contas de armazenamento, máquinas virtuais, cofres de senha e outros recursos poderão se comunicar por redes virtuais privadas e é também necessário dar acesso à VPN corporativa da universidade às redes virtuais no provedor de nuvem para prover acesso operacional.

Um aspecto adicional é a regra de etiquetagem dos recursos para adequado rastreamento dos responsáveis diretos pela aplicação e para distribuição correta de custos pelos diferentes departamentos, caso isso se torne necessário. As tags obrigatórias são:

1. Nome do projeto;
2. Tipo do ambiente (Desenvolvimento, Homologação ou Produção);
3. Nome do time responsável;
4. Nome do lider técnico atual;
5. Lista de e-mail para comunicação com o líder técnico / líderes técnicos;
6. Centro de custo

É fundamental aplicar o conceito de herança de tags para propagar as tags para os recursos hospedados em cada `resource group`. Essa capacidade está disponível tanto na Azure quanto na AWS (ver referências 3 e 4).

Para usufruir de todos os benefícios das `landing zones`, é essencial usar o conceito de infraestrutura como código (`Infrastructure as code`, IaC) para assegurar completa aderência à todas as políticas bem como capacidade de auditoria e reproducibilidade. Embora a disponibilidade seja requisito importante, especialmente nos períodos de pico de demanda, o uso de multi-regiões não será adotado mas sim uma estratégia de zonas de disponibilidade na mesma região com o conceito ativo/passivo para suportar elevações momentâneas da demanda da comunidade acadêmica.

Por fim, para garantir governança sobre todo o ciclo de desenvolvimento e operação, o provisionamento de recursos e alteração dos mesmos em Produção não será permitido ser executado por vias manuais, seja no portal do provedor de nuvem ou via CLI usando autenticação individual. Apenas execução por identidades ligadas à solução de pipeline selecionada (`Azure DevOps` ou `AWS CodePipeline / CodeBuild / CodeDeploy`) será permitida para esse tipo de ambiente. No ambiente de Desenvolvimento, algumas operações manuais serão permitidas via políticas para facilitar a experimentação e testes; exceções feitas à modificações de aspectos de segurança dos recursos e/ou grupos de recursos.

## Consequências

O uso das zonas de pouso (`landing zones`) como descrito na seção anterior traz como benefícios a padronização das práticas de provisionamento de infraestrutura e operação das aplicações do Sistema Acadêmico Modernizado. Por oferecer uma base de referência com os mecanismos de segurança necessários (restrição de visibilidade de recursos nas redes privadas e redução de exposição à internet pública, uso de Web Application Firewall como intermediário à comunicação dos usuários com os recursos do sistema), a superfície de ataque é bastante reduzida e os esforços passam a ser concentrados nas bordas do sistema e em aspectos não cobertos nessa camada, como, por exemplo, tratamento de SQL injection, processamento seguro de arquivos carregados, autenticação e autorização (ver ADR-003).

Um ponto negativo é a necessidade de construção das contas, políticas e pipelines iniciais para as primeiras aplicações a serem migradas, envolvendo eventualmente times diferentes com prioridades e demandas distintas (como times de Identidade e Plataforma) e também a exigência de conhecimento pelos times responsável pelas aplicações na gestão e operação das `landing zones`. As restrições à alterações manuais podem também ser percebidas como bloqueios ou pontos de atrito, sendo eventualmente minimizadas pelo adequado treinamento e documentação dos canais de solicitação aos times de Plataforma.

# Referências:

1. [What is an Azure landing zone?](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/)
2. [What is a landing zone?](https://docs.aws.amazon.com/prescriptive-guidance/latest/migration-aws-environment/understanding-landing-zones.html) na documentação da AWS
3. [Inheritance examples](https://docs.aws.amazon.com/organizations/latest/userguide/inheritance-examples.html)
4. [Group and allocate costs using tag inheritance](https://learn.microsoft.com/en-us/azure/cost-management-billing/costs/enable-tag-inheritance)
5. [DevOps and AWS](https://aws.amazon.com/devops/)
6. [Azure Master Class v3 - Part 6 - Networking](https://www.youtube.com/watch?v=nDtCSQyG_I8) by John Savill
7. [What are resource groups?](https://docs.aws.amazon.com/ARG/latest/userguide/resource-groups.html)
8. [Azure landing zones and multiple Microsoft Entra tenants](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/ready/landing-zone/design-area/multi-tenant/overview)
9. [Best practices for the management account](https://docs.aws.amazon.com/organizations/latest/userguide/orgs_best-practices_mgmt-acct.html)
10. [AWS Organizations](https://aws.amazon.com/organizations/)
11. [Organizing Your AWS Environment Using Multiple Accounts](https://docs.aws.amazon.com/whitepapers/latest/organizing-your-aws-environment/organizing-your-aws-environment.html)
12. [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html)
13. [COST02-BP03 Implement an account structure](https://docs.aws.amazon.com/wellarchitected/2023-04-10/framework/cost_govern_usage_account_structure.html)
14. [Landing zone](https://en.wikipedia.org/wiki/Landing_zone) - military sense
15. [Understanding Landing Zones in Cloud Environments](https://cloudericks.com/blog/understanding-landing-zones-in-cloud-environments/)