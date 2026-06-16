# Lab 00: 環境セットアップ — Zava Corporation AI Agent インフラストラクチャ

## 概要

**Zava Corporation** は、英国および EU 全域で事業を展開する中規模の金融サービスおよびHRコンサルティング企業です。Zava  Corporation は、従業員の機密記録、クライアントの財務データ、および第三者ベンダー契約を管理しています。同組織は、運用効率を向上させるため、HR、財務、及び IT サポート機能全体に AI エージェントを最近導入しました。

セキュリティ構成を開始する前に、Zava Corporation 環境は完全にプロビジョニングされる必要があります。このラボでは、Microsoft Entra ID テナントを構成し、Microsoft Copilot Studio を有効にし、エージェント作成に必要なセキュリティグループを登録し、コース全体を通じてガバナンスの対象となる3つの AI エージェントを作成し、各エージェントを指定された SharePoint ナレッジソースに接続し、Zava のライブデータ環境をシミュレートするサンプルビジネス文書をアップロードします。

後続のすべてのラボは、ここで作成されるエージェント、アイデンティティ、およびファイルに依存しています。Lab 01 に進む前に、3つの演習すべてを順番に完了してください。

---

   > **注記：** 実際の環境では、このような責務は複数のペルソナ（開発者、IT 管理者、セキュリティ管理者、コンプライアンス担当者）に分散され、各々は最小権限の原則とゼロトラストの原則に沿ったスコープ権限で運用されます。

---

## 目的

- Microsoft Entra ID にロール割り当て可能なセキュリティグループを作成し、Privileged Role Administrator ロールを割り当てる。
- Power Platform Admin Center で **copilotagentsecurity** グループを認可された Copilot Studio Authors グループとして有効にする。
- 環境レベルで Copilot Studio の Entra Agent Identity を有効にする。
- Power Apps メーカーポータルで SharePoint をデータソースとして接続する。
- 3つの Copilot Studio エージェントを作成する：Zava HR Assistant、Zava Finance Agent、および Zava IT Support Agent。
- 各エージェントを指定された SharePoint ナレッジソースに接続する。
- 各エージェントを公開し、適切なラボユーザーと共有する。
- Zava サンプルビジネス文書を HR および Finance SharePoint サイトにアップロードする。
- 3つのエージェント全てが Microsoft Agent 365 Agent Registry でアクティブとして表示されることを確認する

---

## ラボ所要時間

推定所要時間：**30分**

---
## 演習 0: Zava HR SharePoint サイトを作成する

1. 新しいブラウザタブを開き、以下のサイトに移動します `https://admin.microsoft.com`. プロンプトが表示された場合は、**ODL_User** の認証情報でサインインしてください。

	- **メールアドレス/ユーザー名:** <inject key="AzureAdUserEmail"></inject>
	- **パスワード:** <inject key="AzureAdUserPassword"></inject>

2. 左側のナビゲーションペインで **[すべてを表示]** をクリックし、**[管理センター]** の下から **[SharePoint]** を選択します。

	![](./media/L00-E0-S2.png)

1. SharePoint 管理センターで、左側のナビゲーションペインから **[サイト]** を展開し、**[アクティブなサイト]** を選択します。その後、**[+ 作成]** をクリックします。

	![](./media/L00-E0-S4.png)

5. **[サイトの作成]** パネルで、**[チームサイト]** を選択します。

	![](./media/L00-E0-S5.png)

6. **[テンプレートの選択]** ページで、**[標準チーム]** を選択します。

	![](./media/l0n1.png)

7. **['標準チーム' テンプレートをプレビューして使用]** ページで、**[テンプレートを使用]** を選択します。

	![](./media/l0n2.png)

6. **[チームサイト]** 構成ページで、以下を入力して **[次へ]** をクリックします。

   - **サイト名:** **HR<inject key="Deployment ID" enableCopy="false"></inject>**
   - **サイトアドレス:** URL パスが **/sites/HR<inject key="Deployment ID" enableCopy="false"></inject>** であることを確認します
   - **グループ所有者:** ODL_User <inject key="Deployment ID" enableCopy="false"></inject>

		![](./media/L00-E0-S7.png)

1. 以下の詳細情報を追加して、**[サイトを作成]** をクリックします。

   - **プライバシー設定:** **[プライベート - メンバーのみがこのサイトにアクセスできます]** を選択します。
   - **言語を選択:** 日本語。
   - **タイムゾーンを選択:** (UTC+9:00) 大阪、札幌、東京

		![](./media/L00-E0-S8.png)

