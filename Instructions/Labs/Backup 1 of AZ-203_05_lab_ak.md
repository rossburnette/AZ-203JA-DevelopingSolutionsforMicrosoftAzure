﻿---
lab:
    title: 'ラボ: Azure にデプロイされた監視サービス'
    type: 'Answer Key'
    module: 'モジュール 5: Azure ソリューションの監視、トラブルシューティング、最適化を行う'
---

# ラボ: Azure にデプロイされた監視サービス
# 受講者ラボ解答キー

## Microsoft Azure ユーザー インターフェイス

Microsoft クラウド ツールのダイナミックな性質を考えると、このトレーニング コンテンツの開発後に Azure ユーザー インターフェイス (UI) の変更が発生する可能性があります。これらの変更により、ラボの指示と手順が一致しない場合があります。

Microsoft は、コミュニティが必要な変更を行うとすぐに、このトレーニング コースを更新します。しかし、クラウド更新が頻繁に起きるため、この研修内容が更新される前に、UI の変更を経験するかも知れません。**その場合は変更に順応して、必要に応じてラボでをそれを処理してください。**

## 指示

### 開始する前に

#### ラボの仮想マシンへのサインイン

次の認証情報を使用して **Windows 10** 仮想マシンにサインインします。
    
-   **ユーザー名**：Admin

-   **パスワード**: Pa55w.rd

> **注記**：ラボ仮想マシンのサインイン手順は、インストラクターから提供されます。

#### インストールされたアプリケーションの検討

**Windows 10** デスクトップの下部にあるタスク バーを確認します。タスク バーには、このラボで使用するアプリケーションのアイコンが含まれています。
    
-   Microsoft Edge

-   エクスプローラ

-   Visual Studio Code

-   Windows PowerShell

#### 練習用ファイルをダウンロードする

1.  タスク バーで、**Windows PowerShell** アイコンを選択します。

1.  PowerShell コマンド プロンプトで、現在の作業ディレクトリを **Allfiles (F):\\** パスに変更します。

    ```
    cd F:
    ```

1.  コマンド プロンプト内で次のコマンドを入力し、Enter キーを押して、GitHub でホストされている **microsoftlearning/AZ-203-DevelopingSolutionsforMicrosoftAzure** プロジェクトを  **Allfiles (F):\\* ドライブにクローンします。

    ```
    git clone --depth 1 --no-checkout https://github.com/microsoftlearning/AZ-203-DevelopingSolutionsForMicrosoftAzure .
    ```

1.  コマンド プロンプト内で次のコマンドを入力し、**Enter** キーを押 して、**AZ-203T05** ラボを完了するために必要なラボ ファイルをチェックアウトします。

    ```
    git checkout master -- Allfiles/*
    ```

1.  現在実行中の **Windows PowerShell** コマンド プロンプト アプリケーションを閉じます。

### エクササイズ 1: Azure リソースの作成と構成

#### タスク 1: Azure portal を開く

1.  タスク バーで、**Microsoft Edge** アイコンを選択します。

