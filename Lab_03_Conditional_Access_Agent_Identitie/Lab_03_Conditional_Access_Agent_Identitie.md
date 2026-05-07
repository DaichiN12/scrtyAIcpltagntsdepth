**Laboratório 03: Acesso condicional para identidades de agentes da Zava**

**Introdução**

O CISO da Zava determinou que apenas agentes de IA revisados e aprovados possam acessar os recursos da empresa. Qualquer agente que não tenha passado pelo processo de revisão de governança deve ser bloqueado automaticamente. Além disso, se a identidade de algum agente apresentar sinais de comprometimento como comportamento anômalo na obtenção de tokens, deve ser bloqueada imediatamente, sem intervenção manual.

O MOD Administrator implementará ambos os controles usando o acesso condicional para identidades de agentes (pré-visualização). Patti Fernandes verificará se a avaliação da política está visível nos registros de entrada. Este laboratório estabelece a base de referência de governança de agentes da Zava sobre a qual todos os laboratórios de segurança subsequentes se baseiam.

O acesso condicional para identidades de agentes é um recurso em pré-visualização no Microsoft Entra ID que estende os controles Zero Trust aos agentes de IA. O MOD Administrator criará atributos de segurança personalizados para classificar o status de aprovação de cada agente Zava, criará uma política de acesso condicional que bloqueia todas as identidades de agente não aprovadas de acessar recursos organizacionais e criará uma segunda política que bloqueia qualquer identidade de agente que exiba comportamento de alto risco com base nos sinais de proteção do Entra ID. As políticas serão primeiro validadas no modo somente relatório antes de serem alteradas para aplicação. Patti Fernandes investigará os registros de entrada de agentes para confirmar a avaliação da política de acesso condicional.

**Objetivos**

- Criar um conjunto de atributos de segurança personalizados e um atributo de status de aprovação para a classificação de agentes.

- Atribuir atributos de status de aprovação às três identidades de agente da Zava.

- Criar uma política de acesso condicional que bloqueie todas as identidades de agentes não aprovados.

- Validar o escopo da política usando a ferramenta “What If” para confirmar se um agente sem tag seria bloqueado.

- Alterar a política para o modo de aplicação.

- Criar uma segunda política de acesso condicional que bloqueie identidades de agentes de alto risco.

- Gerar registros de entrada de agentes invocando o assistente de RH da Zava como Patti Fernandes.

- Investigar a avaliação da política de acesso condicional nos registros de login de identidade do agente.

**Duração do laboratório**

Tempo estimado: **30 minutos**

**Exercício 1: Crie atributos de segurança personalizados para governança de agentes**

**Tarefa 1: Atribua a função de administrador de definição de atributos**

1.  Abra um navegador e acesse https://entra.microsoft.com. Faça login com as credenciais **MOD Administrator**, se solicitado. Em **Entra ID**, selecione **Roles & admins**.

> <img src="media/image1.png" style="width:6.26806in;height:3.54792in" />

2.  Na barra de pesquisa, digite **Attribute Definition Administrator.**

> <img src="media/image2.png" style="width:6.26806in;height:3.54792in" />

3.  Selecione **Attribute Definition Administrator** clicando no nome. Não marque a caixa de seleção.

> <img src="media/image3.png" style="width:6.26806in;height:3.54792in" />

4.  Na página **Attribute Definition Administrator**, selecione **+ Add assignments**.

> <img src="media/image4.png" style="width:6.26806in;height:3.54792in" />

5.  No painel **Add assignments**, selecione **No members selected**.

> <img src="media/image5.png" style="width:6.26806in;height:3.54792in" />

6.  Procure e selecione **MOD Administrator**. Clique em **Select** para confirmar.

> <img src="media/image6.png" style="width:6.26806in;height:3.54792in" />

7.  Selecione **Next**.

> <img src="media/image7.png" style="width:6.26806in;height:3.54792in" />

8.  Em **Assignment type**, selecione **Active**.

> <img src="media/image8.png" style="width:6.26806in;height:3.54792in" />

9.  No painel de ativação, insira uma justificativa — Lab 03 custom security attribute configuration.

> <img src="media/image9.png" style="width:6.26806in;height:3.54792in" />

10. Desmarque a opção **Permanently assigned** e defina a duração para **1 hour**. Selecione **Assign**.

> <img src="media/image10.png" style="width:6.26806in;height:3.54792in" />

11. Confirme se a tarefa aparece na lista em **Active assignments**.

