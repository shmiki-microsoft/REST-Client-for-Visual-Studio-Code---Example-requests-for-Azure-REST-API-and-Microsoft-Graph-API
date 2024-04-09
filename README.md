# 概要
VS Code の拡張機能 Rest Client を使って、 Azure REST API や Microsoft Graph API を使用するために必要な事前準備と
各 API の具体的なリクエストの例を公開するレポジトリです。

* /Azure REST API : Azure REST API のサンプルが入っています。
* /Microsoft Graph API : Microsoft Graph API のサンプルが入っています。
* /Microsoft Identity Platform : Microsoft Entra ID の OAuth/OIDC を勉強したい方に向けたサンプルです。 Microsoft Entra ID で OAuth / OIDC の各種認証フロー簡単に試すためのサンプルが入っています。

# インストールと事前準備
## VSCode にて 拡張機能 REST Client for Visual Studio Code をインストール
Visual Studio Code のマーケットプレイスから 拡張機能 REST Client をインストールします。
https://marketplace.visualstudio.com/items?itemName=humao.rest-client

## Microsoft Entra ID にアプリ登録を行います。
https://portal.azure.com にアクセスし、 [Microsoft Entra ID] > [アプリの登録] にて新規にアプリケーション登録を行います。
具体的な設定内容は下記のキャプチャを参考にしてください。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/36009611-99db-4311-8f85-a62736bf909b)

作成が完了したら、[概要] でアプリケーション (クライアント) ID と ディレクトリ (テナント) ID をメモしておきます。

[証明書とシークレット] にて [新しいクライアントシークレット] を作成し、
生成されたクライアントシークレットの値の列ををメモしておきます。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/c2f56ff0-381c-4de8-af94-f1806c8e3a44)

#### [Microsoft Graph API 向けの設定]
Microsoft Graph API 向けに使用する場合は、以下の設定も行います。 Azure REST API や Microsoft Identity Platform 向けに使用する場合はこの手順をスキップできます。
[API のアクセス許可] にて Microsoft Graph API に必要なアクセス許可を **"アプリケーションの許可"** として追加し [{ドメイン名} に管理者の同意を与えます] をクリックする
必要なアクセス許可は Microsoft Graph API の各 API のアクセス許可のセクションで確認できます。
https://learn.microsoft.com/ja-jp/graph/api/user-list?view=graph-rest-1.0&tabs=http#permissions
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/aeef9f8e-9f38-461e-a51f-b860060ccede)

Microsoft Graph PowerShell のコマンドから確認したい場合は
Find-MgGraphCommand  コマンドが便利です。使い方は公開情報を参照ください。
https://learn.microsoft.com/en-us/powershell/microsoftgraph/find-mg-graph-command?view=graph-powershell-1.0

#### [Azure REST API 向けの設定]
Microsoft Graph API 向けに使用する場合は、以下の設定も行います。 Azure REST API や Microsoft Identity Platform 向けに使用する場合はこの手順をスキップできます。
アプリ登録したアプリに対して Azure ロールの権限を割り当てます。 (Azure の RBAC という仕組みを使います)
詳細は、
https://learn.microsoft.com/ja-jp/azure/role-based-access-control/role-assignments-portal?tabs=delegate-condition

#### [Microsoft Identity Platform 向けの設定]
Microsoft Identity Platform 向けに使用する場合は、以下の設定も行います。 Azure REST API や Microsof Graph API 向けに使用する場合はこの手順をスキップできます。
[認証] > [プラットフォームを追加] > [Web] をクリックします。
リダイレクト URI に http://jwt.ms を入力し、さらにアクセス トークン (暗黙的なフローに使用)と
ID トークン (暗黙的およびハイブリッド フローに使用) の両方にチェックをいれます。
[構成] をクリックし、さらに [保存] をクリックして、設定内容を保存します。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/8fe09995-31d9-4c32-b4b3-d1b30c9156d8)

## 環境変数を生成
VS Code の拡張機能 Rest Client で使用する環境変数 (Microsoft Entra ID のテナント情報など) を定義します。
インストール済みの 拡張機能 Rest Client を表示して、[拡張機能の設定] をクリックします。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/9321891c-5ea3-4826-b3a2-e6deab44e3a1)


Environment Variables の項目の settings.json で編集をクリックします。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/3fb0a5b8-233a-4b5a-a74c-c13921c7a144)

