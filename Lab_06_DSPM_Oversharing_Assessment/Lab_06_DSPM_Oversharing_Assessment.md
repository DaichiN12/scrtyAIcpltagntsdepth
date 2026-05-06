**Laboratório 06: DSPM — Avaliação e remediação do excesso de compartilhamento**

**Introdução**

O Microsoft Purview Data Security Posture Management é a porta de entrada unificada para descobrir, proteger e investigar riscos de dados confidenciais em todo o ambiente digital da Zava, incluindo aplicativos de IA, agentes, sites do SharePoint e interações do usuário. Ao contrário da experiência clássica de DSPM para IA, o novo DSPM combina a postura de segurança de dados tradicional com a observabilidade de IA em uma única solução, organizada em torno de objetivos de segurança baseados em resultados.

Neste laboratório, a análise de risco de dados é iniciada logo no começo do terceiro dia, antes de qualquer outra atividade, para que os resultados estejam disponíveis quando os alunos chegarem ao exercício 4. Os exercícios de geração de sinais criam eventos de interação realistas com o Copilot, fazendo referência a arquivos confidenciais da Zava. O MOD Administrator e Patti Fernandes utilizam então os objetivos do DSPM, políticas de um clique, resultados da avaliação e o Activity Explorer para investigar e remediar os riscos de compartilhamento excessivo em todo o ambiente do agente Zava.

**Cenário**

O CISO da Zava recebeu uma notificação da equipe de conformidade: o Assistente de RH e o agente financeiro podem estar expondo registros confidenciais de funcionários e financeiros a usuários que não deveriam ter acesso a esses dados. A equipe de segurança precisa compreender o escopo total da exposição de dados, ativar políticas de gerenciamento de postura e aplicar controles de remediação antes do final do terceiro dia.

O MOD Administrator lançará uma avaliação personalizada de risco de dados nos sites do SharePoint de RH e finanças da Zava, ativará políticas de um clique do DSPM e usará o painel de objetivos para conduzir a remediação. Adele Vance gerará sinais realistas de interação do Copilot referenciando arquivos marcados como confidenciais. Patti Fernandes investigará as atividades de IA no DSPM Activity Explorer e analisará as conclusões sobre o compartilhamento excessivo da avaliação.

**Objetivos**

- Iniciar uma avaliação de risco de dados DSPM personalizada para os sites do SharePoint de RH e finanças da Zava no início do terceiro dia.

- Gerar sinais de interação realistas do Microsoft 365 Copilot, fazendo referência a arquivos confidenciais rotulados como Adele Vance.

- Explorar a nova experiência do DSPM e revisar o painel de controle de postura.

- Ativar as políticas DSPM com um clique para detecção de uso de IA de risco e proteção de dados confidenciais.

- Analisar os objetivos do DSPM relativos à partilha excessiva de dados e à exposição de dados do Copilot.

- Analisar os resultados da avaliação de risco de dados e aplicar as ações corretivas.

- Investigar a atividade dos agentes da Zava e o acesso a dados confidenciais no painel de controle de aplicativos e agentes.

- Analisar os eventos de interação com IA no Activity Explorer, filtrados para Adele Vance.

- Aplicar a descoberta de conteúdo restrito do SharePoint ao site de RH da Zava.

**Duração do laboratório**

Tempo estimado: **25 minutos**

⚠️ **IMPORTANTE — Conclua a tarefa 1 do exercício 1 antes de qualquer outra coisa no dia 3.**

A análise de risco de dados pode levar de 30 a 60 minutos para ser concluída. Deve ser iniciada primeiro para que os resultados estejam disponíveis quando você chegar ao exercício 4. Não prossiga para o exercício 2 até que a tarefa 1 do exercício 1 esteja concluída.

**Exercício 1: Inicie a avaliação de riscos de dados**

**Tarefa 1: Registre um aplicativo Entra**

1.  Acesse https://entra.microsoft.com. Faça login com as credenciais **MOD Administrator**, se solicitado.

2.  No painel de navegação à esquerda, selecione **App App registrations** \> **+ New registration**.

> <img src="media/image1.png" style="width:6.26806in;height:3.54792in" />

3.  Configure o seguinte:

    - **Name:** Purview DSPM Item Level Scan

    - **Supported account types:** Selecione **Single tenant only - Contoso**

4.  Selecione **Register**.

> <img src="media/image2.png" style="width:6.26806in;height:3.54792in" />

5.  Na página de visão geral do registro do aplicativo, copie e anote o **Application (client) ID**.

