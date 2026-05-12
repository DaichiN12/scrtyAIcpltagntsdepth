**Laboratório 00: Configuração do ambiente — infraestrutura de agente de IA da Zava Corporation**

**Introdução**

**A Zava Corporation** é uma empresa de consultoria de RH e serviços financeiros de médio porte que opera no Reino Unido e na União Europeia. A Zava gerencia registros confidenciais de funcionários, dados financeiros de clientes e contratos com fornecedores terceirizados. Recentemente, a organização implementou agentes de IA em suas funções de RH, finanças e suporte de TI para melhorar a eficiência operacional.

Antes de iniciar qualquer configuração de segurança, o ambiente da Zava Corporation deve estar totalmente provisionado. Neste laboratório, o **MOD Administrator** configurara o locatário do Microsoft Entra ID, habilitara o Microsoft Copilot Studio, registra o grupo de segurança necessário para a criação de agentes, criará os três agentes de IA que servirão como alvos de governança ao longo de todo o curso, conectará cada agente à sua fonte de conhecimento designada no SharePoint e carregará os documentos comerciais de exemplo que simulam o ambiente de dados real da Zava.

Todos os laboratórios seguintes dependem dos agentes, identidades e arquivos criados aqui. Conclua os três exercícios na ordem antes de prosseguir para o laboratório 01.

**Observação:** Em um ambiente real, as responsabilidades descritas aqui seriam distribuídas entre várias funções, como desenvolvedores, administradores de TI, administradores de segurança e responsáveis pela conformidade, cada uma operando com permissões delimitadas e alinhadas aos princípios do privilégio mínimo e do Zero Trust. No entanto, devido a restrições de tempo e de ambiente, este laboratório não reproduz essa separação de funções. Todas as tarefas de instalação e configuração serão realizadas por uma única função: o MOD Administrator, que detém a função de administrador Global na Microsoft.

**Objetivos**

- Criar um grupo de segurança atribuível a funções no Microsoft Entra ID e atribuir a função de administrador de funções privilegiadas.

- Habilitar o grupo **copilotagentsecurity** como o grupo de autores autorizado do Copilot Studio no centro de administração do Power Platform.

- Habilitar a identidade do agente Entra para o Copilot Studio no nível do ambiente.

- Conectar o SharePoint como fonte de dados no portal do Power Apps Maker.

- Criar três agentes do Copilot Studio: assistente de RH da Zava, agente financeiro da Zava e agente de suporte de TI da Zava.

- Conectar cada agente à sua respectiva fonte de conhecimento do SharePoint.

- Publicar cada agente e compartilhe com os usuários apropriados do laboratório.

- Carregar os documentos comerciais de amostra da Zava nos sites do SharePoint de RH e finanças.

- Verificar se os três agentes aparecem como “Ativos” no registro de agentes do Microsoft Agent 365.

**Duração do laboratório**

Tempo estimado: **30 minutos**

**Exercício 1: Configure o Entra ID e habilite autores no Copilot Studio**

**Tarefa 1: Faça login e configure a autenticação multifatorial**

1.  Abra um navegador e acesse `https://entra.microsoft.com`.

    ![](./media/image1.png)

2.  Na página de login, insira as credenciais do **MOD Administrator,** que estão na aba **Resources** do seu ambiente de laboratório.

    ![](./media/image2.png)
    ![](./media/image3.png)

3.  Se aparecer uma janela **Keep your account secure**, selecione **Next**.

    ![](./media/image4.png)

4.  Siga as instruções na tela para configurar o aplicativo Microsoft Authenticator.
    ![](./media/image5.png)
    
    > **Observação:** No seu dispositivo móvel, abra o aplicativo Authenticator, selecione **+** no canto superior direito, selecione **Work or school account** e, em seguida, selecione **Scan a QR code**. Leia o código QR exibido na tela.

5.  Complete todas as instruções restantes para finalizar a configuração do Autenticador.

6.  Se for exibida a pergunta **Stay signed in?**, selecione **Yes**.

7.  Na tela de boas-vindas do centro de administração do Microsoft Entra, selecione **Get Started**.