1.  開いているブラウザ ウィンドウで、[**Azure potal**](https://portal.azure.com)(portal.azure.com)に移動します。

1.  サインイン ページで、Microsoft アカウントの **電子メール アドレス**を入力します。

1.  **次へ** を選択します。

1.  Microsoft アカウントの**パスワード**を入力します。

1.  **サインイン** を選択します。

> **注記**：**Azure potal**に初めてサインインする場合は、ダイアログ ボックスにポータルを見学するオファーが表示されます。ツアーをスキップしてポータルの使用を開始するには、**開始**を選択します。

#### タスク 2: アプリケーション インサイト リソースを作成します。

1.  ポータルの左側のナビゲーション ウィンドウで、**+ リソースを作成**を選択します。

1.  **新規**ブレードの上部にある **マーケットプレースを検索** フィールドを検索します。

1.  検索フィールドに**インサイト**と入力し、Enterを押します。

1.  **すべての** 検索結果ブレードで、**アプリケーションインサイト**の結果を選択します。

1.  **アプリケーションインサイト** ブレードで、**作成**を選択します。

1.  2 番目の [**Application Insights**] ブレードで、[**Basics**] などのブレードのタブを見つけます。

    > **注記**: 各タブは、ワークフロー内の新しい **Application Insights インスタンス**を作成するためのステップを表します。いつでも**レビュー + 作成**を選択して、残りのタブをスキップできます。

1.  **基本**タブで、次の操作を実行します：
    
    1.  **サブスクリプション**テキスト ボックスは既定値のままにします。
    
    1.  **リソース グループ** セクションで、**新規作成**を選択し、**MonitoredAssets**を選択し、**OK**を選択します。
    
    1.  **名前**テキストボックスに **instrm\[小文字で名前を指定\]** と入力します。
    
    1.  **場所** ドロップダウン リストで、 **(US) 米国東部**リージョンを選択します。
    
    1.  **レビュー + 作成**を選択します。

1.  **レビュー + 作成**タブで、前の手順で選択したオプションを確認します。

1.  指定したコンフィギュレーションを使用して Application Insights インスタンスを作成するには、**作成**を選択します。

1.  演習を進める前に、作成タスクが完了するまで待ちます。

1.  ポータルの左側のナビゲーション ウィンドウで、 **リソース グループ**を選択します。

1.  **リソース グループ** ブレードで、 この実習ラボで前に作成した **MonitoredAssets** リソース グループを選択します。

1. **MonitoredAssets** ブレードで、この実習ラボで前に作成した**instrm\**アプリケーション インサイト アカウントを選択します。

1. **アプリケーション インサイト** ブレードで、ブレードの左側の**構成**カテゴリ内で、**プロパティ**リンクを選択します。

1. **プロパティ** セクションで、**インストルメンテーション キー**フィールドの値を確認します。このキーは、クライアント アプリケーションによって Application Insights に接続するために使われます。

#### タスク 3: Web アプリ リソースの作成

1.  ポータルの左側のナビゲーション ウィンドウで、**+ リソースを作成**を選択します。

1.  **新規**ブレードの上部にある **マーケットプレースを検索** フィールドを検索します。

1.  検索フィールドで、**Web**を入力し、Enter キーを押します。

1.  **すべて**の検索結果]ブレードで、**Web アプリ**の結果を選択します。

1.  **Web アプリ** ブレードで、**作成**を選択します。

1.  2 番目の [**Web アプリ**] ブレードで、[**Basics**] などのブレードのタブを見つけます。

    > **注記**: 各タブは、ワークフロー内の新しい **Web アプリ**を作成するためのステップを表します。いつでも**レビュー + 作成**を選択して、残りのタブをスキップできます。

1.  **基本**タブで、次の操作を実行します：
    
    1.  **サブスクリプション**テキスト ボックスは既定値のままにします。
    
    1.  **リソース グループ** ドロップダウン リストで、**MonitoredAssets**を選択 します。
    
    1.  **名前**テキストボックスに **smpapi\[小文字で名前を指定\]** と入力します。

    1.  **発行**セクションで、**コード**を選択します。

    1.  **ランタイム スタック**ドロップダウン リストで、**NET Core 3.0 (現在)**を選択します。

    1.  **オペレーティング システム** セクションで、**Windows**を選択 します。

    1.  **リージョン** ドロップダウン リストで、**米国東部**のリージョンを選択します。

    1.  [**Windows プラン (米国東部)**] セクションで、[**新規作成**] を選択し、[**名前**] テキスト ボックスに [**MonitoredPlan**] の値を入力して、[**OK**] を選択します。

    1.  **SKU と サイズ** セクションを既定値のままにします。

    1.  **次へ** 選択します:** モニタリング**。

1.  **モニタリング**タブで、次の操作を実行します：

    1.  **Application Insights を有効化にする**セクションで**はい**を選択します。

    1.  **Application Insights** ドロップダウン リストで、 この実習ラボで前に作成した **instrm\** Application Insights アカウントを選択します。

    1.  **レビュー + 作成**を選択します。

1.  **レビュー + 作成**タブで、前の手順で選択したオプションを確認します。

1.  指定した構成を使用して Web アプリを作成するには、**作成**を選択します。

1.  演習を進める前に、作成タスクが完了するまで待ちます。

1. ポータルの左側のナビゲーション ウィンドウで、 **リソース グループ**を選択します。

1. **リソース グループ** ブレードで、 この実習ラボで前に作成した **MonitoredAssets** リソース グループを選択します。

1. **MonitoredAssets** ブレードで、 この実習ラボで前に作成した**smpapi\** Web アプリを選択します。

1. **App Services** ブレードで、**設定**カテゴリ内のブレードの左側 で、**構成**リンクを選択 します。

1.  **構成**セクションで、次の操作を実行します：
    
    1.  [**アプリケーション設定**] タブを選択します。

    1.  APIに関連付けられているシークレットを表示するには、**値を表示**を選択します。

    1.  **APPINSIGHTS\_インストルメンテーションキー**キーに対応する値を確認します。この値は、Web アプリ リソースの構築時に自動的に設定されました。

