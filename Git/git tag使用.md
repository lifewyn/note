---
time: 2025-02-14
tags:
  - git
---
`git tag` 是 Git 中的一个命令，用于创建、列出、删除或验证标签（tag）。标签是一种引用，通常用来标记某个特定的提交（commit），例如用于版本发布（如 v1.0、v2.0 等）。以下是 `git tag` 的常见用法：

### 1. **创建标签**

- **轻量级标签（Lightweight Tag）**：  
    轻量级标签只是一个简单的引用，指向某个提交。
    
    bash复制
    
    ```bash
    git tag <tagname> [<commit>]
    ```
    
    例如：
    
    bash复制
    
    ```bash
    git tag v1.0
    ```
    
    如果不指定提交，默认会标记当前的 HEAD。
    
- **带注释的标签（Annotated Tag）**：  
    带注释的标签会存储更多的信息，例如标签的创建者、日期和注释内容。
    
    bash复制
    
    ```bash
    git tag -a <tagname> -m "message" [<commit>]
    ```
    
    例如：
    
    bash复制
    
    ```bash
    git tag -a v1.0 -m "Release version 1.0"
    ```
    

### 2 . **查看标签**

- 列出所有标签：
    
    bash复制
    
    ```bash
    git tag
    ```
    
- 按照特定模式过滤标签：
    
    bash复制
    
    ```bash
    git tag -l "v1.*"
    ```
    

### 3. **推送标签到远程仓库**

- 推送单个标签：
    
    bash复制
    
    ```bash
    git push <remote> <tagname>
    ```
    
    例如：
    
    bash复制
    
    ```bash
    git push origin v1.0
    ```
    
- 推送所有标签：
    
    bash复制
    
    ```bash
    git push --tags
    ```
    

### 4. **删除标签**

- **本地删除标签**：
    
    bash复制
    
    ```bash
    git tag -d <tagname>
    ```
    
    例如：
    
    bash复制
    
    ```bash
    git tag -d v1.0
    ```
    
- **远程删除标签**：
    
    bash复制
    
    ```bash
    git push <remote> --delete <tagname>
    ```
    
    例如：
    
    bash复制
    
    ```bash
    git push origin --delete v1.0
    ```
    

### 5. **查看标签信息**

- 查看标签的详细信息（特别是带注释的标签）：
    
    bash复制
    
    ```bash
    git show <tagname>
    ```
    

### 6. **其他用途**

- **从标签检出代码**：  
    如果需要从某个标签检出代码，可以使用：
    
    bash复制
    
    ```bash
    git checkout <tagname>
    ```
    
    注意：这会将你的工作目录切换到一个“分离头指针”状态，建议创建一个新的分支来操作：
    
    bash复制
    
    ```bash
    git checkout -b <branchname> <tagname>
    ```
    

### 总结

`git tag` 是一个非常有用的工具，尤其在版本发布时，可以帮助你标记重要的提交点。带注释的标签比轻量级标签更推荐，因为它包含了更多有用的信息，方便后续的版本管理和追溯。