**Tarefa 2: Crie o grupo de segurança copilotagentsecurity**

1.  No centro de administração do Microsoft Entra, no painel de navegação à esquerda, expanda **Entra ID**. Em **Entra ID**, selecione **Groups**.

    ![](./media/image6.png)

2.  Na página **Overview**, selecione **New group**.

    ![](./media/image7.png)

3.  Na página **New Group**, configure os seguintes campos:

    - **Group type:** Selecione **Security**.

    - **Group name:** Digite `copilotagentsecurity`.

    - **Microsoft Entra roles can be assigned to the group:** Selecione **Yes**. Se esta opção não estiver visível, ignore este campo e continue.

4.  Em **Owners**, selecione **No owners selected**.

    ![](./media/image8.png)

5.  No painel **Add owners**, procure e selecione **MOD Administrator**. Escolha **Select** para confirmar o proprietário.

    ![](./media/image9.png)

6.  Em **Members**, selecione **No members selected**.

    ![](./media/image10.png)

7.  No painel **Add members**, procure e selecione **MOD Administrator** e **Patti Fernandes**. Clique em **Select** para confirmar os membros.

    ![](./media/image11.png)

8.  Em **Roles**, selecione **No roles selected**.

    ![](./media/image12.png)

9.  No painel **Select roles**, procure por **Global admin**, selecione **Global Administrator** e, em seguida, escolha **select**.

    ![](./media/image13.png)

10. Selecione **Create**.

    ![](./media/image14.png)

11. Na caixa de diálogo de confirmação, selecione **Yes**.

    ![](./media/image15.png)

12. Confirme se uma notificação de sucesso aparece na parte superior da página.

    ![](./media/image16.png)
    ![](./media/image17.png)

**Tarefa 3: Habilite o gerenciamento de acesso para recursos do Azure**

1.  No painel de navegação à esquerda do centro de administração do Microsoft Entra, expanda o **Entra ID**. Em **Entra ID**, selecione **Overview**.

    ![](./media/image18.png)

2.  Na página **Overview**, selecione **Properties** na barra superior.

    ![](./media/image19.png)

3.  Na página **Properties**, localize a opção **Access management for Azure resources** e defina-a como **Yes**.

    ![](./media/image20.png)

4.  Selecione **Manage security defaults**.

    ![](./media/image21.png)

5.  No painel **Security defaults**, em **Security defaults**, selecione **Enabled**. Selecione **Save**.

    ![](./media/image22.png)

6.  Retorne à página **Properties** e selecione **Save**.

    ![](./media/image23.png)

**Tarefa 4: Atribua a função de administrador com privilégios**

1.  No painel de navegação à esquerda do centro de administração do Microsoft Entra, expanda o **Entra ID** e selecione **Roles & admins**.

    ![](./media/image24.png)

2.  Na página **Roles and administrators**, na barra de pesquisa, digite `privileged role admin`.

    ![](./media/image25.png)

3.  Nos resultados da pesquisa, selecione **Privileged Role Administrator** clicando no nome. Não marque a caixa de seleção ao lado.

    ![](./media/image26.png)

4.  Na página **Privileged Role Administrator**, selecione **+ Add assignments**.

    ![](./media/image27.png)

5.  No painel **Add assignments**, selecione **No members selected**.

    ![](./media/image28.png)

6.  No painel **Select members**, procure e selecione **copilotagentsecurity**. Clique em **select** para confirmar.

    ![](./media/image29.png)

7.  Selecione **Next**.

8.  Na etapa **Settings**, em **Assignment type**, selecione **Active**. No campo **Enter justification**, digite `Successful lab completion`.

9.  Selecione **Assign**.

    ![](./media/image30.png)

10. Confirme se a atribuição da função aparece na lista de atribuições.

    ![](./media/image31.png)

**Tarefa 5: Configure os autores do Copilot Studio no centro de administração do Power Platform**

1.  Abra uma nova aba do navegador e acesse `https://admin.powerplatform.microsoft.com`.

2.  No painel de navegação à esquerda, selecione **Manage**.

    ![](./media/image32.png)

