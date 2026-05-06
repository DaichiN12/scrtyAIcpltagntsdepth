**Laboratório 01: Descoberta de agentes e gerenciamento de registros**

**Introdução**

O CISO da Zava solicitou à equipe de segurança que confirme se todos os agentes de IA implementados estão visíveis, sob controle e contabilizados antes do início de qualquer trabalho relacionado às políticas de segurança. Patti Fernandes, administradora de segurança e analista do SOC da Zava, utiliza o registro de agentes no Microsoft Entra ID para inspecionar os três agentes da Zava, testar os controles do ciclo de vida e identificar eventuais lacunas de governança. O espaço de trabalho do Defender XDR e o conector do Defender for Cloud Apps também serão inicializados para que os dados de atividade dos agentes comecem a fluir antes de avançarmos na jornada de segurança.

Este laboratório apresenta o registro de agentes do centro de administração do Microsoft 365 como a principal ferramenta para descoberta de agentes, gerenciamento do ciclo de vida e governança. Patti explorará o registro, analisará os metadados dos agentes, executará ações do ciclo de vida, identificará agentes sem proprietário e concluirá as tarefas de configuração necessárias previamente necessárias para os laboratórios de segurança do dia 2, incluindo a verificação do Purview Audit, provisionamento do Defender XDR e inicialização do Defender for Cloud Apps.

**Objetivos**

- Explorar o painel de visão geral do Agent 365 e interpretar as principais métricas.

- Inspecionar os três agentes da Zava no registro de agentes e revisar seus metadados.

- Bloquear e desbloquear o assistente de RH da Zava para validar os controles do ciclo de vida.

- Exportar o inventário de agentes para confirmar a capacidade de trilha de auditoria.

- Identificar agentes sem proprietário usando o filtro do painel de controle do registro.

- Verificar se o Purview Audit está ativo e executar uma pesquisa inicial de registro de auditoria.

- Provisionar o Microsoft Defender XDR fazendo login no portal do Defender.

- Configurar os detalhes da organização no Defender for Cloud Apps e conectar o conector de aplicativo do Microsoft 365.

**Duração do laboratório**

Tempo estimado: **20 minutos**

**Exercício 1: Explore a visão geral do Agent 365 e o registro de agentes**

**Tarefa 1: Acesse a página de visão geral do Agent 365**

1.  Abra um navegador e acesse https://admin.cloud.microsoft/. Faça login com as credenciais do **MOD Administrator**, se solicitado.

2.  No painel de navegação à esquerda, expanda **Agents** e selecione **Overview**.

> <img src="media/image1.png" style="width:6.26806in;height:3.54792in" />

3.  Na página **Agent Overview**, localize as seguintes métricas e anote seus valores atuais:

    - **Agent Registry** — número total de agentes no locatário.

    - **Active users in Copilot** — usuários únicos que interagiram com um agente nos últimos 30 dias.

    - **Pending requests for agents** — solicitações pendentes para adicionar agentes.

    - **Agents without owners** — agentes cujo proprietário não está mais na empresa.

    - **Agent analytics** — agentes por criador, principais plataformas usadas para criar agentes e usuários ativos no Copilot ao longo do tempo.

> <img src="media/image2.png" style="width:6.26806in;height:3.54792in" />
>
> **Observação:** Em um ambiente recém-configurado, a contagem de usuários ativos e agentes sem proprietários pode mostrar zero. Isso é esperado. As métricas serão atualizadas conforme os agentes forem utilizados ao longo do curso.

**Tarefa 2: Inspecione e aprove os agentes da Zava no registro de agentes**

1.  No painel de navegação à esquerda, selecione **Agents**. Selecione **All agents**. Em seguida, selecione a aba **Requests**.

> <img src="media/image3.png" style="width:6.26806in;height:3.54792in" />

2.  Na lista de agentes, localize **Zava IT Support Agent** e selecione os três pontos verticais (**...**) ao lado do nome.

3.  Entre as duas opções, você pode escolher **Reject submission** ou **Publish to store**. Por enquanto, selecione **Publish to store**.

> <img src="media/image4.png" style="width:6.26806in;height:3.54792in" />

4.  No fluxo **Publish new agent**, em **Select users or groups who can install the agent**, selecione **All users**.

> <img src="media/image5.png" style="width:6.26806in;height:3.54792in" />

5.  Em **Select users or groups who will have the agent pre-installed (optional)**, selecione **Specific users/groups**.

> <img src="media/image6.png" style="width:6.26806in;height:3.54792in" />

6.  Na caixa de pesquisa **Specific users/groups**, procure por **Adele Vance** e selecione-a na lista suspensa.

