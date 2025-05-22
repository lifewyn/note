---
time: 2025-04-25
tags:
  - linux
---
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/source$ aarch64-linux-gnu-gcc -o json_test json_test.c -I/usr/include/cjson -lcjson
json_test.c:2:10: fatal error: cjson/cJSON.h: No such file or directory
 #include <cjson/cJSON.h>
          ^~~~~~~~~~~~~~~
compilation terminated.
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/source$ find /usr/local/include /usr/include -name cJSON.h
/usr/include/cjson/cJSON.h
ocean@ocean-VirtualBox:/opt/tempc_moniter_system/source$

根据你的输出，`cJSON.h` 文件确实存在于 `/usr/include/cjson/cJSON.h`。这意味着头文件路径是正确的，但编译器仍然无法找到它。这可能是因为你的交叉编译器 `aarch64-linux-gnu-gcc` 并未正确配置为查找 `/usr/include` 目录下的头文件。

### 解决方法

#### 方法一：直接指定头文件路径
既然你已经确认了头文件的路径，可以直接在代码中使用绝对路径来包含头文件。修改你的代码如下：
```c
#include "/usr/include/cjson/cJSON.h"
```
然后重新编译：
```bash
aarch64-linux-gnu-gcc -o json_test json_test.c -lcjson
```

这种方法简单直接，但不推荐长期使用，因为它会降低代码的可移植性。

#### 方法二：确保交叉编译器查找正确的头文件路径
交叉编译器可能不会自动查找 `/usr/include` 目录下的头文件。你可以通过以下方式确保交叉编译器查找正确的路径：

1. **指定头文件路径**  
   使用 `-I` 选项明确指定头文件路径：
   ```bash
   aarch64-linux-gnu-gcc -o json_test json_test.c -I/usr/include -lcjson
   ```

2. **检查交叉编译器的头文件路径**  
   你可以通过以下命令查看交叉编译器的头文件搜索路径：
   ```bash
   aarch64-linux-gnu-gcc -E -x c - -v < /dev/null
   ```
   在输出中查找 `#include <...> search starts here:` 部分，确认 `/usr/include` 是否在搜索路径中。如果不在，你需要手动指定。

#### 方法三：检查交叉编译环境的头文件路径
如果你使用的是交叉编译环境，可能需要将头文件复制到交叉编译环境的对应路径下。例如，如果你的交叉编译环境的头文件路径是 `/usr/aarch64-linux-gnu/include`，你可以将头文件复制过去：
```bash
sudo mkdir -p /usr/aarch64-linux-gnu/include/cjson
sudo cp /usr/include/cjson/cJSON.h /usr/aarch64-linux-gnu/include/cjson/
```
然后重新编译：
```bash
aarch64-linux-gnu-gcc -o json_test json_test.c -I/usr/aarch64-linux-gnu/include -lcjson
```

### 总结
根据你的问题，最可能的原因是交叉编译器未正确配置为查找 `/usr/include` 目录下的头文件。你可以尝试以下步骤：
1. 直接在代码中使用绝对路径包含头文件。
2. 使用 `-I` 选项明确指定头文件路径。
3. 检查交叉编译器的头文件搜索路径，并确保 `/usr/include` 在搜索路径中。
4. 如果使用交叉编译环境，将头文件复制到交叉编译环境的对应路径下。

希望这些方法能帮助你解决问题！