1. **[サイト所有者とメンバーを追加]** ページで、**[完了]** をクリックします。

	![](./media/l0n3.png)

8. サイトのプロビジョニングが完了するまで待機します。**[アクティブなサイト]** リストに URL **https://[TenantPrefix].sharepoint.com/sites/HR<inject key="Deployment ID" enableCopy="false"></inject>** で表示されていることを確認します。

   > **注記:** このサイトは Lab 00 演習2で作成される Zava HR Assistant エージェントの SharePoint ナレッジソースです。Copilot Studio のエージェント接続は、特に **/sites/HR<inject key="Deployment ID" enableCopy="false"></inject>** を参照します。異なる URLスラッグを使用しないでください。

9. ステップ3からステップ9までの同じ手順に従い、以下のサイトを作成します。

   - **サイト名:** **Operations<inject key="Deployment ID" enableCopy="false"></inject>**
   - **サイトアドレス:** URL パスが **/sites/Operations<inject key="Deployment ID" enableCopy="false"></inject>** であることを確認します
   - **グループ所有者:** ODL_User <inject key="Deployment ID" enableCopy="false"></inject>
   - **プライバシー設定:** **[プライベート]** を選択します。
   - **言語を選択:** 日本語。
   - **タイムゾーン:** (UTC+9:00) 大阪、札幌、東京

---

## 演習 1: Entra ID を構成し、Copilot Studio Authors を有効にする

### タスク 1: サインインして多要素認証を構成する

1. ブラウザを開き、`https://entra.microsoft.com` に移動します。

2. サインインページで、プロンプトが表示された場合は、ラボ環境の **[環境]** タブから **ODL User** 認証情報を入力します。
	- **メールアドレス/ユーザー名:** <inject key="AzureAdUserEmail"></inject>

	  ![](./media/L00-E1-T1-S2.png)

	- **一時アクセスパス:** <inject key="AzureAdUserPassword"></inject>

	  ![](./media/tap.png)

1. **[サインインしたままにしますか?]** と表示された場合は、**[はい]** を選択します。

	![](./media/l0n4.png)

1. Microsoft Entra 管理センターのウェルカムスクリーンで、**[はじめに]** を選択します。

---

### タスク 2: copilotagentsecurity セキュリティグループを作成する

1. Microsoft Entra 管理センターの左側のナビゲーションペインで、**[Entra ID]** を展開し、**[グループ]** を選択します。

	![](./media/L00-E1-T2-S1.png)

2. **[概要]** ページで、**[新しいグループ]** を選択します。

	![](./media/L00-E1-T2-S2.png)

3. **[新しいグループ]** ページで、以下のフィールドを構成します。

   - **グループタイプ:** **[セキュリティ]** を選択します。
   - **グループ名:** `copilotagentsecurity` を入力します。
   - **Microsoft Entra ロールをこのグループに割り当てることができる:** **[はい]** を選択します。このオプションが表示されない場合は、このフィールドをスキップして続行します。

		![](./media/L00-E1-T2-S3.png)

4. **[所有者]** で、**[所有者が選択されていません]** を選択します。

	![](./media/L00-E1-T2-S4.png)

5. **[所有者を追加]** パネルで、**ODL_USER <inject key="Deployment ID" enableCopy="false"></inject>** を検索して選択します。**[選択]** を選択して所有者を確認します。

	![](./media/L00-E1-T2-S5.png)

6. **[メンバー]** で、**[メンバーが選択されていません]** を選択します。

	![](./media/L00-E1-T2-S6.png)

7. **[メンバーを追加]** パネルで、**ODL User <inject key="Deployment ID" enableCopy="false"></inject>** と **Patti Fernandes** を検索して選択します。**[選択]** を選択してメンバーを確認します。

	![](./media/L00-E1-T2-S7.png)

8. **[ロール]** で、**[ロールが選択されていません]** を選択します。

	![](./media/L00-E1-T2-S8.png)

9. **[ロールを選択]** パネルで、`Global admin` を検索し、**[グローバル管理者]** を選択して、**[選択]** をクリックします。

	![](./media/L00-E1-T2-S9.png)

10. **[作成]** をクリックして新しいグループを作成します。

	![](./media/L00-E1-T2-S10.png)

