**Laboratório 04: Visão geral da Microsoft — rótulos de confidencialidade e DLP para Copilot**

**Introdução**

A equipe de segurança da informação da Zava identificou que o agente financeiro da Zava e o assistente de RH da Zava podem recuperar e exibir conteúdo do SharePoint sem terem consciência do grau de confidencialidade desse conteúdo. O CISO determinou que todos os documentos confidenciais de RH e financeiros devem ser rotulados até o final do segundo dia, e que o Microsoft 365 Copilot deve ser impedido de processar documentos rotulados como dados confidenciais de RH.

A Zava Corporation lida com registros confidenciais de funcionários, dados financeiros e contratos de fornecedores em sites do SharePoint que agora estão conectados a agentes de IA. Sem rótulos de confidencialidade e políticas de proteção contra perda de dados, esses agentes podem exibir conteúdo protegido para qualquer usuário que solicitar, independentemente de seus direitos de acesso ou obrigações de tratamento de dados.

Neste laboratório, o MOD Administrator habilitará o suporte a rótulos de confidencialidade para o SharePoint e o OneDrive, criará a taxonomia de rótulos da Zava usando um grupo de rótulos e rótulos secundários, configurará a rotulagem automática para dados financeiros, publicará rótulos para os usuários e criará uma política de DLP que impeça o Microsoft 365 Copilot de processar conteúdo rotulado. Adele Vance testará se o Copilot está impedido de exibir conteúdo rotulado. Patti Fernandes verificará a trilha de auditoria.

**Objetivos**

- Ativar o suporte à coautoria de rótulos de confidencialidade para o SharePoint e o OneDrive.

- Criar um grupo de rótulos Zava e dois rótulos secundários para dados de RH e financeiros.

- Configurar uma política de rotulagem automática para aplicar automaticamente o rótulo “Dados Financeiros” a conteúdos com tipos de informações financeiras confidenciais.

- Publicar ambos os rótulos para todos os usuários da Zava.

- Aplicar manualmente o rótulo “Dados de RH” aos arquivos da Zava HR no SharePoint.

- Criar uma política de DLP direcionada ao local do Microsoft 365 Copilot para bloquear o processamento de conteúdo rotulado como RH.

- Testar a aplicação da DLP como Adele Vance por meio do chat do Microsoft 365 Copilot.

- Investigar o evento de auditoria de correspondência de DLP como Patti Fernandes no Purview Audit.

**Duração do laboratório**

Tempo estimado: **30 minutos**

**Exercício 1: Habilite o suporte a rótulos de confidencialidade para SharePoint e OneDrive**

**Tarefa 1: Habilite a coautoria para arquivos com rótulos de confidencialidade**

1.  Abra um navegador e acesse `https://purview.microsoft.com`. Faça login com as credenciais **MOD Administrator**, se solicitado. No painel de navegação à esquerda, selecione **Settings**.

2.  Em **Settings**, selecione **Information Protection**.

    ![](./media/image1.png)

3.  Na página **Information Protection settings**, selecione a aba **Co-authoring for files with sensitivity labels Co-authoring for files with sensitivity labels**.

4.  Selecione a caixa de seleção **Turn on co-authoring for files with sensitivity labels**.

    ![](./media/image2.png)

5.  Selecione **Apply** na parte inferior da página.

    ![](./media/image3.png)

	> **Observação:** Habilitar a coautoria também ativa o suporte a rótulos de confidencialidade para arquivos armazenados no SharePoint e no OneDrive. Isso é um pré-requisito para aplicar rótulos a arquivos hospedados no SharePoint e para que o Defender for Cloud Apps verifique os metadados dos rótulos nos arquivos. Sem essa configuração, o botão de confidencialidade não aparecerá no Office para a Web.

**Exercício 2: Crie a taxonomia de rótulos de confidencialidade da Zava**

**Tarefa 1: Crie o grupo de rótulos confidenciais da Zava**

1.  No painel de navegação à esquerda do portal Microsoft Purview, selecione **Solutions**. Selecione **Information Protection**.

    ![](./media/image4.png)

2.  Na subnavegação à esquerda, selecione **Sensitivity labels**.

    ![](./media/image5.png)

3.  Se solicitado, selecione **Get started** para migrar para o novo esquema de rótulos.

    ![](./media/image6.png)

