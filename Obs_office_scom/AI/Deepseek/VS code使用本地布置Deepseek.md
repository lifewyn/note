
- [[Deepseek的模型版本]]

在Linux环境下，使用VS Code部署本地DeepSeek模型的步骤如下。这里我们将通过Ollama来管理DeepSeek模型，并在VS Code中通过插件与模型进行交互。以下是详细的步骤：

### **1. 安装Ollama**

Ollama是一个轻量级的工具，用于管理和运行本地AI模型，包括DeepSeek。

#### **安装Ollama**

打开终端，运行以下命令：

bash复制

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

#### **验证Ollama安装**

运行以下命令验证Ollama是否安装成功：

bash复制

```bash
ollama -v
```

### **2. 下载DeepSeek模型**

使用Ollama下载DeepSeek模型。以下是下载7B版本的DeepSeek模型的步骤：

#### **下载DeepSeek模型**

bash复制

```bash
ollama pull deepseek-r1:7b
```

#### **启动Ollama服务**

启动Ollama服务，以便可以通过网络访问模型：

bash复制

```bash
ollama serve
```

### **3. 安装VS Code扩展**

在VS Code中安装以下扩展，以便更好地使用DeepSeek：

#### **安装Cline插件**

Cline是一个VS Code插件，可以连接到本地运行的DeepSeek模型。

1. 打开VS Code。
    
2. 在扩展市场中搜索`Cline`。
    
3. 安装`Cline`扩展。
    

### **4. 配置Cline插件**

配置Cline插件以连接到本地运行的DeepSeek模型。

1. 打开VS Code的设置（`Ctrl + ,` 或 `Command + ,`）。
    
2. 搜索`Cline`，找到`Cline: Api Provider`设置。
    
3. 配置以下内容：
    
    - **API Provider**：选择`OpenAI Compatible`。
        
    - **Base URL**：输入`http://localhost:11434`。
        
    - **Model ID**：输入`deepseek-r1:7b`。
        

### **5. 测试DeepSeek**

完成配置后，可以通过VS Code中的Cline插件与DeepSeek模型进行交互。

#### **测试模型**

1. 打开一个代码文件。
    
2. 在代码中输入一些注释，描述你想要的功能。例如：
    
    Python复制
    
    ```python
    # Write a function to calculate the factorial of a number
    ```
    
3. 按下`Ctrl + Space`或`Command + Space`，触发Cline插件的代码生成功能。
    
4. 查看生成的代码是否符合你的需求。
    

### **6. 使用Open WebUI（可选）**

如果你希望通过Web界面与DeepSeek模型进行交互，可以安装Open WebUI。

#### **安装Open WebUI**

1. 拉取Open WebUI的Docker镜像：
    
    bash复制
    
    ```bash
    docker pull ghcr.io/open-webui/open-webui:main
    ```
    
2. 运行Open WebUI容器：
    
    bash复制
    
    ```bash
    docker run -d -p 3000:8080 \
      --add-host=host.docker.internal:host-gateway \
      -v open-webui:/app/backend/data \
      --name open-webui \
      --restart always \
      ghcr.io/open-webui/open-webui:main
    ```
    

#### **访问Open WebUI**

打开浏览器，访问`http://localhost:3000`，并选择DeepSeek模型进行交互。

### **7. 验证DeepSeek**

确保DeepSeek模型正常运行。

1. 打开浏览器，访问`http://localhost:11434`，检查Ollama服务是否正常运行。
    
2. 如果一切正常，你将看到Ollama的界面。
    

### **总结**

通过以上步骤，你可以在Linux环境下使用VS Code部署本地DeepSeek模型，并通过Cline插件在VS Code中使用DeepSeek进行AI辅助编程。这将大大提高你的开发效率，帮助你快速生成高质量的代码。