11. 確認ダイアログで、**[はい]** を選択します。

	![](./media/L00-E1-T2-S11.png)

12. ページの上部に成功通知が表示されることを確認します。

	![](./media/L00-E1-T2-S13.png)

---

### タスク 3: Azure リソースのアクセス管理を有効にする

1. Microsoft Entra 管理センターの左側のナビゲーションペインで、**[Entra ID]** を展開し、**[概要]** を選択します。

	![](./media/L00-E1-T3-S1.png)

2. **[概要]** ページで、上部のバーから **[プロパティ]** を選択します。

	![](./media/L00-E1-T3-S2.png)

3. **[プロパティ]** ページで、**[Azureリソースのアクセス管理]** トグルを見つけて、**[はい]** に設定します。

	![](./media/L00-E1-T3-S3.png)

5. **[セキュリティ既定値を管理]** を選択します。

	![](./media/L00-E1-T3-S4.png)

6. **[セキュリティ既定値]** パネルで、**[セキュリティ既定値]** の下で、**[有効]** を選択（まだ有効化されていない場合）してから **[保存]** をクリックします。

	![](./media/L00-E1-T3-S5.png)

7. **[プロパティ]** ページに戻り、**[保存]** を選択します。

	![](./media/L00-E1-T3-S6.png)

---

### タスク 4: Privileged Role Administrator ロールを割り当てる

1. Microsoft Entra 管理センターの左側のナビゲーションペインで、**[Entra ID]** を展開し、**[ロールと管理者]** を選択します。

	![](./media/L00-E1-T4-S1.png)

2. **[ロールと管理者]** ページの検索バーで、`Privileged Role Administrato` を検索し、その名前を選択して **[Privileged Role Administrator]** を選択します。

	![](./media/L00-E1-T4-S2.png)

5. **[Privileged Role Administrator]** ページで、**[+ 割り当てを追加]** を選択します。

	![](./media/pp1.png)

7. **[メンバーを選択]** パネルで、**copilotagentsecurity** を検索して選択します。**[追加]** を選択して確認します。

	![](./media/L00-E1-T4-S6.png)

11. ロール割り当てが割り当てリストに表示されることを確認します。

	![](./media/pp2.png)

---

### タスク 5: Power Platform Admin Center でCopilot Studio Authors を構成する

1. 新しいブラウザタブを開き、`https://admin.powerplatform.microsoft.com` に移動します。

2. 左側のナビゲーションペインで、**[管理(1)] > [環境(2)]** を選択します。**[+新規(3)]** をクリックします。

	![](./media/pp10.png)

1. [新しい環境] ポップアップで、名前を **DevOne-<inject key="Deployment ID" enableCopy="false"></inject>** として指定します。

	![](./media/pp4.png)

1. 下にスクロールし、[既定の設定を変更] ドロップダウンを展開して、**[Dataverse ストアを追加しますか?(1)]** を有効にして、**[次へ(2)]** をクリックします。

	![](./media/pp5.png)

1. **[Dataverse を追加]** ページで、セキュリティグループの下の **[+選択]** をクリックします。

	![](./media/pp6.png)

1. 結果から **[copilotagentsecurity(1)]** グループを選択します。その後、**[完了(2)]** を選択します。

	![](./media/pp7.png)

1. **[保存]** を選択して設定を適用します。

	![](./media/pp8.png)

3. **[管理]** で、**[テナント設定]** を選択します。**[テナント設定]** ページで、リストから **[Copilot Studio Authors]** を見つけて選択します。

	![](./media/L00-E1-T5-S2.png)

4. **[Copilot Studio Authors]** パネルで、セキュリティグループの近くにある **[編集]** アイコンを選択します。

	![](./media/L00-E1-T5-S4.png)

5. 検索フィールドに `copilotagentsecurity` を入力します。結果から **[copilotagentsecurity]** グループを選択します。その後、**[完了]** をクリックします。

	![](./media/L00-E1-T5-S5.png)

6. **[保存]** を選択して設定を適用します。

	![](./media/L00-E1-T5-S6.png)

---

### タスク 6: Copilot Studio 用の Entra Agent Identity を有効にする

1. 左側のナビゲーションペインで、**[Copilot]** を選択します。

	![](./media/L00-E1-T6-S1.png)

2. **[Copilot]** ページで、**[設定]** を選択します。

	![](./media/L00-E1-T6-S2.png)

