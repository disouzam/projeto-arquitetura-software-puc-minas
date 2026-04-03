# Relatório de lacunas arquiteturais

Autor: Dickson Souza em nome do Grupo 1

Data: 2026-04-03

# Introdução

A preparação para a modernização do Sistema Acadêmico Legado, substituindo-o pelo Sistema Acadêmico Modernizado, segue avançando com os estudos e definições de estratégias. Até o momento, registros de decisão arquitetural foram criados, diagramas C4 do estado atual e do estado futuro, blueprint da arquitetura de soluções e modelo técnico de referência. No plano dos registros de decisão arquitetural (ADRs), os seguintes temas foram cobertos:

1. ADR-001: Estratégia geral de modernização
2. ADR-002: Estratégia de Tratamento do Legado
3. ADR-003: Estratégia de Identidade, Autenticação e Autorização
4. ADR-004: Estratégia de Governança, Proteção e Auditoria de Dados
5. ADR-005: Estratégia de Arquitetura Técnica (TRM)
6. ADR-006: Estratégia de Zona de Pouso e Organização em Nuvem

Nesse ponto, é objetivo desse documento atual relacionar as lacunas arquiteturais entre o estado atual e o estado futuro, apresentando:

- O que precisa ser criado para mover em direção ao estado futuro;
- O que precisa ser modificado;
- O que será descontinuado.

Esse documento será dividido em seções que abordarão cada domínio arquitetural separadamente, a saber:

- Aplicação;
- Dados;
- Integração;
- Segurança;
- Infraestrutura;
- Processo e capacidade organizacional

# Lacunas no domínio da aplicação

| Lacuna identificada | Estado Atual                                    | Estado Alvo                   | Ação Necessária                                          | Esforço | Prioridade |
| ------------------- | ----------------------------------------------- | ----------------------------- | -------------------------------------------------------- | ------- | ---------- |
| Criar               | Comunicação Portal - Legado monolítica          | Serviços independentes        | Criação dos serviços para substituição gradual do legado | Grande  | Alta       |
| Criar               | Aplicação web sem suporte a dispositivos móveis | Aplicação Mobile independente | Criação da aplicação mobile                              | Grande  | Média      |

# Lacunas no domínio dos dados

| Lacuna identificada | Estado Atual     | Estado Alvo                                                             | Ação Necessária                                                                              | Esforço | Prioridade |
| ------------------- | ---------------- | ----------------------------------------------------------------------- | -------------------------------------------------------------------------------------------- | ------- | ---------- |
| Modificar           | Dados duplicados | Reorganizar estrutura de dados para garantir consistência sem repetição | Expor serviços de consulta do lado do ERP e LMS para evitar integração por troca de arquivos | Grande  | Média      |

# Lacunas no domínio da integração

| Lacuna identificada | Estado Atual                                     | Estado Alvo                                                       | Ação Necessária                                                                        | Esforço | Prioridade |
| ------------------- | ------------------------------------------------ | ----------------------------------------------------------------- | -------------------------------------------------------------------------------------- | ------- | ---------- |
| Modificar           | Autenticação / Autorização independentes         | Single Sign-On entre Sistema acadêmico, LMS e ERP                 | Implantar serviço de autenticação e autorização central                                | Grande  | Alta       |
| Criar               | Permissões fragmentadas                          | Permissões unificadas para os 3 sistemas                          | Harmonizar permissões, consolidar em diferentes papéis para melhor controle            | Grande  | Alta       |
| Criar               | ERP sem comunicação direta com Sistema Acadêmico | ERP emite eventos que podem ser consumidos pelo sistema acadêmico | Expor eventos para as ações do ERP que precisam ser processadas pelo sistema acadêmico | Grande  | Alta       |
| Criar               | LMS sem comunicação direta com Sistema Acadêmico | LMS emite eventos que podem ser consumidos pelo sistema acadêmico | Expor eventos para as ações do LMS que precisam ser processadas pelo sistema acadêmico | Grande  | Alta       |

# Lacunas no domínio da segurança

| Lacuna identificada | Estado Atual                               | Estado Alvo                                                              | Ação Necessária                                                                         | Esforço | Prioridade |
| ------------------- | ------------------------------------------ | ------------------------------------------------------------------------ | --------------------------------------------------------------------------------------- | ------- | ---------- |
| Descontinuar        | Armazenamento inseguro de senhas           | Gestão integrada de senhas pelo sistema de autenticação / autorização    | Desativar autenticação independente do ERP e do LMS                                     | Grande  | Alta       |
| Criar               | Proteção restrita aos recursos on-premises | Proteção dos recursos on-premieses, na nuvem e da comunicação entre eles | Contratação de serviços de segurança como WAFs, ferramentas de monitoramento de ataques | Médio   | Alta       |

# Lacunas no domínio da infraestrutura

| Lacuna identificada | Estado Atual                          | Estado Alvo                                                                                                        | Ação Necessária                                                                            | Esforço | Prioridade |
| ------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | ------------------------------------------------------------------------------------------ | ------- | ---------- |
| Descontinuar        | Infraestrutura própria                | Uso de infraestrutura em provedores de nuvem                                                                       | Desligamento dos serviços e servidores na infraestrutura on-premise ao término da migração | Médio   | Baixa      |
| Criar               | Nenhum uso de infraestrutura na nuvem | Provisionamento da infraestrutura na nuvem seguindo as diretrizes da ADR-006 quando a segregação de redes e contas | Implementação correta das landing zones                                                    | Grande  | Alta       |

# Lacunas no domínio dos processos e da capacidade organizacional

| Lacuna identificada | Estado Atual                                                    | Estado Alvo                                                                                        | Ação Necessária                                                                                                  | Esforço | Prioridade |
| ------------------- | --------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------- | ---------- |
| Criar               | Time sem experiência com provedores de nuvem                    | Time certificado nas plataformas selecionadas                                                      | Recrutamento e seleção de especialistas em computação em nuvem / Treinamento de membros atuais para certificação | Grande  | Alta       |
| Modificar           | Operação monitora servidores on-premise                         | Monitoramento de servidores on-premise e dos recursos na nuvem                                     | Capacitar o time para uso correto das ferramentas de observabilidade de recursos em nuvem                        | Média   | Baixa      |
| Modificar           | Baixa governança sobre o processo de desenvolvimento e operação | Governança adequada e papéis claros para todas as atividades do ciclo de vida do Sistema Acadêmico | Definir estruturas dos times e papéis, segregar acessos                                                          | Grande  | Alta       |