> <img src="media/image3.png" style="width:6.26806in;height:3.54792in" />

6.  Na subnavegação à esquerda, selecione **API permissions**. Selecione **+ Add a permission**.

> <img src="media/image4.png" style="width:6.26806in;height:3.54792in" />

7.  Selecione **Microsoft Graph**.

> <img src="media/image5.png" style="width:6.26806in;height:3.54792in" />

8.  Selecione **Application permissions**.

> <img src="media/image6.png" style="width:6.26806in;height:3.54792in" />

9.  Procure e adicione as seguintes permissões:

    - Application.Read.All

    - Directory.Read.All

    - Files.ReadWrite.All

    - SensitivityLabels.Read.All

    - Sites.ReadWrite.All

    - User.Read.All

    - SensitivityLabel.Read

10. Selecione **Add permissions**.

> <img src="media/image7.png" style="width:6.26806in;height:3.54792in" />

11. Selecione **Grant admin consent for Contoso**.

> <img src="media/image8.png" style="width:6.26806in;height:3.54792in" />

12. Selecione **Yes**  para confirmar.

> <img src="media/image9.png" style="width:6.26806in;height:3.54792in" />

13. Acesse **Certificates & secrets** \> **+ New client secret** \> defina a expiração para **6 months** \> **Add**.

> <img src="media/image10.png" style="width:6.26806in;height:3.54792in" />

14. Copie o **Value**.

> <img src="media/image11.png" style="width:6.26806in;height:3.54792in" />

15. Salve os valores, pois eles só podem ser copiados uma vez e serão necessários na próxima tarefa.

**Tarefa 2: Execute uma avaliação de risco de dados personalizada nos sites do SharePoint da Zava**

1.  Abra um navegador e acesse https://purview.microsoft.com. Faça login com as credenciais **MOD Administrator,** se solicitado.

2.  No painel de navegação à esquerda, selecione **Solutions** \> \*\*DSPM\*\*.

> **Observação:** Não selecione **DSPM for AI (classic)** ou **Data Security Posture Management (classic)**. A nova experiência é chamada de **DSPM** e é uma entrada separada no menu **Solutions**.
>
> <img src="media/image12.png" style="width:6.26806in;height:3.54792in" />

3.  Na página inicial do **DSPM**, se for solicitado que você conclua as tarefas de configuração inicial, selecione **Get started** e aceite qualquer configuração necessária para ativar a solução. Aguarde a conclusão da configuração antes de prosseguir.

4.  Na subnavegação à esquerda, selecione **Discover**. Em **Discover**, selecione **Data risk assessments**.

> <img src="media/image13.png" style="width:6.26806in;height:3.54792in" />

5.  Na notificação **Item-level scan not setup**, selecione **Setup connection**.

> <img src="media/image14.png" style="width:6.26806in;height:3.54792in" />

6.  Na aba **Client Secret**, insira o **Application ID** e o valor **Client secret** copiados na tarefa 1. Em seguida, selecione **Authenticate**. Após a autenticação bem-sucedida, selecione **Save**.

> <img src="media/image15.png" style="width:6.26806in;height:3.54792in" />

7.  Na página **Data risk assessments**, selecione **+ Create custom assessment**.

> <img src="media/image16.png" style="width:6.26806in;height:3.54792in" />

8.  No painel **Basic details**, configure o seguinte:

    - **Assessment name:** Digite Zava SharePoint Oversharing Assessment.

    - **Description:** Digite Custom assessment to identify potentially overshared sensitive items across Zava HR and Finance SharePoint sites.

9.  Selecione **Next**.

> <img src="media/image17.png" style="width:6.26806in;height:3.54792in" />

10. Na **Select scan level**, escolha **Item-level**.

> <img src="media/image18.png" style="width:6.26806in;height:3.54792in" />

11. Selecione **Next** até chegar em **Add data sources to assess**. Ao lado de **SharePoint**, selecione **Scope sites**.

> <img src="media/image19.png" style="width:6.26806in;height:3.54792in" />

12. No seletor de sites do SharePoint, selecione **Include** \> **From all sites**.

> <img src="media/image20.png" style="width:6.26806in;height:3.54792in" />

13. Procure e selecione os dois sites a seguir:

    - HR

    - Operations

14. Selecione **Done** duas vezes para confirmar a seleção do site.

> <img src="media/image21.png" style="width:6.26806in;height:3.54792in" />
>
> <img src="media/image22.png" style="width:6.26806in;height:3.54792in" />