3.  Em **Manage**, selecione **Tenant Settings**. Na página **Tenant Settings**, localize e selecione **Copilot Studio Authors** na lista.

    ![](./media/image33.png)

4.  No painel **Copilot Studio Authors**, selecione o ícone **Edit** próximo ao grupo de segurança.

    ![](./media/image34.png)

5.  No campo de pesquisa, digite `copilotagentsecurity`. Selecione o grupo **copilotagentsecurity** nos resultados. Em seguida, selecione **Done**.

    ![](./media/image35.png)

6.  Selecione **Save** para aplicar a configuração.

    ![](./media/image36.png)

**Tarefa 6: Habilite a identidade do agente Entra para o Copilot Studio**

1.  Permaneça no centro de administração do Power Platform em `https://admin.powerplatform.microsoft.com`. No painel de navegação à esquerda, selecione **Copilot**.

    ![](./media/image37.png)

2.  Na página **Copilot**, selecione **Settings**.

    ![](./media/image38.png)

3.  Na lista de configurações, na seção **Copilot Studio**, selecione **Entra Agent Identity for Copilot Studio**.

    ![](./media/image39.png)

4.  No painel **Entra Agent Identity for Copilot Studio**, selecione o ambiente **Dev One** na lista de ambientes. Selecione **Edit setting**.

    ![](./media/image40.png)

5.  No painel de configurações, selecione **On**.

    ![](./media/image41.png)

6.  Selecione **Save**.

    ![](./media/image42.png)

7.  Após salvar, feche o painel.

    ![](./media/image42.png)

> **Observação:** Ao ativar a identidade do agente do Entra, os agentes do Copilot Studio recebem automaticamente uma identidade exclusiva no Microsoft Entra ID. Isso é necessário para a governança de identidades, o acesso condicional e a integração com o Defender for Cloud Apps em laboratórios posteriores.

**Tarefa 7: Adicione uma conexão do SharePoint no portal do Power Apps Maker**

1.  Abra uma nova aba do navegador e acesse `https://make.powerapps.com`. Se solicitado, faça login com as credenciais do **MOD Administrator**.

2.  Se solicitado, na tela **Welcome to Power Apps**, selecione **United States** e, em seguida, selecione **Get started**.

    ![](./media/image43.png)

3.  No canto superior direito, confirme se o ambiente **Dev One** está selecionado no seletor de ambientes. Caso contrário, selecione o seletor de ambientes e escolha **Dev One**.

    ![](./media/image44.png)

4.  Na barra de navegação à esquerda, expanda **More** e selecione **Connections**.

    ![](./media/image45.png)

5.  Na página **Connections**, selecione **+ New connection**.

    ![](./media/image46.png)

6.  Na barra de pesquisa de conectores, digite `SharePoint`. Selecione **SharePoint** na lista de conectores disponíveis.

    ![](./media/image47.png)

7.  No painel de conexão do **SharePoint**, selecione **Connect directly (cloud services)**. Selecione **Create**.

    ![](./media/image48.png)

8.  Quando solicitado, faça login com as credenciais do **MOD Administrator** para autorizar a conexão e selecione **Allow access**.

    ![](./media/image49.png)

9.  Confirme se a conexão do SharePoint aparece na lista **Connections** com o status **Connected**.

    ![](./media/image50.png)

**Exercício 2: Crie os agentes da Zava Copilot Studio**

Neste exercício, o MOD Administrator cria os três agentes da Zava no Microsoft Copilot Studio. Cada agente é configurado com um nome, descrição, instruções e uma fonte de conhecimento do SharePoint. Após a publicação, cada agente é compartilhado com as contas de usuário designadas para o laboratório. Esses agentes servem como os alvos de governança ativos nos laboratórios 01 a 07.

**Tarefa 1: Crie o assistente de RH da Zava**

1.  Abra uma nova aba do navegador e acesse `https://copilotstudio.microsoft.com`. Faça login com as credenciais do **MOD Administrator,** se solicitado.

2.  Na tela **Welcome**, localize o seletor de ambiente no canto superior direito da página.

3.  Se o ambiente atual não for **Dev One**, selecione o seletor de ambiente e escolha **Dev One** na lista suspensa.

