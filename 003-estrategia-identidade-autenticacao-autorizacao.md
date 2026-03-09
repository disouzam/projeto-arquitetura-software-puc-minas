# 003. Estratégia de Identidade, Autenticação e Autorização, incluindo SSO

Data: 2026-03-08
Autor: Dickson Souza em nome do Grupo 1

## Status

Proposto

## Contexto

O ponto central e mais complicado do padrão Strangler Fig (figueira estranguladora), proposto como estratégia geral de modernização no ADR-001, é a construção da fachada entre os consumidores do sistema legado e o sistema de backend (ver discussão didática sobre esse aspecto em [The Strangler Pattern | Designing Event-Driven Microservices](https://www.youtube.com/watch?v=BJiFd2KjRYs) no canal da Confluent). Como apenas o sistema acadêmico será substituído enquanto o ERP da TOTVS e o LMS Canvas permanecerão, com adaptações, assume-se aqui, como parte do contexto, que a primeira versão da fachada irá cobrir apenas o sistema acadêmico. Assim, todas as requisições direcionadas ao sistema acadêmico passarão primeiro pela fachada, que num primeiro momento, fará o papel apenas de meio de intercâmbio de dados e nada mais.

Hoje cada um dos três grandes sistemas (acadêmico, ERP e LMS) contam com suas próprias soluções de autenticação e autorização, implicando em cadastros duplicados, senhas e mecanismos de proteção distintos - eventualmente conflitantes. Não existe integração entre sistemas de autorização e autenticação entre os sistemas, ou seja, usuários que necessitam dos 3 sistemas, por exemplo, técnicos administrativos precisam lidar com senhas e nomes de usuários diferentes em cada um deles, levando eventualmente à comportamentos inseguros como uso de mesmas senhas nos diferentes sistemas para facilitar a memorização e conferir rapidez de acesso.

Embora o sistema COBOL seja o menos adaptado a sistemas de autenticação e autorização modernos, essa não é uma preocupação relevante visto que a fachada será responsável por intermediar essa responsabilidade durante a transição para o Sistema Acadêmico modernizado.

O ERP TOTVS suporta diversos tipos de autenticação conforme consulta feita via Copilot integrado ao navegador e suportado parcialmente pelas referências 1 a 4 listadas no final desse ADR. Os detalhes específicos podem variar de acordo com a versão do ERP e produto implementado (Protheus, RM ou Datasul). Nesse estágio da pesquisa de alternativas não se conseguiu localizar uma referência centralizada da própria TOTVS. O sumário produzido pelo Copilot segue abaixo para referência inicial apenas.

**Tabela 1**: Resumo dos mecanismos de autenticação suportados pelo ERP TOTVS
| ERP TOTVS | Senha nativa | LDAP/AD | SSO (SAML/OIDC) | TOTVS ID | Tokens API |
|-----------|--------------|---------|------------------|----------|------------|
| **Protheus** | ✔️ | ✔️ | ✔️ | ✔️ | ✔️ |
| **RM** | ✔️ | ✔️ | ✔️ (SAML) | ✔️ | ✔️ |
| **Datasul** | ✔️ | ✔️ | ✔️ (dependendo da versão) | ✔️ | ✔️ |

\* Fontes primárias não foram localizadas para confirmar a autenticidade dessa tabela

O sistema de LMS Canvas, da Instructure, suporta, segundo sua documentação, diversos provedores de identidade e diversos protocolos de autenticação / autorização e gestão de diretório (OAuth, LDAP, SAML). Um dos documentos consultados é a referência 8 da documentação da Instructure. Outros documentos estão disponíveis na referência 9 de documentos de integração. Essa diversidade de formas de integração traz flexibilidade do lado da plataforma Canvas.

Assim, temos de um lado um sistema potencialmente incompatível com sistemas de autenticação moderna cuja saída será fornecida via fachada (sistema acadêmico legado em COBOL), o Canvas, aparentemente bastante flexível em alternativas, e o TOTVS com algumas possibilidades modernas mas sem uma documentação centralizada que forneça clareza para essa decisão. Note no entanto que essa descrição ainda não define a solução, passo que será dado a seguir.

## Decisão

Como o Sistema Acadêmico Modernizado será hospedado em recursos de computação em nuvem seja da AWS ou da Azure (ou mesmo ambos para redundância e gestão de custos) é sensato escolher um provedor de identidade hospedado ou compatível com ambas.

Como as opções são bastante amplas nesse segmento de tecnologias e a gestão independente pode gerar uma demanda extra para os times de tecnologia, soluções que necessitam de provisionamento e configuração especializada como o IdentityServer não serão consideradas, além do fato de não ser mais uma solução gratuita e open source, removendo o apelo para esse tipo bastante robusto de solução.

Soluções de terceiros como a Okta também não serão inicialmente consideradas pelas mesmas razões, além de adicionarem mais um provedor na cadeia de suprimentos da modernização do sistema acadêmico.

Assim, esses argumentos nos levam para uma lista preliminar de dois candidatos: a família Microsoft Entra de produtos de identidade ou a família AWS Identity Services.

O resumo oferecido pelo Copilot indica uma alta paridade de funcionalidades das duas soluções com favorecimento do Microsoft Entra ID com uma solução mais integrada em um único produto. Niraj Kumar fornece um resumo interessante (referência 11) da comparação entre as duas principais ferramentas:

- Microsoft Entra ID is primarily an **`Identity Provider (IdP)`**. It was born from the enterprise directory (Active Directory).
- AWS IAM is primarily an **`Access Management System`**. It was born from the API (infrastructure control).

Sugere-se então adotar o Microsoft Entra ID como solução integrada de autenticação e autorização, oferecendo flexibilidade para as integrações com a fachada para o sistema acadêmico legado, ERP TOTVS e LMS Canvas. Como mencionado na seção sobre o contexto, deve-se iniciar o processo de implementação através da fachada. Esse processo requer o mapeamento dos papéis e permissões, usando principalmente o conceito RBAC (role-based access control), a serem expostos na fachada e que correspondam aos serviços atuais do sistema acadêmico legado.

A integração via SSO no ERP TOTVS e LMS Canvas requer ajustes do lado dessas duas aplicações para processamento dos tokens gerados pelo Microsoft Entra ID. Os ajustes incluem o processamento de tokens de acesso emitidos pelo Microsoft Entra ID com suas claims customizadas para as duas aplicações (ver referência 12) - abordagem recomendada por permitir a gestão centralizada de perfis de acesso apenas no Microsoft Entra ID.

## Consequências

O que se torna mais fácil ou mais difícil de fazer e quaisquer riscos introduzidos pela mudança que precisarão ser mitigados.

## Referências

1. [Tipos de autenticação e consumo de Web Services e APIs](https://youtu.be/jEihJwXyxTs) do canal TOTVS Gestão no YouTube
2. [Configuração para disponibilização de Web Services e APIs](https://www.youtube.com/watch?v=gPSY1_xxKL4) do canal TOTVS Gestão no YouTube
3. [Token - 1.000](https://api.totvs.com.br/apidetails/Token_v1_000.json)
4. [Autenticação do TOTVS Protheus na Base (parte 2)](https://base.com/pt-BR/ajuda/knowledgebase/autenticacao-do-totvs-protheus-na-base-parte-2/)
5. [As diferenças entre autenticação e autorização no desenvolvimento de software](https://dnxbrasil.com/autenticacao-e-autorizacao-no-desenvolvimento-de-software/)
6. [Autenticação e Autorização](https://profbruno-ufc-qx.github.io/2024.2-fundamentals-of-web-programming/assets/downloads/15-Autentica%C3%A7%C3%A3o-e-Autoriza%C3%A7%C3%A3o.pdf) por professor Bruno Góis Mateus, Universidade Federal do Ceará
7. [Modern Authentication - Autorização e Autenticação: conceitos e aplicações na plataforma Entra ID](https://techcommunity.microsoft.com/blog/desenvolvedoresbr/modern-authentication---autoriza%C3%A7%C3%A3o-e-autentica%C3%A7%C3%A3o-conceitos-e-aplica%C3%A7%C3%B5es-na-pla/4277388) por Pedro Soucheff
8. [Configuring Microsoft OAuth for Canvas Authentication](https://community.instructure.com/en/kb/articles/606219-configuring-microsoft-oauth-for-canvas-authentication)
9. [Canvas Integration Documents](https://community.instructure.com/en/kb/categories/475-canvas-integration-documents)
10. [Comparar soluções de gerenciamento de identidade do AWS e do Azure](https://learn.microsoft.com/pt-br/azure/architecture/aws-professional/security-identity)
11. [The Definitive Comparision Guide: Microsoft Entra ID vs. AWS IAM](https://towardsaws.com/the-definitive-comparision-guide-microsoft-entra-id-vs-aws-iam-afdc036b6fca)
12. [Understanding OAuth 2 Access Token Claims](https://www.cerberauth.com/blog/oauth2-access-token-claims/#custom-claims)