15. Selecione **Next**.

> <img src="media/image23.png" style="width:6.26806in;height:3.54792in" />

16. Selecione **Save and Run**.

> <img src="media/image24.png" style="width:6.26806in;height:3.54792in" />

17. Selecione **Done**.

> <img src="media/image25.png" style="width:6.26806in;height:3.54792in" />

18. Confirme se a avaliação aparece na lista **Data risk assessments** com o status **In progress** ou **Queued**.

> <img src="media/image26.png" style="width:6.26806in;height:3.54792in" />
>
> **Observação:** A avaliação levará de 30 a 60 minutos para ser concluída, dependendo do número de itens nos sites do SharePoint selecionados. Prossiga imediatamente para o exercício 2. Você retornará para revisar os resultados no exercício 4.

**Exercício 2: Gere sinais de interação do Copilot**

Neste exercício, Adele Vance gera eventos de interação realistas do Microsoft 365 Copilot que fazem referência a arquivos confidenciais rotulados nos sites do SharePoint de RH e finanças da Zava. Essas interações aparecerão no DSPM Activity Explorer e nos registros de auditoria, criando os dados de investigação usados nos exercícios 5 e no laboratório 07.

**Tarefa 1: Gere sinais de interação de dados de RH conforme Adele Vance**

1.  Abra uma nova janela **InPrivate** ou **Incognito** do navegador. Acesse https://copilot.microsoft.com. Faça login com as credenciais de **Adele Vance** na aba **Resources**. Conclua as etapas de autenticação, se necessário.

2.  Na navegação, selecione **All agents** \> **Zava HR Assistant** \> **Add**.

> <img src="media/image27.png" style="width:6.26806in;height:3.54792in" />

3.  No campo de entrada, digite a seguinte mensagem:

> Summarise the contents of Zava_Employee_Records.xlsx from the HR SharePoint site
>
> <img src="media/image28.png" style="width:6.26806in;height:3.54792in" />

4.  Aguarde a resposta e anote o que o Copilot retorna.

5.  Digite o seguinte segundo prompt:

> Find all employee salary information across Zava HR documents.

6.  Aguarde a resposta.

7.  Digite o seguinte terceiro prompt:

> What does the Zava payroll report for Q1 2025 contain?

8.  Aguarde a resposta.

**Exercício 3: Explore o painel de postura do DSPM e ative políticas com um clique**

**Tarefa 1: Analise o painel de postura do DSPM**

1.  Retorne à sessão **MOD Administrator** no portal Microsoft Purview em https://purview.microsoft.com .

2.  No painel de navegação à esquerda, selecione **Solutions** \> **DSPM**.

> <img src="media/image12.png" style="width:6.26806in;height:3.54792in" />

3.  Na página inicial do **DSPM**, revise o painel de controle **Posture**.

> <img src="media/image29.png" style="width:6.26806in;height:3.54792in" />

4.  Analise as seguintes seções e observe seus valores atuais:

    - **Security Copilot suggested prompts** — observe as sugestões de prompts disponíveis.

    - **Top objectives to address** — observe quais objetivos estão listados como de maior prioridade.

    - **Data use snapshot** — observe o volume de atividade de dados confidenciais detectada em toda a rede.

    - **30-day trending graph** — analise se a tendência está melhorando ou piorando.

**Tarefa 2: Ative a política de detecção de uso arriscado de IA com um clique**

1.  Na página inicial do **DSPM**, na subnavegação à esquerda, selecione **Tasks and actions**. Em seguida, selecione **Remediation actions**.

> <img src="media/image30.png" style="width:6.26806in;height:3.54792in" />

2.  Selecione **Detect risky interactions in AI apps** para expandir.

> <img src="media/image31.png" style="width:6.26806in;height:3.54792in" />

3.  Selecione **Create Policy** para habilitar a política **DSPM for AI - Detect risky AI usage** do Insider Risk Management.

4.  Confirme que o status da política foi atualizado para **On** ou **Active**. Feche a aba.

> <img src="media/image32.png" style="width:6.26806in;height:3.54792in" />
>
> **Observação:** Esta política de Insider Risk Management detecta prompts e respostas de risco no Microsoft 365 Copilot, agentes e outros aplicativos de IA generativa, incluindo tentativas de injeção de prompts, acesso a materiais protegidos e outros padrões de interação de alto risco. As interações de Adele Vance geradas no exercício 2 serão avaliadas por esta política.