> **Importante:** Se o Copilot Studio não carregar ou não mostrar a opção para selecionar um **ambiente,** como na captura de tela abaixo, siga estas etapas.
>
> Acesse `https://admin.powerplatform.microsoft.com/`. Selecione **Manage** \> **Environments** \> **Dev One** e copie o valor do **Environment ID**.
>
> Volte para a aba do Copilot Studio e abra `https://copilotstudio.microsoft.com/environments/\<EnvironmentID\>` (substituindo \<EnvironmentID\> pelo valor copiado acima).

![](./media/image51.png)

4.  No painel de navegação à esquerda, selecione **Agents**.

    ![](./media/image52.png)

5.  Na página **Create an agent**, selecione **Create blank agent**.

    ![](./media/image53.png)

6.  Na página de configuração do agente, selecione **Edit** em **Details**.

    ![](./media/image54.png)

7.  No campo **Name**, digite `Zava HR Assistant`.

8.  No campo **Description**, digite `An AI assistant that helps Zava employees find HR policies, benefits information, and employee procedures`.

9.  Selecione **Save**.

    ![](./media/image55.png)

10. No campo **Instructions**, selecione **Edit**, insira o seguinte e, em seguida, selecione **Save**.

    ```
    You are the Zava HR Assistant. Answer questions using only the information available in the Zava HR SharePoint knowledge base. Do not speculate or provide information outside the knowledge base. Always respond professionally.
    ```

    ![](./media/image56.png)
    ![](./media/image57.png)

11. Na página de configuração do agente, localize a seção **Knowledge.** Selecione **+ Add knowledge**.

    ![](./media/image58.png)

12. No painel **Add knowledge**, selecione **SharePoint**.

    ![](./media/image59.png)

13. No campo **SharePoint URL**, insira a URL do site de RH do SharePoint no seguinte formato e selecione **Add:** `https://\[TenantPrefix\].sharepoint.com/sites/HR`

> **Observação:** Substitua \[TenantPrefix\] pelo prefixo do seu locatário encontrado na aba **Resources** do seu ambiente de laboratório.

   ![](./media/image60.png)

14. Selecione **Add to agent** para conectar o site do SharePoint como fonte de conhecimento.

    ![](./media/image61.png)

15. No canto superior direito da página de configuração do agente, selecione **Publish**.

    ![](./media/image62.png)

16. Na caixa de diálogo de confirmação, selecione **Publish** para confirmar.

    ![](./media/image63.png)

17. Na página de configuração do agente, localize a aba **Channels** na seção superior (selecione **+2** se ela não estiver visível diretamente).

    ![](./media/image64.png)

18. Selecione **Microsoft 365 Copilot and Microsoft Teams** para adicioná-los como canais.

    ![](./media/image65.png)

19. Em seguida, selecione **Add channel**.

    ![](./media/image66.png)

20. Selecione **Availability options**.

    ![](./media/image67.png)

21. Na página **Microsoft 365 Copilot and Microsoft Teams**, selecione **Show to everyone in my org**.

    ![](./media/image68.png)

22. Selecione **Submit to org catalog**.

    ![](./media/image69.png)

23. Na caixa de diálogo de confirmação **Give everyone access to this agent?**, selecione **Yes**.

    ![](./media/image70.png)

24. Você será redirecionado para **Show in Teams app store for org** e verá uma notificação: **Your agent is submitted and waiting for approval from your Teams admin.**

    ![](./media/image71.png)

**Tarefa 2: Crie o agente financeiro da Zava**

1.  No painel de navegação à esquerda, selecione **Agents**. Em seguida, selecione **Create blank agent**.

2.  Na página **Agent**, selecione **Edit** em **Details**.

    ![](./media/image72.png)

3.  Na página de configuração do agente, no campo **Name**, digite `Zava Finance Agent`.

4.  No campo **Description**, digite `An AI assistant that helps Zava finance team members retrieve budget information, invoice data, and financial reports.` Em seguida, selecione **Save**.

    ![](./media/image73.png)

5.  No campo **Instructions**, selecione **Edit**.

    ![](./media/image74.png)