1. **App Services** ブレードで、**設定**カテゴリ内のブレードの左側 で、**プロパティ**リンクを選択 します。

1. **プロパティ** セクションで、**URL**フィールドの値を記録します。この値は、ラボの後半で使用され、API に対する要求を行います。

#### タスク 4: Web アプリの自動スケール オプションを構成する

1.  **App Services** ブレードで、ブレードの左側にある **設定**カテゴリ内で**スケール アウト (App Service プラン)** リンクを選択します。

1.  **スケール アウト**セクションで、次の操作を実行します。
    
    1.  **カスタム自動スケーリング**を選択する
    
    1.  **自動スケール設定名** フィールドに、**ComputeScaler**と入力 します。
    
    1.  **リソース グループ** の一覧で、**監視対象資産**を選択します。
    
    1.  **縮尺モード**セクションで、**メトリックに基づくスケール**を選択します。
    
    1.  **インスタンスの制限**セクション内の**最小**フィールドに、**2**と入力します。
    
    1.  **インスタンスの制限**セクション内の**最大**フィールドに、**8**と入力します。
    
    1.  **インスタンスの制限**セクション内の**既定**フィールドで、**3**と入力します。
    
    1.  **+ 規則の追加**を選択します。表示される**縮尺 ルール** ポップアップで、すべてのフィールドを既定値に設定したままにし、**追加**を選択します。
    
    1.  ページの上部にある**保存**をクリックします。

1.  演習を進める前に、保存が完了するまで待ちます。

#### 復習

この演習では、このラボの残りの部分で使用するすべてのリソースを作成しました。

### エクササイズ 2: .NET コア Web API アプリケーションのビルド & デプロイ

#### タスク 1: .NET Core Web API プロジェクトの構築

1.  タスク バーで、**Visual Studio コード**アイコンを選択します。

1.  **ファイル** メニューで、**フォルダを開く**を選択します。

1.  開かれるファイル エクスプローラ ペインで、**すべてのファイル (F):\\Allfiles\\Labs\\05\\Starter\\Api** に移動し、**フォルダの選択**を選択します。

1.  Visual Studio コード ウインドウで、コンテキスト メニューにアクセスするか、**エクスプローラ**ペインを右クリックし、**ターミナルで開く**を選択します。

1.  openコマンド プロンプトで次のコマンドを入力し、Enterキーを押して現在のディレクトリに**SimpleApi**という名前の新しいNET Core Web API アプリケーションを作成します。

    ```
    dotnet new webapi --output . --name SimpleApi
    ```

1.  コマンド プロンプトで、次のコマンドを入力し、Enter キーを押して、NuGet から現在のプロジェクトに **Microsoft.ApplicationInsights.AspNetCore** パッケージの**2.8.2**バージョンを追加します。

    ```
    dotnet add package Microsoft.ApplicationInsights.AspNetCore --version 2.8.2
    ```

1.  コマンド プロンプトで次のコマンドを入力し、Enter キーを押して NET Core Web アプリケーションをビルドします。

    ```
    dotnet build
    ```
    
#### タスク 2: HTTPS を無効にして Application Insights を使用するようにアプリケーション コードを更新する

1.  **Visual Studio コード**ウインドウの左側にある**エクスプローラ**ウインドウで、**Startup.cs** ファイルをダブルクリックしてエディタでファイルを開きます。

1.  エディタの **Startup** クラスで、**43**行の次のコード行を検索して削除します。

    ```
    app.UseHttpsRedirection();
    ```

    > **注記**: このコード行は、Web アプリにHTTPSを強制的に使用します。この演習では不要です。

1.  **Startup** クラス内で、**INSTRUMENTATION_KEY** と名付けられた新しい**静的文字列定数**を、このラボで前に作成した **Application Insights** リソースからコピーした**インストルメンテーション キー**に設定された新しい**静的文字列定数**を追加します。

    ```
    private const string INSTRUMENTATION_KEY = "{your_instrumentation_key}";
    ```

    > **注記**: たとえば、 **インストルメンテーション キー**が ``d2bb0eed-1342-4394-9b0c-8a56d21aaa43``の場合、コード行は ``private const string INSTRUMENTATION_KEY = "d2bb0eed-1342-4394-9b0c-8a56d21aaa43";`` になります。

1.  **スタートアップ** クラス内で **ConfigureServices** メソッドを見つけます。

    ```
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);        
    }
    ```

1.  **ConfigureServices** メソッドの最後に新しいコード行を追加し、提供されたインストルメンテーション キーを使用して Application Insights を構成します。

    ```    
    services.AddApplicationInsightsTelemetry(INSTRUMENTATION_KEY);
    ```

