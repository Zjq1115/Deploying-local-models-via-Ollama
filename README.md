# Ollama基础入门

## 一、Ollama简介

Ollama是一个专为在本地环境中运行和定制大型语言模型而设计的工具。它提供了一个简单而高效的接口，用于创建、运行和管理这些模型，同时还提供了一个丰富的预构建模型库，可以轻松集成到各种应用程序中。

## 二、安装Ollama

### 2.1 Windows安装指南

在Windows上安装Ollama需要通过下载安装包并进行手动安装。以下是详细步骤：

* **下载安装包**：访问Ollama官网，下载适用于Windows的安装包。https://ollama.com/download/OllamaSetup.exe
* **安装Ollama**：双击下载的安装包，按照提示完成安装。默认安装路径为C:\Users\{你的电脑账户名}\AppData\Local\Programs\Ollama。
* **配置环境变量**：如果遇到ollama命令无法使用的问题，需要配置环境变量。操作如下：控制面板 → 系统 → 高级系统设置 → 环境变量 → 在系统变量中找到Path → 编辑 → 新建，添加Ollama的安装路径。
* **验证安装**：打开命令提示符，输入ollama --version来验证安装是否成功。

### 2.2 Linux安装指南

在Linux上安装Ollama可以通过包管理器或下载源码编译安装。以下是通过包管理器安装的步骤：

* **更新包列表**：打开终端，输入以下命令：sudo apt-get update
* **安装Ollama**：输入以下命令进行安装：
  curl -fsSL https://ollama.com/install.sh | sh
* **验证安装**：输入ollama --version来验证安装是否成功。

### 2.3 Docker安装指南

使用Docker安装Ollama可以实现跨平台的便捷部署。以下是安装步骤：

* **安装Docker**：根据你的操作系统，从Docker官网下载并安装Docker。
* **拉取Ollama镜像**：打开终端或命令提示符，输入以下命令：
  docker pull ollama/ollama
* **运行Ollama容器**：输入以下命令来运行Ollama容器：
  docker run -d -p 3000:8080 --gpus=all -v ollama:/root/.ollama -v open-webui:/app/backend/data --name open-webui --restart always ollama/ollama
* **验证安装**：打开浏览器，访问http://localhost:3000，如果看到Ollama的界面，则表示安装成功。

## 三、Ollama的库和工具

**（若只使用ollama部署本地模型，第三章可跳过）**

### 3.1 Ollama-python库

Ollama-python库是为Python开发者提供的，用于与Ollama服务进行交互的工具。这个库使得Python开发者能够轻松地在他们的项目中集成和运行大型语言模型。

**主要功能**

* 模型管理：通过Python脚本管理模型的创建、拉取、删除和复制。

* 模型运行：在Python环境中运行Ollama模型，并处理模型的输入输出。

* 自定义模型：支持通过Python脚本自定义模型参数和行为。
  安装方法：pip install ollama-python

* 使用示例：

  ```python
  from ollama_python import OllamaClient
  
  client = OllamaClient("http://localhost:11434")
  
  # 创建模型
  client.create_model("my_model", "path/to/modelfile")
  
  # 运行模型
  response = client.run_model("my_model", "Hello, world!")
  print(response)
  ```



### 3.2 CLI参考

Ollama的命令行界面（CLI）是一个强大的工具，允许用户直接从命令行与Ollama服务交互。CLI提供了丰富的命令集，用于模型的管理、运行和监控。

常用命令：

* 创建模型：ollama create my_model -f ./modelfile
* 拉取模型：ollama pull my_model
* 运行模型：ollama run my_model "Hello, world!"
* 删除模型：ollama rm my_model

### 3.3 REST API

Ollama提供了一个RESTful API，允许开发者通过HTTP请求与Ollama服务进行交互。这个API覆盖了所有Ollama的核心功能，包括模型管理、运行和监控。

常用API端点：

* 生成响应：POST /api/generate

* 模型聊天：POST /api/chat