6.  Insira o seguinte e selecione **Save**.

    ```
    You are the Zava Finance Agent. Answer questions using only the information in the Zava Finance SharePoint knowledge base. Do not share financial data with users who have not been granted access to the Finance SharePoint site. Always respond professionally and flag any requests for data outside your knowledge base.
    ```

    ![](./media/image75.png)

8.  Na página de configuração do agente, localize a seção **Knowledge.** Selecione **+ Add knowledge**.

    ![](./media/image76.png)

9.  No painel **Add knowledge**, selecione **SharePoint**.

    ![](./media/image77.png)

10. No campo **SharePoint URL**, insira a URL do site do SharePoint de Finanças no seguinte formato: `https://\[TenantPrefix\].sharepoint.com/sites/Operations`

    > **Observação:** Substitua \[TenantPrefix\] pelo prefixo do seu locatário na aba **Resources**.

    ![](./media/image78.png)

11. Selecione **Add** para conectar o site do SharePoint como fonte de conhecimento.

    ![](./media/image79.png)

12. Em seguida, selecione **Add to agent**.

    ![](./media/image80.png)

13. Na página de configuração do agente, localize a aba **Channels** na seção superior (selecione **+2** se ela não estiver visível diretamente).

    ![](./media/image81.png)

14. Selecione o **Microsoft 365 Copilot and Microsoft Teams** para adicioná-los como canais.

    ![](./media/image82.png)

15. Em seguida, selecione **Add channel**.

    ![](./media/image83.png)

16. Na caixa de diálogo **Ready to publish?,** selecione **Publish**.

    ![](./media/image84.png)

17. Em **Agent preview**, selecione **Availability options**.

    ![](./media/image85.png)

18. Em **Decide who you want to show your agent to**, selecione **Show to my teammates and shared users**.

    ![](./media/image86.png)

19. No painel **Share "Zava Finance Agent" in Teams**, no campo de pesquisa, digite Patti Fernandes. Selecione **Patti Fernandes** nos resultados.

    ![](./media/image87.png)

20. Selecione **Patti Fernandes** novamente e, no painel à direita, selecione a função **Editor**.

    ![](./media/image88.png)

21. No campo de pesquisa, digite Megan Bowen. Selecione **Megan Bowen** nos resultados.

    ![](./media/image89.png)

22. No campo de pesquisa, digite Alex Wilber. Selecione **Alex Wilber** nos resultados.

    ![](./media/image90.png)

23. Selecione **Update** para aplicar a configuração de compartilhamento.

    ![](./media/image91.png)

**Tarefa 3: Crie o agente de suporte de TI da Zava**

1.  No painel de navegação à esquerda, selecione **Agents**. Em seguida, selecione **Create blank agent**.

    ![](./media/image92.png)

2.  Na página **Agent**, selecione **Edit** em **Details**.

    ![](./media/image93.png)

3.  Na página de configuração do agente, no campo **Name**, digite `Zava IT Support Agent`.

4.  No campo **Description**, digite `An AI assistant that helps Zava employees resolve common IT issues, submit support requests, and find IT policy documentation.` Em seguida, selecione **Save**.

5.  No campo **Instructions**, selecione **Edit**.

    ![](./media/image94.png)

6.  Insira o seguinte e selecione **Save**.

    ```
    You are the Zava IT Support Agent. Help users with common IT questions using publicly available Microsoft support documentation and Zava IT policies. Do not access or share any sensitive financial or HR information. Escalate complex issues to the IT helpdesk.
    ```
    ![](./media/image95.png)

7.  Na página de configuração do agente, localize a seção **Knowledge.** Selecione **+ Add knowledge**.

    ![](./media/image96.png)

8.  No painel **Add knowledge**, selecione **Public Website**.

    ![](./media/image97.png)

9.  No campo **URL,** insira a seguinte URL do site. Em seguida, selecione **Add** para conectar o site como fonte de conhecimento: `https://support.microsoft.com/`

    ![](./media/image98.png)

10. Em seguida, selecione **Add to agent**.

    ![](./media/image99.png)

