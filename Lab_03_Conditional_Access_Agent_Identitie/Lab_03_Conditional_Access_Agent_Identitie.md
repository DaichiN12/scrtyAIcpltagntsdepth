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

1.  Abra um navegador e acesse `https://entra.microsoft.com`. Faça login com as credenciais **MOD Administrator**, se solicitado. Em **Entra ID**, selecione **Roles & admins**.

	![](./media/image1.png)

2.  Na barra de pesquisa, digite `Attribute Definition Administrator`.

	![](./media/image2.png)

3.  Selecione **Attribute Definition Administrator** clicando no nome. Não marque a caixa de seleção.

	![](./media/image3.png)

4.  Na página **Attribute Definition Administrator**, selecione **+ Add assignments**.

	![](./media/image4.png)

5.  No painel **Add assignments**, selecione **No members selected**.

	![](./media/image5.png)

6.  Procure e selecione **MOD Administrator**. Clique em **Select** para confirmar.

	![](./media/image6.png)

7.  Selecione **Next**.

	![](./media/image7.png)

8.  Em **Assignment type**, selecione **Active**.

	![](./media/image8.png)

9.  No painel de ativação, insira uma justificativa — `Lab 03 custom security attribute configuration`.

	![](./media/image9.png)

10. Desmarque a opção **Permanently assigned** e defina a duração para **1 hour**. Selecione **Assign**.

	![](./media/image10.png)

11. Confirme se a tarefa aparece na lista em **Active assignments**.

	![](./media/image11.png)

12. Volte para **Roles & admins**.

13. Na barra de pesquisa, digite `Attribute Assignment Administrator` e repita as etapas para atribuir a função ao **MOD Administrator**.

	![](./media/image12.png)

14. Selecione o ícone da conta **MOD Administrator** no canto superior direito da página. Selecione **Sign out**.

15. Faça login novamente em `https://entra.microsoft.com` com as credenciais **MOD Administrator**.

	> **Observação:** A função de administrador de definição de atributos concede permissões para criar e gerenciar definições personalizadas de atributos de segurança. Essa função foi propositalmente excluída da função de administrador global para garantir a separação de funções. É necessário fazer um novo login para que a nova atribuição de função entre em vigor.

**Tarefa 2: Crie o conjunto de atributos AgentAttributes**

1.  No painel de navegação à esquerda, expanda **Entra ID** e selecione **Custom security attributes**.

	![](./media/image13.png)

2.  Na página **Custom security attributes**, selecione **+ Add attribute set**.

	![](./media/image14.png)

3.  No painel **Add attribute set**, no campo **Attribute set name**, digite `AgentAttributes`.

4.  No campo **Description**, digite `Attribute set for classifying AI agent approval and governance status`.

5.  No campo **Maximum number of attributes**, mantenha o valor padrão.

6.  Selecione **Add** para criar o conjunto de atributos.

	![](./media/image15.png)

7.  Confirme se **AgentAttributes** aparece na lista de conjuntos de atributos.

	![](./media/image16.png)

**Tarefa 3: Crie o atributo AgentApprovalStatus**

1.  Na página **Custom security attributes**, selecione **AgentAttributes** para abrir o conjunto de atributos.

	![](./media/image17.png)

2.  Na página **AgentAttributes**, selecione **+ Add attribute**.

	![](./media/image18.png)

3.  No painel **Add attribute**, configure os seguintes campos:

    - **Nome do atributo:** Digite `AgentApprovalStatus`.

    - **Description:** Digite `Tracks the approval status of each AI agent identity in the Zava governance review process.`

    - **Data type:** Selecione **String**.

    - **Allow multiple values to be assigned:** Selecione **Yes**.

    - **Only allow predefined values to be assigned:** Selecione **Yes**.

4.  Em **Predefined values**, selecione **+ Add value**.

	![](./media/image19.png)

5.  No campo de valor, digite `New`. Em seguida, selecione **Add**.

	![](./media/image20.png)

6.  Selecione **+ Add value**.

	![](./media/image21.png)

7.  No campo de valor, digite `In_Review`. Em seguida, selecione **Add**.

	![](./media/image22.png)

8.  Selecione **+ Add value**.

	![](./media/image23.png)

9.  No campo de valor, digite `HR_Approved`. Em seguida, selecione **Add**.

	![](./media/image24.png)

10. Selecione **+ Add value**.

	![](./media/image25.png)

11. No campo de valor, digite `Finance_Approved`. Em seguida, selecione **Add**.

	![](./media/image26.png)

12. Selecione **+ Add value**.

	![](./media/image27.png)

13. No campo de valor, digite `IT_Approved`. Em seguida, selecione **Add**.

	![](./media/image28.png)

14. Selecione **Save**.

	![](./media/image29.png)

15. Confirme se **AgentApprovalStatus** aparece na lista de atributos em **AgentAttributes**.

	![](./media/image30.png)

**Tarefa 4: Atribua o HR_Approved ao Zava HR Assistant**

1.  No painel de navegação à esquerda, selecione **Agent ID**.

	![](./media/image31.png)