4.  Selecione **Migrate** no painel suspenso e, em seguida, **Confirm migration**.

    ![](./media/image7.png)
>
    ![](./media/image8.png)

5.  Na página **Sensitivity labels**, selecione **+ Create** e, em seguida, selecione **Label group**.

    ![](./media/image9.png)

6.  Na página de configuração **New label group**, na etapa **Provide basic details for this label group**, insira o seguinte:

    - **Name:** `Zava-Confidential`

    - **Display name:** `Zava-Confidential`

    - **Description for users:** `Use this label group for all Zava confidential content requiring restricted handling.`

    - **Description for admins:** `Zava confidential label group. Contains child labels for HR and Financial data classifications.`

7.  Selecione **Next**.

    ![](./media/image10.png)

8.  Na página **Review your settings and finish**, selecione **Create label group**.

    ![](./media/image11.png)

9.  Na página **Your label group was created**, selecione **Done**.

    ![](./media/image12.png)

10. Confirme se **Zava-Confidential** aparece na lista de rótulos.

    ![](./media/image13.png)

**Tarefa 2: Crie o rótulo secundário de dados de RH**

1.  Na página **Sensitivity labels**, localize o grupo de rótulos **Zava-Confidential**. Selecione as reticências verticais (**...**) ao lado de **Zava-Confidential**. Selecione **+ Create label in group** no menu suspenso.

    ![](./media/image14.png)

2.  Na página **Provide basic details for this label**, insira o seguinte:

    - **Name:** `HR-Data`

    - **Display name:** `HR-Data`

    - **Description for users:** `Apply this label to documents containing Zava employee data including personnel files, payroll records, sick leave reports, and termination documentation.`

    - **Description for admins:** `Child label of Zava-Confidential. Used to classify HR documents on the Zava HR SharePoint site. Triggers DLP enforcement in Microsoft 365 Copilot.`

3.  Selecione **Next**.

    ![](./media/image15.png)

4.  Na página **Define the scope for this label**, selecione **Files** e **Emails**. Certifique-se de que **Meetings** esteja desmarcado. Selecione **Next**.

    ![](./media/image16.png)

5.  Na página **Choose protection settings for labeled items**, selecione **Next**.

    ![](./media/image17.png)

6.  Na página **Content marking**, defina o botão de alternância para **On**.

    ![](./media/image18.png)

7.  Selecione a caixa de seleção **Add a header**. Selecione o ícone de edição abaixo para **Add a header.**

    ![](./media/image19.png)

8.  No campo **Header text**, digite `ZAVA CONFIDENTIAL — HR DATA`. Selecione **Save**.

    ![](./media/image20.png)

9.  Selecione a caixa de seleção **Add a footer**. Selecione o ícone de edição abaixo para **Add a footer**.

    ![](./media/image21.png)

10. No campo **Footer text**, digite `Restricted — Zava HR use only`. Selecione **Save**.

    ![](./media/image22.png)

11. Selecione **Next**.

    ![](./media/image23.png)

12. Na página **Auto-labeling for files and emails**, selecione **Next**.

    ![](./media/image24.png)

13. Na página **Define protection settings for groups and sites**, selecione **Next**.

    ![](./media/image25.png)

14. Na página **Review your settings and finish**, selecione **Create label**.

    ![](./media/image26.png)

15. Na página **Your sensitivity label was created**, selecione **Don't create a policy yet**.. Selecione **Done**.

    ![](./media/image27.png)

**Tarefa 3: Crie rótulos secundários de dados financeiros com rotulagem automática**

1.  Na página **Sensitivity labels**, localize o grupo de rótulos **Zava-Confidential**. Selecione as reticências verticais ( **...** ) ao lado da **Zava-Confidential**. Selecione **+ Create label in group** no menu suspenso.

    ![](./media/image28.png)

2.  Na página **Provide basic details for this label**, insira o seguinte:

    - **Name:** `Financial-Data`

    - **Display name:** `Financial-Data`

    - **Description for users:** `Apply this label to documents containing Zava financial data including invoices, budgets, expense reports, credit card numbers, or bank account information.`

    - **Description for admins:** `Child label of Zava-Confidential. Used to classify financial documents on the Zava Finance SharePoint site. Configured with auto-labelling for credit card numbers, ABA routing numbers, and SWIFT codes.`

