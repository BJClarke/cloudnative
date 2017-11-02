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

{{site.data.keyword.dev_cli_long}} 提供一種可延伸的指令驅動方式，以使用 `dev` 外掛程式來建立、開發及部署 Web 專案。最適合想要使用指令行控制開發端對端微服務應用程式的開發人員。

{: shortdesc}

{{site.data.keyword.dev_cli_notm}} 使用兩個容器來協助建置及測試應用程式。第一個是 tools 容器，內含建置及測試應用程式的必要公用程式。此容器的 Dockerfile 是透過 [`dockerfile-tools`](#command-parameters) 參數所定義。您可以將它視為開發容器，因為它包含的工具一般適用於開發特定運行環境。

第二個容器是 run 容器。例如，此容器的形式適用於部署在 {{site.data.keyword.Bluemix}} 中使用。因此，會定義一個啟動應用程式用的進入點。當您選擇透過 {{site.data.keyword.dev_cli_short}} 來執行應用程式時，它會使用此容器。此容器的 Dockerfile 是透過 [`dockerfile-run`](#run-parameters) 參數所定義。


## 新增 {{site.data.keyword.dev_cli_notm}}
{: #add-cli}


### 必要條件
{: #prereq}

您必須取得一些必要條件，才能完整地探索及適當地利用 {{site.data.keyword.dev_cli_short}}，因為它可高度延伸，可讓您運用其他增補技術。

<!--1. Install the [Cloud Foundry CLI ![External link icon](../icons/launch-glyph.svg "External link icon")](https://github.com/cloudfoundry/cli#getting-started "External link icon").-->

1. 安裝 [{{site.data.keyword.Bluemix}} CLI ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](http://clis.ng.bluemix.net/ui/home.html "外部鏈結圖示")。

2. 取得 [{{site.data.keyword.Bluemix_notm}}](https://www.bluemix.net) ID。

3. 如果您打算在本端執行及除錯應用程式，則也必須安裝 [Docker ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.docker.com/get-docker "外部鏈結圖示")。


### 開始之前
{: #before-install}

1. 連接至 [{{site.data.keyword.Bluemix_notm}} 地區](/docs/overview/whatisbluemix.html#ov_intro_reg)中的 API 端點。例如，輸入下列指令以連接至 {{site.data.keyword.Bluemix_notm}} 美國南部地區：

	```
	bx api https://api.ng.bluemix.net
	```
	{: codeblock}
	
2. 提供 IBM ID 及密碼，以登入 {{site.data.keyword.Bluemix_notm}}：

	```
	bx login
	```
	{: codeblock}
	
	**附註：**如果您的認證遭到拒絕，則可以使用「聯合 ID」。請遵循下列步驟，以使用「聯合 ID」來進行鑑別。
	
	<!-- 
	POINT TO IBM CLOUD CLI LOG IN DOCUMENTATION !!!
	
	This link does not work in production yet --> 
	
	1. 登入 [{{site.data.keyword.iamshort}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](https://www.bluemix.net/iam/#/apikeys "外部鏈結圖示"){: new_window}。
	2. 選取**建立 API 金鑰**。
		* 輸入 apiKey 名稱及說明
	3. 下載 apiKey。
	4. 開啟檔案，並複製 `apiKey` 欄位中的值。
	5. 使用下列指令來登入：
	 
		```
		bx login --apikey <value>
		```
		{: codeblock}


### 安裝
{: #installation}

1. 執行下列指令，以安裝 [{{site.data.keyword.dev_cli_short}} ![外部鏈結圖示](../icons/launch-glyph.svg "外部鏈結圖示")](/docs/cli/reference/bluemix_cli/index.html#install_plug-in "外部鏈結圖示"){: new_window}：
 
	```
	bx plugin install dev
	```
	{: codeblock}

2. 	執行下列指令，以驗證外掛程式安裝成功：  
 
	```
	bx dev
	```
	{: codeblock}


## 指令
{: #commands}

使用下列指令，以建立、部署、除錯及測試專案。

### Build
{: #build}

您可以使用 `build` 指令來建置應用程式。`build-cmd-run` 配置元素是用來建置應用程式。`test`、`debug` 及 `run` 指令預期會找到已編譯的專案，因此您必須先至少事先執行一次 build 作業。

在現行專案目錄中執行下列指令，以建置您的應用程式：  

```
bx dev build
```
{: codeblock}


[Build 指令參數](#command-parameters)


### Code
{: #code}

使用 `code` 指令，以在部署之後下載應用程式碼，因此，您可以在本端進行檢閱或進行變更。

執行下列指令，以從指定的專案下載程式碼。

```
bx dev code <projectName>
```
{: codeblock}


### Create
{: #create}

提示輸入所有資訊（包括語言、專案名稱及應用程式型樣類型），以建立專案。會在現行目錄中建立專案。 

若要在現行專案目錄中建立專案，並建立服務與它的關聯，請執行下列指令：

```
bx dev create
```
{: codeblock}


### Debug
{: #debug}

您可以透過 `debug` 指令來除錯應用程式。必須先使用 build 指令完成對專案的建置。當您呼叫 `debug` 指令時，會啟動容器，以提供 `container-port-map-debug` 值所定義的除錯埠。請將最愛的除錯工具連接至埠，以如常除錯應用程式。

**限制**：無法對 Swift 專案進行除錯。

首先，編譯您的專案：

```
bx dev build
```
{: codeblock}

在現行專案目錄中執行下列指令，以除錯您的應用程式：

```
bx dev debug
```
{: codeblock}	

若要結束除錯階段作業，請使用 `CTRL-C`。


#### Debug 指令參數
{: #debug-parameters}

下列參數是 `debug` 指令特有的，可協助除錯應用程式。

##### `container-port-map-debug`
{: #port-map-debug}

* 除錯埠的埠對映。第一個值是在主機 OS 中使用的埠，第二個則是容器 [host-port:container-port] 中的埠。
* 用法：`bx dev debug --container-port-map-debug [7777:7777]`

##### `build-cmd-debug`
{: #build-cmd-debug}

* 用來建置程式碼以進行除錯的參數。
* 用法：`bx dev debug --build-cmd-debug build.command.sh`

##### `debug-cmd`
{: #debug-cmd}

* 用來指定指令以在 tools 容器中呼叫 debug 的參數。如果您的 `build-cmd-debug` 以除錯模式啟動應用程式，請使用此參數。
* 用法：`bx dev debug --debug-cmd /the/debug/command`

#### 本端應用程式除錯：
{: #local-app-dev}

如需本端應用程式除錯的相關資訊，請參閱[本端應用程式除錯](docs/cloudnative/dev_cli_local_debug.html#local-debug)。


### Delete
{: #delete}

使用 `delete` 指令以移除 {{site.data.keyword.Bluemix}} 空間中的專案。您可以執行不含參數的指令，來列出要刪除的可用專案。專案程式碼及目錄不會從本端磁碟空間中予以移除。

執行下列指令，以刪除 {{site.data.keyword.Bluemix}} 中的專案：

```
bx dev delete <projectName>
```
{: codeblock}
 

**附註：****不會**移除 {{site.data.keyword.Bluemix}} 服務。


### Deploy
{: #deploy}

當您的專案根目錄中有 `manifest.yml` 檔案存在時，您可以透過 `deploy` 指令，將應用程式推送至 {{site.data.keyword.Bluemix}}。

在現行專案目錄中執行下列指令，以建置您的應用程式：  

```
bx dev build
```
{: codeblock}

執行下列指令，以將專案部署至 {{site.data.keyword.Bluemix}}：

```
bx dev deploy
```
{: codeblock}


### Help
{: #help}

依預設，如果未傳入動作或引數，或是提供 'help' 動作，則此指令會顯示一般「說明」文字。所顯示的一般說明會包括基礎引數的說明，以及可用動作清單。  

執行下列指令，以顯示一般說明資訊：

```
bx dev help
```
{: codeblock}


### List
{: #list}

您可以列出空間中的所有 {{site.data.keyword.Bluemix_notm}} 專案。

執行下列指令，以列出您的專案：

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


### Run
{: #run}

您可以透過 `run` 指令來執行應用程式。必須先使用 `build` 指令完成對專案的建置。當您呼叫 run 指令時，run 容器會啟動，並公開 `container-port-map` 參數所定義的埠。如果 run 容器 Dockerfile 未包含完成此步驟的進入點，則可以使用 `run-cmd` 參數來呼叫應用程式。 

首先，編譯您的專案：

```
bx dev build
```
{: codeblock}

在現行專案目錄中執行下列指令，以啟動您的應用程式：

```
bx dev run
```
{: codeblock}

若要結束階段作業，請使用 `CTRL-C`。


#### Run 參數
{: #run-parameters}

下列參數是 `run` 指令特有的，可協助管理 run 容器內的應用程式。

##### `container-name-run`
{: #container-name-run}
	
* run 容器的容器名稱。
* 用法：`bx dev run --container-name-run [<projectName>]`

##### `container-path-run`
{: #container-path-run}

* 執行時，容器中要共用的位置。
* 用法：`bx dev run --container-path-run [/path/to/app]`

##### `host-path-run`
{: #host-path-run}

* 執行時，主機系統上容器中要共用的位置。
* 用法：`bx dev run --host-path-run [/path/to/app/bin]`

##### `dockerfile-run`
{: #dockerfile-run}

* run 容器的 Dockerfile。
* 用法：`bx dev run --dockerfile-run [/path/to/Dockerfile.yml]`

##### `image-name-run`
{: #image-name-run}

* 要從 `dockerfile-run` 建立的映像檔。
* 用法：`bx dev run --image-name-run [/path/to/image-name]`

##### `run-cmd`
{: #run-cmd}

* 用來在 run 容器中執行程式碼的參數。如果您的映像檔會啟動應用程式，請使用此參數。
* 用法：`bx dev run --run-cmd [/the/run/command]`
	
### Status
{: #status}

您可以查詢 `container-name-run` 及 `container-name-tools` 所定義之 {{site.data.keyword.dev_cli_short}} 使用的容器的狀態。 

在現行專案目錄中執行下列指令，以檢查容器狀態：

```
bx dev status
```
{: codeblock}


[Status 指令參數](#command-parameters)


### Stop
{: #stop}

您可以透過 `stop` 指令來停止容器。

若要停止 `cli-config.yml` 檔案中定義的 tools 及 run 容器，請執行：

```
bx dev stop
```
{: codeblock}

若要停止 `cli-config.yml` 檔案中未定義的容器，您可以指定額外的指令行參數來置換它。對於 tools 容器，請使用 [`--container-name-tools`](#container-name-tools) 參數，對於 run 容器，請使用 [`--container-name-run`](#container-name-run) 參數。如果 `cli-config.yml` 檔案中或指令行上未指定任何容器，stop 指令只會返回。


### Test
{: #test}

您可以透過 `test` 指令來測試應用程式。必須先使用 `build` 指令完成對專案的建置。接著會使用 tools 容器，針對應用程式呼叫 `test-cmd`。

首先，編譯您的專案：

```
bx dev build
```
{: codeblock}

執行下列指令，以測試應用程式： 

```
bx dev test
```
{: codeblock}


[Test 指令參數](#command-parameters)


## build、debug、run 及 test 的參數
{: #command-parameters}

下列參數可以與 `build|debug|run|test` 指令搭配使用，或直接更新專案的 `cli-config.yml` 檔案。有額外的參數適用於 [`debug`](#debug-parameters) 及 [`run`](#run-parameters) 指令。

**附註**：在指令行上輸入的指令參數，其優先順序高於 `cli-config.yml` 配置。

### `container-name-tools`  
{: #container-name-tools}

* tools 容器的容器名稱。
* 用法：`bx dev <build|debug|run|stop|test> --container-name-tools [<projectName>]`

### `host-path-tools`
{: #host-path-tools}

* 主機上基於建置、除錯、測試而共用的位置。
* 用法：`bx dev <build|debug|run|test> --host-path-tools [/path/to/build/tools]`

### `container-path-tools`
{: #container-path-tools}

* 容器中基於建置、除錯、測試而共用的位置。
* 用法：`bx dev <build|debug|run|test> --container-path-tools [/path/for/build]`

### `container-port-map`
{: #container-port-map}

* 容器的埠對映。第一個值是在主機 OS 中使用的埠，第二個則是容器 [host-port:container-port] 中的埠。
* 用法：`bx dev <build|debug|run|test> --container-port-map [8090:8090,9090,9090]`

### `dockerfile-tools`
{: #dockerfile-tools}

* tools 容器的 Dockerfile。
* 用法：`bx dev <build|debug|run|test> --dockerfile-tools [path/to/dockerfile]`

### `image-name-tools`
{: #image-name-tools}

* 要從 `dockerfile-tools` 建立的映像檔。
* 用法：`bx dev <build|debug|run|test> --image-name-tools [path/to/image-name]`

### `build-cmd-run`
{: #build-cmd-run}

* 用來指定指令以建置程式碼，進行 DEBUG 以外的所有用途。
* 用法：`bx dev <build|debug|run|test> --build-cmd-run [some.build.command]`

### `test-cmd`
{: #test-cmd}

* 用來指定指令以在 tools 容器中測試程式碼的參數。
* 用法：`bx dev <build|debug|run|test> --test-cmd [/the/test/command]`

### `trace`
{: #trace}

* 若要提供詳細輸出，請使用此參數。
* 用法：`bx dev <build|debug|run|test> --trace`