#### [Microsoft Graph API 向けの設定]
rest-client.environmentVariables の項目に下記を記載
```json:settings.json
"rest-client.environmentVariables": {
    "環境変数の名前": {
        "aadV2TenantId": "テナントID",
        "aadV2ClientId" : "クライアントID",
        "aadV2ClientSecret" : "クライアントシークレットの値",
        "aadV2AppUri" : "https://graph.microsoft.com",
    }
}
```
aadV2AppUri は OAuth / OIDC の Scope や resorce パラメータに当たる値になります。

#### [Azure REST API 向けの設定]
rest-client.environmentVariables の項目に下記を記載
```json:settings.json
"rest-client.environmentVariables": {
    "環境変数の名前": {
        "aadV2TenantId": "テナントID",
        "aadV2ClientId" : "クライアントID",
        "aadV2ClientSecret" : "クライアントシークレットの値",
        "aadV2AppUri" : "https://management.azure.com/",
        "subscriptionId" : "サブスクリプションID",
    }
}
```
(aadV2AppUri は OAuth / OIDC の Scope や resorce パラメータに当たる値になります。Azure REST API の場合は API ごとに異なる場合があるので適宜修正してご使用ください)

#### [Microsoft Identity Platform 向けの設定]
rest-client.environmentVariables の項目に下記を記載
```json:settings.json
"rest-client.environmentVariables": {
    "環境変数の名前": {
        "baseUrl": "https://login.microsoftonline.com",
        "tenantId_or_tenantName" : "テナント ID もしくは テナント名",
        "clientId" : "クライアント ID ",
        "clientSecret" :"クライアントシークレットの値",
        "redirectUri": "https://jwt.ms"
    },
}
```
#### 設定例
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/d6c8701f-0077-4246-b340-d06f8f159864)

## 本ソースコードをダウンロードします。
以下のようにして ZIP ファイルをダウンロードし、展開したら、展開後にできたフォルダーを VS Code で開きます。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/4e88bd2d-b6cf-4374-9154-46dc58943cd2)

# 使い方
## ダウンロードしたサンプルを使用する
使用したいサンプルのファイルをVS Code のエスクプローラーから開き、
使用したいリクエストの上に表示される [Send Request] というボタンをクリックすることで
Graph API などのリクエストを行うことができます。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/1165764a-2816-4ba7-ae93-1415893896ff)

## 新しい API のリクエストの作成方法
.http 拡張子のファイルを作成する
下記のように API のリクエスト内容を記載する
#### [Graph API の場合]
"###" の間に Microsoft Graph API の HTTP リクエストを記載
Authorization ヘッダーには 必ず Bearer {{$aadV2Token appOnly}} と記載する
```
// Graph API のバージョン指定 v1.0 or beta
@graph_endpoint = https://graph.microsoft.com/v1.0

###
// ユーザーの一覧を取得
GET {{graph_endpoint}}/users
Authorization: Bearer {{$aadV2Token appOnly}}
###
```

#### [Azure REST API の場合]
"###" の間に Azure REST API の HTTP リクエストを記載
Authorization ヘッダーには 必ず Bearer {{$aadV2Token appOnly}} と記載する
```
// API の バージョン指定
@api_version = 2022-12-01

###
// サブスクリプションの一覧
GET https://management.azure.com/subscriptions?api-version={{api_version}} HTTP/1.1
Authorization: Bearer {{$aadV2Token appOnly}}
###
```

## API のリクエスト方法
.http ファイルを開いたらまず初めにどの環境変数を使用するか選択します。
右下の方に環境変数のメニューがあるので、こちらをクリックして使用する環境変数をクリックします。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/6bc867f2-150e-4427-a5b2-071304e53aff)

Send Request を押すと API のリクエストが実行されます。そのまま右側に Response が表示され API のリクエスト結果が表示されます。
![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/60956702-930e-4be5-8fdd-8b638e55796d)

## API のリクエストからコードを自動生成する方法。
各言語の標準ライブラリを使ったコードを自動生成させることができます。
記述された API のリクエスト上で右クリックし、 [Generate Code Snippet] をクリックすることで各言語の標準ライブラリを使ったコードが生成されます。

![image](https://github.com/shmiki-microsoft/REST-Client-for-Visual-Studio-Code---Example-requests-for-Azure-REST-API-and-Microsoft-Graph-API/assets/74346899/93c93982-a84e-4c95-bb7c-85ee36fefc99)

Azure SDK や Microsoft Graph SDK などといった Microsoft 社から提供されているライブラリを使用したいときは公式ドキュメントを参照したり、
ChatGPT や Copilot などを使って生成するのがよいかと思います。

