---

copyright:
  years: 2017
lastupdated: "2017-11-02"

---
{:new_window: target="_blank"}  
{:shortdesc: .shortdesc}  
{:screen: .screen}  
{:codeblock: .codeblock}  
{:pre: .pre}  

# {{site.data.keyword.dev_cli_short}}
{: #developercli}	

{{site.data.keyword.dev_cli_long}} では、`dev` プラグインで Web プロジェクトを作成、開発、デプロイするための拡張可能なコマンド駆動型アプローチを提供します。エンドツーエンドのマイクロサービス・アプリケーションを開発する際にコマンド・ライン制御を使用したい開発者に理想的です。

{: shortdesc}

{{site.data.keyword.dev_cli_notm}} では、アプリケーションのビルドおよびテストを容易にするために、2 つのコンテナーを使用します。1 つ目は、アプリケーションのビルドとテストに必要なユーティリティーを含むツール・コンテナーです。このコンテナーの Dockerfile は、[`dockerfile-tools`](#command-parameters) パラメーターで定義されます。これは、特定のランタイムの開発に通常役立つツールを含むため、開発コンテナーとして考えることもできます。

2 つ目のコンテナーは実行コンテナーです。このコンテナーは、例えば {{site.data.keyword.Bluemix}} などで使用するためにデプロイされるのに適した形式のものです。結果として、アプリケーションを開始するエントリー・ポイントが定義されます。{{site.data.keyword.dev_cli_short}} でアプリケーションを実行することを選択すると、このコンテナーが使用されます。このコンテナーの Dockerfile は、[`dockerfile-run`](#run-parameters) パラメーターで定義されます。


## {{site.data.keyword.dev_cli_notm}} の追加
{: #add-cli}


### 前提条件
{: #prereq}

{{site.data.keyword.dev_cli_short}} は追加の無料テクノロジーを活用できるように高度に拡張可能であるため、これをフルに探索して適切に利用するには、いくつかの前提条件を入手する必要があります。

<!--1. Install the [Cloud Foundry CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#getting-started "External link icon").-->

1. [{{site.data.keyword.Bluemix}} CLI ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](http://clis.ng.bluemix.net/ui/home.html "外部リンク・アイコン") をインストールします。

2. [{{site.data.keyword.Bluemix_notm}}](https://www.bluemix.net) ID を入手します。

3. ローカルでのアプリケーションの実行およびデバッグを予定している場合は、[Docker ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.docker.com/get-docker "外部リンク・アイコン") のインストールも必要です。


### 始めに
{: #before-install}

1. 自分の [{{site.data.keyword.Bluemix_notm}} 地域](/docs/overview/whatisbluemix.html#ov_intro_reg)内の API エンドポイントに接続します。例えば、{{site.data.keyword.Bluemix_notm}} 米国南部地域に接続するには、次のコマンドを入力します。

	```
	bx api https://api.ng.bluemix.net
	```
	{: codeblock}
	
2. IBM ID とパスワードを入力して {{site.data.keyword.Bluemix_notm}} にログインします。

	```
	bx login
	```
	{: codeblock}
	
	**注:** 資格情報が拒否された場合、フェデレーテッド ID を使用している可能性があります。フェデレーテッド ID を使用して認証するには、以下のステップに従ってください。
	
	<!-- 
	POINT TO IBM CLOUD CLI LOG IN DOCUMENTATION !!!
	
	This link does not work in production yet --> 
	
	1. [{{site.data.keyword.iamshort}} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](https://www.bluemix.net/iam/#/apikeys "外部リンク・アイコン"){: new_window} にログインします。
	2. **「API キーの作成」**を選択します。
		* apiKey の名前と説明を入力します。
	3. apiKey をダウンロードします。
	4. ファイルを開いて、`apiKey` フィールドから値をコピーします。
	5. 以下のコマンドを使用してログインします。
	 
		```
		bx login --apikey <value>
		```
		{: codeblock}


### インストール
{: #installation}

1. 以下のコマンドを実行して、[{{site.data.keyword.dev_cli_short}} ![外部リンク・アイコン](../icons/launch-glyph.svg "外部リンク・アイコン")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in "外部リンク・アイコン"){: new_window} をインストールします。
 
	```
	bx plugin install dev
	```
	{: codeblock}

2. 	以下のコマンドを実行して、プラグインのインストールが成功したことを確認します。  
 
	```
	bx dev
	```
	{: codeblock}


## コマンド
{: #commands}

プロジェクトの作成、デプロイ、デバッグ、およびテストには、以下のコマンドを使用します。

### build
{: #build}

`build` コマンドを使用してアプリケーションをビルドすることができます。アプリケーションのビルドには、`build-cmd-run` 構成エレメントが使用されます。`test`、`debug`、および `run` の各コマンドは、コンパイルされたプロジェクトを検出することを予期しているため、事前に少なくとも 1 度はビルド操作を実行しておく必要があります。

アプリケーションをビルドするには、現行プロジェクト・ディレクトリーで以下のコマンドを実行します。  

```
bx dev build
```
{: codeblock}


[build コマンドのパラメーター](#command-parameters)


### コード
{: #code}

`code` コマンドを使用して、デプロイ後にアプリケーション・コードをダウンロードします。これにより、ローカルで確認したり、変更を行ったりすることができます。

指定されたプロジェクトからコードをダウンロードするには、以下のコマンドを実行します。

```
bx dev code <projectName>
```
{: codeblock}


### create
{: #create}

プロジェクトを作成します。言語、プロジェクト名、アプリ・パターン・タイプなど、すべての情報の入力を求められます。プロジェクトが現行ディレクトリーに作成されます。 

現行プロジェクト・ディレクトリーにプロジェクトを作成して、それにサービスを関連付けるには、以下のコマンドを実行します。

```
bx dev create
```
{: codeblock}


### debug
{: #debug}

`debug` コマンドを使用してアプリケーションをデバッグすることができます。まず、build コマンドを使用して、プロジェクトに対してビルドを完了する必要があります。`debug` コマンドを呼び出すと、`container-port-map-debug` の値で定義されたデバッグ・ポート (複数可) を提供するコンテナーが開始されます。任意のデバッグ・ツールをポートに接続して、アプリケーションを通常どおりデバッグできます。

**制限事項**: Swift プロジェクトをデバッグに使用できません。

まず、プロジェクトをコンパイルします。

```
bx dev build
```
{: codeblock}

アプリケーションをデバッグするには、現行プロジェクト・ディレクトリーで以下のコマンドを実行します。

```
bx dev debug
```
{: codeblock}	

デバッグ・セッションを終了するには、`CTRL-C` を使用します。


#### debug コマンドのパラメーター
{: #debug-parameters}

以下のパラメーターは `debug` コマンド専用で、アプリケーションのデバッグを支援します。

##### `container-port-map-debug`
{: #port-map-debug}

* デバッグ・ポートのポート・マッピング。最初の値は、ホスト OS で使用するポートで、2 つ目の値はコンテナー内のポートです [host-port:container-port]。
* 使用法: `bx dev debug --container-port-map-debug [7777:7777]`

##### `build-cmd-debug`
{: #build-cmd-debug}

* デバッグ用にコードをビルドするために使用されるパラメーター。
* 使用法: `bx dev debug --build-cmd-debug build.command.sh`

##### `debug-cmd`
{: #debug-cmd}

* ツール・コンテナーで debug を呼び出すコマンドを指定するために使用されるパラメーター。`build-cmd-debug` がデバッグでアプリケーションを開始する場合に、このパラメーターを使用します。
* 使用法: `bx dev debug --debug-cmd /the/debug/command`

#### ローカル・アプリケーション・デバッグ:
{: #local-app-dev}

ローカル・アプリケーション・デバッグについて詳しくは、[ローカル・アプリケーション・デバッグ](docs/cloudnative/dev_cli_local_debug.html#local-debug)を参照してください。


### delete
{: #delete}

`delete` コマンドを使用して、{{site.data.keyword.Bluemix}} スペースからプロジェクトを削除します。パラメーターを指定せずにこのコマンドを実行すると、削除可能なプロジェクトをリストすることができます。プロジェクト・コードおよびディレクトリーは、ローカル・ディスク・スペースから削除されません。

{{site.data.keyword.Bluemix}} からプロジェクトを削除するには、以下のコマンドを実行します。

```
bx dev delete <projectName>
```
{: codeblock}
 

**注:** {{site.data.keyword.Bluemix}} サービスは**削除されません**。


### deploy
{: #deploy}

プロジェクトのルート・ディレクトリーに `manifest.yml` ファイルがある場合は、`deploy` コマンドによってアプリケーションを {{site.data.keyword.Bluemix}} にプッシュできます。

アプリケーションをビルドするには、現行プロジェクト・ディレクトリーで以下のコマンドを実行します。  

```
bx dev build
```
{: codeblock}

プロジェクトを {{site.data.keyword.Bluemix}} にデプロイするには、以下のコマンドを実行します。

```
bx dev deploy
```
{: codeblock}


### help
{: #help}

デフォルトでは、アクションも引数も渡されない場合や、「help」アクションが指定された場合、このコマンドでは一般的な「ヘルプ」テキストが表示されます。表示される一般ヘルプには、基本引数の説明と、使用可能なアクションのリストが含まれます。  

一般ヘルプ情報を表示するには、以下のコマンドを実行します。

```
bx dev help
```
{: codeblock}


### リスト
{: #list}

スペース内のすべての {{site.data.keyword.Bluemix_notm}} プロジェクトをリストできます。

プロジェクトをリストするには、以下のコマンドを実行します。

```
bx dev list
```
{: codeblock}


<!--
### Editing a project
{: #edit}

You can edit a project, such as changing the name, pattern or starter type, or adding services to your project. Run the following command:

```
bx dev edit
```
{: codeblock}
-->


### run
{: #run}

`run` コマンドを使用してアプリケーションを実行することができます。まず、`build` コマンドを使用して、プロジェクトに対してビルドを完了する必要があります。run コマンドを呼び出すと、実行コンテナーが開始され、`container-port-map` パラメーターで定義されたポートを公開します。このステップを完了するためのエントリー・ポイントが実行コンテナー Dockerfile に含まれない場合は、`run-cmd` パラメーターを使用してアプリケーションを呼び出すことができます。 

まず、プロジェクトをコンパイルします。

```
bx dev build
```
{: codeblock}

アプリケーションを開始するには、現行プロジェクト・ディレクトリーで以下のコマンドを実行します。

```
bx dev run
```
{: codeblock}

セッションを終了するには、`CTRL-C` を使用します。


#### run のパラメーター
{: #run-parameters}

以下のパラメーターは `run` コマンド専用で、実行コンテナー内でのアプリケーションの管理を支援します。

##### `container-name-run`
{: #container-name-run}
	
* 実行コンテナーのコンテナー名。
* 使用法: `bx dev run --container-name-run [<projectName>]`

##### `container-path-run`
{: #container-path-run}

* 実行に関して共有するコンテナー内の場所。
* 使用法: `bx dev run --container-path-run [/path/to/app]`

##### `host-path-run`
{: #host-path-run}

* 実行に関してコンテナーで共有するホスト・システム上の場所。
* 使用法: `bx dev run --host-path-run [/path/to/app/bin]`

##### `dockerfile-run`
{: #dockerfile-run}

* 実行コンテナーの Dockerfile。
* 使用法: `bx dev run --dockerfile-run [/path/to/Dockerfile.yml]`

##### `image-name-run`
{: #image-name-run}

* `dockerfile-run` から作成するイメージ。
* 使用法: `bx dev run --image-name-run [/path/to/image-name]`

##### `run-cmd`
{: #run-cmd}

* 実行コンテナーでコードを実行するのに使用されるパラメーター。イメージによってアプリケーションが開始される場合に、このパラメーターを使用してください。
* 使用法: `bx dev run --run-cmd [/the/run/command]`
	
### status
{: #status}

`container-name-run` と `container-name-tools` で定義された、{{site.data.keyword.dev_cli_short}} で使用されるコンテナーの状況を照会できます。 

コンテナーの状況を確認するには、現行プロジェクト・ディレクトリーで以下のコマンドを実行します。

```
bx dev status
```
{: codeblock}


[status コマンドのパラメーター](#command-parameters)


### stop
{: #stop}

`stop` コマンドを使用してコンテナーを停止することができます。

`cli-config.yml` ファイルで定義されているツールを停止してコンテナーを実行するには、以下を実行してください。

```
bx dev stop
```
{: codeblock}

`cli-config.yml` ファイルで定義されていないコンテナーを停止するには、追加のコマンド・ライン・パラメーターを指定してオーバーライドします。ツール・コンテナーに対しては [`--container-name-tools`](#container-name-tools) パラメーターを使用し、実行コンテナーに対しては[`--container-name-run`](#container-name-run) パラメーターを使用します。`cli-config.yml` ファイルでもコマンド・ラインでもコンテナーが指定されていない場合、stop コマンドは単にリターンします。


### test
{: #test}

`test` コマンドを使用してアプリケーションをテストすることができます。まず、`build` コマンドを使用して、プロジェクトに対してビルドを完了する必要があります。その後、ツール・コンテナーを使用して、アプリケーションの `test-cmd` を呼び出します。

まず、プロジェクトをコンパイルします。

```
bx dev build
```
{: codeblock}

アプリケーションをテストするには、以下のコマンドを実行します。 

```
bx dev test
```
{: codeblock}


[test コマンドのパラメーター](#command-parameters)


## build、debug、run、および test のパラメーター
{: #command-parameters}

以下のパラメーターは、`build|debug|run|test` コマンドと一緒に、またはプロジェクトの `cli-config.yml` ファイルを直接更新することによって使用できます。[`debug`](#debug-parameters) コマンドと [`run`](#run-parameters) コマンドには追加のパラメーターが使用可能です。

**注**: コマンド・ラインで入力されたコマンド・パラメーターのほうが、`cli-config.yml` の構成より優先されます。

### `container-name-tools`  
{: #container-name-tools}

* ツール・コンテナーのコンテナー名。
* 使用法: `bx dev <build|debug|run|stop|test> --container-name-tools [<projectName>]`

### `host-path-tools`
{: #host-path-tools}

* build、debug、test の場合に共有するホスト上の場所。
* 使用法: `bx dev <build|debug|run|test> --host-path-tools [/path/to/build/tools]`

### `container-path-tools`
{: #container-path-tools}

* build、debug、test の場合に共有するコンテナー内の場所。
* 使用法: `bx dev <build|debug|run|test> --container-path-tools [/path/for/build]`

### `container-port-map`
{: #container-port-map}

* コンテナーのポート・マッピング。最初の値は、ホスト OS で使用するポートで、2 つ目の値はコンテナー内のポートです [host-port:container-port]。
* 使用法: `bx dev <build|debug|run|test> --container-port-map [8090:8090,9090,9090]`

### `dockerfile-tools`
{: #dockerfile-tools}

* ツール・コンテナーの Dockerfile。
* 使用法: `bx dev <build|debug|run|test> --dockerfile-tools [path/to/dockerfile]`

### `image-name-tools`
{: #image-name-tools}

* `dockerfile-tools` から作成するイメージ。
* 使用法: `bx dev <build|debug|run|test> --image-name-tools [path/to/image-name]`

### `build-cmd-run`
{: #build-cmd-run}

* デバッグ以外のすべての用途でコードをビルドするコマンドを指定するために使用されるパラメーター。
* 使用法: `bx dev <build|debug|run|test> --build-cmd-run [some.build.command]`

### `test-cmd`
{: #test-cmd}

* ツール・コンテナーでコードをテストするコマンドを指定するために使用されるパラメーター。
* 使用法: `bx dev <build|debug|run|test> --test-cmd [/the/test/command]`

### `trace`
{: #trace}

* このパラメーターは、詳細出力を提供するために使用します。
* 使用法: `bx dev <build|debug|run|test> --trace`


