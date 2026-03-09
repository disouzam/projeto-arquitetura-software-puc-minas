# 004. Estratégia de governança, proteção e auditoria de dados

Data: 2026-03-08

Autor: Dickson Souza em nome do Grupo 1

## Status

Proposto

## Contexto

Na discussão da `ADR 003 - Estratégia de Identidade, Autenticação e Autorização, incluindo SSO` o foco foi na unificação dos processos de autenticação e autorização para todo o Sistema Acadêmico Modernizado, com vistas a facilitar tanto a manutenção quanto a experiência dos usuários finais. Esse passo é fundamental para o que será discutido com respeito à governança, proteção e auditoria de dados. Assim a premissa de leitura e compreensão dessa ADR é que os fundamentos já tenham sido consolidados, mesmo que a implementação completa do Microsoft Entra ID ainda não tenha acontecido.

Hoje, os dados do Sistema Acadêmico Legado junto com ERP TOTVS e LMS Canvas estão descentralizados e duplicados. A gestão e governança dos dados também são separadas, não possibilitando a análise coordenada de informações da universidade que reside nos três sistemas bem como uma governança unificada. Por exemplo, a trajetória acadêmica do aluno no Canvas precisa ser consulta de forma isolada às informações salvas no Sistema Acadêmico Legado. Ou então, cruzar alunos com alto absenteísmo e inadimplência não pode ser feito de forma contínua, sem um elaborado esquema de integração. Embora os bancos de dados de cada sistema permanecerão em grande medida isolados, a deduplicação e a governança unificada são aspectos que merecem maior atenção.

## Decisão

O primeiro passo opera no nível corporativo da universidade para garantir que existam regras claras, papéis ligados à gestão e operação das bases de dados e processos de acesso, modificação, backup e segurança dos dados. Nesse aspecto, deve-se estabelecer um comitê de governança para supervisionar as operações de evolução do sistema legado e integrações com o ERP TOTVS e LMS Canvas. O primeiro  papel desse comitê será identificar a(s) bases de dados de cada um dos três sistemas, mapear as pessoas que possuem acesso e efetuar um diagnóstico da operação e seus riscos. Também de responsabilidade desse papel é identificar a necessidade de novos papéis a serem preenchidos por meio de contratação ou, preferencialmente, formação de times com os profissionais já envolvidos na gestão dos bancos de dados. Outro aspecto importante é implantar acesso mediado pelo Microsoft Entra ID em todos os bancos de dados ou servidores que hospedam os bancos de dados.

Ainda no aspecto da governança, várias ferramentas poderiam ser indicadas aqui para as seguintes tarefas:

1. Catálogo de dados
2. Linhagem de dados
3. Dicionário de dados
4. Glossário de termos do negócio
5. Ferramentas de qualidade dos dados
6. Gestão de metadados
7. Inventário de fonte de dados

Porém o objetivo dessa ADR não é resolver todos esses problemas de uma única só vez e sim estabelecer primeiro uma estrutura de papéis responsáveis (através do comitê de governança) que irão gradualmente adotar ferramentas para solucionarem essas demandas / tarefas. Dentre as ferramentas que deverão ser consideradas numa ADR futura estão:

1. Alation Data Governance
2. Apache Atlas
3. Ataccama One
4. Collibra Data Governance
5. Erwin Data Intelligence by Quest
6. IBM Cloud Pak for Data
7. Oracle Enterprise Data Management

Essas 7 ferramentas foram selecionadas de uma lista de 16 apresentadas na referência 2.

Na área de proteção de dados, devem ser observadas práticas (e ferramentas) para atingir os seguintes objetivos:

1. Garantir controle de acesso baseado em papéis (RBAC), profundamente ligado com a já mencionada ADR anterior sobre identidade, autenticação e autorização.
2. Criptografia em repouso (bancos, arquivos e backups) e em trânsito (camada de aplicação - comunicação front-end para o back-end)
3. Mascaramento e anonimização, com especial preocupação com a Lei Geral de Proteção de Dados brasileira (Lei Nº 13.709, de 14 de Agosto de 2018)
4. Monitoramento e detecção de ameaças (usando SIEM (Security Information and Event Management), DLP (Data Loss Prevention) e outras ferramentas)
5. Gestão de vulnerabilidades
6. Políticas de retenção e descarte seguro

Mais uma vez, dado o amplo escopo de demandas de proteção de dados, não serão tomadas todas as decisões nessa ADR mas sim um primeiro direcionamento para avaliação posterior. É importante frisar também que a natureza heterogênea dos sistemas atuais e hospedagem em servidor on premises como padrão atual podem tornar mais difícil a seleção de ferramentas que atendam todos os casos de usos necessários. Para seguir a mesma dinâmica da parte de governança, algumas ferramentas serão citadas abaixo:

1. Oracle Audit Vault and Database Firewall (AVDF) para monitoramento de atividades nos bancos Oracle e não Oracle ou Oracle Data Safe (as duas ferramentas tem características complementares e a existência de um firewall corporativo tende a mover a preferência para o Oracle Data Safe, que já possui mascaramento de dados embutido) - ferramentas escolhidas devido à presença do banco Oracle para o Sistema Acadêmico Legado.
2. Para o LMS Canvas e o ERP TOTVS, a avaliação de ferramentas será feita em momento futuro e por profissionais especializados nas duas ferramentas. Os sumários consultados, criados via Copilot, apontam ferramentas de uso geral, válidas para a parte de governança ou independentes de tecnologia específica. Para ser menos vago, algumas práticas e nomes são listados abaixo:
   1. Uso de gestão de acesso e identidade (já discutido na ADR 003)
   2. Ferramentas de DLP como o Microsoft Purview DLP
   3. SIEM e monitoramento via Microsoft Sentinel

Por fim, a auditoria será inicialmente integrada às ferramentas de monitoramento e observabilidade para o registro e salvamento de logs antes de uma decisão futura ser tomada para outras necessidades de auditoria (como evidências, conformidade e controles, acessos e configurações, regras, duplicidades e consistência)

## Consequências

Essa ADR tomou a forma de um primeiro balizamento de iniciativas com a constituição de um comitê de governance e a formação de um time de profissionais preferencialmente já ativo na gestão e operação das fontes de dados do Sistema Acadêmico Legado.

O risco principal é ter apresentado um conjunto de diretrizes muito vagas sem orientar a ação. Por outro lado, as demandas dessas três áreas são amplas e podem variar bastante para cada um dos três sistemas em questão (sistema acadêmico, LMS e ERP).

Outra consequência dessa ADR é contribuir para o aumento de headcount e custos administrativos pela formação de um time de especialistas. A contra-medida é formar esse time internamente.

# Referências:

1. [10 melhores softwares de governança de dados em 2025 (avaliações e preços)](https://clickup.com/pt-BR/blog/127137/software-de-governanca-de-dados)
2. [16 top data governance tools to know about in 2025](https://www.techtarget.com/searchdatamanagement/feature/15-top-data-governance-tools-to-know-about)
3. [What is Data Governance? How Does it Impact Businesses?](https://youtu.be/BqdPuwvwPk4)
4. [Lei Geral de Proteção de Dados brasileira (Lei Nº 13.709, de 14 de Agosto de 2018)](https://www.planalto.gov.br/ccivil_03/_ato2015-2018/2018/lei/l13709.htm)
5. [Oracle Audit Vault and Database Firewall vs. Oracle Data Safe Comparison Chart](https://sourceforge.net/software/compare/Oracle-Audit-Vault-and-Database-Firewall-vs-Oracle-Data-Safe)