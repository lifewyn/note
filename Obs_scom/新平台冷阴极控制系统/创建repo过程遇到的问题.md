---
time: 2025-03-25
tags:
  - scom
---



```plaintext
HOSTCC scripts/asn1_compiler HOSTCXX -fPIC scripts/gcc-plugins/arm_ssp_per_task_plugin.o HOSTCC scripts/extract-cert /opt/TQT113_linux_V2.0/kernel/linux-5.4/scripts/extract-cert.c:21:10: fatal error: openssl/bio.h: No such file or directory 21 | #include <openssl/bio.h> | ^~~~~~~~~~~~~~~ compilation terminated. make[2]: *** [scripts/Makefile.host:107: scripts/extract-cert] Error 1 make[2]: *** Waiting for unfinished jobs.... HOSTLLD -shared scripts/gcc-plugins/arm_ssp_per_task_plugin.so make[1]: *** [/opt/TQT113_linux_V2.0/kernel/linux-5.4/Makefile:1199: scripts] Error 2 make[1]: Leaving directory '/opt/TQT113_linux_V2.0/out/t113_s4/kernel/build' make: *** [Makefile:179: sub-make] Error 2 ERROR: build Failed INFO: build kernel failed
```


根据你提供的错误信息，编译 Linux 内核时出现以下问题：

```
scripts/extract-cert.c:21:10: fatal error: openssl/bio.h: No such file or directory
```

这表明编译过程中缺少 `openssl/bio.h` 头文件，通常是因为系统中未安装 OpenSSL 开发包。

### 解决方法
根据你的系统类型，安装对应的 OpenSSL 开发包：

- **对于基于 Debian 的系统（如 Ubuntu）**：
  ```bash
  sudo apt install libssl-dev
  ```
  如果你的系统是 Ubuntu 18.04 或更早版本，可能需要安装 `libssl1.0-dev`。

- **对于基于 Red Hat 的系统（如 CentOS 或 Fedora）**：
  - 对于 CentOS 或旧版 Fedora：
    ```bash
    sudo yum install openssl-devel
    ```
  - 对于新版 Fedora：
    ```bash
    sudo dnf install openssl-devel
    ```

安装完成后，重新运行编译命令，问题应该会得到解决。

### 其他注意事项
如果安装了正确的 OpenSSL 开发包后问题仍然存在，可能需要检查以下内容：
1. 确保安装的 OpenSSL 版本与你的系统和编译环境兼容。
2. 如果你使用的是自定义的编译环境或交叉编译工具链，确保工具链能够正确找到安装的 OpenSSL 头文件。