**Tarefa 3: Ative a política de proteção de dados confidenciais com um clique**

1.  Na página **Remediation actions**, selecione **Safeguard sensitive data in Microsoft 365 Copilot interactions** para expandi-la.

> <img src="media/image33.png" style="width:6.26806in;height:3.54792in" />

2.  Selecione **Get started** para ativar esta política de DLP.

> <img src="media/image34.png" style="width:6.26806in;height:3.54792in" />

3.  No painel de dados, selecione **view** ao lado de **Sensitive info types**.

> <img src="media/image35.png" style="width:6.26806in;height:3.54792in" />

4.  Selecione **Credit Card Number**. Em seguida, selecione **Add**.

> <img src="media/image36.png" style="width:6.26806in;height:3.54792in" />

5.  Em **Add**, selecione **Restrict user prompts from being processed**. Em seguida, selecione **Create policy**.

> <img src="media/image37.png" style="width:6.26806in;height:3.54792in" />

6.  Retorne à página **Remediation actions** e selecione **Safeguard sensitive data in Microsoft 365 Copilot interactions**. Em seguida, selecione **Get started**.

> <img src="media/image38.png" style="width:6.26806in;height:3.54792in" />

7.  Selecione **Enforce policy** para ativar a política **Default DLP policy - Protect sensitive M365 Copilot interactions**.

> <img src="media/image39.png" style="width:6.26806in;height:3.54792in" />

8.  Confirme se a política está ativa.

> <img src="media/image40.png" style="width:6.26806in;height:3.54792in" />

**Exercício 4: Analise os resultados da avaliação de risco de dados e aplique as medidas corretivas.**

**Observação:** As avaliações podem levar algum tempo. Você pode retornar a este exercício ao final das aulas práticas, caso a avaliação ainda esteja em andamento.

**Tarefa 1: Retorne aos resultados da avaliação de risco de dados**

1.  Na subnavegação à esquerda, selecione **Discover**. Selecione **Data risk assessments**.

2.  Na página **Data risk assessments**, localize **Zava SharePoint Oversharing Assessment**.

3.  Confirme se o status mostra **Completed**. Se o status ainda mostrar **In progress**, aguarde a conclusão antes de continuar.

4.  Selecione **Zava SharePoint Oversharing Assessment** para abrir os resultados.

**Tarefa 2: Analise itens compartilhados em excesso**

1.  Na página de resultados da avaliação, selecione a aba **Items.**

2.  Analise a lista de itens potencialmente compartilhados em excesso encontrados nos sites do SharePoint de RH e finanças da Zava.

3.  Observe o seguinte para cada item:

    - **File name**

    - **Sensitivity label** — confirme se os arquivos com o rótulo HR-Data estão visíveis.

    - **Sharing scope** — observe se os itens são compartilhados com **Everyone**, **All authenticated users** ou grupos específicos.

    - **Sensitive info types detected**

4.  Localize o **Zava_Employee_Records.xlsx** nos resultados e selecione-o.

5.  Analise o painel de detalhes do item — observe os sensitive info types detectadas, as permissões de compartilhamento e o rótulo aplicado.

6.  Feche o painel de detalhes do item.

**Tarefa 3: Remediação aplicada — acesso restrito por rótulo**

1.  Na página de resultados da avaliação, selecione a aba **Protect.**

2.  Localize a ação corretiva **Restrict access by label.**

3.  Selecione **Restrict access by label**.

4.  No painel de remediação, confirme se **Zava-Confidential/HR-Data** está listado como o rótulo a ser restringido.

5.  Analise a ação — isso criará ou fará referência a uma política de DLP que restringe o acesso a itens com o rótulo HR-Data.

6.  Selecione **Apply** ou **Confirm** para ativar a remediação.

7.  Confirme se o status da ação corretiva foi atualizado para **Applied**.

**Tarefa 4: Remediação aplicada — descoberta de conteúdo restrito habilitada no SharePoint**

1.  Na aba **Protect**, localize a ação de remediação **Restrict all items** ou **Enable Restricted Content Discovery**.

2.  Selecione a ação para abrir o painel de configuração.

3.  Leia a descrição — A descoberta de conteúdo restrito do SharePoint impede que itens do site selecionado sejam exibidos nas respostas do Microsoft 365 Copilot para usuários que não possuem acesso explícito.

4.  Confirme se o escopo está definido para o **Zava HR SharePoint site**.

5.  Selecione **Apply** ou **Enable** para ativar a descoberta de conteúdo restrito para o site Zava HR.