3. 設定リストで、**[Copilot Studio]** セクションの下から **[Copilot Studio 用のEntra Agent Identity]** を選択します。

	![](./media/L00-E1-T6-S3.png)

4. **[Copilot Studio 用のEntra Agent Identity]** パネルで、環境リストから **[DevOne-<inject key="Deployment ID" enableCopy="false"></inject>]** 環境を選択して、**[設定を編集]** を選択します。

	![](./media/L00-E1-T6-S4.png)

5. Copilot Studio 用の Entra Agent Identity の設定パネルで、**[オン]** を選択（まだの場合）して、**[保存]** をクリックします。

	![](./media/L00-E1-T6-S6.png)

1. 保存後、パネルを閉じます。

	![](./media/L00-E1-T6-S7.png)

      >**注記:** Entra Agent Identity を有効にすると、Copilot Studio エージェントに Microsoft Entra ID で一意のアイデンティティが自動的に割り当てられます。これは後続のラボでのアイデンティティガバナンス、条件付きアクセス、および Defender for Cloud Apps 統合に必要です。

---

### タスク 7: Power Apps メーカーポータルで SharePoint 接続を追加する

1. 新しいブラウザタブを開き、`https://make.powerapps.com` に移動してサインインします。プロンプトが表示された場合は **ODL_User** の認証情報を使用します。

	- **メールアドレス/ユーザー名:** <inject key="AzureAdUserEmail"></inject>
	- **パスワード:** <inject key="AzureAdUserPassword"></inject>

2. **[Power Appsへようこそ]** スクリーンが表示されたら、**[はじめに]** をクリックします。

	![](./media/image45.png)

3. 右上隅で、環境スイッチャーで **[DevOne-<inject key="Deployment ID" enableCopy="false"></inject>]** 環境が選択されていることを確認します。そうでない場合は、環境スイッチャーを選択して **[DevOne-<inject key="Deployment ID" enableCopy="false"></inject>]** を選択します。

	![](./media/pp11.png)

4. 左側のナビゲーションバーで、**[その他(1)]** を展開し、**[接続(2)]** を選択します。

	![](./media/pp12.png)

5. **[接続]** ページで、**[+ 新しい接続]** を選択します。

	![](./media/pp13.png)

6. コネクタ検索バーに `SharePoint` と入力し、利用可能なコネクタのリストから **[SharePoint]** を選択します。

	![](./media/pp14.png)

7. **[SharePoint]** 接続パネルで、**[直接接続(クラウドサービス)]** を選択します。**[作成]** を選択します。

8. プロンプトが表示されたら、**ODL_User** の認証情報を使用してサインインし、接続を認可します。
   
	![](./media/L00-E1-T7-S7.png)

9. **[確認が必要]** ポップアップで、**[このリクエストを確認しました。このソースは信頼できます(1)]** のチェックボックスをオンにして、**[アクセスを許可(2)]** を選択します。

	![](./media/pp3.png)

11. SharePoint 接続が **[接続]** リストに **[接続済み]** ステータスで表示されていることを確認します。

	![](./media/pp15.png)

---

## 演習 2: Zava Copilot Studio エージェントを作成する

この演習では、Microsoft Copilot Studio で3つの Zava エージェントをすべて作成します。各エージェントは、名前、説明、指示、およびSharePoint ナレッジソースで構成されます。公開後、各エージェントは指定されたラボユーザーアカウントと共有されます。これらのエージェントはLab 01 から 07 全体を通じてライブガバナンスターゲットとして機能します。

---

### タスク 1: Zava HR Assistantを作成する

1. 新しいブラウザタブを開き、`https://copilotstudio.microsoft.com` に移動します。プロンプトが表示された場合は **ODL_User** の認証情報でサインインします。

	- **メールアドレス/ユーザー名:** <inject key="AzureAdUserEmail"></inject>
	- **パスワード:** <inject key="AzureAdUserPassword"></inject>

1. Copilot Studio が読み込まれない場合は、以下の手順に従います。

	- `https://admin.powerplatform.microsoft.com/` を開きます。**[管理] > [環境] > [dev-one-<inject key="Deployment ID" enableCopy="false"></inject>]** を選択し、**[環境ID]** の値をコピーします。

		![](./media/pp20.png)
   
   - Copilot Studio タブに戻り、`https://copilotstudio.microsoft.com/environments/<EnvironmentID>` を開きます。（`<EnvironmentID>` を上記でコピーした値に置き換えます）