1.  **ConfigureServices** メソッドは次のようになります。

    ```
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);   
        services.AddApplicationInsightsTelemetry(INSTRUMENTATION_KEY);        
    }
    ```

1.  **Startup.cs** ファイルを**保存**します。

1.  画面の下部にあるコマンド プロンプトに注目してください。コマンド プロンプトで次のコマンドを入力し、Enter キーを押して.NET Core Webアプリケーションをビルドします。

    ```
    dotnet build
    ```

#### タスク 3: API アプリケーションをローカルでテスト

1.  画面の下部にあるコマンド プロンプトに注目してください。コマンド プロンプトで次のコマンドを入力し、Enter キーを押して.NET Core Webアプリケーションを実行します。

    ```
    dotnet run
    ```
1.  タスク バーで、**Microsoft Edge** アイコンを選択します。

1.  開いているブラウザ ウインドウで、ポート **5000** の **localhost**でホストされているテスト アプリケーションの**/api/values** 相対パスに移動します。
    
    > **注記**：完全なURLはhttp://localhost:5000/api/values です

1.  同じブラウザ ウインドウで、ポート **5000** の **localhost** でホストされているテスト アプリケーションの **/api/values/7** 相対パスに移動します。
    
    >  **注記**：完全なURLはhttp://localhost:5000/api/values/7 です。

1.  http://localhost:5000/api/values/7 アドレスを表示するブラウザウインドウを閉じます。

1.  現在実行中の **Visual Studio Code** アプリケーションを閉じます。

#### タスク 4: アプリケーション インサイトでのメトリックの表示

1.  **Azure potal**を表示する、現在開いているブラウザ ウインドウに戻ります。

1.  ポータルの左側で、**リソース グループ**を選択します。

1.  **リソース グループ** ブレードで、 この実習ラボで前に作成した **MonitoredAssets** リソース グループを見つけて選択します。

1.  **MonitoredAssets** ブレードで、この実習ラボで前に作成した**instrm\** アプリケーション インサイト アカウントを選択します。

1.  **アプリケーション インサイト**ブレードにおいて、ブレードの中央にあるタイルで、表示されるメトリックを確認します。具体的には、発生した**サーバー 要求**の数と平均**サーバー応答時間**を確認します。

    > **注記**: 要求が Application Insights のメトリック グラフに表示されるまでに最大 5 分かかる場合があります。

#### タスク 5: Web アプリにアプリケーションをデプロイする

1.  タスク バーで、**Visual Studio コード**アイコンを選択します。

1.  **ファイル** メニューで、**フォルダを開く**を選択します。

1.  開かれるファイル エクスプローラ ペインで、**すべてのファイル (F):\\Allfiles\\Labs\\Starter\\Api** に移動し、**フォルダの選択**を選択します。

1.  Visual Studio コード ウインドウで、コンテキスト メニューにアクセスするか、**エクスプローラ**ペインを右クリックし、**ターミナルで開く**を選択します。

1.  openコマンド プロンプトで次のコマンドを入力し、Enterキーを押して Azure CLI にサインインします。

    ```
    az login
    ```

1.  表示されるブラウザー ウィンドウで、次の操作を実行します：
    
    1.  Microsoft アカウントの**電子メール アドレス**を入力します。
    
    1.  **次へ** を選択します。
    
    1.  Microsoft アカウントの**パスワード**を入力します。
    
    1.  **サインイン** を選択します。

1.  現在開いている**コマンド プロンプト**アプリケーションに戻ります。サインイン プロセスが完了するのを待ちます。

1.  コマンド プロンプトで次のコマンドを入力し、Enterキーを押して、**MonitoredAssets** リソース グループ内のすべての**アプリ**を一覧表示します。

    ```
    az webapp list --resource-group MonitoredAssets
    ```

1.  次のコマンドを入力し、Enterキーを押すと、プレフィックス **smpapi\** を持つ**アプリ**が見つかります。

    ```
    az webapp list --resource-group MonitoredAssets --query "[?starts_with(name, 'smpapi')]"
    ```

1. 次のコマンドを入力し、Enter キーを押して、プレフィックス **smpapi\** を持つ単一のアプリの名前のみを印刷します。

    ```
    az webapp list --resource-group MonitoredAssets --query "[?starts_with(name, 'smpapi')].{Name:name}" --output tsv
    ```

1. 次のコマンドを入力し、Enter キーを押して現在のディレクトリを、デプロイ ファイルを含む**すべてのファイル(F):\\Allfiles\\Labs\\05\\Starter** ディレクトリに変更します。

    ```
    cd F:\Allfiles\Labs\05\Starter\
    ```