> <img src="media/image7.png" style="width:6.26806in;height:3.54792in" />

7.  Inclua também **Patti Fernandes.**

> <img src="media/image8.png" style="width:6.26806in;height:3.54792in" />

8.  Em seguida, selecione **Next**.

> <img src="media/image9.png" style="width:6.26806in;height:3.54792in" />

9.  Em **Apply security template**, selecione **Next**.

> <img src="media/image10.png" style="width:6.26806in;height:3.54792in" />

10. Em **Review permissions**, selecione **Next**.

> <img src="media/image11.png" style="width:6.26806in;height:3.54792in" />

11. Selecione a aba **Registry**, procure por **Zava IT Support Agent**, selecione-o e anote as informações no painel de detalhes.

> <img src="media/image12.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 3: Aprove um agente no centro de administração do Teams**

1.  Abra uma nova aba do navegador e acesse https://admin.teams.microsoft.com/ e faça login usando as credenciais do MOD Administrator.

2.  Na navegação à esquerda, em **Teams apps**, selecione **Manage apps**.

> <img src="media/image13.png" style="width:6.26806in;height:3.54792in" />

3.  Na barra de pesquisa, procure por Zava e selecione **Zava HR Assistant**.

> <img src="media/image14.png" style="width:6.26806in;height:3.54792in" />

4.  Na página **Zava HR Assistant**, selecione **Publish**.

> <img src="media/image15.png" style="width:6.26806in;height:3.54792in" />

5.  Na caixa de diálogo de confirmação, selecione **Publish** novamente.

> <img src="media/image16.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 4: Bloqueio e desbloqueio do assistente de RH da Zava**

1.  Acesse novamente https://admin.cloud.microsoft/. Faça login com as credenciais do **MOD Administrator,** se solicitado.

2.  Na página **All agents**, selecione a aba **Registry** e, em seguida, procure e selecione **Zava HR Assistant** na lista de agentes.

> <img src="media/image17.png" style="width:6.26806in;height:3.54792in" />

3.  No painel de detalhes, abaixo do nome do agente, selecione **Block**.

> <img src="media/image18.png" style="width:6.26806in;height:3.54792in" />

4.  No painel **Block agent**, revise a mensagem que confirma que o bloqueio impedirá que todos os usuários da organização acessem o agente. Marque a caixa ao lado de **Block agent**. Selecione **Save**.

> <img src="media/image19.png" style="width:6.26806in;height:3.54792in" />

5.  Confirme se o **Zava HR Assistant** agora exibe o status **Blocked.**

> <img src="media/image20.png" style="width:6.26806in;height:3.54792in" />

6.  Abaixo do nome do agente, selecione **Unblock**.

> <img src="media/image21.png" style="width:6.26806in;height:3.54792in" />

7.  No painel **Unblock agent**, selecione a caixa de seleção **Unblock agent**. Selecione **Save**. Feche o painel de detalhes.

> <img src="media/image22.png" style="width:6.26806in;height:3.54792in" />

8.  Na lista de agentes, confirme que o **Zava HR Assistant** agora exibe o status **Active**.

> <img src="media/image23.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 5: Exporte o inventário de agentes**

1.  Na aba **Registry**, selecione **Export** na barra de ferramentas acima da lista de agentes.

> **Observação:** Se o botão **Export** não estiver visível na barra de ferramentas, selecione o menu de reticências (**...**) na barra de ferramentas para localizar a opção de exportação.
>
> <img src="media/image24.png" style="width:6.26806in;height:3.54792in" />

2.  Confirme o download na caixa de diálogo de confirmação. Aguarde até que o arquivo de exportação seja gerado e baixado para sua máquina virtual de laboratório.

> <img src="media/image25.png" style="width:6.26806in;height:3.54792in" />

3.  Abra o arquivo CSV baixado.

4.  Confirme se o arquivo contém linhas para **Zava HR Assistant**, **Zava Finance Agent**, e **Zava IT Support Agent**.

5.  Verifique se as seguintes colunas estão presentes: nome do agente, editor, criador, data de criação, produtos hospedados e status de disponibilidade.

> <img src="media/image26.png" style="width:6.26806in;height:3.76875in" />

6.  Feche o arquivo CSV.

**Tarefa 6: Identifique os agentes sem proprietário**

1.  Na aba **Registry**, selecione o cartão **Missing an owner**.

> <img src="media/image27.png" style="width:6.26806in;height:3.54792in" />

2.  Analise a lista de agentes que é exibida após a aplicação do filtro de agentes sem proprietário.

> <img src="media/image28.png" style="width:6.26806in;height:3.54792in" />

3.  Observe se algum dos três agentes da Zava aparece nesta lista filtrada.