11. No canto superior direito da página de configuração do agente, selecione **Publish**.

    ![](./media/image100.png)

12. Na caixa de diálogo de confirmação, selecione **Publish** para confirmar.

    ![](./media/image101.png)

13. Na página de configuração do agente, localize a aba **Channels** na seção superior (selecione **+2** se ela não estiver visível diretamente).

    ![](./media/image102.png)

14. Selecione o **Microsoft 365 Copilot and Microsoft Teams** para adicioná-los como canais.

    ![](./media/image103.png)

15. Em seguida, selecione **Add channel**.

    ![](./media/image104.png)

16. Selecione **Availability options**.

    ![](./media/image105.png)

17. Na página do **Microsoft 365 Copilot and Microsoft Teams**, selecione **Show to everyone in my org**.

    ![](./media/image106.png)

18. Selecione **Submit to org catalog**.

    ![](./media/image107.png)

19. Na caixa de diálogo de confirmação **Give everyone access to this agent?**, selecione **Yes**.

    ![](./media/image108.png)

20. Você será redirecionado para **Show in Teams app store for org** e verá uma notificação: **Your agent is submitted and waiting for approval from your Teams admin.**

**Exercício 3: Carregue os arquivos de conhecimento da Zava para o SharePoint**

Neste exercício, o MOD Administrator carrega os documentos comerciais de amostra da Zava nos sites de RH e finanças do SharePoint. Esses arquivos contêm dados confidenciais, incluindo informações pessoais de funcionários, registros de folha de pagamento, números de cartão de crédito e previsões financeiras, que acionarão detecções de segurança e correspondências de políticas de DLP ao longo dos Laboratórios 04, 05 e 07.

**Tarefa 1: Carregue os arquivos para o site do SharePoint de RH da Zava**

1.  Abra uma nova aba do navegador e navegue até `https://\[TenantPrefix\].sharepoint.com/sites/HR`.

> **Observação:** Substitua \[TenantPrefix\] pelo prefixo do seu locatário na aba **Resources**.

2.  Na barra de navegação superior, selecione **Benefits @ Contoso** e, em seguida, no painel de subnavegação à esquerda, selecione **Documents**.

    ![](./media/image109.png)

3.  Na página **Documents**, selecione **Create or upload**. Em seguida, selecione **Files upload**.

    ![](./media/image110.png)

4.  No seletor de arquivos, navegue até a pasta **Lab Files** \> **HR** na área de trabalho da sua máquina virtual de laboratório.

5.  Selecione os seguintes arquivos e, em seguida, selecione **Open** para carregá-los:

| **Nome do arquivo** | **Conteúdo** |
|----|----|
| Zava_HR_Policy_2024.docx | Política de licenças e medidas disciplinares, sem informações de identificação pessoal. |
| Zava_Employee_Records.xlsx | IDs de funcionários (formato: ZVA123456), nomes, data de nascimento, salário. |
| Zava_Payroll_Q1_2025.xlsx | Dados da folha de pagamento com números de cartão de crédito na coluna de despesas. |
| Zava_Onboarding_Guide.docx | Conteúdo padrão de integração. |
| Zava_Benefits_Summary.pdf | Detalhes sobre seguro e previdência. |
| Zava_Org_Chart.docx | Canais de comunicação e estrutura de gestão. |
| Zava_Termination_Checklist.docx | Processo de desligamento de funcionários, com nomes e datas. |
| Zava_Sick_Leave_Report.xlsx | Nomes dos funcionários e motivos de afastamento por doença. |

6.  Aguarde até que todos os 8 arquivos terminem de ser carregados.

7.  Na página **Documents**, confirme se todos os 8 arquivos aparecem na biblioteca de documentos.

    ![](./media/image111.png)

8.  Selecione **Zava_Employee_Records.xlsx** para abri-lo.

9.  Confirme se o arquivo abre e exibe os dados dos funcionários, incluindo IDs, nomes e informações salariais.

10. Feche o arquivo e retorne à biblioteca **Documents**.

**Tarefa 2: Carregue os arquivos para o site do SharePoint de finanças da Zava**