1. 次のコマンドを入力し、Enterキーを押して、この実習ラボで作成済みの **Web アプリ**に **api.zip** ファイルをデプロイします。

    ```
    az webapp deployment source config-zip --resource-group MonitoredAssets --src api.zip --name <name-of-your-api-app>
    ```

    > **注記**: **\<name-of-your-api-app>**を、この実習ラボで前に作成した Web アプリの名前に置き換えます。このアプリ名は、以前のステップで最近クエリしました。

1. この演習を進める前に、展開が完了するのを待ちます。

1. 現在実行中の **Visual Studio Code** アプリケーションを閉じます。

1. ポータルの左側のナビゲーション ウインドウで、**リソース グループ**を選択します。

1. **リソース グループ** ブレードで、 この実習ラボで前に作成した **MonitoredAssets** リソース グループを選択します。

1. **MonitoredAssets** ブレードで、 この実習ラボで前に作成した**smpapi\** Web アプリを選択します。

1. **App Services** ブレードで、ブレードの上部にある**参照**を選択します。

1. 新しいブラウザ ウインドウまたはタブが開き、**404 (見つかりません)**エラーが返されます。ブラウザのアドレス バーで、現在のURLの末尾にサフィックス**/api/値**を追加してURL を更新し、Enterキーを押します。

    > **注記**：たとえば、URL がhttp://smpapistudent.azurewebsites.net の場合、新しい URL はhttp://smpapistudent.azurewebsites.net/api/values となります。

1. API を使用した結果として返される JSON 配列を確認してください。

#### 復習

この演習では、ASP.NET Core を使用してAPIを作成し、アプリケーション メトリックをアプリケーション インサイトにストリーミングするように構成しました。次に、アプリケーション インサイト ダッシュボードを使用して、Web アプリとアプリで実行されている API に関するパフォーマンスの詳細を表示しました。

### エクササイズ 3: .NET Core を使ったクライアント アプリケーションの構築

#### タスク 1: .NET Core コンソール プロジェクトの構築

1.  タスク バーで、**Visual Studio コード**アイコンを選択します。

1.  **ファイル** メニューで、**フォルダを開く**を選択します。

1.  開くファイル エクスプローラ ウインドウで、**すべてのファイル (F):\Allfiles\\Starter\\Console**に移動し、**フォルダの選択**を選択します。

1.  Visual Studio コード ウインドウで、コンテキスト メニューにアクセスするか、**エクスプローラ**ペインを右クリックし、**ターミナルで開く**を選択します。

1.  open コマンド プロンプトで、次のコマンドを入力し、Enter キーを押して、現在の ディレクトリに **SimpleConsole** という名前の新しい .NET Core コンソール アプリケーションを作成します。

    ```
    dotnet new console --output . --name SimpleConsole
    ```

1.  コマンド プロンプトで次のコマンドを入力し、Enterキーを押してNuGetから **Polly パッケージの**7.1.0**バージョンを**現在のプロジェクトに追加します。

    ```
    dotnet add package Polly --version 7.1.0
    ```

1.  コマンド プロンプトで次のコマンドを入力し、Enter キーを押して NET Core Web アプリケーションをビルドします。

    ```
    dotnet build
    ```

#### タスク 2: HTTPクライアント コードの追加

1.  **Visual Studio コード**ウインドウの左側にある**エクスプローラ**ペインで、**Program.cs** ファイルをダブルクリックしてエディタでファイルを開きます。

1.  エディタ で、**System.Net.Http** 名前空間の次の**using** ディレクティブを追加 します。

    ```
    using System.Net.Http;
    ```

1.  エディタで、**System.Threading.Tasks** 名前空間の次の **using** ディレクティブを追加します。

    ```
    using System.Threading.Tasks;
    ```

1.  **SimpleConsole** の名前空間で、次のクラスを **7** 行目で見つけます。

    ```
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
    ```

1.  **プログラム**クラス全体を次の実装に置き換えます。

    ```
    class Program
    {
        private const string _api = "";
        private static HttpClient _client = new HttpClient(){ BaseAddress = new Uri(_api) };
    
        static void Main(string[] args)
        {
            Run().Wait();
        }
    
        static async Task Run()
        {
    
        }
    }
    ```

1.  **9** 行目で **\_api** 定数を見つけます:

    ```
    private const string _api = "";
    ```

1.  この演習で前に記録した Web アプリの**URL**に変数の値を設定して、**\_api**定数を更新します。

    > **注記**: たとえば、URL がhttp://smpapistudent.azurewebsites.net の場合、新しいコード行は次のようになります: プライベート const 文字列 \_api =  "http://smpapistudent.azurewebsites.net"。