> <img src="media/image11.png" style="width:6.26806in;height:3.54792in" />

12. Volte para **Roles & admins**.

13. Na barra de pesquisa, digite **Attribute Assignment Administrator** e repita as etapas para atribuir a função ao **MOD Administrator**.

> <img src="media/image12.png" style="width:6.26806in;height:3.54792in" />

14. Selecione o ícone da conta **MOD Administrator** no canto superior direito da página. Selecione **Sign out**.

15. Faça login novamente em https://entra.microsoft.com com as credenciais **MOD Administrator**.

> **Observação:** A função de administrador de definição de atributos concede permissões para criar e gerenciar definições personalizadas de atributos de segurança. Essa função foi propositalmente excluída da função de administrador global para garantir a separação de funções. É necessário fazer um novo login para que a nova atribuição de função entre em vigor.

**Tarefa 2: Crie o conjunto de atributos AgentAttributes**

1.  No painel de navegação à esquerda, expanda **Entra ID** e selecione **Custom security attributes**.

> <img src="media/image13.png" style="width:6.26806in;height:3.54792in" />

2.  Na página **Custom security attributes**, selecione **+ Add attribute set**.

> <img src="media/image14.png" style="width:6.26806in;height:3.54792in" />

3.  No painel **Add attribute set**, no campo **Attribute set name**, digite AgentAttributes.

4.  No campo **Description**, digite Attribute set for classifying AI agent approval and governance status.

5.  No campo **Maximum number of attributes**, mantenha o valor padrão.

6.  Selecione **Add** para criar o conjunto de atributos.

> <img src="media/image15.png" style="width:6.26806in;height:3.54792in" />

7.  Confirme se **AgentAttributes** aparece na lista de conjuntos de atributos.

> <img src="media/image16.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 3: Crie o atributo AgentApprovalStatus**

1.  Na página **Custom security attributes**, selecione **AgentAttributes** para abrir o conjunto de atributos.

> <img src="media/image17.png" style="width:6.26806in;height:3.54792in" />

2.  Na página **AgentAttributes**, selecione **+ Add attribute**.

> <img src="media/image18.png" style="width:6.26806in;height:3.54792in" />

3.  No painel **Add attribute**, configure os seguintes campos:

    - **Nome do atributo:** Digite AgentApprovalStatus.

    - **Description:** Digite Tracks the approval status of each AI agent identity in the Zava governance review process.

    - **Data type:** Selecione **String**.

    - **Allow multiple values to be assigned:** Selecione **Yes**.

    - **Only allow predefined values to be assigned:** Selecione **Yes**.

4.  Em **Predefined values**, selecione **+ Add value**.

> <img src="media/image19.png" style="width:6.26806in;height:3.54792in" />

5.  No campo de valor, digite **New**. Em seguida, selecione **Add**.

> <img src="media/image20.png" style="width:6.26806in;height:3.54792in" />

6.  Selecione **+ Add value**.

> <img src="media/image21.png" style="width:6.26806in;height:3.54792in" />

7.  No campo de valor, digite In_Review. Em seguida, selecione **Add**.

> <img src="media/image22.png" style="width:6.26806in;height:3.54792in" />

8.  Selecione **+ Add value**.

> <img src="media/image23.png" style="width:6.26806in;height:3.54792in" />

9.  No campo de valor, digite HR_Approved. Em seguida, selecione **Add**.

> <img src="media/image24.png" style="width:6.26806in;height:3.54792in" />

10. Selecione **+ Add value**.

> <img src="media/image25.png" style="width:6.26806in;height:3.54792in" />

11. No campo de valor, digite Finance_Approved. Em seguida, selecione **Add**.

> <img src="media/image26.png" style="width:6.26806in;height:3.54792in" />

12. Selecione **+ Add value**.

> <img src="media/image27.png" style="width:6.26806in;height:3.54792in" />

13. No campo de valor, digite IT_Approved. Em seguida, selecione **Add**.

> <img src="media/image28.png" style="width:6.26806in;height:3.54792in" />

14. Selecione **Save**.

> <img src="media/image29.png" style="width:6.26806in;height:3.54792in" />

15. Confirme se **AgentApprovalStatus** aparece na lista de atributos em **AgentAttributes**.

> <img src="media/image30.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 4: Atribua o HR_Approved ao Zava HR Assistant**

1.  No painel de navegação à esquerda, selecione **Agent ID**.