* 使用示例

  ```shell
  curl -X POST http://localhost:11434/api/generate -d '{"model":"my_model","prompt":"Hello, world!"}'
  ```

## 四、运行和自定义模型

### 4.1 运行模型

* **启动Ollama服务**：首先，确保Ollama服务已经安装并运行。在命令行中输入ollama start以启动服务。（*请注意，并非在前台再次启动一个独立的server，ollama安装成功后已经是启动状态，不用输入ollama start，如果再次开机，想要使用ollama，则直接在cmd中输入ollama list查看已部署模型列表，ollama会自启动*）。
* **选择模型**：使用ollama list命令查看已部署的模型列表。选择你想要运行的模型。
* **运行模型**：通过ollama run [模型名称]命令来运行选定的模型。例如，如果你想运行名为gemma的模型，你应该输入ollama run gemma。
* **交互**：模型启动后，你可以开始与模型进行交互，输入提示（prompts）并接收模型的响应。

### 4.2 访问模型库

Ollama的模型库包含了多种预训练的大型语言模型，用户可以根据自己的需求选择合适的模型。以下是访问模型库的步骤：

* **查看模型列表**：使用

  ```shell
  ollama list
  ```

  命令可以列出所有可用的模型。

* **获取模型详情**：对于特定的模型，你可以使用

  ```
  ollama model details [模型名称]
  ```

  来获取更详细的模型信息，包括模型的描述、版本、大小等。

* **下载模型**：使用

  ```
  ollama download [模型名称]
  ```

  命令来下载模型到本地（建议在ollama官网上查看模型下载命令。）

* **更新模型**：定期检查模型库中的更新，使用

  ```
  ollama update [模型名称]
  ```

  来更新已下载的模型。

### 4.3 自定义模型

Ollama允许用户根据自己的需求对模型进行自定义。这包括调整模型的参数、添加特定的数据集或修改模型的结构。以下是自定义模型的基本步骤：

* **选择基础模型**：首先，从模型库中选择一个基础模型作为自定义的起点。

* **调整参数**：使用

  ```
  ollama customize [模型名称] --params [参数设置]
  ```

  命令来调整模型的参数。例如，你可以调整模型的学习率、批量大小等。

* **训练模型**：如果你有特定的数据集，可以使用

  ```
  ollama train [模型名称] --dataset [数据集路径]
  ```

  命令来训练模型。

* **验证和测试**：训练完成后，使用

  ```
  ollama test [模型名称]
  ```

  命令来验证模型的性能。

**从GGUF、PyTorch或Safetensors导入模型**
在Ollama中，Modelfile是一个关键的工具，用于定制和创建个性化的模型。Modelfile允许用户从现有的模型库中选择基础模型，并通过添加特定的参数和设置来调整模型的行为。以下是如何使用Modelfile进行模型定制的步骤：

* 创建Modelfile：首先，需要创建一个Modelfile文件。这个文件通常包含模型的基本信息，如模型类型、参数设置和任何特定的系统消息。

  ```shell
  FROM: gemma:latest
  PARAMETER:
    - temperature: 1
    - num_ctx: 4096
  TEMPLATE: "完整的提示词模板"
  SYSTEM:
    message: "自定义的系统消息"
  ```

  - 设置参数：在Modelfile中，通过PARAMETER指令设置模型的各种参数，如温度和上下文窗口大小，以调整模型的行为。

  - 定义提示模板：使用TEMPLATE指令定义模型的提示模板，这决定了模型如何响应用户的输入。

  - 创建和运行模型：使用Ollama提供的命令行工具来创建和运行你的模型。

  ```shell
  ollama create -f your_modelfile.yaml
  ollama run gemma-custom-model
  ```

自定义模型自由度更高，可参考4.4离线配置模型的具体流程来完成自己的模型配置。

### 4.4 离线配置模型

如果想要在一台隔离网络的单机上部署模型，流程如下：

* 创建一个临时的模型存储文件夹，用于临时保存一会儿要下载的模型权重文件，比如我创建了**E:\LinShi\qwen2.5-3b**文件夹：

