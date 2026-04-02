## ACESSOS IMPORTANTES
1. https://bancointer.identitynow.com/ui/d/mysailpoint - Inter Access

2. https://jira-inter.atlassian.net/servicedesk/customer/user/login?destination=portals - Solicitações (SD) [usar: 3rdparty_tulio.silva@inter.co]

3. https://xwiki.sharedservices.local/bin/view/CAT/RPA/?srid=PgB88iKG - RPA Wiki

4. https://jira-inter.atlassian.net/servicedesk/customer/portal/12/create/319 - Solicitação de Acesso ADM ao NTB

5. https://gitlab.sharedservices.local/governanca/rpa/microservices - GitLab [Governanca_Microservicos]

6. https://nexus.sharedservices.local/#browse/search - Repositório de Bibliotecas do Python

7. https://dev.portal/home - Portal para Criação de Projetos

8. https://xwiki.sharedservices.local/wiki/basedeconhecimento/view/KB/Seguran%C3%A7a/Procedimentos%20e%20Documentos%20de%20Refer%C3%AAncia/Endpoint%20Privilege%20Management%20(EPM)/?srid=h0cHYDvL#HCasodeuso - Modo de Uso do EPM

9. https://servicedesk.sydle.com/t/68ecf044ec56cc1283dd40ed?searchCode=NXJVY6 - Consulta Ticket Service Desk do Sydle ONE

10. https://wikijs.sharedservices.local/pt-br/information_tecnology/it_services/big_data/01_ai_and_machine_learning/01_Generative_AI_Platform/01_platform_consume - Página Documental sobre Inter IA

11. https://docs.iac.app/overview/install - Instalação e configuração do IAC

12. https://config-generator.bancointer.com.br/ - Geração de configuração AWS

13. https://bancointer.identitynow.com/passwordreset/default/flow-selection - Reset de senha

14. https://jira-inter.atlassian.net/servicedesk/customer/portal/12/create/243 - Fila de Solicitações ao CoE

Telefone HelpDesk: 2101-7045

====================================================================================
## ATIVIDADES DE TRABALHO PARA DESENVOLVIMENTO

###Pré Requisitos para Atividades:
1. Homebrew instalado
2. Python e dependências instalados
3. GIT instalado e configurado
4. Acessos concedidos à repositórios do GITLAB
5. Acessos às ferramentas envolvidas para cadastro de projetos (DEV.PORTAL e IAC)

### Processo para Criação de Projetos
1. Acesso à plataforma DEV.PORTAL
2. Criação de um projeto utilizando templates de Robotização (Microsserviço ou JOB) e informando a grupo de atuação. Informar o Cluster-Type como [conta-digital-api]
3. Após criado projeto, acessar o repositório do GITLAB conforme GRUPO informado no item 2
4. Através de comando GIT, clonar o projeto disponível no repositório do GIT. Link do projeto estará disponível no repositório do GIT
5. Criação de uma BRANCH espelho da MASTER, para atuação/desenvolvimento
6. Através de comandos GIT realizar atividades de COMMIT para envio das implementações ao repositório

### Processo para Criação de IAC
1. Acessar o repositório no GITLAB referente ao IAC
2. Através de comandos GIT, clonar o repositório de IAC disponível (diretório geral)
3. Com o IAC instalado, executar os comandos do IAC para criação de repositório referente ao projeto a ser trabalhado
4. Criação de uma BRANCH espelho da MASTER, para atuação/desenvolvimento
5. Realizar as configurações necessárias nos arquivos de configuração
6. Através de comandos GIT, realizar atividades de COMMIT para envio das implementações ao repositório

### Dicas:
1. Commit de projetos quando incluso no comentário a info [#deployuat], automatiza o envio da implementação para o pipeline de publicação em UAT
2. Commit de projetos quando incluso no comentário a info [#autodeploy], automatiza a execução do pipeline para publicação em UAT (comando normalmente precede o informado no ITEM 1)
3. Para atividades relativas ao IAC, quando enviados dados à PARAMETERSTORE (dados sensíveis como usuários/senhas/tokens e afins), deve-se utilizar ferramenta de criptografia de informações
4. Para atividade relativas ao IAC, deve-se validar e viabilizar acessos à solução da AWS (AWS CLI)
5. Comandos GIT mais usados: clone (replicar repositório) / checkout -b <nova branch> (replica branch acessada para nova branch) / commit -m "mensagem" (envio de informações com mensagem de publicação) / push origin <branch> (envio de comit local p/ remoto)

====================================================================================

## Sydle One

1. Explorer > Login Info > Dados de Login
- Lista, por usuário, as últimas tentativas de Login realizadas

2. Explorer > Sistema > Autenticador
- Lista os modelos de conexão do sistema (integração LDAP)

3. Explorer > Configuração