> <img src="media/image31.png" style="width:6.26806in;height:3.54792in" />

2.  Na página **All agent identities (Preview)**, selecione **Zava HR Assistant (Microsoft Copilot Studio).**

> <img src="media/image32.png" style="width:6.26806in;height:3.54792in" />

3.  Na página **Overview (Preview)**, na subnavegação à esquerda, selecione **Custom security attributes (Preview)**.

> <img src="media/image33.png" style="width:6.26806in;height:3.54792in" />

4.  Na página **Custom security attributes**, selecione **+ Add assignment**.

> <img src="media/image34.png" style="width:6.26806in;height:3.54792in" />

5.  No painel **Add custom security attribute assignment**, configure o seguinte:

    - **Attribute set:** Selecione **AgentAttributes**.

    - **Attribute:** Selecione **AgentApprovalStatus**.

    - **Assigned values:** Selecione **Add value** \> **HR_Approved** e selecione **Save**.

> <img src="media/image35.png" style="width:6.26806in;height:3.54792in" />
>
> <img src="media/image36.png" style="width:6.26806in;height:3.54792in" />

6.  Selecione **Save** para aplicar a tarefa.

> <img src="media/image37.png" style="width:6.26806in;height:3.54792in" />

7.  Confirme se o **AgentApprovalStatus** aparece com o valor **HR_Approved** na página de atributos de segurança personalizados.

8.  Da mesma forma, atribua os seguintes atributos aos respectivos agentes.

    - **Zava Finance Agent (Microsoft Copilot Studio)**: HR_Approved

    - **Zava HR Assistant (Microsoft Copilot Studio)**: HR_Approved

**Exercício 2: Crie uma política de acesso condicional para bloquear identidades de agentes não aprovadas.**

**Tarefa 1: Crie a política e configure as atribuições**

1.  No painel de navegação à esquerda do centro de administração do Microsoft Entra, expanda **Entra ID** e selecione **Conditional Access**.

2.  Na página **Conditional Access**, selecione **Policies**.

> <img src="media/image38.png" style="width:6.26806in;height:3.54792in" />

3.  Na página **Policies**, selecione **+ New policy**.

> <img src="media/image39.png" style="width:6.26806in;height:3.54792in" />

4.  Na página **New Conditional Access policy**, no campo **Name**, digite Zava - Block Unapproved Agent Identities.

5.  Em **Assignments**, selecione **0 users or agents (Preview) selected** em **Users or agents**.

6.  No painel de atribuições, em **What does this policy apply to?,** selecione **Agents (Preview)**.

> <img src="media/image40.png" style="width:6.26806in;height:3.54792in" />

7.  Em **Include**, selecione **All agent identities (Preview)**.

> <img src="media/image41.png" style="width:6.26806in;height:3.54792in" />

8.  Em **Exclude**, selecione **Select agent identities based on attributes**.

> <img src="media/image42.png" style="width:6.26806in;height:3.54792in" />

9.  Defina **Configure** como **Yes**.

> <img src="media/image43.png" style="width:6.26806in;height:3.54792in" />

10. Na configuração da expressão, em **AgentAttributes**, selecione o atributo **AgentApprovalStatus**. Defina **Operator** como **Contains**. Defina **Value** como **HR_Approved**.

> <img src="media/image44.png" style="width:6.26806in;height:3.54792in" />

11. Selecione **Done** para confirmar a configuração de exclusão.

> <img src="media/image45.png" style="width:6.26806in;height:3.54792in" />

12. Em **Target resources**, selecione **No target resources selected**.

> <img src="media/image46.png" style="width:6.26806in;height:3.54792in" />

13. Em **Include**, selecione **All resources (formerly 'All cloud apps')**.

> <img src="media/image47.png" style="width:6.26806in;height:3.54792in" />

14. Em **Access controls**, no painel **Grant**, confirme se **Block access** está selecionada.

15. Na **Enable policy**, mantenha **Report-only**.

16. Selecione **Create** para salvar a política.

> <img src="media/image48.png" style="width:6.26806in;height:3.54792in" />

**Tarefa 2: Valide a política usando a ferramenta "What If "**

1.  Na página da política, selecione **"What If"** para abrir a visualização de impacto somente para o relatório.

> **Observação:** A ferramenta “What If” permite simular se uma identidade específica seria afetada por esta política sem aplicá-la.
>
> <img src="media/image49.png" style="width:6.26806in;height:3.54792in" />