1.  **Run** メソッド内で、次のコード行を追加して、**/api/values/** の相対パスに対して文字列を渡す **HttpClient.GetStringAsync** メソッドを非同期的に呼び出します。

    ```
    string response = await _client.GetStringAsync("/api/values/");
    ```

1.  **Run** メソッド内で、**GET** 要求からの応答をコンソールに書き出すコード行を追加します。

    ```
    Console.WriteLine(response);
    ```

1. **Program.cs** ファイルには、次のコードが必要です。

    ```
    using System;
    using System.Net.Http;
    using System.Threading.Tasks;

    namespace SimpleConsole
    {
        class Program
        {
            private const string _api = "http://<your-api-name>.azurewebsites.net/";
            private static HttpClient _client = new HttpClient(){ BaseAddress = new Uri(_api) };
        
            static void Main(string[] args)
            {
                Run().Wait();
            }
        
            static async Task Run()
            {
                string response = await _client.GetStringAsync("/api/values/");
                Console.WriteLine(response);    
            }
        }
    }
    ```

1. **Program.cs** ファイルを**保存**します。

#### タスク 3: コンソール アプリケーションをローカルでテスト

1.  画面の下部にあるコマンド プロンプトで次のコマンドを入力し、Enterキーを押して .NET Core Web アプリケーションを実行します。

    ```
    dotnet run
    ```

1.  アプリケーションが Azure で Web アプリを正常に呼び出し、この実習で前に説明したのと同じJSONアレイを返すことに注意してください。結果は、次のJSONコンテンツと同様に表示されます。

    ```
    ["value1","value2"]
    ```

1.  **Azure potal**を表示する、現在開いているブラウザ ウインドウに戻ります。

1.  ポータルの左側で、**リソース グループ**を選択します。

1.  **リソース グループ** ブレードで、 この実習ラボで前に作成した **MonitoredAssets** リソース グループを見つけて選択します。

1.  **MonitoredAssets** ブレードで、 この実習ラボで前に作成した**smpapi\** Web アプリを選択します。

1.  **App Services**ブレードで、ブレードの上の**停止**を選択して Web アプリの実施を停止します。

1.  **Web アプリの停止**確認ダイアログ ボックスで**はい**を選択します。

1.  タスク バーで、**Visual Studio コード**アイコンを選択します。

1. **ファイル** メニューで、**フォルダを開く**を選択します。

1. 開かれるファイル エクスプローラ ペインで、**すべてのファイル (F):\\Allfiles\\Labs\\05\\Starter\\Console** に移動し、**フォルダの選択**を選択します。

1. Visual Studio コード ウインドウで、コンテキスト メニューにアクセスするか、**エクスプローラ**ペインを右クリックし、**ターミナルで開く**を選択します。

1. open コマンド プロンプトで次のコマンドを入力し、Enterキーを押して.NET Core Webアプリケーションを実行します。

    ```
    dotnet run
    ```

1. アプリケーションの実行が失敗し、次の例外メッセージに似た **HttpRequestException** メッセージが表示されます。

    ```
    System.Net.Http.HttpRequestException: Response status code does not indicate

    success: 403 (Site Disabled).

    at System.Net.Http.HttpResponseMessage.EnsureSuccessStatusCode()

    at System.Net.Http.HttpClient.GetStringAsyncCore(Task\`1 getTask)

    at SimpleConsole.Program.Run() in F:\Allfiles\Labs\05\Starter\Console\Program.cs:line 20
    ```

    > **注記**: この例外は、Web アプリが使用できなくなったために発生します。

#### タスク 4: Pollyを使用して再試行ロジックを追加する

1.  **Visual Studio コード**ウインドウの左側にある**エクスプローラ**ウインドウで、**PollyHandler.cs** ファイルをダブル クリックしてエディタでファイルを開きます。

1.  **PollyHandler** クラス内で、**13～24**行目を確認します。これらのコード行は、**NET Foundation** の **Polly** ライブラリを使用して、失敗した HTTP 要求を5分ごとに再試行する再試行ポリシーを作成します。

1.  **Visual Studio コード**ウインドウの左側にある**エクスプローラ**ペインで、**Program.cs** ファイルをダブルクリックしてエディタでファイルを開きます。

1.  **10**行目で **\_client** 定数を見つける:

    ```
    private static HttpClient _client = new HttpClient(){ BaseAddress = new Uri(_api) }; 
    ```

1.  **PollyHandler** クラスの新しいインスタンスを使用するように **HttpClient** コンストラクタを更新して **\_client** 定数を更新します。

    ```
    private static HttpClient _client = new HttpClient(new PollyHandler()){ BaseAddress = new Uri(_api) };
    ```

1.  **Program.cs** ファイルを**保存**します。

#### タスク 5: 再試行ロジックを検証する

1.  画面の下部にあるコマンド プロンプトで次のコマンドを入力し、Enterキーを押して .NET Core Web アプリケーションを実行します。

    ```
    dotnet run
    ```

1.  HTTP要求の実行は引き続き失敗し、5秒ごとに再試行されます。アプリケーションが失敗している間は、コンソールで次のメッセージが表示されます。

    ```
    失敗しました
    ```

1.  コンソールアプリケーションを実行したままにします。成功するまで、Web アプリに無限にアクセスしようとします。

1.  **Azure potal**を表示する、現在開いているブラウザ ウインドウに戻ります。

1.  ポータルの左側で、**リソース グループ**を選択します。

1.  **リソース グループ** ブレードで、 この実習ラボで前に作成した **MonitoredAssets** リソース グループを見つけて選択します。

1.  **MonitoredAssets** ブレードで、 この実習ラボで前に作成した**smpapi\** Web アプリを選択します。

1.  **App Services**ブレードで、ブレードの上の**開始**を選択して Web アプリの実施を再開します。

1.  **Web アプリの停止**確認ダイアログ ボックスで**はい**を選択します。

1. 現在実行中の **Visual Studio Code** アプリケーションに戻ります。

1. アプリケーションが最終的に Azure で Web アプリを正常に呼び出し、この実習で前に説明したのと同じ JSON アレイを返すことに注意してください。結果は、次のJSONコンテンツのようになります。

    ```
    ["value1","value2"]
    ```

1. 現在実行中の **Visual Studio Code** アプリケーションを閉じます。

#### 復習

この演習では、条件付き再試行ロジックを使用してAPIにアクセスするコンソール アプリケーションを作成しました。Web アプリが利用可能かどうかにかかわらず、アプリケーションは引き続き動作します。

### エクササイズ 4: テスト Web アプリの読み込み

#### タスク 1: Web アプリでパフォーマンス テストを実行する

1.  **Azure potal**を表示する、現在開いているブラウザ ウインドウに戻ります。

1.  ポータルの左側で、**リソース グループ**を選択します。

1.  **リソース グループ** ブレードで、 この実習ラボで前に作成した **MonitoredAssets** リソース グループを見つけて選択します。

1.  **MonitoredAssets** ブレードで、 この実習ラボで前に作成した**smpapi\** Web アプリを選択します。

1.  **App Services** ブレードで、**開発ツール**カテゴリ内のブレードの左側で、**パフォーマンス テスト**を選択します。

1.  **パフォーマンス テスト** セクションの上部にある**新規**を選択します。

1.  **新しいパフォーマンス テスト**ブレードで、次の操作を実行します。
    
    1.  **名前**フィールドに**LoadTest**と入力します。
    
    1.  **読み込み元の生成** リストで、**米国東部 (Web アプリの場所)**を選択します。
    
    1.  **経費の合計** フィールドで、**1000**と入力します。
    
    1.  **期間** フィールドに、**10**と入力します。
    
    1.  **使用してテストを構成する**を選択します。

1.  **使用したテストを構成する**ブレードで、次の操作を実行します。
    
    1.  **テストの種類**の一覧で、**手動テスト**を選択します。
    
    1.  **URL**フィールドで、サフィックス**/api/値**を現在のURLの末尾に追加してURLを更新します。

        > **注意**: たとえば、URL がhttp://smpapistudent.azurewebsites.net の場合、新しい URL はhttp://smpapistudent.azurewebsites.net/api/values となります。

    1.  **完了**を選択します。