6.  Confirme se o status da ação foi atualizado para **Applied**.

> **Observação:** A descoberta de conteúdo restrito do SharePoint é um dos controles mais eficazes disponíveis para impedir que agentes de IA e o Copilot exibam conteúdo de um site do SharePoint para usuários que não possuem permissão explícita. Isso difere do DLP, pois opera no nível de descoberta do site, em vez do nível de classificação de conteúdo.

**Exercício 5: Investigue a atividade do agente e as interações da IA**

**Tarefa 1: Analise o painel de controle de aplicativos e agentes**

1.  Na subnavegação à esquerda, selecione **Discover**.

2.  Selecione **Apps and agents**.

3.  No painel de controle **Apps and agents**, revise a lista de aplicativos de IA detectados em todo o locatário.

4.  Localize o **Copilot Studio** no filtro de plataforma e aplique-o para exibir somente os agentes do Copilot Studio.

5.  Confirme se os três agentes da Zava aparecem no painel de controle.

6.  Selecione **Zava HR Assistant** para abrir os detalhes do agente.

7.  No painel de detalhes do agente, revise o seguinte:

    - **Sensitive data accessed** — tipos e volume de conteúdo confidencial que o agente consultou.

    - **Policy coverage** — quais políticas da Purview protegem os dados acessados por este agente.

    - **Users** — quais usuários interagiram com este agente.

8.  Feche o painel de detalhes do agente.

**Tarefa 2: Investigue atividades de IA no Activity Explorer**

1.  Na subnavegação à esquerda, selecione **Users**.

2.  Selecione **Activity explorer**.

3.  Na página **Activity explorer**, selecione a aba **AI activities**.

4.  Na barra de filtros, selecione **User**  e digite Adele Vance.

5.  Selecione **Apply**  para filtrar os resultados e mostrar apenas as interações de Adele.

6.  Analise os eventos de interação listados na visualização filtrada.

7.  Selecione um evento de interação que faça referência a um arquivo confidencial, por exemplo, um que faça referência a **Zava_Employee_Records.xlsx** ou **Zava_Payroll_Q1_2025.xlsx.**

8.  No painel de detalhes do evento, revise os seguintes campos:

    - **Date and time**

    - **User**

    - **Activity type**

    - **AI app**

    - **File referenced**

    - **Sensitivity label on file**

    - **DLP rule matched** — se aplicável

9.  Verifique se a política DLP **Zava - Block HR Data in M365 Copilot** aparece como correspondente para alguma das interações de arquivos com o rótulo de HR.

10. Feche o painel de detalhes do evento.

11. Remova o filtro de usuário e aplique um filtro para **Sensitivity label** definido como **Zava-Confidential/HR-Data**.

12. Analise os resultados, eles mostram todas as interações de IA em todo o locatário que envolveram um arquivo com o rótulo HR-Data.

**Resumo**

Neste laboratório, você iniciou uma avaliação personalizada de risco de dados do DSPM nos sites do SharePoint de RH e finanças da Zava no início do terceiro dia, garantindo que os resultados estivessem disponíveis para análise posteriormente no laboratório. Você registrou um aplicativo Entra e configurou a conexão de verificação no nível do item exigida pelo DSPM. Na função de Adele Vance, você gerou três eventos realistas de interação do Microsoft 365 Copilot, referenciando arquivos rotulados como confidenciais, incluindo registros de funcionários, dados de folha de pagamento e projeções financeiras, criando os sinais de atividade de IA necessários para investigação ao longo do dia 3.

Você explorou o painel de postura do DSPM e analisou suas principais métricas, principais objetivos e sugestões do Security Copilot. Você ativou duas políticas com um clique: a política de gerenciamento de riscos internos do DSPM para uso arriscado de IA e a política de DLP de detecção de informações confidenciais para interações do Copilot. Você revisou os resultados da avaliação de risco de dados, identificou itens confidenciais compartilhados em excesso nos sites de RH e finanças da Zava e aplicou duas ações de remediação: restringir o acesso pela classificação de confidencialidade de dados de RH e habilitar a descoberta de conteúdo restrito do SharePoint no site de RH da Zava. Por fim, como Patti Fernandes, você investigou os eventos de interação da Adele Vance com o Copilot na aba de atividades de IA do DSPM Activity Explorer, analisando referências de arquivos, rótulos de confidencialidade e registros de correspondência de DLP, criando a base de evidências para a revisão de conformidade do dia 3.