2.  No painel **“What If”,** em **User or workload identity**, selecione **Agent identities**.

> <img src="media/image50.png" style="width:6.26806in;height:3.54792in" />

3.  Selecione **Edit agent identity**.

> <img src="media/image51.png" style="width:6.26806in;height:3.54792in" />

4.  No campo de pesquisa de identidade do agente, pesquise e selecione **Zava Finance Agent (Microsoft Copilot Studio)**.

> <img src="media/image52.png" style="width:6.26806in;height:3.54792in" />

5.  Em **Target resource**, defina **Select target type** como **Cloud apps**. Selecione **+ Select cloud app**.

6.  No campo de pesquisa, digite Office 365 SharePoint Online. Selecione **Office 365 SharePoint Online** nos resultados. Clique em **Select** para confirmar.

> <img src="media/image53.png" style="width:6.26806in;height:3.54792in" />

7.  Selecione **“What If”** para executar a simulação.

> <img src="media/image54.png" style="width:6.26806in;height:3.54792in" />

8.  Analise os resultados e confirme se a política **Zava - Block Unapproved Agent Identities** aparece como **Applied**, pois o agente financeiro da Zava NÃO está excluído pelo atributo HR_Approved.

9.  Volte ao link **Edit agent identity** e altere o agente para **Zava HR Assistant**.

> <img src="media/image55.png" style="width:6.26806in;height:3.54792in" />
>
> <img src="media/image56.png" style="width:6.26806in;height:3.54792in" />

10. Selecione **“What If”** para executar a simulação.

> <img src="media/image57.png" style="width:6.26806in;height:3.54792in" />

11. Analise os resultados e confirme se a política **Zava - Block Unapproved Agent Identities** aparece como **Not applied**, porque o assistente de RH da Zava está excluído pelo atributo HR_Approved.

> <img src="media/image58.png" style="width:6.26806in;height:3.54792in" />

12. Selecione **Close** para sair do painel “What If”.

**Tarefa 3: Alterne a política para o modo de aplicação (somente leitura)**

**Observação:** Você não poderá executar esta tarefa no ambiente atual porque as configurações de segurança padrão foram ativadas no laboratório 00 para permitir a publicação de agentes do Copilot Studio.

1.  Na página da política **Zava - Block Unapproved Agent Identities**, selecione **Edit**.

2.  Em **Enable policy**, selecione **On**.

3.  Selecione **Save** para aplicar a alteração.

4.  Na página **Policies**, confirme se a opção **Zava - Block Unapproved Agent Identities** está com o status **On**.

**Exercício 3: Crie uma política de acesso condicional para bloquear identidades de agentes de alto risco.**

**Tarefa 1: Crie a política e configure as atribuições**

1.  Na página **Conditional Access**, selecione **+ Create new policy**.

> <img src="media/image59.png" style="width:6.26806in;height:3.54792in" />

2.  No campo **Name**, digite Zava - Block High Risk Agent Identities.

3.  Em **Assignments**, selecione **0 users or agents (Preview) selected** em **Users or agents**

> <img src="media/image60.png" style="width:6.26806in;height:3.54792in" />

4.  Em **What does this policy apply to?,** selecione **Agents (Preview)**.

> <img src="media/image61.png" style="width:6.26806in;height:3.54792in" />

5.  Em **Include**, selecione **All agent identities (Preview)**.

> <img src="media/image62.png" style="width:6.26806in;height:3.54792in" />

6.  Em **Target resources**, selecione **No target resources selected** e, em seguida selecione **All resources (formerly 'All cloud apps')**.

> <img src="media/image63.png" style="width:6.26806in;height:3.54792in" />

7.  Em **Conditions**, selecione **0 Conditions selected**. Em seguida, selecione **Not Configured** em **Agent Risk**.

> <img src="media/image64.png" style="width:6.26806in;height:3.54792in" />

8.  No painel **Agent risk**, defina **Configure** como **Yes**. Em **Configure agent risk levels needed for policy to be enforced**, selecione **High**. Selecione **Done** para confirmar a condição.

> <img src="media/image65.png" style="width:6.26806in;height:3.54792in" />

9.  Em **Access controls**, na seção **Grant**, certifique-se de que **Block access** esteja selecionada.

10. Em **Enable policy**, selecione **Report-only**.