2. **[ウェルカム]** スクリーンで、**[はじめに]** をクリックします。

	 ![](./media/pp21.png)

4. 左側のナビゲーションペインで、**[エージェント]** を選択します。**[エージェントを作成]** ページで、**[空白のエージェントを作成]** を選択します。

	 ![](./media/pp22.png)

7. **[名前]** フィールドに `Zava HR Assistant` と入力し、**[作成]** をクリックします。

	 ![](./media/pp23.png)

1. **[編集]** をクリックします。

	 ![](./media/pp24.png)

8. **[説明]** フィールドに `Zava従業員がHRポリシー、給付情報、および従業員手順を見つけるのに役立つAIアシスタント。` と入力し、**[保存]** を選択します。

	 ![](./media/pp25.png)

10. **[指示]** フィールドまでスクロールダウンし、**[編集]** をクリックして、以下を入力してから **[保存]** を選択します。

    ```
    Zava HR Assistant です。Zava HR SharePoint ナレッジベースで利用可能な情報のみを使用して質問に答えてください。推測したり、ナレッジベース外の情報を提供したりしないでください。常にプロフェッショナルに対応してください。
    ```

12. エージェント構成ページで、**[ナレッジ]** セクションを見つけます。**[+ ナレッジを追加]** を選択します。

    ![](./media/kn.png)   

13. **[ナレッジを追加]** パネルで、**[SharePoint]** を選択します。

	![](./media/L00-E2-T1-S12.png)

14. **[SharePoint URL]** フィールドに、次の形式で SharePoint HR サイトの URL を入力して、**[追加]** を選択します。
    **https://[TenantPrefix].sharepoint.com/sites/HR<inject key="Deployment ID" enableCopy="false"></inject>**

    > **注記:** `[TenantPrefix]` をラボ環境の **[環境]** タブから見つかるテナントプレフィックスで置き換えるか、演習0から取得したURLを使用します。

       ![](./media/pp27.png)

16. **[エージェントに追加]** を選択して SharePoint サイトをナレッジソースとして接続します。

    ![](./media/l0e2t1s12.png)  

17. エージェント構成ページの右上隅で、**[公開]** を選択します。

    ![](./media/l0e2t1s13.png)

18. 確認ダイアログで、**[公開]** を選択して確認します。

	![](./media/image65.png)

19. エージェント構成ページで、上部セクションにある **[チャネル]** タブを見つけます。（直接表示されていない場合は **[+]** を選択）

	   ![](./media/pp30.png)

20. **[Microsoft 365 Copilot および Microsoft Teams]** を選択してチャネルとして追加します。

	   ![](./media/pp31.png)

21. その後、**[チャネルを追加]** を選択します。

	![](./media/L00-E2-T1-S19.png)

22. **[可用性オプション]** を選択します。

	![](./media/L00-E2-T1-S20.png)

23. **[Microsoft 365 Copilot および Microsoft Teams]** ページで、**[組織内の全員に表示]** を選択します。

	![](./media/L00-E2-T1-S21.png)

24. **[組織カタログに送信]** を選択します。

	![](./media/L00-E2-T1-S22.png)

25. **[このエージェントへのアクセスをすべてのユーザーに付与しますか?]** 確認ダイアログで、**[はい]** を選択します。

	![](./media/L00-E2-T1-S23.png)

26. **[Teams アプリストアで組織向けに表示]** にリダイレクトされ、通知が表示されます。**[閉じる]** をクリックします。

	![](./media/L00-E2-T1-S24.png)

---

### タスク 2: Zava Finance Agent を作成する

1. 左側のナビゲーションペインで、**[エージェント]** を選択します。その後、**[空白のエージェントを作成]** を選択します。

	![](./media/pp22.png)

3. **[名前]** フィールドに `Zava Finance Agent` と入力し、**[作成]** をクリックします。

	![](./media/pp50.png)

4. **[説明]** フィールドに ` Zava 財務チームメンバーが予算情報、請求書データ、および財務レポートを取得するのに役立つ AI アシスタント。` と入力します。その後、**[保存]** を選択します。

5. **[指示]** フィールドで、**[編集]** を選択します。