1.  **新しいパフォーマンス テスト**ブレードに戻り、**テストの実行**を選択します。

1.  **パフォーマンス テスト**セクションに戻り、**最近の実行**の一覧で、**LoadTest** を選択します。

1. **LoadTest** ブレードで、テストが開始されるのを待ってから、演習を続行します。

    > **注記**：ほとんどのロード テストでは、リソースの収集と開始に約10～15分かかります。ロード テストが開始されると自動的にリフレッシュするので、このブレードで待てます。

1. ロード テストが完了するのを待ってから、ラボを続行します。Web アプリの使用率が増加するにつれて、ライブ チャートが更新します。

    > **注記**: ロード テストには、演習の前の手順で指定した10分かかります。

#### タスク 2: パフォーマンス テスト後にAzure モニタのメトリックを使用する

1.  Azureポータルの左のナビゲーション ペインで、**すべてのサービス**をクリックします。

1.  **すべてのサービス** ブレードで、**モニタ** を選択します。

1.  **モニタ**ブレードの左側で、**メトリック**を選択します。

1.  **メトリック**セクションで、次の操作を実行します。
    
    1.  **リソース** セクションで、**リソースを選択**を選択します。
    
    1.  **リソース グループ**リストで表示される**リソースの選択** ウインドウで **MonitoredAssets** を選択します。次に、**リソース**リストで、**instrm\**アプリケーション インサイト アカウント オプションを選択します。最後に、**適用**を選択してウインドウを閉じ、選択内容を確認します。
    
    1.  **標準**カテゴリの**メトリック名前空間** リストで、**標準メトリック (プレビュー)**を選択します。
    
    1.  **メトリック** リストで、**パフォーマンス カウンタ**カテゴリで**CPUの処理**を選択します。
    
    1.  **集合**リストで**平均** を選択します。
    
    1.  セクションの上部で、**過去24時間 (自動 - 5分)**を選択します。表示されるウインドウで**時間範囲**カテゴリで**過去30分**を 選択し、**適用**を選択して選択範囲を保存します。
    
    1.  セクションの上部で、**ライン チャート**を選択します。表示されるメニューで、**面グラフ**を選択します。