> **Observação:** Em um ambiente de laboratório onde os agentes foram criados pelo MOD Administrator, os agentes podem ou não aparecer como sem proprietário, dependendo de como a propriedade é propagada a partir do Copilot Studio. Se nenhum agente aparecer, isso confirma que a propriedade foi atribuída corretamente durante a criação. Se os agentes aparecerem, isso representa uma falha de governança que seria resolvida reatribuindo a propriedade.

4.  Selecione **Clear filter** ou redefinir os filtros para retornar à lista completa de agentes.

> <img src="media/image29.png" style="width:6.26806in;height:3.54792in" />

**Exercício 2: Prepare o Purview Audit para o dia 2**

**Tarefa 1: Verifique se o Purview Audit está ativo**

1.  Abra uma nova aba do navegador e acesse https://purview.microsoft.com. Faça login com as credenciais do **MOD Administrator,** se solicitado. Selecione **Get started**.

> <img src="media/image30.png" style="width:6.26806in;height:3.54792in" />

2.  No painel de navegação à esquerda, selecione **Solutions** e, em seguida, selecione **Audit**.

> <img src="media/image31.png" style="width:6.26806in;height:3.54792in" />

3.  Na página **Audit**, verifique se aparece um banner solicitando que você inicie a gravação da atividade do usuário e do administrador.

    - Se um banner for exibido, selecione **Start recording user and admin activity** para ativar a auditoria.

> <img src="media/image32.png" style="width:6.26806in;height:3.54792in" />

- Se nenhum banner for exibido, a auditoria já está ativada. Prossiga para a próxima etapa.

4.  Configure a pesquisa com os seguintes valores:

    - **Start date:** Selecione a data de hoje menos 3 dias.

    - **End date:** Selecione a data de hoje.

    - **Activities – friendly names:** Deixe em branco para pesquisar todas as atividades.

    - **Users:** Deixe em branco.

    - **Record type:** Deixe em branco.

5.  Selecione **Search**.

> <img src="media/image33.png" style="width:6.26806in;height:3.54792in" />

6.  Aguarde a conclusão da tarefa de pesquisa.

7.  Analise os resultados para confirmar se os registros de auditoria estão sendo retornados.

> **Observação:** Se a pesquisa não retornar resultados, isso pode indicar que ainda não ocorreram atividades auditadas no locatário ou que a importação do registro de auditoria requer tempo adicional após o provisionamento inicial. Isso é esperado em um novo ambiente de laboratório. Os registros de auditoria gerados ao longo deste e dos próximos laboratórios poderão ser pesquisados a partir do segundo dia.

**Exercício 3: Inicie o Microsoft Defender XDR e o Defender for Cloud Apps**

**Tarefa 1: Provisione o Microsoft Defender XDR**

1.  Abra uma nova aba do navegador e acesse https://security.microsoft.com. Faça login com as credenciais do **MOD Administrator,** se solicitado.

2.  Na tela de boas-vindas do portal do **Microsoft Defender**, verifique a mensagem de provisionamento, caso seja exibida.

> **Observação:** O Microsoft Defender XDR é provisionado automaticamente quando um administrador qualificado acessa o portal pela primeira vez. Se o provisionamento estiver em andamento, uma mensagem indicará a localização do data center em uso e o tempo estimado de conclusão. Aguarde a conclusão do provisionamento antes de prosseguir.

3.  Após o carregamento completo do portal, confirme se o painel de navegação à esquerda exibe as seguintes seções: **Home**, **Incidents & alerts**, **Hunting**, **Threat intelligence**, **Assets**, **Identities**, **Endpoints**, **Email & collaboration**, **Cloud Apps** e **Settings**.

> <img src="media/image34.png" style="width:6.26806in;height:3.54792in" />

4.  Selecione **Home** para confirmar se o painel inicial do Defender XDR carrega sem erros.

> <img src="media/image35.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 2: Configure os detalhes da organização no Defender for Cloud Apps**

1.  No portal do Microsoft Defender em https://security.microsoft.com, no painel de navegação à esquerda, selecione **Settings**.

> <img src="media/image36.png" style="width:6.26806in;height:3.54792in" />

2.  Na página **Settings**, selecione **Cloud Apps**.

> <img src="media/image37.png" style="width:6.26806in;height:3.54792in" />

3.  Selecione **Organisation details**.

> <img src="media/image38.png" style="width:6.26806in;height:3.54792in" />

4.  Na página **Organisation details**, no campo **Organisation display name**, digite **Zava Corporation**.

5.  No campo **Environment name**, digite **Dev One**.

6.  No campo **Managed domains**, insira o domínio principal do seu locatário no seguinte formato: \[TenantPrefix\].onmicrosoft.com