6. 以下を入力して、**[保存]** を選択します。

    ```
    Zava Finance Agent です。Zava Finance SharePoint ナレッジベースの情報のみを使用して質問に答えてください。Finance SharePoint サイトへのアクセス権を付与されていないユーザーと財務データを共有しないでください。常にプロフェッショナルに対応し、ナレッジベース外のデータリクエストにはフラグを立ててください。
    ```

	![](./media/pp51.png)

7. 下にスクロールし、エージェント構成ページで **[ナレッジ]** セクションを見つけます。**[+ ナレッジを追加]** を選択します。

    ![](./media/kn.png) 

8. **[ナレッジを追加]** パネルで、**[SharePoint]** を選択します。

	![](./media/image82.png)

9. **[SharePoint URL]** フィールドに、次の形式で SharePoint Finance サイトのURLを入力します。
    **https://[TenantPrefix].sharepoint.com/sites/Operations<inject key="Deployment ID" enableCopy="false"></inject>**

    > **注記:** `[TenantPrefix]` を **[環境]** タブから取得したテナントプレフィックスに置き換えます。

10. **[追加]** を選択して SharePoint サイトをナレッジソースとして接続します。

	![](./media/pp52.png)

11. その後、**[エージェントに追加]** を選択します。

    ![](./media/l0e2t2s10.png)

12. エージェント構成ページで、上部セクションにある **[チャネル]** タブを見つけます。（直接表示されていない場合は **[+]** を選択）

	![](./media/pp53.png)

13. **[Microsoft 365 Copilot および Microsoft Teams]** を選択してチャネルとして追加します。

	![](./media/pp54.png)

14. その後、**[チャネルを追加]** を選択します。

	![](./media/L00-E2-T2-S14.png)

15. **[公開の準備はできていますか?]** ダイアログで、**[公開]** を選択します。タブを閉じます。

	![](./media/image89.png)

---

### タスク 3: Zava IT Support Agent を作成する

1. 左側のナビゲーションペインで、**[エージェント]** を選択します。その後、**[空白のエージェントを作成]** を選択します。

	![](./media/pp22.png)

3. **[名前]** フィールドに `Zava IT Support Agent` と入力し、**[作成]** をクリックします。

	![](./media/pp55.png)

4. **[説明]** フィールドに `Zava 従業員が一般的な IT 問題を解決し、サポートリクエストを送信し、IT ポリシードキュメントを見つけるのに役立つ AI アシスタント。` と入力します。その後、**[保存]** を選択します。

5. **[指示]** フィールドで、**[編集]** を選択します。

6. 以下を入力して、**[保存]** を選択します。

    ```
    Zava IT Support Agent です。公開されている Microsoft サポートドキュメントと Zava IT ポリシーを使用して、一般的な IT 質問をユーザーに支援してください。機密の財務または HR 情報にアクセスしたり共有したりしないでください。複雑な問題については IT ヘルプデスクにエスカレーションしてください。
    ```

	 ![](./media/pp56.png)

7. エージェント構成ページで、**[ナレッジ]** セクションを見つけます。**[+ ナレッジを追加]** を選択します。

    ![](./media/kn.png) 

8. **[ナレッジを追加]** パネルで、**[パブリックウェブサイト]** を選択します。

	![](./media/image103.png)

9. **[URL]** フィールドに `https://support.microsoft.com/` と入力し、**[追加]** を選択してサイトをナレッジソースとして接続します。

	![](./media/image104.png)

10. その後、**[エージェントに追加]** を選択します。

	![](./media/image105.png)

11. エージェント構成ページの右上隅で、**[公開]** を選択します。

	![](./media/L00-E2-T3-S11.png)

12. 確認ダイアログで、**[公開]** を選択して確認します。

	![](./media/image89.png)

13. エージェント構成ページで、上部セクションにある **[チャネル]** タブを見つけます。（直接表示されていない場合は **[+]** を選択）

	![](./media/pp58.png)

15. **[Microsoft 365 Copilot および Microsoft Teams]** を選択してチャネルとして追加します。

	![](./media/L00-E2-T3-S14.png)

16. その後、**[チャネルを追加]** を選択します。

	![](./media/L00-E2-T3-S15.png)

17. **[可用性オプション]** を選択します。

	![](./media/L00-E2-T3-S16.png)

18. **[Microsoft 365 Copilot および Microsoft Teams]** ページで、**[組織内の全員に表示]** を選択します。

	![](./media/L00-E2-T3-S17.png)

19. **[組織カタログに送信]** を選択します。

	![](./media/L00-E2-T3-S18.png)

