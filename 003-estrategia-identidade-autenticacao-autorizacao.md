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



## Decisão

A mudança que estamos propondo ou concordamos em implementar.

## Consequências

O que se torna mais fácil ou mais difícil de fazer e quaisquer riscos introduzidos pela mudança que precisarão ser mitigados.

## Referências

1. [How To | Tipos de autenticação e consumo de Web Services e APIs](https://youtu.be/jEihJwXyxTs) do canal TOTVS Gestão no YouTube
2. [How To | Configuração para disponibilização de Web Services e APIs](https://www.youtube.com/watch?v=gPSY1_xxKL4) do canal TOTVS Gestão no YouTube
3. [Token - 1.000](https://api.totvs.com.br/apidetails/Token_v1_000.json)
4. [Autenticação do TOTVS Protheus na Base (parte 2)](https://base.com/pt-BR/ajuda/knowledgebase/autenticacao-do-totvs-protheus-na-base-parte-2/)
5. [As diferenças entre autenticação e autorização no desenvolvimento de software](https://dnxbrasil.com/autenticacao-e-autorizacao-no-desenvolvimento-de-software/)
6. [Autenticação e Autorização](https://profbruno-ufc-qx.github.io/2024.2-fundamentals-of-web-programming/assets/downloads/15-Autentica%C3%A7%C3%A3o-e-Autoriza%C3%A7%C3%A3o.pdf) por professor Bruno Góis Mateus, Universidade Federal do Ceará