2.  Na página **All agent identities (Preview)**, selecione **Zava HR Assistant (Microsoft Copilot Studio).**

	![](./media/image32.png)

3.  Na página **Overview (Preview)**, na subnavegação à esquerda, selecione **Custom security attributes (Preview)**.

	![](./media/image33.png)

4.  Na página **Custom security attributes**, selecione **+ Add assignment**.

	![](./media/image34.png)

5.  No painel **Add custom security attribute assignment**, configure o seguinte:

    - **Attribute set:** Selecione **AgentAttributes**.

    - **Attribute:** Selecione **AgentApprovalStatus**.

    - **Assigned values:** Selecione **Add value** \> **HR_Approved** e selecione **Save**.

	![](./media/image35.png)

	![](./media/image36.png)

6.  Selecione **Save** para aplicar a tarefa.

	![](./media/image37.png)

7.  Confirme se o **AgentApprovalStatus** aparece com o valor **HR_Approved** na página de atributos de segurança personalizados.

8.  Da mesma forma, atribua os seguintes atributos aos respectivos agentes.

    - **Zava Finance Agent (Microsoft Copilot Studio)**: HR_Approved

    - **Zava HR Assistant (Microsoft Copilot Studio)**: HR_Approved

**Exercício 2: Crie uma política de acesso condicional para bloquear identidades de agentes não aprovadas.**

**Tarefa 1: Crie a política e configure as atribuições**

1.  No painel de navegação à esquerda do centro de administração do Microsoft Entra, expanda **Entra ID** e selecione **Conditional Access**.

2.  Na página **Conditional Access**, selecione **Policies**.

	![](./media/image38.png)

3.  Na página **Policies**, selecione **+ New policy**.

	![](./media/image39.png)

4.  Na página **New Conditional Access policy**, no campo **Name**, digite `Zava - Block Unapproved Agent Identities`.

5.  Em **Assignments**, selecione **0 users or agents (Preview) selected** em **Users or agents**.

6.  No painel de atribuições, em **What does this policy apply to?,** selecione **Agents (Preview)**.

	![](./media/image40.png)

7.  Em **Include**, selecione **All agent identities (Preview)**.

	![](./media/image41.png)

8.  Em **Exclude**, selecione **Select agent identities based on attributes**.

	![](./media/image42.png)

9.  Defina **Configure** como **Yes**.

	![](./media/image43.png)

10. Na configuração da expressão, em **AgentAttributes**, selecione o atributo **AgentApprovalStatus**. Defina **Operator** como **Contains**. Defina **Value** como **HR_Approved**.

	![](./media/image44.png)

11. Selecione **Done** para confirmar a configuração de exclusão.

	![](./media/image45.png)

12. Em **Target resources**, selecione **No target resources selected**.

	![](./media/image46.png)

13. Em **Include**, selecione **All resources (formerly 'All cloud apps')**.

	![](./media/image47.png)

14. Em **Access controls**, no painel **Grant**, confirme se **Block access** está selecionada.

15. Na **Enable policy**, mantenha **Report-only**.

16. Selecione **Create** para salvar a política.

	![](./media/image48.png)

**Tarefa 2: Valide a política usando a ferramenta "What If "**

1.  Na página da política, selecione **"What If"** para abrir a visualização de impacto somente para o relatório.

	> **Observação:** A ferramenta “What If” permite simular se uma identidade específica seria afetada por esta política sem aplicá-la.

	![](./media/image49.png)

2.  No painel **“What If”,** em **User or workload identity**, selecione **Agent identities**.

	![](./media/image50.png)

3.  Selecione **Edit agent identity**.

	![](./media/image51.png)

4.  No campo de pesquisa de identidade do agente, pesquise e selecione **Zava Finance Agent (Microsoft Copilot Studio)**.

	![](./media/image52.png)

5.  Em **Target resource**, defina **Select target type** como **Cloud apps**. Selecione **+ Select cloud app**.

6.  No campo de pesquisa, digite `Office 365 SharePoint Online`. Selecione **Office 365 SharePoint Online** nos resultados. Clique em **Select** para confirmar.

	![](./media/image53.png)

7.  Selecione **“What If”** para executar a simulação.

	![](./media/image54.png)

8.  Analise os resultados e confirme se a política **Zava - Block Unapproved Agent Identities** aparece como **Applied**, pois o agente financeiro da Zava NÃO está excluído pelo atributo `HR_Approved`.

9.  Volte ao link **Edit agent identity** e altere o agente para **Zava HR Assistant**.

	![](./media/image55.png)

	![](./media/image56.png)

10. Selecione **“What If”** para executar a simulação.

	![](./media/image57.png)

11. Analise os resultados e confirme se a política **Zava - Block Unapproved Agent Identities** aparece como **Not applied**, porque o assistente de RH da Zava está excluído pelo atributo `HR_Approved`.

	![](./media/image58.png)

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

	![](./media/image59.png)

2.  No campo **Name**, digite Zava - Block High Risk Agent Identities.

3.  Em **Assignments**, selecione **0 users or agents (Preview) selected** em **Users or agents**

	![](./media/image60.png)