20. **[このエージェントへのアクセスをすべてのユーザーに付与しますか?]** 確認ダイアログで、**[はい]** を選択します。

	![](./media/image114.png)

21. **[Teams アプリストアで組織向けに表示]** にリダイレクトされ、通知が表示されます。タブを閉じます。

---

## 演習 3: Zava ナレッジファイルを SharePoint にアップロードする

この演習では、Zava サンプルビジネス文書を SharePoint HR および Finance サイトにアップロードします。これらのファイルには、Lab 04、05、および 07 全体を通じてセキュリティ検出と DLP ポリシーマッチをトリガーする機密データ（従業員の個人識別情報、給与記録、クレジットカード番号、財務予測を含む）が含まれます。

---

### タスク 1: Zava HR SharePoint サイトにファイルをアップロードする

1. 新しいブラウザタブを開き、**https://[TenantPrefix].sharepoint.com/sites/HR<inject key="Deployment ID" enableCopy="false"></inject>** に移動します。

   > **注記:** `[TenantPrefix]` を **[環境]** タブから取得したテナントプレフィックスに置き換えます。

3. 左側のナビゲーションメニューから、**[ドキュメント(1)]** をクリックし、**[作成またはアップロード(2)]** を選択します。その後、**[ファイルのアップロード(3)]** を選択します。

	![](./media/pp59.png)

4. ファイルピッカーで、ラボVMデスクトップの **C:\LabFiles\lab file\HR** フォルダに移動します。

5. 以下のファイルを選択してから、**[開く]** を選択してアップロードします。

   | ファイル名 | 内容 |
   |---|---|
   | `Zava_HR_Policy_2024.docx` | 休暇および懲戒方針 — PII なし |
   | `Zava_Employee_Records.xlsx` | 従業員ID(形式: ZVA123456)、名前、生年月日、給与 |
   | `Zava_Payroll_Q1_2025.xlsx` | 経費列にクレジットカード番号を含む給与データ |
   | `Zava_Onboarding_Guide.docx` | 標準的なオンボーディングコンテンツ |
   | `Zava_Benefits_Summary.pdf` | 保険と年金の詳細 |
   | `Zava_Org_Chart.docx` | 報告ラインと管理構造 |
   | `Zava_Termination_Checklist.docx` | 離職従業員プロセス（名前と日付付き）|
   | `Zava_Sick_Leave_Report.xlsx` | 従業員名と病気の理由 |

6. 8つのファイルすべてのアップロードが完了するまで待機します。

7. **[ドキュメント]** ページで、8つのファイルすべてがドキュメントライブラリに表示されることを確認します。

	![](./media/l0e3t1s6.png)

---

### タスク 2: Zava Finance SharePoint サイトにファイルをアップロードする

1. 新しいブラウザタブを開き、**https://[TenantPrefix].sharepoint.com/sites/Operations<inject key="Deployment ID" enableCopy="false"></inject>** に移動します。

   > **注記:** `[TenantPrefix]` を **[環境]** タブから取得したテナントプレフィックスに置き換えます。

2. 左側のナビゲーションメニューから、**[ドキュメント(1)]** をクリックし、**[作成またはアップロード(2)]** を選択します。その後、**[ファイルのアップロード(3)]** を選択します。
   
	![](./media/pp60.png)

4. ファイルピッカーで、ラボVMデスクトップの **C:\LabFiles\lab file\Operations** フォルダに移動します。

5. 以下のファイルを選択してから、**[開く]** を選択してアップロードします。

   | ファイル名 | 内容 |
   |---|---|
   | `Zava_Budget_2025.xlsx` | 部門予算とコストセンター |
   | `Zava_Invoice_Log.xlsx` | IBANと口座番号が記載されたベンダー請求書 |
   | `Zava_Expense_Report_Alex.xlsx` | Visa クレジットカード番号を含む Alex Wilber の経費 |
   | `Zava_Audit_Report_2024.docx` | 内部監査結果 — Confidential (機密)とマーク |
   | `Zava_Contracts_External.docx` | 第三者ベンダー契約 — 外部で共有 |
   | `Zava_Financial_Projections.xlsx` | 広範な SharePoint 権限を持つ収益予測 |

6. 6つのファイルすべてのアップロードが完了するまで待機します。

