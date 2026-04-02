Credenciais
Usuário: srvc.coealteryx.prd
Display Name: COEALTERYX PRD
Senha: 1u-TM+kp&9Dz2295Qy8%
Email: srvc.coealteryx.prd@inter.co

https://citrix.bi.local/Citrix/BancoInter/discovery

https://citrix.bi.local/Citrix/BancoInterWeb/

https://uat-alteryx.bi.local/gallery/#!/admin/users

https://alteryx.bi.local/gallery/#!/app

https://jira-inter.atlassian.net/jira/dashboards/10175

http(s)://{yourhostname}/webapi/swagger

smb://fsx-prd.intermedium.local

=========

PREZADOS, boa tarde.

Para fluxos novos não há necessidade de procedimento de rollback, uma vez que se trata de implementação de fluxo inédito que ainda não existe no Alteryx Server. Como não há versão anterior do fluxo em produção, não existe estado anterior para o qual seja possível retornar, tornando o rollback inaplicável para este tipo de operação.

O processo de rollback é aplicável apenas em situações de alteração ou atualização de fluxos já existentes no ambiente, onde há uma versão funcional prévia que pode ser restaurada em caso de necessidade.

Qualquer dúvida estou à disposição.

AT.TE

======