> **Observação:** Esta política está definida como apenas relatório porque os sinais de risco de agente da proteção da Entra ID exigem o uso ativo do agente ao longo do tempo antes que os níveis de risco sejam gerados. Em um ambiente de laboratório recém-configurado, ainda não haverá sinais de risco. O modo apenas relatório permite que a política seja avaliada em relação a eventos futuros de login sem bloquear o acesso prematuramente. Em um ambiente de produção, esta política seria ativada assim que os dados de referência dos sinais de risco fossem estabelecidos.

11. Selecione **Create** para salvar a política.

> <img src="media/image66.png" style="width:6.26806in;height:3.54792in" />

12. Na página **Policies**, confirme se a opção **Zava - Block High Risk Agent Identities** aparece com o status **Report-only**.

**Exercício 4: Gere registros de entrada de agentes e investigue a avaliação da política de acesso condicional.**

**Tarefa 1: Acione o assistente de RH da Zava**

1.  Abra uma nova janela **InPrivate** ou **Incognito** do navegador.

2.  Acesse https://copilot.microsoft.com.

3.  Faça login com as credenciais **Patti Fernandes** na aba **Resources**. (Você pode usar pattif@TenantName como seu ID e a senha do usuário na aba recursos.)

4.  Na interface de chat do Microsoft 365 Copilot, selecione **All agents** na barra de navegação. Pesquise e selecione **Zava HR Assistant**.

> <img src="media/image67.png" style="width:6.26806in;height:3.54792in" />

5.  Em seguida, selecione **Add**.

> <img src="media/image68.png" style="width:6.26806in;height:3.54792in" />

6.  No campo de entrada do chat, digite o seguinte:

> What is Zava's leave policy?

7.  Aguarde a resposta do assistente de RH da Zava.

> <img src="media/image69.png" style="width:6.26806in;height:3.54792in" />

8.  Digite uma segunda mensagem no campo de entrada do chat:

> How do I submit a sick leave request?

9.  Aguarde a resposta.

> <img src="media/image70.png" style="width:6.26806in;height:3.54792in" />
>
> **Observação:** Essas interações geram registros de entrada do agente, pois o assistente de RH da Zava se autentica para acessar sua fonte de conhecimento do SharePoint. Esses eventos aparecerão nos registros de login do Entra e terão a avaliação da política de acesso condicional registrada.

10. Feche a janela do navegador InPrivate.

**Tarefa 2: Investigue os registros de login de agentes no Entra**

1.  Retorne à sessão do navegador **MOD Administrator** https://entra.microsoft.com.

2.  No painel de navegação à esquerda, expanda **Entra ID** e selecione **Monitoring & health**. Selecione **Sign-in logs**.

> <img src="media/image71.png" style="width:6.26806in;height:3.54792in" />

3.  Na página **Sign-in logs**, selecione a aba **Service principal sign-ins.**

> <img src="media/image72.png" style="width:6.26806in;height:3.54792in" />

4.  Na barra de filtros, selecione **+ Add filters**. Selecione **Is Agent** como campo do filtro.

> <img src="media/image73.png" style="width:6.26806in;height:3.54792in" />

5.  Selecione **Yes** e, em seguida, selecione **Apply** para aplicar o filtro.

> <img src="media/image74.png" style="width:6.26806in;height:3.54792in" />

6.  Analise as entradas de login retornadas na visualização filtrada.

> <img src="media/image75.png" style="width:6.26806in;height:3.54792in" />

**Resumo**

Neste laboratório, você criou um conjunto de atributos de segurança personalizado chamado **AgentAttributes** com um atributo **AgentApprovalStatus** contendo cinco valores de governança predefinidos. Você atribuiu o status de aprovação **HR_Approved** ao assistente de RH da Zava, estabelecendo um modelo estruturado de classificação de agentes no Entra ID. Você criou a política de acesso condicional **Zava - Block Unapproved Agent Identities**, direcionada a todas as identidades de agentes e excluindo aquelas com valores de atributo aprovados. Você usou a ferramenta “What If” no modo somente relatório para validar se um agente aprovado foi corretamente excluído da política de bloqueio e, em seguida, alterou a política para o modo de aplicação. Você criou a política **Zava - Block High Risk Agent Identities** política utilizando os sinais de risco do agente de proteção Entra ID e configurou apenas para geração de sinais de risco pendentes. Patti Fernandes utilizou o assistente Zava HR para gerar registros de entrada, que você então investigou nos registros de login de entidades de serviço, filtrados por tipo de agente. As identidades dos agentes da Zava agora são regidas pelos controles de acesso condicional do Zero Trust.