7. **[ドキュメント]** ページで、6つのファイルすべてがドキュメントライブラリに表示されることを確認します。

    ![](./media/l0e3t2s6.png)

---

### タスク 3: Microsoft Agent 365 Agent Registry でエージェントを確認する

1. 新しいブラウザタブを開き、`https://admin.cloud.microsoft/` に移動します。プロンプトが表示された場合は **ODL_User** の認証情報でサインインします。

	- **メールアドレス/ユーザー名:** <inject key="AzureAdUserEmail"></inject>

	- **パスワード:** <inject key="AzureAdUserPassword"></inject>

2. 左側のナビゲーションペインで、**[エージェント]** を展開してから **[すべてのエージェント]** を選択します。

    ![](./media/l0e3t3s1.png)

3. このページで、次の3つのエージェントがリストに表示されていることを確認します。検索ボックスで `Zava` を検索して結果をフィルタリングできます。

   | エージェント名 | ステータス |
   |---|---|
   | Zava HR Assistant | 利用可能 | 
   | Zava Finance Agent | 利用可能 | 
   | Zava IT Support Agent | 利用可能 |

	  ![](./media/pp61.png)

	  >**注記:** Copilot Studio での公開後、エージェントが Agent Registry に表示されるまで最大10分かかる場合があります。エージェントが表示されない場合は、10分待ってページを更新してください。
	  
---

## 演習 4: 組織的なセットアップを有効にする

1. 以下のURLを使用して **Exchange Admin Center** に移動します

    ```
    https://admin.cloud.microsoft/exchange
	```
1. プロンプトが表示された場合は **ODL_User** の認証情報でサインインします。

	- **メールアドレス/ユーザー名:** <inject key="AzureAdUserEmail"></inject>
	- **パスワード:** <inject key="AzureAdUserPassword"></inject>

1. Exchange 管理センターで、ページの右上隅から **[クラウドシェルアイコン]** を選択して Azure Cloud Shell セッションを起動します

	![](./media/ex-1.png)

	>**注記**: 要求された場合は、続行する前に Cloud Shell 初期化を完了してください。

1. Cloud Shell セッションが準備完了になり、PowerShell プロンプトが表示されたら、次のコマンドを実行して現在の Exchange Online セッションを切断します

    ```
    Disconnect-ExchangeOnline -Confirm:$false	
	```

	![](./media/ex-2.png)

1. デバイス認証を使用して Exchange Online に接続するには、次のコマンドを実行して新しい Exchange Online 接続を開始します。

    ```
    Connect-ExchangeOnline -Device
    ```

	![](./media/ex-3.png)

    - 注記: デバイスコードとサインイン URL が表示されます。URLを開いてコードを貼り付け、認証を完了します

	    ![](./media/ex-4.png)

		![](./media/ex-7.png)

1. Exchange Online PowerShell セッションが正常に接続された後、次のコマンドを実行して組織のカスタマイズを有効にします。

    ```
    Enable-OrganizationCustomization
	```

	![](./media/ex-8.png)

     >**注記**: このコマンドは、高度な構成タスク用に Exchange Online 組織を準備します。組織のカスタマイズが既に有効化されている場合、コマンドはそれ以上のアクションが必要ないことを示すメッセージを返します。ラボの次のステップに進んでください。これは有効になるまで最大24時間かかる場合があります。
	

---

## まとめ

このラボでは、Zava Corporation AI セキュリティコースの完全な環境ベースラインを完了しました。Microsoft Entra 管理センターでロール割り当て可能なセキュリティグループを作成し、所有者およびメンバーとして ODL User を構成し、Privileged Role Administrator ロールを割り当て、Power Platform Admin CenterでCopilot Studio Authors 承認グループとしてグループを有効にしました。環境レベルで Copilot Studio 用の Entra Agent Identity を有効にし、Power Apps メーカーポータルで SharePoint 接続を追加し、3つの Copilot Studio エージェント（Zava HR Assistant、Zava Finance Agent、Zava IT Support Agent）を作成しました。各エージェントは指定されたナレッジソースに接続され、Teams および Microsoft 365 チャネル全体で公開されました。Zava HR および Finance SharePoint サイト全体に14のサンプルビジネス文書（リアルな機密データを含む）をアップロードし、3つのエージェントすべてが Microsoft Agent 365 Agent Registry に登録され、アクティブであることを確認しました。環境はLab 01 から 07 でのセキュリティ構成に向けて完全に準備完了です。
