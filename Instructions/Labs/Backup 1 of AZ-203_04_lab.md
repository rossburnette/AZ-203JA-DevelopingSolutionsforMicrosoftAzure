﻿---
lab:
    title: 'ラボ: サービス間でリソース シークレットに安全にアクセスする'
     module: 'モジュール 4: Azure Security の実装'
---

# ラボ: サービス間でリソース シークレットに安全にアクセスする
# 受講者ラボ マニュアル

## ラボ シナリオ

あなたの組織は、データ共有企業間 (B2B) 契約を地元の企業と締結しており、毎晩削除されたファイルを解析する必要があります。この手順を簡易化するために、地元の企業は毎晩 Microsoft Azure Storage BLOB としてファイルを削除することにしました。ここで、ファイルに安全にアクセスし、ファイルをインターネットに公開せずに BLOB にアクセスするために任意の内部システムで使用できるセキュアな URL を生成する方法を考案する必要があります。Microsoft Azure Key Vault を使用して、ストレージ アカウントと Azure Functions の認証情報を格納し、認証情報をプレーン テキストで格納したり、ファイルをインターネットに公開したりすることなく、ファイルに安全にアクセスするために必要なコードを記述することにしました。

## 目的

このモジュールを修了すると、次のことが可能になります：

-   Azure Key Vault を作成し、シークレットを Key Vault に格納します。

-   Azure App Service インスタンス用のサーバー割り当てマネージド ID を作成します。

-   Azure Active Directory ID またはアプリケーション用の Azure Key Vault アクセス ポリシーを作成します。

-   Azure Storage .NET ソフトウェア開発キット (SDK) を使用して、BLOB を安全にダウンロードします。

## ラボのセットアップ

-   **予想時間**：45 分

## 指示

### 開始する前に

#### ラボの仮想マシンへのサインイン

次の認証情報を使用して、**Windows 10** 仮想マシンにサインインしていることを確認します。
    
-   **ユーザー名**：Admin

-   **パスワード**: Pa55w.rd

#### インストールされたアプリケーションの検討

**Windows 10** デスクトップの下部にあるタスク バーを確認します。タスク バーには、このラボで使用するアプリケーションのアイコンが含まれています。

-   Microsoft Edge

-   エクスプローラ

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

1.  コマンド プロンプト内で次のコマンドを入力し、**Enter** キーを押 して、**AZ-203T04** ラボを完了するために必要なラボ ファイルをチェックアウトします。

    ```
    git checkout master -- Allfiles/*
    ```

1.  現在実行中の **Windows PowerShell** コマンド プロンプト アプリケーションを閉じます。

### エクササイズ 1: Azureのリソースを作成

#### タスク 1: Azure portal を開く