1.  新規作成されたグラフを確認してください。

1.  セクションの上部で、**メトリックを追加**を選択します。

1.  **メトリック**セクションで、リスト内の新しいメトリック項目を使用して次のアクションを実行します。
    
    1.  **標準**カテゴリの**メトリック名前空間** リストで、**ログベースのメトリック**を選択します。
    
    1.  **サーバー**カテゴリの**メトリック**ボックスで、**サーバー応答時間**を選択します。
    
    1. **集合**リストで**平均** を選択します。

1.  更新されたグラフを確認してください。グラフに表示される情報を確認してください。アプリケーションの負荷が増大するにつれて、サーバーの応答時間と CPU 時間とどのように相関するかを確認してください。

#### 復習

この演習では、Azure で使用できるツールを使用して、Web アプリのパフォーマンス（読み込み）テストを実行しました。ロード テストを実行した後、Azure Monitorインターフェイスでメトリックを使用してAPI アプリの動作を測定できます。

### エクササイズ 5: サブスクリプションのクリーンアップ 

#### タスク 1: Cloud Shell を開く

1.  Azure potalの上部で、**Cloud Shell** アイコンを選択して新しいシェル インスタンスを開きます。

    > **注記**: **Cloud Shell** アイコンは、シンボルとアンダースコア文字より大きい文字で表されます。

1.  サブスクリプションを使用して初めて **Cloud Shell** を開く場合は、初めて使用する **Cloud Shell** を構成できる**Azure Cloud シェルへようこそウィザード**が表示されます。ウィザードで次の操作を実行します。
    
    1.  ダイアログボックスを表示すると、シェルを使用して開始する新しいストレージ アカウントを作成するよう求めるメッセージが表示されます。既定の設定を受け入れ、**ストレージの作成**を選択します。
    
    1.  **Cloud Shell** が初回のセットアップ手順を完了するのを待ってから、ラボを進めます。

    > **注記**: **Cloud Shell** の構成オプションが表示されない場合は、このコースの演習で既存のサブスクリプションを使用している可能性が高いと考えられます。演習は、新しいサブスクリプションを使用しているという推定から記述されます。

1.  **Cloud Shell** コマンド プロンプトのポータルの下部にある次のコマンドを入力し、Enter キーを押してサブスクリプション内のすべてのリソース グループを一覧表示します。

    ```
    az group list
    ```

1.  次のコマンドを入力し、Enterキーを押して、リソース グループを削除する可能性のあるコマンドの一覧を表示します。

    ```
    az group delete --help
    ```

#### タスク 2: リソース グループを削除する

1.  次のコマンドを入力し、Enter キーを押して **MonitoredAssets** リソース グループを削除します。

    ```
    az group delete --name MonitoredAssets --no-wait --yes
    ```
    
1.  ポータルの下部にある **Cloud Shell** ペインを閉じます。

#### タスク 3: アクティブなアプリケーションを閉じる

1.  現在実行中の **Microsoft Edge** アプリケーションを閉じます。

1.  現在実行中の **Visual Studio Code** アプリケーションを閉じます。

#### 復習

この実習では、この演習で使用する **リソース グループ** を削除してサブスクリプションをクリーンアップしました。