3.  Selecione **Next**.

    ![](./media/image29.png)

4.  Na página **Define the scope for this label**, selecione **Files** e **Emails**. Certifique-se de que **Meetings** esteja desmarcado. Selecione **Next**.

    ![](./media/image30.png)

5.  Na página **Choose protection settings for labeled items**, selecione **Apply content marking**. Selecione **Next**.

    ![](./media/image31.png)

6.  Na página, defina o botão de alternância **Content marking** como **On**.

7.  Selecione a caixa de seleção **Add a footer**. Selecione o ícone de edição ao lado de **Add a footer.**

    ![](./media/image32.png)

8.  No campo **Footer text**, digite `Restricted — Zava Finance use only`. Selecione **Save**.

    ![](./media/image33.png)

9.  Selecione **Next**.

    ![](./media/image34.png)

10. Na página **Auto-labeling for files and emails**, defina o botão de alternância para **On**.

    ![](./media/image35.png)

11. Em **Detect content that matches these conditions**, selecione **+ Add condition**. Selecione **Content contains**.

    ![](./media/image36.png)

12. Na seção **Content contains**, selecione **Add**. Selecione **Sensitive info types**.

    ![](./media/image37.png)

13. No painel **Sensitive info types**, pesquise e selecione os seguintes **Sensitive info types**:

    - `Credit Card Number`

    - `ABA Routing Number`

    - `SWIFT Code`

14. Selecione **Add** para confirmar a seleção.

    ![](./media/image38.png)

15. Selecione **Next**.

    ![](./media/image39.png)

16. Na página **Define protection settings for groups and sites**, selecione **Next**.

    ![](./media/image40.png)

17. Na página **Review your settings and finish**, selecione **Create label**.

    ![](./media/image41.png)

18. Na página **Your sensitivity label was created**, selecione **Automatically apply label to sensitive content**. Selecione **Done**.

    ![](./media/image42.png)

19. Na página suspensa **Create auto-labeling policy**, selecione **Review policy**.

    ![](./media/image43.png)

**Tarefa 4: Configure e salve a política de rotulagem automática de dados financeiros**

1.  Na página **Name your auto-labeling policy**, confirme se o nome padrão reflete o rótulo **Financial-Data** e selecione **Next**.

    ![](./media/image44.png)

2.  Na página **Choose a label to auto-apply**, confirme se **Zava-Confidential/Financial-Data** está selecionado e, em seguida, selecione **Next**.

    ![](./media/image45.png)

3.  Na página **Assign admin units**, selecione **Next**.

    ![](./media/image46.png)

4.  Na página **Choose locations where you want to apply the label**, selecione os seguintes locais:

    - **Exchange email**

    - **SharePoint sites**

    - **OneDrive accounts**

5.  Selecione **Next**.

    ![](./media/image47.png)

6.  Na página **Set up common or advanced rules**, deixe a opção **Common rules** selecionada e clique em **Next**.

    ![](./media/image48.png)

7.  Na página **Define rules for content in all locations**, expanda a **Financial-Data rule** para confirmar se o número do cartão de crédito, Número de roteamento ABA e código SWIFT estão listados como condições.

8.  Selecione **Next**.

    ![](./media/image49.png)

9.  Na página **Additional label settings**, selecione **Next**.

    ![](./media/image50.png)

10. Na página **Decide if you want to test out the policy now or later**, selecione **Run policy in simulation mode**.

11. Selecione a caixa de seleção **Automatically turn on policy if not modified after 7 days in simulation**.

12. Selecione **Next**.

    ![](./media/image51.png)

13. Na página **Review and finish**, selecione **Create policy**.

    ![](./media/image52.png)

14. Na página **Your auto-labeling policy was created**, selecione **Done**.

    ![](./media/image53.png)

	> **Observação:** A política de rotulagem automática analisará o conteúdo existente no SharePoint, OneDrive e Exchange no modo de simulação. Os arquivos Zava_Expense_Report_Alex.xlsx, Zava_Payroll_Q1_2025.xlsx e Zava_Invoice_Log.xlsx carregados no laboratório 00 contêm números de cartão de crédito e valores IBAN e serão correspondidos por esta política. Após 7 dias em simulação sem modificações, a política será ativada automaticamente e começará a aplicar o rótulo de dados financeiros aos arquivos correspondentes.

