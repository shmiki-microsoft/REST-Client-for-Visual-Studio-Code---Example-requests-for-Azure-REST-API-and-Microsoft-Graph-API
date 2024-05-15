OBO フローを使う場合には事前に以下の作業が必要
1. Microsoft Entra ID で 3 つのアプリ登録を作成する<br/>
それぞれ、クライアントアプリ、 Web API1 , Web API2 用に[Azure Portal] > [Microsoft Entra ID] > [アプリ登録] にて新規作成する<br/>

1. クライアントアプリ用に作成したアプリ登録にてリダイレクト URI を設定する<br/>
[Azure Portal] > [Microsoft Entra ID] > [アプリ登録] >"該当のアプリ">[認証]>[プラットフォームを追加] からWeb を選択してリダイレクト URI に https://jwt.ms を入力して保存する<br/>

1. クライアントアプリ /, Web API 用のアプリ登録でクライアントシークレットを発行する<br/>
   
1. Web API1 / Web API2 用のアプリ登録で API の公開を行う<br/>
[Azure Portal] > [Microsoft Entra ID] > [アプリ登録] >"該当のアプリ">[API の公開]にて
アプリケーション ID の URIの追加をクリックする<br/>
[Scope] の追加で、  WebAPI1 の場合は a.read, Web API2の場合は b.read で作成する<br/>

1. クライアントアプリ 用に作成したアプリ登録にて Web API1 のアクセス許可を追加する <br/>
[Azure Portal] > [Microsoft Entra ID] > [アプリ登録] >"該当のアプリ"> [API のアクセス許可] > [アクセス許可の追加] > [所属する組織で使用している API] より<br/>
WebAPI1 用に作成したアプリ登録を検索し a.read にチェックを入れて [アクセス許可の追加] をクリックする<br/>

1. Web API1 用のアプリ登録にて Web API2 のアクセス許可を追加する <br/>
[Azure Portal] > [Microsoft Entra ID] > [アプリ登録] >"該当のアプリ"> [API のアクセス許可] > [アクセス許可の追加] > [所属する組織で使用している API] より<br/>
WebAPI2 用に作成したアプリ登録を検索し b.read にチェックを入れて [アクセス許可の追加] をクリックする<br/>

1. Web API2 用のアプリ登録にて Web API1 がアクセス許可の同意なしにWeb API2 のスコープを使えるように設定する<br/>
[Azure Portal] > [Microsoft Entra ID] > [アプリ登録] >"該当のアプリ">[API の公開]>[クライアント アプリケーションの追加] にて Web API1 用のアプリ登録のアプリケーション ID (クライアント ID) を追加します。<br/>

1. OBOFlow.http に各種設定を投入する<br/>
クライアントアプリ / Web API1 / Web API2 用のアプリ登録のそれぞれの、アプリケーション ID (クライアント ID) やクライアントシークレット、スコープを<br/>
ファイルの先頭に用意した変数に設定する。 変数の値は " " で囲わないように注意