![image-20251122170839679](\pics\image-20251122170839679.png)

* 随后在**qwen2.5-3b**文件夹内，右键新建一个**文本文档（.txt）**，然后将这个**txt文件**的**.txt后缀**删掉，并命名为**Modelfile**。

![image-20251122170923723](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20251122170923723.png)

* 使用一台联网的计算机进入[modelscope.cn/]()，点击**模型库**选择一个需要部署的模型，这里我选择**qwen2.5-3b-instruct**作为样例，搜索时加入**gguf**关键词。

![image-20251122171201275](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20251122171201275.png)

* 进入**模型文件**，选择**qwen2.5-3b-instruct-q4_k_m.gguf**作为样例，将其下载到本地的**qwen2.5-3b**文件夹中。

![image-20251122171236505](C:\Users\Lenovo\AppData\Roaming\Typora\typora-user-images\image-20251122171236505.png)

* 下载好后，右键点击**Modelfile**，用记事本打开，写如下的模型安装配置代码：

  ```python
  FROM ./qwen2.5-3b-instruct-q4_k_m.gguf
  
  TEMPLATE """
  <|im_start|>system
  {{ .System }}<|im_end|>
  
  {{- range .Messages }}
  <|im_start|>{{ .Role }}
  {{ .Content }}<|im_end|>
  {{- end }}
  
  <|im_start|>assistant
  """
  
  SYSTEM """
  你是一个专业且友好的中文助手。
  """
  
  PARAMETER temperature 0.7
  PARAMETER top_p 0.8
  PARAMETER top_k 20
  PARAMETER repeat_penalty 1.1
  PARAMETER num_ctx 8192
  PARAMETER num_predict 2048
  ```

* 随后打开cmd终端，进入当前**qwen2.5-3b**文件夹路径，输入：

  ```shell
  ollama create qwen2.5-3b-chat -f Modelfile
  ```

* 等待加载完成后，可使用**ollama list**命令查看模型qwen2.5-3b-chat是否成功部署，使用**ollama run qwen2.5-3b-chat**测试模型是否成运行。

## 五、故障排除与FAQ

### 5.1 常见问题解决

**模型加载失败：**确保模型文件完整且路径正确。如果使用的是自定义模型，检查模型的格式是否符合Ollama的要求。检查系统资源是否充足，如内存和CPU。查看Ollama的日志文件以获取错误信息。

**性能问题**：调整模型参数，如降低num_ctx以减少内存使用。
升级硬件资源，如增加内存或使用更强大的CPU。

**兼容性问题**：确保使用的Ollama版本与操作系统兼容。
查看Ollama的官方文档或社区论坛获取帮助。

### 5.2 如何升级Ollama

升级Ollama以获取最新功能和改进是非常重要的。以下是升级步骤：

* 检查当前版本：ollama --version
* 下载最新版本：访问Ollama的官方网站或GitHub页面，下载最新版本的安装包。
* 安装新版本：根据操作系统类型，执行相应的安装命令。
  在Linux上，通常是解压并替换旧版本。
  在Windows上，运行安装程序并按照提示操作。
* 验证升级：ollama --version

### 5.3 日志查看

默认情况下，Ollama的日志文件位于安装目录下的logs文件夹中。

### 5.4 如何配置Ollama服务器

配置Ollama服务器以优化性能和安全性是必要的。以下是配置步骤：

* 编辑配置文件：找到Ollama的配置文件，通常位于安装目录下。
  使用文本编辑器打开并编辑配置文件。
* 配置选项：调整服务器设置，如端口、内存限制等。
  配置安全选项，如启用HTTPS。
* 重启Ollama服务：sudo systemctl restart ollama

### 5.5 模型存储位置

了解模型存储位置对于管理和备份模型至关重要。默认情况下，模型一版会存储在以下位置：

* Linux：/var/lib/ollama/models
* Windows：C:\ProgramData\Ollama\models
* macOS：/Library/Application Support/Ollama/models