> **Observação:** substitua \[TenantPrefix\] pelo prefixo do seu locatário na aba **Resources**. Adicionar domínios gerenciados garante que os usuários internos sejam identificados corretamente nos relatórios e alertas do Cloud Apps.

7.  Selecione **Save**.

> <img src="media/image39.png" style="width:6.26806in;height:3.54792in" />

8.  Confirme se aparece uma notificação de sucesso confirmando que as configurações foram salvas.

> <img src="media/image40.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 3: Habilite o monitoramento de arquivos no Defender for Cloud Apps**

1.  No portal do Microsoft Defender, no painel de navegação à esquerda, selecione **Settings**.

> <img src="media/image36.png" style="width:6.26806in;height:3.54792in" />

2.  Na página **Settings**, selecione **Cloud Apps**.

> <img src="media/image37.png" style="width:6.26806in;height:3.54792in" />

3.  Em **Information Protection**, selecione **Files**. Na página **Files**, marque a caixa **Enable file monitoring**. Selecione **Save**.

> <img src="media/image41.png" style="width:6.26806in;height:3.54792in" />

4.  Confirme se aparece uma notificação de sucesso confirmando que o monitoramento de arquivos foi ativado.

**Tarefa 4: Conecte o conector de aplicativo do Microsoft 365**

1.  No portal do Microsoft Defender, no painel de navegação à esquerda, selecione **Settings**.

> <img src="media/image36.png" style="width:6.26806in;height:3.54792in" />

2.  Na página **Settings**, selecione **Cloud Apps**.

> <img src="media/image37.png" style="width:6.26806in;height:3.54792in" />

3.  Em **Connected apps**, selecione **App Connectors**.

> <img src="media/image42.png" style="width:6.26806in;height:3.54792in" />

4.  Na página **App Connectors**, selecione **+ Connect an app**.

5.  Na lista de aplicativos, selecione **Microsoft 365**.

6.  Na página **Select Microsoft 365 components**, confirme se todos os componentes estão selecionados por padrão. Se algum componente estiver desmarcado, selecione-o para habilitá-lo.

7.  Selecione **Connect Microsoft 365**.

> <img src="media/image43.png" style="width:6.26806in;height:3.54792in" />

8.  Aguarde a conexão ser concluída. Em seguida, selecione **Done**.

> <img src="media/image44.png" style="width:6.26806in;height:3.54792in" />

9.  Na página **App Connectors**, confirme se o **Microsoft 365** aparece na lista de conectores com o status **Connected**.

10. Na página **App Connectors**, selecione a caixa de seleção ao lado de **Microsoft 365** e, nas opções superiores, selecione **Connect Microsoft Azure Instance**.

> <img src="media/image45.png" style="width:6.26806in;height:3.54792in" />

11. Selecione **Connect Microsoft Azure**. Aguarde a conclusão da conexão.

> <img src="media/image46.png" style="width:6.26806in;height:3.54792in" />
>
> **Observação:** Após a conexão, o Defender for Cloud Apps começa a analisar a atividade do Microsoft 365. Os dados iniciais da semana anterior serão exibidos no portal. A primeira análise completa pode levar várias horas, dependendo do tamanho do locatário. Este conector é necessário para o monitoramento de atividades, a aplicação de políticas de DLP e a geração de alertas nos laboratórios do dia 2 e do dia 3.

**Resumo**

Neste laboratório, você explorou o painel de visão geral do Agent 365 e analisou as principais métricas de governança do locatário Zava. Você inspecionou os três agentes Zava no registro de agentes, analisando seus metadados, produtos de hospedagem e fontes de conhecimento. Você aprovou o envio do agente de suporte de TI da Zava, publicou o assistente de RH da Zava por meio do centro de administração do Teams e, em seguida, bloqueou e desbloqueou o assistente de RH da Zava para verificar se os controles de ciclo de vida funcionam corretamente. Você exportou o inventário de agentes para um arquivo CSV para confirmar a capacidade de trilha de auditoria e usou o filtro de agentes sem proprietário para verificar se há lacunas de governança na propriedade dos agentes.

Em seguida, você preparou a infraestrutura de monitoramento do dia 2, verificando se o Purview Audit está ativo e executando uma pesquisa de registro de auditoria de linha de base. Você provisionou o Microsoft Defender XDR, configurou o Defender for Cloud Apps com os detalhes da organização da Zava Corporation e o domínio gerenciado, conectou o conector de aplicativo do Microsoft 365 para iniciar a ingestão de dados de atividade e habilitou o monitoramento de arquivos.

O ambiente do agente Zava agora está totalmente visível, governado e pronto para a configuração da política de segurança no dia 2.