4.  Em **What does this policy apply to?,** selecione **Agents (Preview)**.

	![](./media/image61.png)

5.  Em **Include**, selecione **All agent identities (Preview)**.

	![](./media/image62.png)

6.  Em **Target resources**, selecione **No target resources selected** e, em seguida selecione **All resources (formerly 'All cloud apps')**.

	![](./media/image63.png)

7.  Em **Conditions**, selecione **0 Conditions selected**. Em seguida, selecione **Not Configured** em **Agent Risk**.

	![](./media/image64.png)

8.  No painel **Agent risk**, defina **Configure** como **Yes**. Em **Configure agent risk levels needed for policy to be enforced**, selecione **High**. Selecione **Done** para confirmar a condição.

	![](./media/image65.png)

9.  Em **Access controls**, na seção **Grant**, certifique-se de que **Block access** esteja selecionada.

10. Em **Enable policy**, selecione **Report-only**.

> **Observação:** Esta política está definida como apenas relatório porque os sinais de risco de agente da proteção da Entra ID exigem o uso ativo do agente ao longo do tempo antes que os níveis de risco sejam gerados. Em um ambiente de laboratório recém-configurado, ainda não haverá sinais de risco. O modo apenas relatório permite que a política seja avaliada em relação a eventos futuros de login sem bloquear o acesso prematuramente. Em um ambiente de produção, esta política seria ativada assim que os dados de referência dos sinais de risco fossem estabelecidos.

11. Selecione **Create** para salvar a política.

	![](./media/image66.png)

12. Na página **Policies**, confirme se a opção **Zava - Block High Risk Agent Identities** aparece com o status **Report-only**.

**Exercício 4: Gere registros de entrada de agentes e investigue a avaliação da política de acesso condicional.**

**Tarefa 1: Acione o assistente de RH da Zava**

1.  Abra uma nova janela **InPrivate** ou **Incognito** do navegador.

2.  Acesse `https://copilot.microsoft.com`.

3.  Faça login com as credenciais **Patti Fernandes** na aba **Resources**. (Você pode usar `pattif@TenantName` como seu ID e a senha do usuário na aba recursos.)

4.  Na interface de chat do Microsoft 365 Copilot, selecione **All agents** na barra de navegação. Pesquise e selecione **Zava HR Assistant**.

	![](./media/image67.png)

5.  Em seguida, selecione **Add**.

	![](./media/image68.png)

6.  No campo de entrada do chat, digite o seguinte:

    ```
	What is Zava's leave policy?
    ```

7.  Aguarde a resposta do assistente de RH da Zava.

	![](./media/image69.png)

8.  Digite uma segunda mensagem no campo de entrada do chat:

	```
	How do I submit a sick leave request?
    ```

9.  Aguarde a resposta.

	![](./media/image70.png)

	> **Observação:** Essas interações geram registros de entrada do agente, pois o assistente de RH da Zava se autentica para acessar sua fonte de conhecimento do SharePoint. Esses eventos aparecerão nos registros de login do Entra e terão a avaliação da política de acesso condicional registrada.

10. Feche a janela do navegador InPrivate.

**Tarefa 2: Investigue os registros de login de agentes no Entra**

1.  Retorne à sessão do navegador **MOD Administrator** `https://entra.microsoft.com`.

2.  No painel de navegação à esquerda, expanda **Entra ID** e selecione **Monitoring & health**. Selecione **Sign-in logs**.

	![](./media/image71.png)

3.  Na página **Sign-in logs**, selecione a aba **Service principal sign-ins.**

	![](./media/image72.png)

4.  Na barra de filtros, selecione **+ Add filters**. Selecione **Is Agent** como campo do filtro.

	![](./media/image73.png)

5.  Selecione **Yes** e, em seguida, selecione **Apply** para aplicar o filtro.

	![](./media/image74.png)

6.  Analise as entradas de login retornadas na visualização filtrada.

	![](./media/image75.png)

**Resumo**

Neste laboratório, você criou um conjunto de atributos de segurança personalizado chamado **AgentAttributes** com um atributo **AgentApprovalStatus** contendo cinco valores de governança predefinidos. Você atribuiu o status de aprovação **HR_Approved** ao assistente de RH da Zava, estabelecendo um modelo estruturado de classificação de agentes no Entra ID. Você criou a política de acesso condicional **Zava - Block Unapproved Agent Identities**, direcionada a todas as identidades de agentes e excluindo aquelas com valores de atributo aprovados. Você usou a ferramenta “What If” no modo somente relatório para validar se um agente aprovado foi corretamente excluído da política de bloqueio e, em seguida, alterou a política para o modo de aplicação. Você criou a política **Zava - Block High Risk Agent Identities** política utilizando os sinais de risco do agente de proteção Entra ID e configurou apenas para geração de sinais de risco pendentes. Patti Fernandes utilizou o assistente Zava HR para gerar registros de entrada, que você então investigou nos registros de login de entidades de serviço, filtrados por tipo de agente. As identidades dos agentes da Zava agora são regidas pelos controles de acesso condicional do Zero Trust.