1.  **Azure potal** (<https://portal.azure.com>)にサインインします。

1.  Azure potal に初めてサインインする場合は、ポータルのツアーを提供するダイアログ ボックスが表示されます。**スタート**をクリックしてツアーをスキップします。

#### タスク 2: Azure Storage  アカウントを作成する

1.  次の詳細で新規**ストレージ アカウント**を作成します:
    
      - **新しいリソースグループ**: SecureFunction
    
      - **名前**: securestor\[あなたの名前を小文字で入力\]
    
      - **場所**：米国東部
    
      - **パフォーマンス**: Standard
    
      - **アカウントの種類**: StorageV2 (汎用 v2)
    
      - **レプリケーション**: ローカル冗長ストレージ (LRS)
    
      - **アクセス階層**：ホット

> **注記**：Azure が Storage アカウントの作成を完了するのを待ってから、ラボを進みます。アカウントの作成時に通知が届きます。

1.  新しく作成した**ストレージ アカウント インスタンス**の**アクセス キー**ブレードを開 きます。

1.  **接続文字列**フィールドに値を記録します。これらの値は、この演習の後半で使用します。

#### タスク 3: Azure Key Vault を作成します。

1. 次の詳細で新規**キー ボルト**を作成します:

    - **既存のリソース グループ**: SecureFunction

    - **名前**: securevault\[小文字で名前\]

    - **リージョン**: 米国東部

    - **価格層：**Standard

    > **注記**: Azure が Key Vault の作成を完了するのを待ってから、ラボに進みます。ボルトの作成時に通知が届きます。

#### タスク 4: Azure Functions アプリの作成

1. 次の詳細で新規**関数アプリ**を作成します:

    - **既存のリソース グループ**: SecureFunction

    - **アプリ名**: securefunc\[小文字で名前はこちら\]

    - **公開**: コード

    - **ランタイム スタック**: .NET Core

    - **リージョン**: 米国東部

    - **ストレージ アカウント**: securestor\[小文字で名前はこちら\]

    - オペレーティング システムWindows

    - **計画**: 消費

    - **Application Insights を有効にする**: いいえ

        > **注意**: Azure が関数アプリの作成を完了するのを待ってから、ラボを進みます。アプリの作成時に通知が届きます。

#### 復習

この演習では、このラボで使用するすべてのリソースを作成しました。

### エクササイズ 2: シークレットと ID の構成 

#### タスク 1: システムに割り当てられた管理サービス ID の構成

1.  この演習で作成済みの**securefunc\** 関数アプリにアクセスします。

1.  **プラットフォームの機能**タブにある**ID**設定に移動します。

1.  **システムに割り当てられた**管理 ID を有効にし、変更内容を保存します。

#### タスク 2: Key Vaultを作成します

1.  この実習ラボで前に作成した**secureVault\**キー ボルトにアクセスします。

1.  **設定**セクションにある**シークレット** リンクに 移動します。

1.  次の設定で新しい**シークレット**を作成してください:
    
    - **名前**: storagecredentials

    - **値**: \<Storage Connection String\>

    - **有効**: はい

        > **注意**: このシークレットの**値**については、この演習で前に記録したストレージ アカウント**接続文字列**を使用 します。

1.  シークレットをクリックすると、最新バージョンのメタデータが表示されます。

1.  シークレット**識別子**フィールドの値を記録します。

#### タスク 3: キー ボルト アクセス ポリシーの構成

1.  この実習ラボで前に作成した **secureVault\** キー ボルトにアクセスします。

1.  **設定**セクションにある**アクセス ポリシー**リンクに 移動します。

1.  次の設定で新しい**アクセス ポリシー**を作成します:
    
    - **プリンシパル**: securefunc\[小文字で名前を指定\]

    - **キーパーミッション**: なし

    - **シークレットパーミッション**: GET

    - **証明書のアクセス許可**: なし

    - **認証済みアプリケーション**: なし

1.  変更内容を**アクセス ポリシー**の一覧に**保存**します。

#### 復習

この演習では、関数アプリにサーバー割り当てが行われた管理サービス ID を作成し、Key Vault でシークレットの値を取得するための適切なアクセス許可をその ID に付与しました。最後に、関数アプリ内で使用するシークレットを作成しました。

### エクササイズ 3: 関数アプリケーション コードの作成 

#### タスク 1: キー ボルト派生アプリケーション設定を作成する 

1.  この演習で作成済みの **securefunc\** 関数アプリにアクセスします。

1.  **プラットフォームの機能**タブにある**構成**設定に移動します。

1.  次の詳細を使用して、新しい**アプリケーション設定**を作成します。
    
    - **名前**: StorageConnectionString

    - **値**: @Microsoft.KeyVault(SecretUri=\<Secret Identifier\>)
    
    - **デプロイ スロットの設定**: 非選択

        > **注意**: 上記の構文を使用して、**シークレット識別子**への参照を構築する必要があります。たとえば、シークレット識別子 **がhttps://securevaultstudent.vault.azure.net/secrets/storagecredentials/17b41386df3e4191b92f089f5efb4cbf**の場合、値は **@Microsoft.KeyVault(SecretUri= https://securevaultstudent.vault.azure.net/secrets/storagecredentials/17b41386df3e4191b92f089f5efb4cbf)**になります。

1.  変更内容を**アプリケーション設定**に保存します。

#### タスク 2: HTTP トリガ関数の作成

1.  この演習で作成済みの**securefunc\**関数アプリにアクセスします。

1.  次の設定を使用して、新しい**関数**を作成します。
    
    - **開発環境**: ポータル内

    - **template**: HTTP トリガー

    - **名前**: FileParser

    - **権限レベル**: 匿名

1.  関数エディタで、関数スクリプトの例を次のプレースホルダ C\# コードに置き換えます。

    ```
    using System.Net;
    using Microsoft.AspNetCore.Mvc;
    
    public static async Task<IActionResult> Run(HttpRequest req)
    {
        return new OkObjectResult("Test Successful"); 
    }
    ```

1.  **保存して実行**をクリックして、関数のテスト実行を実行します。実行からの出力は**テスト成功**である必要があります。

#### タスク 3: Key Vault 派生アプリケーション設定をテストする

1.  **実行**メソッド内の既存のコードをすべて削除します。

1.  **Environment.GetEnvironmentVariable** メソッドを使用して、**StorageConnectionString** アプリケーション設定の値を取得します。

    ```
    string connectionString = Environment.GetEnvironmentVariable("StorageConnectionString");
    ```

1.  **OkObjectResult** クラス コンストラクタを使用して、**connectionString** 変数の値を返します。

    ```
    return new OkObjectResult(connectionString);
    ```

1.  **保存して実行**をクリックして、関数のテスト実行を実行します。実行からの出力は、**Azure Key Vault** に格納されている**ストレージ アカウント**接続文字列である必要があります。

#### 復習

この演習では、サービス ID を安全に使用して、**Azure Key Vault** に格納されているシークレットの値を読み取り、**Azure Function**の結果としてその値を返 します。

### エクササイズ 4: ストレージ アカウントの BLOB にアクセス

#### タスク 1: サンプル ストレージ BLOB のアップロード

1.  この実習ラボで作成済みの**securestor\** ストレージ アカウントにアクセスします。

1.  **Blob service** セクションにある**コンテナ**リンクに移動します。

1.  次の設定で新しい**コンテナ**を作成してください:
    
      - **名前**: drop
    
      - **パブリックアクセスレベル**: BLOB (BLOB の場合のみ匿名読み取りアクセス)

1.  新しい**ドロップ** コンテナーに移動します。

1.  [**アップロード**] をクリックして、にある **records.json** ファイルをアップロードします。ラボ マシン上の **Allfiles (F): \\Allfiles\\Labs\\04\\Starter** フォルダ。

    > **注:** **ファイルが既に存在する場合は、上書きを**有効にすることをお勧めします。

1.  bloB の一覧にあるBLOBエントリをクリックして、**records.json** BLOB のメタデータを表示します。

1.  新しいブラウザ タブを使用して、BLOB の **URL**に移動 し、BLOB の内容を表示します。

1.  **パブリック アクセス レベル**を**プライベート(匿名アクセスなし)**に変更して、コンテナの**アクセスレベル**を更新します。

1.  新しいブラウザ ウィンドウまたはタブを使用して、BLOB の**URL**に移動 し、BLOB の内容を表示します。リソースが見つからなかったことを示すエラー メッセージが表示されます。

    > **注記**: エラー メッセージが表示されない場合は、ブラウザーがファイルをキャッシュしている可能性があります。エラー メッセージが表示されるまで、**Ctrl + F5** を使用してページを更新します。

#### タスク 2: NuGet からストレージ アカウント SDK を取得する

1.  この演習で作成済みの **securefunc\** 関数アプリにアクセスします。

1.  **FileParser**関数のエディタを開きます。

1.  [**ファイルの表示**] タブを使用して、次の内容を含む新しい **function.proj** ファイルを作成します。

    ```
    <Project Sdk="Microsoft.NET.Sdk">
        <PropertyGroup>
            <TargetFramework>netstandard2.0</TargetFramework>
        </PropertyGroup>
        <ItemGroup>
            <PackageReference Include="Azure.Storage.Blobs" Version="12.0.0" />
        </ItemGroup>
    </Project>
    ```

1.  新しく作成した **function.proj** ファイルを**保存**します。  

    > **注記**: この **.proj** ファイルには、[Azure.Storage.Blobs](https://www.nuget.org/packages/Azure.Storage.Blobs/12.0.0) パッケージのインポートに必要な NuGet パッケージ参照が含まれています。

1.  **ファイルの表示**タブでファイルをクリックして、**function.proj**ファイルの内容を表示します。

1.  **ファイルの表示**タブで** run.csx** ファイルをクリックして、**FileParser** 関数のエディタに戻ります。

1.  **Azure.Storage**、**Azure.Storage.Blobs** および **Azure.Storage.Blobs.Models** の名前空間の 2 つの **using** ディレクティブを追加します。

1.  **実行**メソッド内の既存のコードをすべて削除します。

#### タスク 3: ストレージ アカウント コードの作成

1.  **Environment.GetEnvironmentVariable** メソッドを使用して、**StorageConnectionString** アプリケーション設定の値を取得します。

    ```
    string connectionString = Environment.GetEnvironmentVariable("StorageConnectionString");
    ```

1.  *connectionString* 変数をコンストラクターに渡すことによって、**BlobServiceClient** クラスの新しいインスタンスを作成します。

    ```
    BlobServiceClient serviceClient = new BlobServiceClient(connectionString);
    ```

1.  **ドロップ**コンテナ名を渡しながら**BlobServiceClient.GetBlobContainerClient** メソッドを使用して、この実習ラボで前に作成したコンテナを参照する**BlobContainerClient** クラスの新しいインスタンスを作成します。

    ```
    BlobContainerClient containerClient = serviceClient.GetBlobContainerClient("drop");
    ```

1.  このラボで前にアップロードした BLOB を参照する **BlobClient** クラスの新しいインスタンスを作成するには、**rrecords.json** Blob 名を渡しながら **BlobContainerClient.GetBlobClient** メソッドを使用します。

    ```
    BlobClient blobClient = containerClient.GetBlobClient("records.json");
    ```

#### タスク 4: BLOB のダウンロード

1.  **BlobClient.DownloadAsync** メソッドを使用して、参照される BLOB の内容を非同期にダウンロードし、その結果を*response*という名前の変数に格納します。

    ```
    var response = await blobClient.DownloadAsync();
    ```

1.  **FileStreamResult** クラス コンストラクタを使用して、*コンテンツ*変数に格納された様々なコンテンツ値を返します。

    ```
    return new FileStreamResult(response?.Value?.Content, response?.Value?.ContentType);
    ```

1.  **保存して実行**をクリックして、関数のテスト実行を実行します。実行からの出力は、 ストレージ アカウントに格納されている **$/drop/records.json** BLOB の内容である必要があります。

#### 確認

この演習では、C\# コードを使用してストレージ アカウントに安全にアクセスし、BLOB の内容をダウンロードしました。

### エクササイズ 5: サブスクリプションのクリーンアップ 

#### タスク 1: Azure Cloud Shellを開き、リソース グループを一覧表示する

1.  ポータルの上部にある **Cloud Shell** アイコンをクリックして、新しいシェル インスタンスを開きます。

1.  **クラウドシェル**がまだ構成されていない場合は、既定の設定を使用して Bash のシェルを構成します。

1.  ポータルの下部にある **Cloud Shell** コマンド プロンプトで次のコマンドを入力し、Enter キーを押してサブスクリプション内のすべてのリソース グループを一覧表示します。

    ```
    az group list
    ```

1.  次のコマンドを入力し、Enterキーを押して、リソース グループを削除する可能性のあるコマンドの一覧を表示します。

    ```
    az group delete --help
    ```

#### タスク 2: リソース グループの削除

1.  次のコマンドを入力し、Enter キーを押して **SecureFunction** リソース グループを削除します。

    ```
    az group delete --name SecureFunction --no-wait --yes
    ```
    
1.  ポータルの下部にある **Cloud Shell** ペインを閉じます。

#### タスク 3: アクティブなアプリケーションを閉じる

> 現在実行中の **Microsoft Edge** アプリケーションを閉じます。

#### 復習

この実習では、この演習で使用する **リソース グループ** を削除してサブスクリプションをクリーンアップしました。