**Exercício 3: Publique rótulos de confidencialidade para usuários da Zava**

**Tarefa 1: Publique os rótulos confidenciais da Zava**

1.  Na página **Sensitivity labels**, selecione **Publish labels**.

    ![](./media/image54.png)

2.  Na página **Choose sensitivity labels to publish**, selecione **Choose sensitivity labels to publish**.

    ![](./media/image55.png)

3.  No painel suspenso **Sensitivity labels to publish**, selecione as caixas de seleção para ambos os rótulos a seguir:

    - **Zava-Confidential/HR-Data**

    - **Zava-Confidential/Financial-Data**

4.  Selecione **Add** na parte inferior do painel suspenso.

    ![](./media/image56.png)

5.  Na página **Choose sensitivity labels to publish**, selecione **Next**.

    ![](./media/image57.png)

6.  Em seguida, selecione **Next** até chegar à página de nomeação.

7.  Na página **Name your policy**, insira o seguinte:

    - **Name:** `Zava-Confidential Label Policy`

    - **Description:** `Publishes Zava-Confidential HR-Data and Financial-Data labels to all Zava users for manual and auto-labelling of sensitive content.`

8.  Selecione **Next**.

    ![](./media/image58.png)

9.  Na página **Review and finish**, selecione **Submit**.

    ![](./media/image59.png)

10. Na página **New policy created**, selecione **Done**.

    ![](./media/image60.png)

	> **Observação:** A propagação da política de rótulos pode levar até 24 horas para que o botão de confidencialidade apareça no Office para a Web para todos os usuários. Neste laboratório, o MOD Administrator aplicará rótulos diretamente por meio da coluna de confidencialidade da biblioteca de documentos do SharePoint no próximo exercício, o que não depende do botão de confidencialidade do aplicativo do Office.

**Exercício 4: Aplique o rótulo de dados de RH aos arquivos do SharePoint de RH da Zava**

**Tarefa 1: Aplique rótulos de confidencialidade por meio da biblioteca de documentos do SharePoint**

1.  Abra uma nova aba do navegador e navegue até `https://\[TenantName\].sharepoint.com/sites/HR`.

	> **Observação:** Substitua \[TenantName\] pelo prefixo do seu locatário da aba **Resources**.

2.  No painel de navegação superior, selecione **HR** \> **Benefits@Contoso**.

    ![](./media/image61.png)

3.  Selecione **Documents** na navegação à esquerda e localize o arquivo **Zava_Employee_Records.xlsx**. Marque a caixa de seleção à esquerda de **Zava_Employee_Records.xlsx** para selecionar.

    ![](./media/image62.png)

4.  Na barra de ferramentas acima da biblioteca de documentos, selecione **⋯** (Mais opções) ou o ícone **Details**.

    ![](./media/image63.png)

5.  No **Details pane** ou no menu de contexto ao clicar com o botão direito, selecione o campo **Sensitivity** e, em seguida, escolha **Zava-Confidential/HR-Data** na lista suspensa.

6.  Repita as etapas 3 a 5 para os seguintes arquivos:

    - Zava_Payroll_Q1_2025.xlsx

    - Zava_Sick_Leave_Report.xlsx

    - Zava_Termination_Checklist.docx

7.  Confirme se todos os quatro arquivos mostram **Zava-Confidential/HR-Data** na coluna **Sensitivity**.

	> **Observação:** Se a coluna de confidencialidade não estiver visível na biblioteca de documentos, selecione **Add column** na linha do cabeçalho da coluna e adicione a coluna **Sensitivity**. Se as opções de rótulo de confidencialidade ainda não aparecerem devido a um atraso na propagação, aguarde de 15 a 30 minutos e tente novamente. Como alternativa, abra cada arquivo no Office para a Web, selecione **Sensitivity** na faixa de opções e aplique o rótulo diretamente no documento.

**Exercício 5: Crie uma política de DLP para o Microsoft 365 Copilot**

**Tarefa 1: Crie a política de DLP**

1.  Retorne ao portal Microsoft Purview em `https://purview.microsoft.com`. No painel de navegação à esquerda, selecione **Solutions**. Selecione **Data Loss Prevention**.

    ![](./media/image64.png)

2.  Na subnavegação à esquerda, selecione **Policies**.

    ![](./media/image65.png)