1.  Abra uma nova aba do navegador e navegue até `https://\[TenantPrefix\].sharepoint.com/sites/Operations`.

> **Observação:** Substitua \[TenantPrefix\] pelo prefixo do seu locatário na aba **Resources**.

2.  No painel de navegação à esquerda, selecione **Documents**.

    ![](./media/image112.png)

3.  Na página **Documents**, selecione **Create or upload** \> **Files upload**.

    ![](./media/image113.png)

4.  No seletor de arquivos, navegue até a pasta **Lab Files \> Operations** na área de trabalho da sua VM de laboratório.

5.  Selecione os seguintes arquivos e, em seguida, selecione **Open** para carregá-los:

| **Nome do arquivo** | **Conteúdo** |
|----|----|
| Zava_Budget_2025.xlsx | Orçamentos departamentais e centros de custo. |
| Zava_Invoice_Log.xlsx | Faturas de fornecedores com IBAN e números de conta. |
| Zava_Expense_Report_Alex.xlsx | Despesas de Alex Wilber com o número do cartão de crédito Visa. |
| Zava_Audit_Report_2024.docx | Resultados de auditoria interna — classificados como confidenciais. |
| Zava_Contracts_External.docx | Contrato de fornecedor terceirizado — compartilhado externamente. |
| Zava_Financial_Projections.xlsx | Previsões de receita com permissões amplas no SharePoint. |

6.  Aguarde até que todos os 6 arquivos terminem de ser carregados.

7.  Na página **Documents**, confirme se todos os 6 arquivos aparecem na biblioteca de documentos.

8.  Selecione **Zava_Expense_Report_Alex.xlsx** para abri-lo.

9.  Confirme que o arquivo é aberto e exibe dados de despesas, incluindo informações de cartão de crédito.

10. Feche o arquivo e retorne à biblioteca **Documents**.

**Tarefa 3: Verifique os agentes no registro de agentes do Microsoft Agent 365**

1.  Abra uma nova aba do navegador e acesse `https://admin.cloud.microsoft/`. Faça login com as credenciais do **MOD Administrator,** se solicitado.

2.  No painel de navegação à esquerda, selecione **Agents**. Se esta opção não estiver visível, selecione **IA** e, em seguida, selecione **Agents**. Depois, selecione **All agents**.

3.  Nesta página, confirme se os três agentes a seguir aparecem na lista. Você pode pesquisar por Zava na caixa de pesquisa para filtrar os resultados.

    ![](./media/image114.png)

| **Nome do agente**    | **Status** | **Editor**    |
|-----------------------|------------|---------------|
| Zava HR Assistant     | Ativo      | Editor padrão |
| Zava Finance Agent    | Ativo      | Editor padrão |
| Zava IT Support Agent | Ativo      | Editor padrão |

    > **Observação:** Pode levar até 10 minutos após a publicação no Copilot Studio para que os agentes apareçam no registro de agentes. Se os agentes não estiverem visíveis, aguarde 10 minutos e atualize a página.

**Resumo**

Neste laboratório, você concluiu a configuração básica do ambiente para o curso de segurança de IA da Zava Corporation. Você criou um grupo de segurança com atribuição de função no centro de administração do Microsoft Entra, configurou o MOD Administrator como proprietário e membro, atribuiu a função de administrador de função privilegiada e habilitou o grupo como o grupo autorizado de autores do Copilot Studio no centro de administração do Power Platform. Você habilitou a identidade do agente Entra para o Copilot Studio no nível do ambiente, adicionou uma conexão com o SharePoint no portal Power Apps maker e criou três agentes do Copilot Studio, assistente de RH da Zava, agente financeiro da Zava e agente de suporte de TI da Zava, cada um conectado a uma fonte de conhecimento designada, publicada nos canais do Teams e do Microsoft 365. Você carregou 14 documentos comerciais de exemplo contendo dados confidenciais realistas nos sites do SharePoint de RH e Finanças da Zava e verificou se todos os três agentes estão registrados e ativos no registro de agentes do Microsoft Agent 365. O ambiente agora está totalmente preparado para a configuração de segurança nos laboratórios 01 a 07.
