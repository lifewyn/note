

ocean@ocean-VirtualBox:/opt/tempc_moniter_system/source$ aarch64-linux-gnu-gcc -o json_test json_test.c -I/usr/include/cjson -lcjson json_test.c:2:10: fatal error: cjson/cJSON.h: No such file or directory #include <cjson/cJSON.h> ^~~~~~~~~~~~~~~ compilation terminated. ocean@ocean-VirtualBox:/opt/tempc_moniter_system/source$

从你的描述来看，问题可能出在以下几个方面：

1. **头文件路径问题**：即使你指定了`-I/usr/include/cjson`，编译器仍然无法找到`cjson/cJSON.h`。这可能是因为头文件的实际路径与你指定的路径不匹配。
2. **头文件的实际位置**：需要确认`cJSON.h`文件的确切位置。

### 步骤一：确认头文件的实际位置
首先，确认`cJSON.h`文件的确切位置。运行以下命令：
```bash
find /usr/local/include /usr/include -name cJSON.h
```
这个命令会搜索`/usr/local/include`和`/usr/include`目录下是否存在`cJSON.h`文件。

### 可能的结果
1. **如果找到了`cJSON.h`文件**：
   - 假设输出是`/usr/include/cjson/cJSON.h`，说明头文件确实存在，但路径可能没有正确指定。
   - 重新指定头文件路径：
     ```bash
     aarch64-linux-gnu-gcc -o json_test json_test.c -I/usr/include -lcjson
     ```
     注意：这里直接指定`-I/usr/include`，而不是`-I/usr/include/cjson`。

2. **如果没有找到`cJSON.h`文件**：
   - 说明头文件未正确安装。你需要重新安装cJSON库。

### 步骤二：重新安装cJSON库
如果头文件未正确安装，可以按照以下步骤重新安装cJSON库：

#### 1. 克隆cJSON源码
```bash
git clone https://github.com/DaveGamble/cJSON.git
cd cJSON
```

#### 2. 编译并安装
```bash
make clean
make
sudo make install
```

#### 3. 验证头文件位置
重新运行以下命令，确认头文件是否已正确安装：
```bash
find /usr/local/include /usr/include -name cJSON.h
```

### 步骤三：重新编译
根据步骤一的结果，重新编译你的程序：
```bash
aarch64-linux-gnu-gcc -o json_test json_test.c -I/usr/include -lcjson
```

### 注意事项
1. **头文件路径问题**：
   - 如果头文件路径是`/usr/include/cjson`，那么在`#include`指令中应该写为`#include <cjson/cJSON.h>`，并且在编译时指定`-I/usr/include`。
   - 如果头文件路径是`/usr/local/include/cjson`，那么在`#include`指令中应该写为`#include <cjson/cJSON.h>`，并且在编译时指定`-I/usr/local/include`。

2. **交叉编译环境**：
   - 你使用的是`aarch64-linux-gnu-gcc`，这是一个交叉编译器。确保cJSON库也安装在交叉编译环境的正确路径下。如果cJSON库安装在宿主机的路径下，可能需要手动将头文件和库文件复制到交叉编译环境的对应路径下。

### 总结
1. 确认头文件的实际位置。
2. 根据头文件的实际位置，调整编译命令中的`-I`选项。
3. 如果头文件未安装，重新安装cJSON库。

希望这些步骤能帮助你解决问题！