3.  Na página **Policies**, selecione **+ Create policy**.

    ![](./media/image66.png)

4.  No painel **What info do you want to protect?**, selecione **Enterprise applications and devices**.

    ![](./media/image67.png)

5.  Na página **Start with a template or create a custom policy**, selecione **Custom** em **Categories**. Selecione **Custom policy** em **Regulations**. Selecione **Next**.

    ![](./media/image68.png)

6.  Na página **Name your DLP policy**, digite o seguinte:

    - **Name:** `Zava - Block HR Data in M365 Copilot`

    - **Description:** `Prevents Microsoft 365 Copilot and Copilot Chat from processing or surfacing documents labelled as Zava-Confidential HR-Data.`

7.  Selecione **Next**.

    ![](./media/image69.png)

8.  Na página **Assign admin units**, selecione **Next**.

    ![](./media/image70.png)

9.  Na página **Choose locations to apply the policy**, desmarque todos os locais que estão ativados por padrão.

10. Localize **Microsoft 365 Copilot e Copilot Chat** na lista de locais. Ative os botões de alternância para **Microsoft 365 Copilot e Copilot Chat.**

    ![](./media/image71.png)

11. Confirme se todas as outras localizações permanecem **Off**.

12. Selecione **Next**.

    ![](./media/image72.png)

	> **Observação:** A localização Microsoft 365 Copilot e Copilot Chat aplica controles de política de DLP às interações no Microsoft 365 Copilot Chat e em experiências com tecnologia Copilot. Não se aplica a agentes personalizados do Copilot Studio acessados diretamente. O teste no exercício 6 utilizará o M365 Copilot Chat em copilot.microsoft.com, e não diretamente o Zava HR Assistant, para validar a aplicação das políticas.

13. Na página **Define policy settings**, selecione **Create or customize advanced DLP rules**. Selecione **Next**.

    ![](./media/image73.png)

14. Na página **Customize advanced DLP rules**, selecione **+ Create rule**.

    ![](./media/image74.png)

15. No painel **Create rule**, no campo **Name**, digite `Block Copilot access to HR-labelled content`.

16. No campo **Description**, digite `Blocks Microsoft 365 Copilot from processing files labelled Zava-Confidential/HR-Data.`

17. Em **Conditions**, selecione **+ Add condition**. Selecione **Content contains**.

    ![](./media/image75.png)

18. Na seção **Content contains**, selecione **Add**. Selecione **Sensitivity labels**.

19. No painel suspenso **Sensitivity labels**, procure e selecione **Zava-Confidential/HR-Data**. Selecione **Add** para confirmar.

20. Em **Actions**, selecione **+ Add an action**. Selecione **Restrict Copilot from processing contents**.

21. Em **Restrict Copilot from processing contents**, selecione a caixa de seleção ao lado de **Accessing knowledge sources**.

22. Selecione **Save** para salvar a regra.

23. Confirme se **Block Copilot access to HR-labelled content** aparece na lista de regras na página **Customize advanced DLP rules**. Selecione **Next**.

24. Na página **Policy mode**, selecione **Turn the policy on immediately**. Selecione **Next**.

25. Na página **Review and finish**, revise a configuração da política e selecione **Submit**.

26. Na página **New policy created**, selecione **Done**.

27. Na página **Policies**, confirme se **Zava - Block HR Data in M365 Copilot** aparece na lista com o status **On**.

	> **Observação:** A propagação da política de DLP para a localização Microsoft 365 Copilot pode levar até quatro horas. Se o teste no exercício 6 não gerar um bloqueio imediatamente, isso é esperado. Prossiga com o teste, registre a resposta e, se a aplicação ainda não estiver ativa, retorne a este teste após concluir o laboratório 05 ou ao final do dia. O evento no registro de auditoria confirmará quando a aplicação for acionada pela primeira vez.

**Exercício 6: Teste a aplicação de DLP por meio do Microsoft 365 Copilot Chat**

**Tarefa 1: Tentativa de acesso a conteúdo rotulado de RH como Adele Vance**

1.  Abra uma nova janela do navegador em modo **InPrivate** ou **Incognito**.

2.  Acesse `https://copilot.microsoft.com`.

3.  Faça login com as credenciais de **Adele Vance** na aba **Resources**.

4.  No campo de entrada do chat do Microsoft 365 Copilot, digite a seguinte mensagem:
 
    ```
    Summarise the contents of Zava_Employee_Records.xlsx
    ```

5.  Aguarde a resposta.

6.  Analise a resposta com atenção:

    - **Se a DLP estiver em vigor:** O Copilot retornará uma resposta indicando que não pode acessar ou compartilhar o conteúdo devido a uma política de proteção de dados. Uma dica sobre a política pode ser exibida.

    - **Se a propagação do DLP ainda estiver em andamento:** O Copilot poderá retornar um resumo parcial ou um link de referência para o arquivo. Anote a resposta e retorne a esta etapa após concluir o laboratório 05.

7.  Digite um segundo comando:

    ```
	What employee salary information is in the HR SharePoint site?
    ```

8.  Analise a resposta e observe se o Copilot restringe ou exibe o conteúdo.

9.  Feche a janela do navegador em modo **InPrivate.**

**Exercício 7: Investigue o evento de correspondência DLP no Purview Audit**

**Tarefa 1: Pesquise eventos de correspondência DLP como Patti Fernandes**

1.  Abra uma nova janela do navegador em modo **InPrivate** ou **Incognito**.

2.  Acesse `https://purview.microsoft.com`.

3.  Faça login com as credenciais **Patti Fernandes** na aba **Resources**.

4.  No painel de navegação à esquerda, selecione **Solutions**.

5.  Selecione **Audit**.

6.  Na página **Audit**, selecione a aba **New Search**.

7.  Configure a pesquisa com os seguintes valores:

    - **Start date:** Selecione a data de hoje menos 1 dia.

    - **End date:** Selecione a data de hoje.

    - **Activities – friendly names:** Digite `DLP` e selecione **DLP rule matched** na lista suspensa.

    - **Users:** Deixe em branco.

8.  Selecione **Search**.

9.  Aguarde a conclusão da tarefa de pesquisa.

10. Analise os resultados para quaisquer entradas associadas a **Adele Vance** e à política **Zava - Block HR Data in M365 Copilot**.

11. Se uma correspondência for encontrada, selecione a entrada para abrir o painel de detalhes do registro de auditoria.

12. No painel de detalhes, observe os seguintes campos:

    - **Date**

    - **User**

    - **Activity**

    - **Policy name**

    - **Rule name**

    - **Sensitivity label**

    - **Location**

13. Feche o painel de detalhes.

14. Feche a janela do navegador **InPrivate**.

	> **Observação:** Se nenhum evento de correspondência DLP aparecer no registro de auditoria, isso indica que a política DLP ainda não foi totalmente propagada ou que a interação de teste no exercício 6 não acionou a aplicação da política. Os eventos de auditoria DLP para o local do Copilot podem levar até uma hora para aparecer no registro de auditoria após a aplicação da política. Retorne a esta pesquisa após concluir o laboratório 05 se os resultados ainda não estiverem disponíveis.

**Resumo**

Neste laboratório, você habilitou o suporte de coautoria de rótulos de confidencialidade no Microsoft Purview, ativando a detecção de rótulos para arquivos do SharePoint e OneDrive. Você criou o grupo de rótulos **Zava-Confidential** e dois rótulos secundários **HR-Data** e **Financial-Data** com marcações de conteúdo para identificar documentos classificados.

Você configurou uma política de rotulagem automática para Financial-Data, que detecta números de cartão de crédito, números de roteamento ABA e códigos SWIFT no SharePoint, OneDrive e Exchange, operando em modo de simulação com aplicação automática após sete dias.

Você publicou ambos os rótulos para todos os usuários da Zava por meio da política de rótulos **Zava-Confidential Label Policy** e aplicou manualmente o rótulo **HR-Data** a quatro documentos confidenciais de RH no site do SharePoint de RH da Zava.

Você criou a política de DLP **Zava - Block HR Data in M365 Copilot**, direcionada às localizações Microsoft 365 Copilot e Copilot Chat, bloqueando o acesso a qualquer conteúdo com o rótulo HR-Data. Adele Vance testou se o Microsoft 365 Copilot poderia acessar conteúdo de RH rotulado, e Patti Fernandes pesquisou o registro do Purview Audit para verificar o evento de correspondência da política de DLP. Agora, os dados confidenciais da Zava estão classificados, e o Microsoft 365 Copilot está governado por controles de acesso baseados em políticas.
